# enum D3D11_BLEND
D3D11_BLEND enum은 Direct3D 11에서 색상 및 알파 블렌딩 작업에 사용되는 블렌드 인수를 정의하는 열거형이다.

이 열거형은 픽셀 셰이더 출력과 렌더 타깃의 기존 색상을 혼합할 때 사용할 블렌드 인수를 지정한다.

정의는 다음과 같다.

```cpp
typedef enum D3D11_BLEND
{
    D3D11_BLEND_ZERO                   = 1,
    D3D11_BLEND_ONE                    = 2,
    D3D11_BLEND_SRC_COLOR              = 3,
    D3D11_BLEND_INV_SRC_COLOR          = 4,
    D3D11_BLEND_SRC_ALPHA              = 5,
    D3D11_BLEND_INV_SRC_ALPHA          = 6,
    D3D11_BLEND_DEST_ALPHA             = 7,
    D3D11_BLEND_INV_DEST_ALPHA         = 8,
    D3D11_BLEND_DEST_COLOR             = 9,
    D3D11_BLEND_INV_DEST_COLOR         = 10,
    D3D11_BLEND_SRC_ALPHA_SAT          = 11,
    D3D11_BLEND_BLEND_FACTOR           = 14,
    D3D11_BLEND_INV_BLEND_FACTOR       = 15,
    D3D11_BLEND_SRC1_COLOR             = 16,
    D3D11_BLEND_INV_SRC1_COLOR         = 17,
    D3D11_BLEND_SRC1_ALPHA             = 18,
    D3D11_BLEND_INV_SRC1_ALPHA         = 19
} D3D11_BLEND;
```

각 enum의 의미는 다음과 같다.

* D3D11_BLEND_ZERO
  * 블렌드 인수가 0이다.
* D3D11_BLEND_ONE
  * 블렌드 인수가 1이다.
* D3D11_BLEND_SRC_COLOR
  * 소스 색상을 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_SRC_COLOR
  * 소스 색상의 보색(1 - 소스 색상)을 블렌드 인수로 사용한다.
* D3D11_BLEND_SRC_ALPHA
  * 소스 알파를 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_SRC_ALPHA
  * 소스 알파의 보색(1 - 소스 알파)을 블렌드 인수로 사용한다.
* D3D11_BLEND_DEST_ALPHA
  * 대상 알파를 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_DEST_ALPHA
  * 대상 알파의 보색(1 - 대상 알파)을 블렌드 인수로 사용한다.
* D3D11_BLEND_DEST_COLOR
  * 대상 색상을 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_DEST_COLOR
  * 대상 색상의 보색(1 - 대상 색상)을 블렌드 인수로 사용한다.
* D3D11_BLEND_SRC_ALPHA_SAT
  * 소스 알파의 포화도를 블렌드 인수로 사용한다.
* D3D11_BLEND_BLEND_FACTOR
  * 설정된 블렌드 팩터를 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_BLEND_FACTOR
  * 설정된 블렌드 팩터의 보색(1 - 블렌드 팩터)을 블렌드 인수로 사용한다.
* D3D11_BLEND_SRC1_COLOR
  * 소스1 색상을 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_SRC1_COLOR
  * 소스1 색상의 보색(1 - 소스1 색상)을 블렌드 인수로 사용한다.
* D3D11_BLEND_SRC1_ALPHA
  * 소스1 알파를 블렌드 인수로 사용한다.
* D3D11_BLEND_INV_SRC1_ALPHA
  * 소스1 알파의 보색(1 - 소스1 알파)을 블렌드 인수로 사용한다.
