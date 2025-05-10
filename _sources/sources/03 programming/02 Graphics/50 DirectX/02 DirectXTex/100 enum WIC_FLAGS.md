# enum WIC_FLAGS
WIC_FLAGS 열거형은 DirectXTex 라이브러리에서 이미지 로드 및 저장 시 사용되는 플래그를 정의하는 열거형이다.

이 열거형은 이미지를 처리할 때 다양한 옵션을 설정할 수 있게 해준다.
```cpp
typedef enum WIC_FLAGS
{
    WIC_FLAGS_NONE              = 0x00000000,
    WIC_FLAGS_FORCE_SRGB        = 0x00000001,
    WIC_FLAGS_IGNORE_SRGB       = 0x00000002,
    WIC_FLAGS_ALPHA_PREMULTIPLIED = 0x00000004,
    WIC_FLAGS_DITHER            = 0x00000008,
    WIC_FLAGS_DITHER_DIFFUSION  = 0x00000010,
    WIC_FLAGS_FILTER_POINT      = 0x00000020,
    WIC_FLAGS_FILTER_LINEAR     = 0x00000040,
    WIC_FLAGS_FILTER_CUBIC      = 0x00000080,
    WIC_FLAGS_FILTER_FANT       = 0x00000100,
    WIC_FLAGS_FILTER_BOX        = 0x00000200,
    WIC_FLAGS_FILTER_TRIANGLE   = 0x00000400,
    WIC_FLAGS_FILTER_LINEAR1    = 0x00000800,
    WIC_FLAGS_FILTER_LINEAR24   = 0x00001000,
    WIC_FLAGS_DITHER_THRESHOLD  = 0x00002000
} WIC_FLAGS;
```

각 WIC_FLAGS의 의미는 다음과 같다.

* WIC_FLAGS_NONE
  * 추가적인 플래그 없이 기본 설정으로 이미지를 처리한다.

* WIC_FLAGS_FORCE_SRGB
  * 이미지를 강제로 sRGB 색상 공간으로 변환하여 처리한다.

* WIC_FLAGS_IGNORE_SRGB
  * 이미지의 sRGB 메타데이터를 무시하고 처리한다.

* WIC_FLAGS_ALPHA_PREMULTIPLIED
  * 이미지의 알파 채널을 사전곱셈된 상태로 처리한다.

* WIC_FLAGS_DITHER
  * 이미지를 로드할 때 디더링을 사용하여 색상 밴딩을 줄인다.

* WIC_FLAGS_DITHER_DIFFUSION
  * 디더링을 확산 방법으로 사용하여 색상 밴딩을 줄인다.

* WIC_FLAGS_FILTER_POINT
  * 이미지 스케일링 시 포인트 필터링을 사용하여 텍셀을 샘플링한다.

* WIC_FLAGS_FILTER_LINEAR
  * 이미지 스케일링 시 선형 필터링을 사용하여 텍셀을 샘플링한다.

* WIC_FLAGS_FILTER_CUBIC
  * 이미지 스케일링 시 입방체 필터링을 사용하여 텍셀을 샘플링한다.

* WIC_FLAGS_FILTER_FANT
  * 이미지 스케일링 시 판타 필터링을 사용하여 텍셀을 샘플링한다.

* WIC_FLAGS_FILTER_BOX
  * 이미지 스케일링 시 박스 필터링을 사용하여 텍셀을 샘플링한다.

* WIC_FLAGS_FILTER_TRIANGLE
  * 이미지 스케일링 시 삼각형 필터링을 사용하여 텍셀을 샘플링한다.

* WIC_FLAGS_FILTER_LINEAR1
  * 이미지 스케일링 시 선형 필터링을 사용하여 텍셀을 샘플링하며, 입력 및 출력의 크기가 동일할 경우 단일 필터링을 적용한다.

* WIC_FLAGS_FILTER_LINEAR24
  * 이미지 스케일링 시 선형 필터링을 사용하여 텍셀을 샘플링하며, 입력 및 출력의 크기가 2:1 비율일 경우 24bpp 색상 체계를 사용한다.

* WIC_FLAGS_DITHER_THRESHOLD
  * 디더링 시 색상 밴딩을 줄이기 위해 사용되는 임계값을 설정한다.
