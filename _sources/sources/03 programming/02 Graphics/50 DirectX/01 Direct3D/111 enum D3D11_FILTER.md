# enum D3D11_FILTER
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