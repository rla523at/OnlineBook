# enum D3D11_STENCIL_OP
D3D11_STENCIL_OP enum은 Direct3D 11에서 스텐실 버퍼의 값을 업데이트할 때 수행되는 작업을 정의하는 열거형이다.

이 열거형은 스텐실 테스트의 결과에 따라 스텐실 버퍼의 값을 어떻게 변경할지를 지정한다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_STENCIL_OP
    {
        D3D11_STENCIL_OP_KEEP     = 1,
        D3D11_STENCIL_OP_ZERO     = 2,
        D3D11_STENCIL_OP_REPLACE  = 3,
        D3D11_STENCIL_OP_INCR_SAT = 4,
        D3D11_STENCIL_OP_DECR_SAT = 5,
        D3D11_STENCIL_OP_INVERT   = 6,
        D3D11_STENCIL_OP_INCR     = 7,
        D3D11_STENCIL_OP_DECR     = 8
    } 	D3D11_STENCIL_OP;
```
각 enum의 의미는 다음과 같다.

* D3D11_STENCIL_OP_KEEP
  * 스텐실 값을 유지한다.
* D3D11_STENCIL_OP_ZERO
  * 스텐실 값을 0으로 설정한다.
* D3D11_STENCIL_OP_REPLACE
  * 스텐실 값을 참조 값으로 대체한다.
* D3D11_STENCIL_OP_INCR_SAT
  * 스텐실 값을 1만큼 증가시키되, 최대값을 넘지 않도록 한다.
* D3D11_STENCIL_OP_DECR_SAT
  * 스텐실 값을 1만큼 감소시키되, 0보다 작아지지 않도록 한다.
* D3D11_STENCIL_OP_INVERT
  * 스텐실 값의 비트를 반전시킨다.
* D3D11_STENCIL_OP_INCR
  * 스텐실 값을 1만큼 증가시킨다. 값이 최대값을 넘으면 0으로 순환한다.
* D3D11_STENCIL_OP_DECR
  * 스텐실 값을 1만큼 감소시킨다. 값이 0보다 작아지면 최대값으로 순환한다.
