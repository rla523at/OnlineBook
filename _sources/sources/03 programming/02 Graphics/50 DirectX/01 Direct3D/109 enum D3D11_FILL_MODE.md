# enum D3D11_FILL_MODE

D3D11_FILL_MODE enum은 Direct3D 11에서 폴리곤이 렌더링되는 방식을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_FILL_MODE
    {
        D3D11_FILL_WIREFRAME = 2,
        D3D11_FILL_SOLID     = 3
    } 	D3D11_FILL_MODE;
```
각 enum의 의미는 다음과 같다.

* D3D11_FILL_WIREFRAME
  * 폴리곤의 모서리만 렌더링하여 와이어프레임 형태로 보여준다.
* D3D11_FILL_SOLID
  * 폴리곤의 내부를 채워서 렌더링한다.
