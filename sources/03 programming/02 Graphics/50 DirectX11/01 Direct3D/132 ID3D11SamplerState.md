# ID3D11SamplerState
ID3D11SamplerState는 Direct3D 11에서 텍스처 샘플링에 사용되는 상태 객체를 나타내는 인터페이스다. 

이 클래스는 텍스처 데이터를 샘플링할 때 어떻게 할 것인지를 나타내는 클래스로 샘플링에 적용되는 필터링 모드, 주소 지정 모드, 경계 색상 등을 정의한다.

ID3D11SamplerState의 주요 역할은 텍스처 샘플링 시 사용되는 다양한 상태를 설정하는 것이다. 필터링 모드는 텍스처의 픽셀 간 보간 방법을 결정하며, 주로 `Point`, `Linear`, `Anisotropic` 등의 모드가 사용된다. 이 필터링 모드는 텍스처가 확대 또는 축소될 때의 품질을 조정하는 데 중요하다. 예를 들어, `Linear` 필터링을 사용하면 텍스처가 부드럽게 보간되지만, `Point` 필터링을 사용하면 각 텍스처 픽셀이 명확하게 구분된다.

주소 지정 모드는 텍스처 좌표가 [0, 1] 범위를 벗어날 때 텍스처를 어떻게 처리할지를 정의한다. 주요 주소 지정 모드로는 `Wrap`, `Mirror`, `Clamp`, `Border` 등이 있다. 예를 들어, `Wrap` 모드는 텍스처가 반복되도록 하고, `Clamp` 모드는 텍스처 좌표가 경계를 벗어나지 않도록 한다. `Border` 모드는 경계를 벗어났을 때 특정 색상을 사용한다.

또한, ID3D11SamplerState는 경계 색상을 설정하여 주소 지정 모드가 `Border`일 때 텍스처 경계 밖의 색상을 지정할 수 있다. 경계 색상은 텍스처 좌표가 경계를 벗어났을 때 적용되는 색상이다. 이 외에도 샘플러 상태 객체는 MIP 맵 레벨 간의 전환을 정의하여 텍스처의 다른 해상도 레벨을 사용할 때 적용되는 필터링 방식을 설정할 수 있다.

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
    * enum D3D11_TEXTURE_ADDRESS_MODE 값을 사용한다.
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
    * enum D3D11_COMPARISON_FUNC 값
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
