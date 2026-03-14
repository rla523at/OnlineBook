# enum D3D11_BLEND_OP
D3D11_BLEND_OP enum은 Direct3D 11에서 색상 및 알파 블렌딩 작업에 사용되는 블렌드 연산을 정의하는 열거형이다.

이 열거형은 픽셀 셰이더 출력과 렌더 타깃의 기존 색상을 혼합할 때 사용할 블렌드 연산을 지정한다.

정의는 다음과 같다.

```cpp
typedef enum D3D11_BLEND_OP
{
    D3D11_BLEND_OP_ADD          = 1,
    D3D11_BLEND_OP_SUBTRACT     = 2,
    D3D11_BLEND_OP_REV_SUBTRACT = 3,
    D3D11_BLEND_OP_MIN          = 4,
    D3D11_BLEND_OP_MAX          = 5
} D3D11_BLEND_OP;
```

각 enum의 의미는 다음과 같다.

* D3D11_BLEND_OP_ADD
  * 소스와 대상의 색상 값을 더한다.
* D3D11_BLEND_OP_SUBTRACT
  * 대상의 색상 값에서 소스의 색상 값을 뺀다.
* D3D11_BLEND_OP_REV_SUBTRACT
  * 소스의 색상 값에서 대상의 색상 값을 뺀다.
* D3D11_BLEND_OP_MIN
  * 소스와 대상의 색상 값 중 작은 값을 선택한다.
* D3D11_BLEND_OP_MAX
  * 소스와 대상의 색상 값 중 큰 값을 선택한다.
