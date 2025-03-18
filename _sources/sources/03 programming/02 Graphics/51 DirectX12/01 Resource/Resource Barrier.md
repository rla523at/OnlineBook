# Resource Barrier

https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-resource-barriers-to-synchronize-resource-states-in-direct3d-12
* Aliasing Barrier
  * D3D12에서는 고급 기능으로, 여러 리소스(예: 텍스처, 버퍼)를 같은 Heap(메모리) 상의 겹치는 범위를 통해 할당**(Alias)**할 수 있습니다.
  * 왜 필요한가?
    * 대용량 텍스처 A가 필요할 때와 텍스처 B가 필요할 때가 완전히 시간적으로 분리되어 있다면, 동일 메모리 공간을 재사용하여 메모리 사용량을 절약 가능.
    * 하지만 GPU 입장에서, “이 메모리를 지금 A라는 리소스로 사용 중인 것인지, 아니면 B라는 리소스로 사용할 차례인지” 정확히 알아야 안전하게 파이프라인 동작을 보장할 수 있습니다.
    * 따라서 “이제부터는 텍스처 A 대신 텍스처 B가 이 물리 메모리를 사용할 것”이라는 것을 GPU에게 알려줘야 하는데, 이것을 위해 Aliasing Barrier를 사용합니다.

## Implicit State Transitions

| State flag                     | Buffers and Simultaneous-Access Textures | Non-Simultaneous-Access Textures |
|---------------------------------|-----------------------------------------|----------------------------------|
| VERTEX_AND_CONSTANT_BUFFER      | Yes                                     | No                               |
| INDEX_BUFFER                    | Yes                                     | No                               |
| RENDER_TARGET                   | Yes                                     | No                               |
| UNORDERED_ACCESS                | Yes                                     | No                               |
| DEPTH_WRITE                     | No*                                     | No                               |
| DEPTH_READ                      | No*                                     | No                               |
| NON_PIXEL_SHADER_RESOURCE       | Yes                                     | Yes                              |
| PIXEL_SHADER_RESOURCE           | Yes                                     | Yes                              |
| STREAM_OUT                      | Yes                                     | No                               |
| INDIRECT_ARGUMENT               | Yes                                     | No                               |
| COPY_DEST                       | Yes                                     | Yes                              |
| COPY_SOURCE                     | Yes                                     | Yes                              |
| RESOLVE_DEST                    | Yes                                     | No                               |
| RESOLVE_SOURCE                  | Yes                                     | No                               |
| PREDICATION                     | Yes                                     | No                               |


https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-resource-barriers-to-synchronize-resource-states-in-direct3d-12#implicit-state-transitions
* All buffer resources as well as textures with the D3D12_RESOURCE_FLAG_ALLOW_SIMULTANEOUS_ACCESS flag set are implicitly promoted from D3D12_RESOURCE_STATE_COMMON to the relevant state on first GPU access, including GENERIC_READ to cover any read scenario. Any resource in the COMMON state can be accessed as through it were in a single state with 1 WRITE flag, or 1 or more READ flag    

## Enhanced Barrier

https://microsoft.github.io/DirectX-Specs/d3d/D3D12EnhancedBarriers.html
* Excessive sync latency
  * App developers must assume that a State Transition Barrier will flush all preceding GPU work potentially-using StateBefore, and block all subsequent GPU work potentially-using StateAfter until the barrier is completed.
    * why?
      * 상황 가정
        * 이전 그래픽 파이프라인 단계에서 텍스처 T로 씬(Scene)을 렌더링 중이었다고 합시다.
        * T는 D3D12_RESOURCE_STATE_RENDER_TARGET 상태로 사용되고 있었습니다.
        * 후속 파이프라인 단계에서 T에 그려진 결과물을 화면에 합성하기 위해, T를 픽셀 쉐이더에서 샘플링하려고 합니다.
        * 따라서 T가 D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE 상태로 전환되어야 합니다.
        * 문제는 “렌더 타깃으로 쓰기가 다 끝나기도 전에, 픽셀 쉐이더에서 읽기를 시작해버리면” 데이터가 올바르지 않을 수 있다는 것입니다. 그래서 이 두 상태 전환 사이에 Barrier가 필요합니다.
      * Barrier 삽입
        * StateBefore (RENDER_TARGET) 관련 작업 flush
          * GPU는 T를 RENDER_TARGET 상태로 사용하던 모든 이전 명령이 완전히 끝날 때까지 대기합니다(= “flush”).
          * 아직 T에 픽셀을 그리는 중이었다면, 그 렌더링이 전부 완료될 때까지 GPU가 보장해줍니다.
        * StateAfter (PIXEL_SHADER_RESOURCE) 작업 block
          * Barrier가 완료되기 전까지, T를 픽셀 쉐이더에서 읽으려는(샘플링하려는) 모든 다음 명령은 시작되지 못하고 대기 상태에 있습니다(= “block”).
          * Barrier가 끝나면 T가 올바른 상태(픽셀 쉐이더 리소스)로 전환되었으므로, 그제야 안전하게 텍스처를 읽기 시작할 수 있습니다.
    * Barrier is Completed    
      * Flush 가 완료되서 Block 이 끝난 상태
      * Barrier가 “완료되었다”는 것은 GPU가 이전 상태(StateBefore)를 사용하는 모든 명령(작업)을 완전히 끝냈고, 동시에 해당 리소스가 다음 상태(StateAfter)로 안전하게 전환되었음을 보장한다는 의미
