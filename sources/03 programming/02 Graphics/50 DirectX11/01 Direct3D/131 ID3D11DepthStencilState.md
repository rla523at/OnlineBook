# ID3D11DepthStencilState
ID3D11DepthStencilState는 Direct3D 11에서 깊이 및 스텐실 테스트 상태를 관리하는 인터페이스다. 

깊이 테스트는 렌더링 과정에서 픽셀의 깊이 값을 비교하여, 더 가까운 객체가 화면에 렌더링되도록 한다. 이를 통해 장면의 깊이 정보를 정확하게 유지할 수 있다. 예를 들어, 캐릭터가 건물 앞에 서 있을 때, 깊이 테스트를 통해 캐릭터가 건물보다 앞에 렌더링되도록 한다.

스텐실 테스트는 특정 영역에서 픽셀 렌더링을 제어하는 데 사용된다. 스텐실 버퍼는 픽셀별로 값이 저장되며, 이 값과 지정된 스텐실 연산을 통해 픽셀이 그려질지 결정한다. 이는 그림자, 반사, 또는 복잡한 마스킹 효과를 구현하는 데 유용하다.

## D3D11_DEPTH_STENCIL_DESC 구조체
ID3D11DepthStencilState를 생성하기 위해서는 D3D11_DEPTH_STENCIL_DESC 구조체를 사용하여 상태를 정의해야 한다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_DEPTH_STENCIL_DESC {
    BOOL DepthEnable;
    D3D11_DEPTH_WRITE_MASK DepthWriteMask;
    D3D11_COMPARISON_FUNC DepthFunc;
    BOOL StencilEnable;
    UINT8 StencilReadMask;
    UINT8 StencilWriteMask;
    D3D11_DEPTH_STENCILOP_DESC FrontFace;
    D3D11_DEPTH_STENCILOP_DESC BackFace;
} D3D11_DEPTH_STENCIL_DESC;
```

각 멤버변수는 다음과 같다.

* BOOL DepthEnable
  * 깊이 테스트를 활성화할지 여부를 지정한다.
  * 가능한 값
      * TRUE: 깊이 테스트를 활성화한다.
      * FALSE: 깊이 테스트를 비활성화한다.
  * 기본값은 TRUE이다.

* D3D11_DEPTH_WRITE_MASK DepthWriteMask
  * 깊이 값이 쓰여질 수 있는지 여부를 지정한다.
  * 가능한 값
      * D3D11_DEPTH_WRITE_MASK_ZERO: 깊이 값이 쓰여지지 않도록 설정한다.
      * D3D11_DEPTH_WRITE_MASK_ALL: 깊이 값이 쓰여지도록 설정한다.
  * 기본값은 D3D11_DEPTH_WRITE_MASK_ALL이다.

* D3D11_COMPARISON_FUNC DepthFunc
  * 깊이 값을 비교할 함수를 지정한다.
  * 가능한 값
    * enum D3D11_COMPARISON_FUNC 값
  * 기본값은 D3D11_COMPARISON_LESS이다.

* BOOL StencilEnable
  * 스텐실 테스트를 활성화할지 여부를 지정한다.
  * 가능한 값
      * TRUE: 스텐실 테스트를 활성화한다.
      * FALSE: 스텐실 테스트를 비활성화한다.
  * 기본값은 FALSE이다.

* UINT8 StencilReadMask
  * 스텐실 테스트에서 읽기 마스크를 지정한다. 각 비트가 1이면 해당 비트를 테스트에 포함시킨다.
  * 기본값은 0xFF이다.

* UINT8 StencilWriteMask
  * 스텐실 테스트에서 쓰기 마스크를 지정한다. 각 비트가 1이면 해당 비트가 쓸 수 있다.
  * 기본값은 0xFF이다.

* D3D11_DEPTH_STENCILOP_DESC FrontFace
  * 전면 삼각형에 대한 스텐실 테스트 설정을 지정한다.

* D3D11_DEPTH_STENCILOP_DESC BackFace
  * 후면 삼각형에 대한 스텐실 테스트 설정을 지정한다.

## D3D11_DEPTH_STENCILOP_DESC 구조체
D3D11_DEPTH_STENCILOP_DESC 구조체는 스텐실 테스트에서 사용할 스텐실 연산을 정의한다. 

FrontFace와 BackFace 멤버는 각각 전면 및 후면 삼각형에 대해 별도의 설정을 할 수 있다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_DEPTH_STENCILOP_DESC {
    D3D11_STENCIL_OP StencilFailOp;
    D3D11_STENCIL_OP StencilDepthFailOp;
    D3D11_STENCIL_OP StencilPassOp;
    D3D11_COMPARISON_FUNC StencilFunc;
} D3D11_DEPTH_STENCILOP_DESC;
```

각 멤버변수는 다음과 같다.

* D3D11_STENCIL_OP StencilFailOp
  * 스텐실 테스트에 실패할 때 수행할 작업을 지정한다.
  * 기본값은 D3D11_STENCIL_OP_KEEP이다.

* D3D11_STENCIL_OP StencilDepthFailOp
  * 깊이 테스트에 실패할 때 수행할 작업을 지정한다.
  * 기본값은 D3D11_STENCIL_OP_KEEP이다.

* D3D11_STENCIL_OP StencilPassOp
  * 깊이 및 스텐실 테스트에 모두 성공할 때 수행할 작업을 지정한다.
  * 기본값은 D3D11_STENCIL_OP_KEEP이다.

* D3D11_COMPARISON_FUNC StencilFunc
  * 스텐실 값을 비교할 함수를 지정한다.
  * 기본값은 D3D11_COMPARISON_ALWAYS이다.