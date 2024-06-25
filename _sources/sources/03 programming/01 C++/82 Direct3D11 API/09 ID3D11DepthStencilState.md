# ID3D11DepthStencilState
ID3D11DepthStencilState는 Direct3D 11에서 깊이-스텐실 테스트 상태를 관리하는 데 사용되는 인터페이스다. 

이 인터페이스는 깊이 테스트와 스텐실 테스트의 동작을 정의하는 다양한 설정을 포함하며, 렌더링 파이프라인에서 깊이 및 스텐실 테스트가 어떻게 수행될지를 결정한다.

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

## D3D11_STENCIL_OP enum

D3D11_STENCIL_OP enum의 정의는 다음과 같다.
```cpp
typedef 
enum D3D11_STENCIL_OP
    {
        D3D11_STENCIL_OP_KEEP	= 1,
        D3D11_STENCIL_OP_ZERO	= 2,
        D3D11_STENCIL_OP_REPLACE	= 3,
        D3D11_STENCIL_OP_INCR_SAT	= 4,
        D3D11_STENCIL_OP_DECR_SAT	= 5,
        D3D11_STENCIL_OP_INVERT	= 6,
        D3D11_STENCIL_OP_INCR	= 7,
        D3D11_STENCIL_OP_DECR	= 8
    } 	D3D11_STENCIL_OP;
```

각 enum의 의미는 다음과 같다.
* D3D11_STENCIL_OP_KEEP: 현재 스텐실 값을 유지한다.
* D3D11_STENCIL_OP_ZERO: 스텐실 값을 0으로 설정한다.
* D3D11_STENCIL_OP_REPLACE: 참조 값으로 스텐실 값을 대체한다.
* D3D11_STENCIL_OP_INCR_SAT: 스텐실 값을 증가시키고, 최대값을 넘지 않도록 한다.
* D3D11_STENCIL_OP_DECR_SAT: 스텐실 값을 감소시키고, 0보다 작아지지 않도록 한다.
* D3D11_STENCIL_OP_INVERT: 스텐실 값을 반전시킨다.
* D3D11_STENCIL_OP_INCR: 스텐실 값을 증가시킨다. 최대값을 넘으면 0으로 순환한다.
* D3D11_STENCIL_OP_DECR: 스텐실 값을 감소시킨다. 0보다 작아지면 최대값으로 순환한다.

## D3D11_COMPARISON_FUNC enum
D3D11_COMPARISON_FUNC enum의 정의는 다음과 같다.

```cpp
typedef 
enum D3D11_COMPARISON_FUNC
    {
        D3D11_COMPARISON_NEVER	= 1,
        D3D11_COMPARISON_LESS	= 2,
        D3D11_COMPARISON_EQUAL	= 3,
        D3D11_COMPARISON_LESS_EQUAL	= 4,
        D3D11_COMPARISON_GREATER	= 5,
        D3D11_COMPARISON_NOT_EQUAL	= 6,
        D3D11_COMPARISON_GREATER_EQUAL	= 7,
        D3D11_COMPARISON_ALWAYS	= 8
    } 	D3D11_COMPARISON_FUNC;
```

각 enum의 의미는 다음과 같다.
* D3D11_COMPARISON_NEVER: 절대 실패한다.
* D3D11_COMPARISON_LESS: 참조 값이 스텐실 값보다 작을 때 성공한다.
* D3D11_COMPARISON_EQUAL: 참조 값이 스텐실 값과 같을 때 성공한다.
* D3D11_COMPARISON_LESS_EQUAL: 참조 값이 스텐실 값보다 작거나 같을 때 성공한다.
* D3D11_COMPARISON_GREATER: 참조 값이 스텐실 값보다 클 때 성공한다.
* D3D11_COMPARISON_NOT_EQUAL: 참조 값이 스텐실 값과 다를 때 성공한다.
* D3D11_COMPARISON_GREATER_EQUAL: 참조 값이 스텐실 값보다 크거나 같을 때 성공한다.
* D3D11_COMPARISON_ALWAYS: 항상 성공한다.