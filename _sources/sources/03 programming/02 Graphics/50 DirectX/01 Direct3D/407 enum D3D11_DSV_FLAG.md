# enum D3D11_DSV_FLAG
D3D11_DSV_FLAG enum은 깊이 스텐실 뷰(Depth-Stencil View, DSV)의 특성을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_DSV_FLAG
    {
        D3D11_DSV_READ_ONLY_DEPTH       = 0x1L,
        D3D11_DSV_READ_ONLY_STENCIL     = 0x2L
    } 	D3D11_DSV_FLAG;
```
각 enum의 의미는 다음과 같다.

* D3D11_DSV_READ_ONLY_DEPTH
  * 깊이 데이터가 읽기 전용으로 설정된다.
* D3D11_DSV_READ_ONLY_STENCIL
  * 스텐실 데이터가 읽기 전용으로 설정된다.
