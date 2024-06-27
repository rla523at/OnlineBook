# ID3D11SamplerState
ID3D11SamplerState는 텍스처 샘플링에 사용되는 상태 객체를 나타낸다. 

이 객체는 텍스처가 셰이더에서 샘플링될 때 어떻게 처리될지를 결정한다.

## D3D11_SAMPLER_DESC 구조체
D3D11_SAMPLER_DESC 구조체는 Direct3D 11에서 샘플러 상태를 정의하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_SAMPLER_DESC {
    D3D11_FILTER Filter;
    D3D11_TEXTURE_ADDRESS_MODE AddressU;
    D3D11_TEXTURE_ADDRESS_MODE AddressV;
    D3D11_TEXTURE_ADDRESS_MODE AddressW;
    FLOAT MipLODBias;
    UINT MaxAnisotropy;
    D3D11_COMPARISON_FUNC ComparisonFunc;
    FLOAT BorderColor[4];
    FLOAT MinLOD;
    FLOAT MaxLOD;
} D3D11_SAMPLER_DESC;
```
각각의 멤버변수는 다음과 같다.

* D3D11_FILTER Filter
  * 텍스처 필터링 방법을 지정한다.
  * 가능한 값
      * D3D11_FILTER enum 값을 사용한다.
  * 기본값은 D3D11_FILTER_MIN_MAG_MIP_POINT이다.

* D3D11_TEXTURE_ADDRESS_MODE AddressU
* D3D11_TEXTURE_ADDRESS_MODE AddressV
* D3D11_TEXTURE_ADDRESS_MODE AddressW
  * 텍스처 좌표 U,V,W 방향의 D3D11_TEXTURE_ADDRESS_MODE를 지정한다.
  * 가능한 값
    * D3D11_TEXTURE_ADDRESS_MODE enum 값을 사용한다.
  * 기본값은 D3D11_TEXTURE_ADDRESS_WRAP이다.

* FLOAT MipLODBias
  * MIP 지도 레벨 오프셋을 지정한다.
  * 가능한 값
      * 음수나 양수의 실수 값
  * 기본값은 0.0f이다.

* UINT MaxAnisotropy
  * 최대 이방성 수준을 지정한다. 이 값은 이방성 필터링이 사용될 때만 유효하다.
  * 가능한 값
      * 1 이상의 정수 값
  * 기본값은 1이다.

* D3D11_COMPARISON_FUNC ComparisonFunc
  * 비교 함수를 지정한다. 
    * 주로 그림자 맵핑과 같은 기술에서 사용된다.
    * 깊이 텍스처는 각 픽셀의 깊이 값을 저장하고 이 깊이 값을 비교하여 픽셀이 그림자 안에 있는지 여부를 결정할 수 있다. 
    * 비교 함수는 텍스처 샘플링 시 이러한 비교 작업을 수행하기 위해 필요하다.
  * 가능한 값
    * D3D11_COMPARISON_FUNC enum 값
  * 기본값은 D3D11_COMPARISON_ALWAYS이다.

* FLOAT BorderColor[4]
  * 보더 모드에서 텍스처의 경계 색을 지정한다.
  * 가능한 값
      * 네 개의 실수 값으로 표현되는 RGBA 색상 값
  * 기본값은 {0.0f, 0.0f, 0.0f, 0.0f}이다.

* FLOAT MinLOD
  * 최소 MIP 지도 수준을 지정한다.
  * 가능한 값
    * 음수나 양수의 실수 값
  * 기본값은 0.0f이다.

* FLOAT MaxLOD
  * 최대 MIP 지도 수준을 지정한다.
  * 가능한 값
    * 음수나 양수의 실수 값
  * 기본값은 D3D11_FLOAT32_MAX이다.

## D3D11_FILTER enum
D3D11_FILTER enum은 텍스처 필터링 방법을 정의하는 열거형이다.

이 열거형은 텍스처의 다양한 `축소(minification; MIN)`, `확대(magnification; MAG)`, 및 `MIP 맵(mipmap; MIP)` 레벨 간의 샘플링 방법을 지정한다.

텍스처 축소 필터링은 텍스처를 화면에 표시할 때 텍스처의 크기가 화면의 픽셀보다 작은 경우 어떻게 샘플링할지를 결정한다. 예를 들어 텍스처가 작은 객체에 매핑되어 멀리서 보이는 경우, 축소된 텍스처 샘플링을 수행한다.

텍스처 확대 필터링은 텍스처를 화면에 표시할 때 텍스처의 크기가 화면의 픽셀보다 큰 경우 어떻게 샘플링할지를 결정한다. 예를 들어 텍스처가 큰 객체에 매핑되어 가까이서 보이는 경우, 확대된 텍스처 샘플링을 수행한다.

MIP 맵 필터링은 텍스처에 여러 레벨의 상세도(MIP 맵)가 있는 경우, 서로 다른 MIP 맵 레벨 간의 샘플링을 결정한다. MIP 맵은 원본 텍스처의 축소 버전으로, 렌더링 성능을 향상시키고 텍스처의 시각적 품질을 높이는 데 사용된다. 예를 들어 객체가 카메라에서 멀어질수록 더 작은 MIP 맵 레벨을 사용하여 텍스처의 세부 사항을 줄인다.

필터링 방법은 크게 `포인트(point)`, `선형(linear)`, `이방성(anisotropic)` 필터링으로 나뉜다.

포인트 필터링은 가장 가까운 텍스처 샘플을 선택하여 단일 텍셀 값을 사용한다. 이는 빠르며 성능이 좋지만 시각적으로 부드럽지 않고, 텍셀 경계가 뚜렷하게 보인다.

선형 필터링은 주변 텍셀 값을 선형 보간하여 텍셀 값을 계산한다. 이는 부드럽고 자연스러운 텍스처 샘플링을 제공하지만 포인트 필터링보다 계산이 더 복잡하고, 성능이 약간 떨어질 수 있다.

이방성 필터링은 텍스처의 비스듬한 각도에서 샘플링 품질을 향상시키는 고급 필터링 방법이다. 여러 방향에서 텍셀 샘플을 보간하여 텍스처의 세부 사항을 유지한다. 이는 높은 시각적 품질을 제공하며, 특히 텍스처가 비스듬하게 보일 때 유리하지만 가장 성능이 요구되는 필터링 방법 중 하나다

이 때, `텍셀(texel)`이란 `텍스처 요소(Texture Element)`의 줄임말로, 텍스처 이미지의 개별 화소를 의미한다. 텍셀은 2D 텍스처 이미지의 기본 구성 요소로, 각 텍셀은 특정 색상 및 기타 속성을 가진다. 텍셀은 화면에 렌더링될 때, 해당 위치의 색상과 특성을 결정하는 데 사용된다.


정의는 다음과 같다.
```cpp
typedef 
enum D3D11_FILTER
    {
        D3D11_FILTER_MIN_MAG_MIP_POINT                          = 0,
        D3D11_FILTER_MIN_MAG_POINT_MIP_LINEAR                   = 0x1,
        D3D11_FILTER_MIN_POINT_MAG_LINEAR_MIP_POINT             = 0x4,
        D3D11_FILTER_MIN_POINT_MAG_MIP_LINEAR                   = 0x5,
        D3D11_FILTER_MIN_LINEAR_MAG_MIP_POINT                   = 0x10,
        D3D11_FILTER_MIN_LINEAR_MAG_POINT_MIP_LINEAR            = 0x11,
        D3D11_FILTER_MIN_MAG_LINEAR_MIP_POINT                   = 0x14,
        D3D11_FILTER_MIN_MAG_MIP_LINEAR                         = 0x15,
        D3D11_FILTER_ANISOTROPIC                                = 0x55,
        D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT               = 0x80,
        D3D11_FILTER_COMPARISON_MIN_MAG_POINT_MIP_LINEAR        = 0x81,
        D3D11_FILTER_COMPARISON_MIN_POINT_MAG_LINEAR_MIP_POINT  = 0x84,
        D3D11_FILTER_COMPARISON_MIN_POINT_MAG_MIP_LINEAR        = 0x85,
        D3D11_FILTER_COMPARISON_MIN_LINEAR_MAG_MIP_POINT        = 0x90,
        D3D11_FILTER_COMPARISON_MIN_LINEAR_MAG_POINT_MIP_LINEAR = 0x91,
        D3D11_FILTER_COMPARISON_MIN_MAG_LINEAR_MIP_POINT        = 0x94,
        D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR              = 0x95,
        D3D11_FILTER_COMPARISON_ANISOTROPIC                     = 0xd5,
        D3D11_FILTER_MINIMUM_MIN_MAG_MIP_POINT                  = 0x100,
        D3D11_FILTER_MINIMUM_MIN_MAG_POINT_MIP_LINEAR           = 0x101,
        D3D11_FILTER_MINIMUM_MIN_POINT_MAG_LINEAR_MIP_POINT     = 0x104,
        D3D11_FILTER_MINIMUM_MIN_POINT_MAG_MIP_LINEAR           = 0x105,
        D3D11_FILTER_MINIMUM_MIN_LINEAR_MAG_MIP_POINT           = 0x110,
        D3D11_FILTER_MINIMUM_MIN_LINEAR_MAG_POINT_MIP_LINEAR    = 0x111,
        D3D11_FILTER_MINIMUM_MIN_MAG_LINEAR_MIP_POINT           = 0x114,
        D3D11_FILTER_MINIMUM_MIN_MAG_MIP_LINEAR                 = 0x115,
        D3D11_FILTER_MINIMUM_ANISOTROPIC                        = 0x155,
        D3D11_FILTER_MAXIMUM_MIN_MAG_MIP_POINT                  = 0x180,
        D3D11_FILTER_MAXIMUM_MIN_MAG_POINT_MIP_LINEAR           = 0x181,
        D3D11_FILTER_MAXIMUM_MIN_POINT_MAG_LINEAR_MIP_POINT     = 0x184,
        D3D11_FILTER_MAXIMUM_MIN_POINT_MAG_MIP_LINEAR           = 0x185,
        D3D11_FILTER_MAXIMUM_MIN_LINEAR_MAG_MIP_POINT           = 0x190,
        D3D11_FILTER_MAXIMUM_MIN_LINEAR_MAG_POINT_MIP_LINEAR    = 0x191,
        D3D11_FILTER_MAXIMUM_MIN_MAG_LINEAR_MIP_POINT           = 0x194,
        D3D11_FILTER_MAXIMUM_MIN_MAG_MIP_LINEAR                 = 0x195,
        D3D11_FILTER_MAXIMUM_ANISOTROPIC                        = 0x1d5
    } 	D3D11_FILTER;