* Resource State Promotion and Decay
  * Promotion and decay reflect the natural consequences of ExecuteCommandLists boundaries.
    * Promotion
      * 리소스가 COMMON 상태에 있을 때, 읽기 전용(예: PIXEL_SHADER_RESOURCE, NON_PIXEL_SHADER_RESOURCE, COPY_SOURCE 등)으로 사용된다면, 명시적 ResourceBarrier 없이도 내부적으로 COMMON → (Read-Only State)로 “암묵적 전환”이 가능합니다.
      * 이때 개발자가 “수동으로 Transition Barrier를 호출하지 않아도” D3D12가 이 과정을 허용하는 것은, “어차피 읽기만 할 것이고 COMMON도 GPU 접근이 사실상 막혀있는 상태가 아니기 때문”입니다.
    * Decay
      * 특정 리소스가 읽기 전용 상태로 마지막에 사용되고, 해당 커맨드 리스트 안에서 이후에 더 이상 사용되지 않는다면, 커맨드 리스트가 끝나는 시점에 자동으로 COMMON 상태로 “복귀(Decay)”시킬 수 있습니다.
      * 이는 GPU가 “마지막에 읽기만 했으니, 추가 쓰기나 압축(Decompression) 없이 곧장 COMMON으로 되돌려도 무방하겠구나”라는 판단을 해주는 것입니다.
    * **“reflect the natural consequences of ExecuteCommandLists boundaries”**
      * ExecuteCommandLists 호출을 경계로 GPU가 명시적/암묵적(implicit) 파이프라인 동기화를 진행하는데, 그 과정에서 이 Promotion/Decay 규칙이 적용된다는 뜻입니다.
      * 즉, 커맨드 리스트 실행을 마무리하는 시점에서 **“마지막 접근이 읽기 전용이었다면 해당 리소스는 COMMON으로 되돌아갈 수 있다”** 라고 보면 됩니다.
  * However, some developers incorrectly assume that hidden barriers are being inserted behind the scenes. As such, it is common for promotion and decay to be ignored, resulting in excessive use of unnecessary barriers with noticeable performance impact.
    * “어차피 Promotion/Decay가 있으니, 드라이버나 D3D12 런타임이 자동으로 ResourceBarrier 같은 것을 몰래 호출해줄 거야”라고 착각하는 경우가 많습니다.
      * 하지만 실제로는 개발자가 의도하지 않은 불필요한 배리어가 자동으로 삽입되는 일이 아니라, 아주 최소한의 규칙만 적용하는 것입니다.
      * 이는 “엄밀히 말하면 배리어 호출 없이도 허용되는 최적화”이지, 엔진 레벨에서 실제 GPU 스톨(Barrier)이 실행되는 게 아니라는 점이 중요합니다.
    * Promotion/Decay를 무시하면 생기는 문제
      * 문구 후반부에서 언급했듯이, 개발자가 Promotion/Decay 규칙을 모르거나, 굳이 쓰지 않고 모든 상태 전환을 명시적으로 해버리는 경우, 성능 문제가 생길 수 있습니다.
      * 예: “리소스가 COMMON 상태인데, 단순히 텍스처를 샘플하는 PIXEL_SHADER_RESOURCE로 접근할 때조차 개발자가 매번 Transition Barrier를 건다.”
      * 사실 D3D12는 “읽기 전용 목적”이라면 Promotion이 가능해 굳이 배리어를 안 써도 괜찮은데, 매번 배리어를 선언하면 불필요한 GPU 동기화가 추가됩니다.
      * 배리어(Transition Barrier)는 GPU 파이프라인에 스톨(정지)을 유발할 수 있으므로, 배리어를 자주 사용하면 사용할수록 프레임 시간(렌더링 성능)이 저하됩니다.
      * 결과적으로 “Promotion/Decay를 잘 활용하면 애플리케이션이 자체적으로 명시적 배리어를 추가할 필요가 크게 줄어들고, 그만큼 성능도 좋아질 수 있는데, 이를 모르고/무시하고 COMMON → (Read-Only State) → COMMON을 전부 명시 배리어로 처리해버리면 ‘의미 없는 중복 배리어’가 생겨 성능에 악영향을 준다”는 것입니다.

https://ssinyoung.tistory.com/40

