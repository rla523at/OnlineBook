# enum D3D11_CULL_MODE
D3D11_CULL_MODE enum은 Direct3D 11에서 삼각형을 렌더링할 때 컬링할(그리지 않을) 삼각형의 면을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_CULL_MODE
    {
        D3D11_CULL_NONE  = 1,
        D3D11_CULL_FRONT = 2,
        D3D11_CULL_BACK  = 3
    } 	D3D11_CULL_MODE;
```
각 enum의 의미는 다음과 같다.

* D3D11_CULL_NONE
  * 삼각형의 어느 면도 컬링하지 않는다. 모든 삼각형이 렌더링된다.
* D3D11_CULL_FRONT
  * 삼각형의 앞면을 컬링한다. 앞면을 향하는 삼각형은 렌더링되지 않는다.
* D3D11_CULL_BACK
  * 삼각형의 뒷면을 컬링한다. 뒷면을 향하는 삼각형은 렌더링되지 않는다.
