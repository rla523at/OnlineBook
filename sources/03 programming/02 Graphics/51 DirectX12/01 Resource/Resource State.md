# Resource State
GPU 의 관점에서 현재 Resource 가 어떤 상태인지를 나타내는 enum 이다.

> Reference  
> [learn.microsoft - d3d12_resource_states](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_states)  

## D3D12_RESOURCE_STATE_COMMON

<details> <summary> <h3 style="display:inline-block"> Copy Queue, Copy Command List </h3></summary>

* https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_states
  * Your application should transition to this state only for accessing a resource across different graphics engine types.
  * Specifically, a resource must be in the COMMON state before being used on a COPY queue (when previously used on DIRECT/COMPUTE), and before being used on DIRECT/COMPUTE (when previously used on COPY). This restriction doesn't exist when accessing data between DIRECT and COMPUTE queues.
  * The COMMON state can be used for all usages on a Copy queue using the implicit state transitions.

</details>

## D3D12_RESOURCE_STATE_RENDER_TARGET
ID3D12GraphicsCommandList::ClearRenderTargetView 함수를 호출하기 전에 Resource State 가 D3D12_RESOURCE_STATE_RENDER_TARGET 여야 한다.
* [learn.microsoft - id3d12graphicscommandlist-clearrendertargetview](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-clearrendertargetview#debug-layer)  
  * The debug layer will issue an error if the subresources referenced by the view are not in the appropriate state. For ClearRenderTargetView, the state must be D3D12_RESOURCE_STATE_RENDER_TARGET.

## D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER vs D3D12_RESOURCE_STATE_GENERIC_READ
단순히 Vertex, Constant Buffer 를 위한 Read Only resource 를 만들기 위해서라면 어느 것을 사용해도 기능적으로 큰 차이는 없다.


<details> <summary> <h3 style="display:inline-block"> 두 상태의 의미 </h3></summary>
1. **D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER**  
   말 그대로 “정점 버퍼(vertex buffer) 또는 상수 버퍼(constant buffer)로 GPU가 읽을 준비가 된 상태”를 의미합니다.  
   - 이 상태로 설정해두면, 파이프라인에서 이 리소스를 VSSetConstantBuffers나 IASetVertexBuffers 같은 명령으로 그대로 사용할 수 있습니다.  
   - 다른 읽기 용도(Index Buffer, Shader Resource, Copy Source 등)로 사용하려면 리소스 배리어(transition)를 거쳐 다른 상태로 전환해야 합니다.

2. **D3D12_RESOURCE_STATE_GENERIC_READ**  
   여러 “읽기 전용” 상태를 묶어놓은 상위(슈퍼셋) 상태입니다.  
   - VERTEX_AND_CONSTANT_BUFFER, INDEX_BUFFER, NON_PIXEL_SHADER_RESOURCE, PIXEL_SHADER_RESOURCE, INDIRECT_ARGUMENT, COPY_SOURCE 등의 읽기 용도를 전부 포괄합니다.  
   - 즉, “GPU 쪽에서 읽기 전용으로 사용”하기 위한 대부분의 상황을 한 번에 커버합니다.
</details>


<details> <summary> <h3 style="display:inline-block"> 어떤 힙(Heap)에서 주로 쓰이는가? </h3></summary>
- **업로드 힙(Upload Heap)**:  
  CPU가 데이터를 자주 갱신하는(업로드하는) 용도로 주로 사용합니다. 업로드 힙은 항상 D3D12_HEAP_TYPE_UPLOAD 속성을 갖고, 리소스 상태로 D3D12_RESOURCE_STATE_GENERIC_READ를 사용합니다.  
  - Vertex/Constant 용도로만 쓰더라도 굳이 상태를 VERTEX_AND_CONSTANT_BUFFER로 바꿀 필요가 없습니다.

- **디폴트 힙(Default Heap)**:  
  GPU가 주로 접근(읽고 쓰는)을 담당하는 힙입니다. 여기서는 리소스를 만들 때 초기 상태를 지정한 후, 파이프라인 사용 시점에 따라 적절히 D3D12_RESOURCE_BARRIER로 상태 전환을 해줘야 합니다.  
  - 만약 “오직 정점/상수 버퍼”로만 사용할 예정이라면, 미리 D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER로 세팅하거나, 혹은 필요 시점에 그 상태로 전환해서 사용합니다.  
  - 여러 가지 용도(복사, 쉐이더 리소스, 인덱스 버퍼 등)로 쓰인다면, GENERIC_READ 같은 상위 상태를 사용하거나, 필요에 따라 상태를 바꿔가며 쓸 수 있습니다.
</details>


<details> <summary> <h3 style="display:inline-block"> 실제로는 어떤 방식을 많이 쓰나요? </h3></summary>
- **업로드 힙으로 정점/상수 데이터를 올리는 경우**  
  보통 리소스를 생성할 때 `D3D12_RESOURCE_STATE_GENERIC_READ`로 두고(혹은 생성 시 `D3D12_HEAP_TYPE_UPLOAD`이면 드라이버에서 자동으로 세팅), CPU에서 데이터를 업데이트한 뒤에 GPU에 넘깁니다.  
  - 이 때 GPU에서 쓸 때도 별도 상태 전환 없이 그대로 `VERTEX/CONSTANT BUFFER`로 읽습니다(“GENERIC_READ ⊇ VERTEX_AND_CONSTANT_BUFFER” 관계).  

- **디폴트 힙에 두고, 초기에 업로드 → 이후에 정점/상수만 쓰는 경우**  
  1. 업로드 힙에서 디폴트 힙으로 복사(Copy) 작업을 진행할 때는 `COPY_DEST` 상태로 전환해둡니다.  
  2. 복사가 끝나면, 실제 렌더링에서 필요한 상태(`VERTEX_AND_CONSTANT_BUFFER`)로 전환합니다.  
  3. 이후 계속 정점/상수 버퍼 용도로만 쓸 예정이라면 굳이 다른 상태로 전환할 일은 많지 않습니다.
</details>


## Resource Barrier
GPU 명령 스트림에 삽입되어 리소스(resource)의 “상태(state)” 전이를 안전하게 관리하고, 파이프라인 내 읽기·쓰기 충돌(hazard)을 방지하도록 도와주는 수단이다.

왜 필요한가
* GPU는 한 리소스를 여러 용도로 사용할 수 있다. 예를 들어, 같은 텍스처를 “렌더 타깃”으로 쓰다가 “셰이더 리소스”로 읽기도 한다.
* 하지만 이러한 용도 전환이 파이프라인 내부에서 적절히 동기화되지 않으면, “읽기 중에 쓰기” 혹은 “쓰는 중에 읽기” 같은 레이스 컨디션이 발생할 수 있다.
* **ResourceBarrier** 는 이런 충돌 지점을 명시적으로 표시해 주어, Direct3D 12가 내부에서 필요한 파이프라인 플러시나 캐시 무효화 등 동기화 작업을 수행하게 만든다.

주요 타입
1. **Transition Barrier**
   * 리소스의 사용 목적(state)을 변경할 때 사용
   * 예: 렌더 타깃 → 셰이더 리소스, 복사 대상 → 복사 소스 등
   * 구조체 예시
     ```cpp
     D3D12_RESOURCE_BARRIER barrier = {};
     barrier.Type                   = D3D12_RESOURCE_BARRIER_TYPE_TRANSITION;
     barrier.Flags                  = D3D12_RESOURCE_BARRIER_FLAG_NONE;
     barrier.Transition.pResource   = myTexture;
     barrier.Transition.Subresource = D3D12_RESOURCE_BARRIER_ALL_SUBRESOURCES;
     barrier.Transition.StateBefore = D3D12_RESOURCE_STATE_RENDER_TARGET;
     barrier.Transition.StateAfter  = D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE;
     ```
2. **UAV Barrier**
   * Unordered Access View(UAV) 리소스에 대한 쓰기·읽기 충돌을 막기 위해 사용
   * 특정 리소스가 아닌, *모든* UAV 쓰기 작업 사이의 순서를 보장
   * 구조체 필드:
     ```cpp
     barrier.Type = D3D12_RESOURCE_BARRIER_TYPE_UAV;
     barrier.UAV.pResource = myUAVResource;  // nullptr 가능
     ```

3. **Aliasing Barrier**
   * 서로 다른 리소스가 같은 힙(heap) 영역을 공유(alias)할 때 사용
   * 이전 리소스 사용 이후 다음 리소스로 전환할 때 필요한 동기화를 수행


효과와 주의점
* **효과**: GPU 내부 캐시 무효화, 명령 순서 보장, 메모리 가시성 보장
* **주의**: 불필요하게 자주 삽입하면 성능 저하 발생
  * 상태가 이미 일치(stateBefore == stateAfter)하면 barrier를 생략
  * 여러 전이를 묶어서 한 번에 호출하는 것이 좋음

> Reference  
> [learn.microsoft - using-resource-barriers-to-synchronize-resource-states-in-direct3d-12](https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-resource-barriers-to-synchronize-resource-states-in-direct3d-12)  


### Aliasing Barrier
* D3D12에서는 고급 기능으로, 여러 리소스(예: 텍스처, 버퍼)를 같은 Heap(메모리) 상의 겹치는 범위를 통해 할당**(Alias)**할 수 있습니다.
* 왜 필요한가?
  * 대용량 텍스처 A가 필요할 때와 텍스처 B가 필요할 때가 완전히 시간적으로 분리되어 있다면, 동일 메모리 공간을 재사용하여 메모리 사용량을 절약 가능.
  * 하지만 GPU 입장에서, “이 메모리를 지금 A라는 리소스로 사용 중인 것인지, 아니면 B라는 리소스로 사용할 차례인지” 정확히 알아야 안전하게 파이프라인 동작을 보장할 수 있습니다.
  * 따라서 “이제부터는 텍스처 A 대신 텍스처 B가 이 물리 메모리를 사용할 것”이라는 것을 GPU에게 알려줘야 하는데, 이것을 위해 Aliasing Barrier를 사용합니다.

> Reference  
> [learn.microsoft - using-resource-barriers-to-synchronize-resource-states-in-direct3d-12](https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-resource-barriers-to-synchronize-resource-states-in-direct3d-12)  


### Implicit State Transitions

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

* All buffer resources as well as textures with the D3D12_RESOURCE_FLAG_ALLOW_SIMULTANEOUS_ACCESS flag set are implicitly promoted from D3D12_RESOURCE_STATE_COMMON to the relevant state on first GPU access, including GENERIC_READ to cover any read scenario. Any resource in the COMMON state can be accessed as through it were in a single state with 1 WRITE flag, or 1 or more READ flag    

> Reference  
> [learn.microsoft - using-resource-barriers-to-synchronize-resource-states-in-direct3d-12](https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-resource-barriers-to-synchronize-resource-states-in-direct3d-12)  




<details> <summary> <h4 style="display:inline-block"> Simultaneous-Access Textures </h3></summary>

Resource 를 설정할 떄, D3D12_RESOURCE_FLAG_ALLOW_SIMULTANEOUS_ACCESS 를 사용하면 된다.

* https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_flags
  * Allows a resource to be simultaneously accessed by multiple different queues, devices, or processes
  * Simultaneous access allows multiple readers and one writer, as long as the writer doesn't concurrently modify the texels that other readers are accessing.
  * your application should avoid setting this flag when multiple readers are not required during frequent, non-overlapping writes to textures. Use of this flag can compromise resource fences to perform waits, and prevent any compression being used with a resource.
  * The following restrictions and interactions apply:
    * Can't be used with D3D12_RESOURCE_DIMENSION_BUFFER; but buffers always have the properties represented by this flag.
    * Can't be used with MSAA textures.
    * Can't be used with D3D12_RESOURCE_FLAGS::D3D12_RESOURCE_FLAG_ALLOW_DEPTH_STENCIL.

</details>




### Enhanced Barrier

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

