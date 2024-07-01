# enum D3D11_USAGE 
D3D11_USAGE enum은 리소스의 사용 방법을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_USAGE
    {
        D3D11_USAGE_DEFAULT    = 0,
        D3D11_USAGE_IMMUTABLE  = 1,
        D3D11_USAGE_DYNAMIC    = 2,
        D3D11_USAGE_STAGING    = 3
    } 	D3D11_USAGE;
```
각 enum의 의미는 다음과 같다.

* D3D11_USAGE_DEFAULT
  * 리소스가 GPU에서만 접근 가능하며, 일반적인 용도로 사용된다.
* D3D11_USAGE_IMMUTABLE
  * 리소스가 생성된 후 변경할 수 없으며, 초기화 시 데이터가 설정된다.
* D3D11_USAGE_DYNAMIC
  * 리소스가 CPU에서 자주 갱신될 수 있으며, 주로 매 프레임마다 업데이트되는 데이터에 사용된다.
* D3D11_USAGE_STAGING
  * 리소스가 CPU와 GPU 간의 데이터 전송을 위해 사용되며, 읽기와 쓰기 모두 가능하다.