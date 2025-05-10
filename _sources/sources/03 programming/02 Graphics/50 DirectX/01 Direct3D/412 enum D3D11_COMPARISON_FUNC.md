# enum D3D11_COMPARISON_FUNC
D3D11_COMPARISON_FUNC enum은 비교 연산의 종류를 정의하는 열거형이다. 주로 깊이 스텐실 테스트 및 텍스처 샘플링에 사용된다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D11_COMPARISON_FUNC
    {
        D3D11_COMPARISON_NEVER          = 1,
        D3D11_COMPARISON_LESS           = 2,
        D3D11_COMPARISON_EQUAL          = 3,
        D3D11_COMPARISON_LESS_EQUAL     = 4,
        D3D11_COMPARISON_GREATER        = 5,
        D3D11_COMPARISON_NOT_EQUAL      = 6,
        D3D11_COMPARISON_GREATER_EQUAL  = 7,
        D3D11_COMPARISON_ALWAYS         = 8
    } 	D3D11_COMPARISON_FUNC;
```

각 enum의 의미는 다음과 같다.

* D3D11_COMPARISON_NEVER
  * 비교가 항상 거짓이다.
* D3D11_COMPARISON_LESS
  * 비교 대상 값이 참조 값보다 작을 때 참이다.
* D3D11_COMPARISON_EQUAL
  * 비교 대상 값이 참조 값과 같을 때 참이다.
* D3D11_COMPARISON_LESS_EQUAL
  * 비교 대상 값이 참조 값보다 작거나 같을 때 참이다.
* D3D11_COMPARISON_GREATER
  * 비교 대상 값이 참조 값보다 클 때 참이다.
* D3D11_COMPARISON_NOT_EQUAL
  * 비교 대상 값이 참조 값과 다를 때 참이다.
* D3D11_COMPARISON_GREATER_EQUAL
  * 비교 대상 값이 참조 값보다 크거나 같을 때 참이다.
* D3D11_COMPARISON_ALWAYS
  * 비교가 항상 참이다.