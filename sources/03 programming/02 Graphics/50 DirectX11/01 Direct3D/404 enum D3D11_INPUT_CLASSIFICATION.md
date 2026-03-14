# enum D3D11_INPUT_CLASSIFICATION
D3D11_INPUT_CLASSIFICATION enum은 입력 데이터의 종류를 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_INPUT_CLASSIFICATION
    {
        D3D11_INPUT_PER_VERTEX_DATA    = 0,
        D3D11_INPUT_PER_INSTANCE_DATA  = 1
    } 	D3D11_INPUT_CLASSIFICATION;
```
각 enum의 의미는 다음과 같다.

* D3D11_INPUT_PER_VERTEX_DATA
  * 입력 데이터가 정점별로 제공된다.
* D3D11_INPUT_PER_INSTANCE_DATA
  * 입력 데이터가 인스턴스별로 제공된다.
