# ID3D11RasterizerState
ID3D11RasterizerState는 Direct3D 11에서 래스터화 단계의 동작을 제어하는 상태 객체이다. 

래스터화 단계는 기하학적 프리미티브(예: 삼각형)에 속하는 픽셀을 결정하는 과정으로, 이 단계에서 다양한 렌더링 옵션을 설정할 수 있다. 

ID3D11RasterizerState를 사용하여 이 과정에서의 다양한 동작을 정의하고 제어할 수 있다.

## D3D11_RASTERIZER_DESC 구조체
D3D11_RASTERIZER_DESC 구조체는 Direct3D 11에서 래스터화 상태를 기술하기 위한 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct D3D11_RASTERIZER_DESC {
    D3D11_FILL_MODE FillMode;
    D3D11_CULL_MODE CullMode;
    BOOL FrontCounterClockwise;
    INT DepthBias;
    FLOAT DepthBiasClamp;
    FLOAT SlopeScaledDepthBias;
    BOOL DepthClipEnable;
    BOOL ScissorEnable;
    BOOL MultisampleEnable;
    BOOL AntialiasedLineEnable;
} D3D11_RASTERIZER_DESC;
```

각각의 멤버변수는 다음과 같다.

* D3D11_FILL_MODE FillMode
  * 다각형의 채움 모드를 지정한다.
  * 가능한 값
      * D3D11_FILL_WIREFRAME: 와이어프레임 모드로 그린다.
      * D3D11_FILL_SOLID: 솔리드 모드로 그린다.
  * 기본값은 D3D11_FILL_SOLID이다.

* D3D11_CULL_MODE CullMode
  * 제거할 다각형의 면을 지정한다.
  * 가능한 값
      * D3D11_CULL_NONE: 제거하지 않는다.
      * D3D11_CULL_FRONT: 앞면을 제거한다.
      * D3D11_CULL_BACK: 뒷면을 제거한다.
  * 기본값은 D3D11_CULL_BACK이다.

* BOOL FrontCounterClockwise
  * 앞면 다각형의 시계방향 여부를 지정한다. TRUE면 반시계방향, FALSE면 시계방향이다.
  * 기본값은 FALSE이다.

* INT DepthBias
  * 깊이 바이어스를 설정한다. 깊이 값을 일정하게 오프셋한다.
  * 기본값은 0이다.

* FLOAT DepthBiasClamp
  * 깊이 바이어스의 최대값을 클램프한다.
  * 기본값은 0.0f이다.

* FLOAT SlopeScaledDepthBias
  * 기울기에 따라 깊이 바이어스를 스케일링한다.
  * 기본값은 0.0f이다.

* BOOL DepthClipEnable
  * 깊이 클립을 활성화할지 여부를 지정한다.
  * 기본값은 TRUE이다.

* BOOL ScissorEnable
  * 시저링(Scissoring)을 활성화할지 여부를 지정한다.
  * 기본값은 FALSE이다.

* BOOL MultisampleEnable
  * 멀티샘플링을 활성화할지 여부를 지정한다.
  * 기본값은 FALSE이다.

* BOOL AntialiasedLineEnable
  * 안티앨리어싱을 활성화할지 여부를 지정한다. 이 옵션은 라인 그리기에만 적용된다.
  * 기본값은 FALSE이다.


## Multisampling in Rasterization
Resterization 단계에서 MSAA가 쓰이지 않을 떄에는 이 픽셀의 중심좌표로 삼각형안에 들어있는지 판단한다. 하지만 MSAA가 쓰일 때에는 여러 샘플링 포인트를 모두 고려하여 삼각형 안에 있는지를 판단한다.

> REFERENCE    
> [MSDN - multisample-anti-aliasing-rasterization-rules](https://learn.microsoft.com/en-us/windows/win32/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage-rules#multisample-anti-aliasing-rasterization-rules)