```
각 enum의 의미는 다음과 같다.

* D3D11_FILTER_MIN_MAG_MIP_POINT
  * 최소, 최대, MIP 모두에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MIN_MAG_POINT_MIP_LINEAR
  * 최소 및 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MIN_POINT_MAG_LINEAR_MIP_POINT
  * 최소에서 포인트 샘플링을, 최대에서 선형 샘플링을, MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MIN_POINT_MAG_MIP_LINEAR
  * 최소에서 포인트 샘플링을, 최대 및 MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MIN_LINEAR_MAG_MIP_POINT
  * 최소에서 선형 샘플링을, 최대 및 MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MIN_LINEAR_MAG_POINT_MIP_LINEAR
  * 최소에서 선형 샘플링을, 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MIN_MAG_LINEAR_MIP_POINT
  * 최소 및 최대에서 선형 샘플링을, MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MIN_MAG_MIP_LINEAR
  * 최소, 최대, MIP 모두에서 선형 샘플링을 사용한다.
* D3D11_FILTER_ANISOTROPIC
  * 이방성 필터링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT
  * 비교를 위해 최소, 최대, MIP 모두에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_MAG_POINT_MIP_LINEAR
  * 비교를 위해 최소 및 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_POINT_MAG_LINEAR_MIP_POINT
  * 비교를 위해 최소에서 포인트 샘플링을, 최대에서 선형 샘플링을, MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_POINT_MAG_MIP_LINEAR
  * 비교를 위해 최소에서 포인트 샘플링을, 최대 및 MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_LINEAR_MAG_MIP_POINT
  * 비교를 위해 최소에서 선형 샘플링을, 최대 및 MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_LINEAR_MAG_POINT_MIP_LINEAR
  * 비교를 위해 최소에서 선형 샘플링을, 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_MAG_LINEAR_MIP_POINT
  * 비교를 위해 최소 및 최대에서 선형 샘플링을, MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR
  * 비교를 위해 최소, 최대, MIP 모두에서 선형 샘플링을 사용한다.
* D3D11_FILTER_COMPARISON_ANISOTROPIC
  * 비교를 위해 이방성 필터링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_MAG_MIP_POINT
  * 최소화된 최소, 최대, MIP 모두에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_MAG_POINT_MIP_LINEAR
  * 최소화된 최소 및 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_POINT_MAG_LINEAR_MIP_POINT
  * 최소화된 최소에서 포인트 샘플링을, 최대에서 선형 샘플링을, MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_POINT_MAG_MIP_LINEAR
  * 최소화된 최소에서 포인트 샘플링을, 최대 및 MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_LINEAR_MAG_MIP_POINT
  * 최소화된 최소에서 선형 샘플링을, 최대 및 MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_LINEAR_MAG_POINT_MIP_LINEAR
  * 최소화된 최소에서 선형 샘플링을, 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_MAG_LINEAR_MIP_POINT
  * 최소화된 최소 및 최대에서 선형 샘플링을, MIP에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_MIN_MAG_MIP_LINEAR
  * 최소화된 최소, 최대, MIP 모두에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MINIMUM_ANISOTROPIC
  * 최소화된 이방성 필터링을 사용한다.
* D3D11_FILTER_MAXIMUM_MIN_MAG_MIP_POINT
  * 최대화된 최소, 최대, MIP 모두에서 포인트 샘플링을 사용한다.
* D3D11_FILTER_MAXIMUM_MIN_MAG_POINT_MIP_LINEAR
  * 최대화된 최소 및 최대에서 포인트 샘플링을, MIP에서 선형 샘플링을 사용한다.
* D3D11_FILTER_MAXIMUM_MIN_POINT_MAG_LINEAR_MIP_POINT
  * 최대화된 최소에서 포인트

## D3D11_TEXTURE_ADDRESS_MODE enum
D3D11_TEXTURE_ADDRESS_MODE enum은 텍스처 좌표가 텍스처 범위를 벗어났을 때 텍스처를 샘플링하는 방법을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_TEXTURE_ADDRESS_MODE
    {
        D3D11_TEXTURE_ADDRESS_WRAP          = 1,
        D3D11_TEXTURE_ADDRESS_MIRROR        = 2,
        D3D11_TEXTURE_ADDRESS_CLAMP         = 3,
        D3D11_TEXTURE_ADDRESS_BORDER        = 4,
        D3D11_TEXTURE_ADDRESS_MIRROR_ONCE   = 5
    } 	D3D11_TEXTURE_ADDRESS_MODE;
```
각 enum의 의미는 다음과 같다.

* D3D11_TEXTURE_ADDRESS_WRAP
  * 텍스처 좌표가 범위를 벗어날 때, 좌표를 텍스처 크기로 나눈 나머지를 사용한다. 즉, 텍스처가 반복된다.
* D3D11_TEXTURE_ADDRESS_MIRROR
  * 텍스처 좌표가 범위를 벗어날 때, 좌표를 텍스처 크기로 나눈 몫의 홀짝 여부에 따라 좌표를 반사시킨다. 즉, 텍스처가 거울처럼 반복된다.
* D3D11_TEXTURE_ADDRESS_CLAMP
  * 텍스처 좌표가 범위를 벗어날 때, 가장자리 좌표를 사용한다. 즉, 텍스처가 고정된 가장자리 값을 사용한다.
* D3D11_TEXTURE_ADDRESS_BORDER
  * 텍스처 좌표가 범위를 벗어날 때, 지정된 테두리 색상을 사용한다.
* D3D11_TEXTURE_ADDRESS_MIRROR_ONCE
  * 텍스처 좌표가 범위를 처음 벗어날 때는 반사시키고, 그 이후에는 고정된 가장자리 좌표를 사용한다.

## D3D11_COMPARISON_FUNC enum
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

