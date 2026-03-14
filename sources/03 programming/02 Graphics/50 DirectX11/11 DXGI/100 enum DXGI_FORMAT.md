# enum DXGI_FORMAT
DXGI_FORMAT enum은 DirectX Graphics Infrastructure(DXGI)에서 사용되는 다양한 데이터 형식을 정의하는 열거형이다.

이 열거형은 텍스처, 렌더 타깃, 깊이-스텐실 버퍼 등에서 사용되는 데이터의 형식을 지정한다.

정의는 다음과 같다.
```cpp
typedef 
enum DXGI_FORMAT
    {
        DXGI_FORMAT_UNKNOWN                      = 0,
        DXGI_FORMAT_R32G32B32A32_TYPELESS        = 1,
        DXGI_FORMAT_R32G32B32A32_FLOAT           = 2,
        DXGI_FORMAT_R32G32B32A32_UINT            = 3,
        DXGI_FORMAT_R32G32B32A32_SINT            = 4,
        DXGI_FORMAT_R32G32B32_TYPELESS           = 5,
        DXGI_FORMAT_R32G32B32_FLOAT              = 6,
        DXGI_FORMAT_R32G32B32_UINT               = 7,
        DXGI_FORMAT_R32G32B32_SINT               = 8,
        DXGI_FORMAT_R16G16B16A16_TYPELESS        = 9,
        DXGI_FORMAT_R16G16B16A16_FLOAT           = 10,
        DXGI_FORMAT_R16G16B16A16_UNORM           = 11,
        DXGI_FORMAT_R16G16B16A16_UINT            = 12,
        DXGI_FORMAT_R16G16B16A16_SNORM           = 13,
        DXGI_FORMAT_R16G16B16A16_SINT            = 14,
        DXGI_FORMAT_R32G32_TYPELESS              = 15,
        DXGI_FORMAT_R32G32_FLOAT                 = 16,
        DXGI_FORMAT_R32G32_UINT                  = 17,
        DXGI_FORMAT_R32G32_SINT                  = 18,
        DXGI_FORMAT_R32G8X24_TYPELESS            = 19,
        DXGI_FORMAT_D32_FLOAT_S8X24_UINT         = 20,
        DXGI_FORMAT_R32_FLOAT_X8X24_TYPELESS     = 21,
        DXGI_FORMAT_X32_TYPELESS_G8X24_UINT      = 22,
        DXGI_FORMAT_R10G10B10A2_TYPELESS         = 23,
        DXGI_FORMAT_R10G10B10A2_UNORM            = 24,
        DXGI_FORMAT_R10G10B10A2_UINT             = 25,
        DXGI_FORMAT_R11G11B10_FLOAT              = 26,
        DXGI_FORMAT_R8G8B8A8_TYPELESS            = 27,
        DXGI_FORMAT_R8G8B8A8_UNORM               = 28,
        DXGI_FORMAT_R8G8B8A8_UNORM_SRGB          = 29,
        DXGI_FORMAT_R8G8B8A8_UINT                = 30,
        DXGI_FORMAT_R8G8B8A8_SNORM               = 31,
        DXGI_FORMAT_R8G8B8A8_SINT                = 32,
        DXGI_FORMAT_R16G16_TYPELESS              = 33,
        DXGI_FORMAT_R16G16_FLOAT                 = 34,
        DXGI_FORMAT_R16G16_UNORM                 = 35,
        DXGI_FORMAT_R16G16_UINT                  = 36,
        DXGI_FORMAT_R16G16_SNORM                 = 37,
        DXGI_FORMAT_R16G16_SINT                  = 38,
        DXGI_FORMAT_R32_TYPELESS                 = 39,
        DXGI_FORMAT_D32_FLOAT                    = 40,
        DXGI_FORMAT_R32_FLOAT                    = 41,
        DXGI_FORMAT_R32_UINT                     = 42,
        DXGI_FORMAT_R32_SINT                     = 43,
        DXGI_FORMAT_R24G8_TYPELESS               = 44,
        DXGI_FORMAT_D24_UNORM_S8_UINT            = 45,
        DXGI_FORMAT_R24_UNORM_X8_TYPELESS        = 46,
        DXGI_FORMAT_X24_TYPELESS_G8_UINT         = 47,
        DXGI_FORMAT_R8G8_TYPELESS                = 48,
        DXGI_FORMAT_R8G8_UNORM                   = 49,
        DXGI_FORMAT_R8G8_UINT                    = 50,
        DXGI_FORMAT_R8G8_SNORM                   = 51,
        DXGI_FORMAT_R8G8_SINT                    = 52,
        DXGI_FORMAT_R16_TYPELESS                 = 53,
        DXGI_FORMAT_R16_FLOAT                    = 54,
        DXGI_FORMAT_D16_UNORM                    = 55,
        DXGI_FORMAT_R16_UNORM                    = 56,
        DXGI_FORMAT_R16_UINT                     = 57,
        DXGI_FORMAT_R16_SNORM                    = 58,
        DXGI_FORMAT_R16_SINT                     = 59,
        DXGI_FORMAT_R8_TYPELESS                  = 60,
        DXGI_FORMAT_R8_UNORM                     = 61,
        DXGI_FORMAT_R8_UINT                      = 62,
        DXGI_FORMAT_R8_SNORM                     = 63,
        DXGI_FORMAT_R8_SINT                      = 64,
        DXGI_FORMAT_A8_UNORM                     = 65,
        DXGI_FORMAT_R1_UNORM                     = 66,
        DXGI_FORMAT_R9G9B9E5_SHAREDEXP           = 67,
        DXGI_FORMAT_R8G8_B8G8_UNORM              = 68,
        DXGI_FORMAT_G8R8_G8B8_UNORM              = 69,
        DXGI_FORMAT_BC1_TYPELESS                 = 70,
        DXGI_FORMAT_BC1_UNORM                    = 71,
        DXGI_FORMAT_BC1_UNORM_SRGB               = 72,
        DXGI_FORMAT_BC2_TYPELESS                 = 73,
        DXGI_FORMAT_BC2_UNORM                    = 74,
        DXGI_FORMAT_BC2_UNORM_SRGB               = 75,
        DXGI_FORMAT_BC3_TYPELESS                 = 76,
        DXGI_FORMAT_BC3_UNORM                    = 77,
        DXGI_FORMAT_BC3_UNORM_SRGB               = 78,
        DXGI_FORMAT_BC4_TYPELESS                 = 79,
        DXGI_FORMAT_BC4_UNORM                    = 80,
        DXGI_FORMAT_BC4_SNORM                    = 81,
        DXGI_FORMAT_BC5_TYPELESS                 = 82,
        DXGI_FORMAT_BC5_UNORM                    = 83,
        DXGI_FORMAT_BC5_SNORM                    = 84,
        DXGI_FORMAT_B5G6R5_UNORM                 = 85,
        DXGI_FORMAT_B5G5R5A1_UNORM               = 86,
        DXGI_FORMAT_B8G8R8A8_UNORM               = 87,
        DXGI_FORMAT_B8G8R8X8_UNORM               = 88,
        DXGI_FORMAT_R10G10B10_XR_BIAS_A2_UNORM   = 89,
        DXGI_FORMAT_B8G8R8A8_TYPELESS            = 90,
        DXGI_FORMAT_B8G8R8A8_UNORM_SRGB          = 91,
        DXGI_FORMAT_B8G8R8X8_TYPELESS            = 92,
        DXGI_FORMAT_B8G8R8X8_UNORM_SRGB          = 93,
        DXGI_FORMAT_BC6H_TYPELESS                = 94,
        DXGI_FORMAT_BC6H_UF16                    = 95,
        DXGI_FORMAT_BC6H_SF16                    = 96,
        DXGI_FORMAT_BC7_TYPELESS                 = 97,
        DXGI_FORMAT_BC7_UNORM                    = 98,
        DXGI_FORMAT_BC7_UNORM_SRGB               = 99,
        DXGI_FORMAT_AYUV                         = 100,
        DXGI_FORMAT_Y410                         = 101,
        DXGI_FORMAT_Y416                         = 102,
        DXGI_FORMAT_NV12                         = 103,
        DXGI_FORMAT_P010                         = 104,
        DXGI_FORMAT_P016                         = 105,
        DXGI_FORMAT_420_OPAQUE                   = 106,
        DXGI_FORMAT_YUY2                         = 107,
        DXGI_FORMAT_Y210                         = 108,
        DXGI_FORMAT_Y216                         = 109,
        DXGI_FORMAT_NV11                         = 110,
        DXGI_FORMAT_AI44                         = 111,
        DXGI_FORMAT_IA44                         = 112,
        DXGI_FORMAT_P8                           = 113,
        DXGI_FORMAT_A8P8                         = 114,
        DXGI_FORMAT_B4G4R4A4_UNORM               = 115,
        DXGI_FORMAT_P208                         = 130,
        DXGI_FORMAT_V208                         = 131,
        DXGI_FORMAT_V408                         = 132,
        DXGI_FORMAT_SAMPLER_FEEDBACK_MIN_MIP_OPAQUE = 189,
        DXGI_FORMAT_SAMPLER_FEEDBACK_MIP_REGION_USED_OPAQUE = 190
    } 	DXGI_FORMAT;
```
각 enum의 의미는 다음과 같다.

* DXGI_FORMAT_UNKNOWN
  * 형식이 정의되지 않았다.
* DXGI_FORMAT_R32G32B32A32_TYPELESS
  * 32비트 타입이 지정되지 않은 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R32G32B32A32_FLOAT
  * 32비트 부동 소수점 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R32G32B32A32_UINT
  * 32비트 부호 없는 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R32G32B32A32_SINT
  * 32비트 부호 있는 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R32G32B32_TYPELESS
  * 32비트 타입이 지정되지 않은 각 채널을 가진 RGB 형식.
* DXGI_FORMAT_R32G32B32_FLOAT
  * 32비트 부동 소수점 각 채널을 가진 RGB 형식.
* DXGI_FORMAT_R32G32B32_UINT
  * 32비트 부호 없는 정수 각 채널을 가진 RGB 형식.
* DXGI_FORMAT_R32G32B32_SINT
  * 32비트 부호 있는 정수 각 채널을 가진 RGB 형식.
* DXGI_FORMAT_R16G16B16A16_TYPELESS
  * 16비트 타입이 지정되지 않은 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R16G16B16A16_FLOAT
  * 16비트 부동 소수점 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R16G16B16A16_UNORM
  * 16비트 부호 없는 정규화된 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R16G16B16A16_UINT
  * 16비트 부호 없는 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R16G16B16A16_SNORM
  * 16비트 부호 있는 정규화된 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R16G16B16A16_SINT
  * 16비트 부호 있는 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R32G32_TYPELESS
  * 32비트 타입이 지정되지 않은 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R32G32_FLOAT
  * 32비트 부동 소수점 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R32G32_UINT
  * 32비트 부호 없는 정수 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R32G32_SINT
  * 32비트 부호 있는 정수 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R32G8X24_TYPELESS
  * 32비트 타입이 지정되지 않은 R 채널과 8비트 스텐실, 24비트 깊이의 형식.
* DXGI_FORMAT_D32_FLOAT_S8X24_UINT
  * 32비트 부동 소수점 깊이와 8비트 스텐실, 24비트 패딩의 형식.
* DXGI_FORMAT_R32_FLOAT_X8X24_TYPELESS
  * 32비트 부동 소수점 R 채널과 8비트 타입이 지정되지 않은 깊이와 24비트 패딩의 형식.
* DXGI_FORMAT_X32_TYPELESS_G8X24_UINT
  * 32비트 타입이 지정되지 않은 깊이와 8비트 부호 없는 정수 G 채널, 24비트 패딩의 형식.
* DXGI_FORMAT_R10G10B10A2_TYPELESS
  * 10비트 타입이 지정되지 않은 RGB 채널과 2비트 타입이 지정되지 않은 A 채널의 형식.
* DXGI_FORMAT_R10G10B10A2_UNORM
  * 10비트 부호 없는 정규화된 RGB 채널과 2비트 부호 없는 정규화된 A 채널의 형식.
* DXGI_FORMAT_R10G10B10A2_UINT
  * 10비트 부호 없는 정수 RGB 채널과 2비트 부호 없는 정수 A 채널의 형식.
* DXGI_FORMAT_R11G11B10_FLOAT
  * 11비트 부동 소수점 R과 G 채널, 10비트 부동 소수점 B 채널의 형식.
* DXGI_FORMAT_R8G8B8A8_TYPELESS
  * 8비트 타입이 지정되지 않은 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R8G8B8A8_UNORM
  * 8비트 부호 없는 정규화된 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R8G8B8A8_UNORM_SRGB
  * 8비트 부호 없는 정규화된 각 채널을 가진 RGBA 형식으로, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_R8G8B8A8_UINT
  * 8비트 부호 없는 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R8G8B8A8_SNORM
  * 8비트 부호 있는 정규화된 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R8G8B8A8_SINT
  * 8비트 부호 있는 정수 각 채널을 가진 RGBA 형식.
* DXGI_FORMAT_R16G16_TYPELESS
  * 16비트 타입이 지정되지 않은 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R16G16_FLOAT
  * 16비트 부동 소수점 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R16G16_UNORM
  * 16비트 부호 없는 정규화된 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R16G16_UINT
  * 16비트 부호 없는 정수 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R16G16_SNORM
  * 16비트 부호 있는 정규화된 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R16G16_SINT
  * 16비트 부호 있는 정수 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R32_TYPELESS
  * 32비트 타입이 지정되지 않은 R 채널의 형식.
* DXGI_FORMAT_D32_FLOAT
  * 32비트 부동 소수점 깊이의 형식.
* DXGI_FORMAT_R32_FLOAT
  * 32비트 부동 소수점 R 채널의 형식.
* DXGI_FORMAT_R32_UINT
  * 32비트 부호 없는 정수 R 채널의 형식.
* DXGI_FORMAT_R32_SINT
  * 32비트 부호 있는 정수 R 채널의 형식.
* DXGI_FORMAT_R24G8_TYPELESS
  * 24비트 타입이 지정되지 않은 R 채널과 8비트 타입이 지정되지 않은 G 채널의 형식.
* DXGI_FORMAT_D24_UNORM_S8_UINT
  * 24비트 부호 없는 정규화된 깊이와 8비트 부호 없는 정수 스텐실의 형식.
* DXGI_FORMAT_R24_UNORM_X8_TYPELESS
  * 24비트 부호 없는 정규화된 R 채널과 8비트 타입이 지정되지 않은 패딩의 형식.
* DXGI_FORMAT_X24_TYPELESS_G8_UINT
  * 24비트 타입이 지정되지 않은 패딩과 8비트 부호 없는 정수 G 채널의 형식.
* DXGI_FORMAT_R8G8_TYPELESS
  * 8비트 타입이 지정되지 않은 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R8G8_UNORM
  * 8비트 부호 없는 정규화된 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R8G8_UINT
  * 8비트 부호 없는 정수 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R8G8_SNORM
  * 8비트 부호 있는 정규화된 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R8G8_SINT
  * 8비트 부호 있는 정수 각 채널을 가진 RG 형식.
* DXGI_FORMAT_R16_TYPELESS
  * 16비트 타입이 지정되지 않은 R 채널의 형식.
* DXGI_FORMAT_R16_FLOAT
  * 16비트 부동 소수점 R 채널의 형식.
* DXGI_FORMAT_D16_UNORM
  * 16비트 부호 없는 정규화된 깊이의 형식.
* DXGI_FORMAT_R16_UNORM
  * 16비트 부호 없는 정규화된 R 채널의 형식.
* DXGI_FORMAT_R16_UINT
  * 16비트 부호 없는 정수 R 채널의 형식.
* DXGI_FORMAT_R16_SNORM
  * 16비트 부호 있는 정규화된 R 채널의 형식.
* DXGI_FORMAT_R16_SINT
  * 16비트 부호 있는 정수 R 채널의 형식.
* DXGI_FORMAT_R8_TYPELESS
  * 8비트 타입이 지정되지 않은 R 채널의 형식.
* DXGI_FORMAT_R8_UNORM
  * 8비트 부호 없는 정규화된 R 채널의 형식.
* DXGI_FORMAT_R8_UINT
  * 8비트 부호 없는 정수 R 채널의 형식.
* DXGI_FORMAT_R8_SNORM
  * 8비트 부호 있는 정규화된 R 채널의 형식.
* DXGI_FORMAT_R8_SINT
  * 8비트 부호 있는 정수 R 채널의 형식.
* DXGI_FORMAT_A8_UNORM
  * 8비트 부호 없는 정규화된 A 채널의 형식.
* DXGI_FORMAT_R1_UNORM
  * 1비트 부호 없는 정규화된 R 채널의 형식.
* DXGI_FORMAT_R9G9B9E5_SHAREDEXP
  * 9비트 R, G, B 채널과 5비트 공유 지수(exp)의 형식.
* DXGI_FORMAT_R8G8_B8G8_UNORM
  * 8비트 부호 없는 정규화된 R, G, B, G 채널의 형식으로, 2:2 G 인터리브를 사용한다.
* DXGI_FORMAT_G8R8_G8B8_UNORM
  * 8비트 부호 없는 정규화된 G, R, G, B 채널의 형식으로, 2:2 R 인터리브를 사용한다.
* DXGI_FORMAT_BC1_TYPELESS
  * 블록 압축 1형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC1_UNORM
  * 블록 압축 1형식으로, 부호 없는 정규화된 형식.
* DXGI_FORMAT_BC1_UNORM_SRGB
  * 블록 압축 1형식으로, 부호 없는 정규화된 형식이며, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_BC2_TYPELESS
  * 블록 압축 2형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC2_UNORM
  * 블록 압축 2형식으로, 부호 없는 정규화된 형식.
* DXGI_FORMAT_BC2_UNORM_SRGB
  * 블록 압축 2형식으로, 부호 없는 정규화된 형식이며, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_BC3_TYPELESS
  * 블록 압축 3형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC3_UNORM
  * 블록 압축 3형식으로, 부호 없는 정규화된 형식.
* DXGI_FORMAT_BC3_UNORM_SRGB
  * 블록 압축 3형식으로, 부호 없는 정규화된 형식이며, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_BC4_TYPELESS
  * 블록 압축 4형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC4_UNORM
  * 블록 압축 4형식으로, 부호 없는 정규화된 형식.
* DXGI_FORMAT_BC4_SNORM
  * 블록 압축 4형식으로, 부호 있는 정규화된 형식.
* DXGI_FORMAT_BC5_TYPELESS
  * 블록 압축 5형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC5_UNORM
  * 블록 압축 5형식으로, 부호 없는 정규화된 형식.
* DXGI_FORMAT_BC5_SNORM
  * 블록 압축 5형식으로, 부호 있는 정규화된 형식.
* DXGI_FORMAT_B5G6R5_UNORM
  * 5비트 부호 없는 정규화된 B, 6비트 G, 5비트 R 채널의 형식.
* DXGI_FORMAT_B5G5R5A1_UNORM
  * 5비트 부호 없는 정규화된 B, G, R 채널과 1비트 부호 없는 정규화된 A 채널의 형식.
* DXGI_FORMAT_B8G8R8A8_UNORM
  * 8비트 부호 없는 정규화된 B, G, R, A 채널의 형식.
* DXGI_FORMAT_B8G8R8X8_UNORM
  * 8비트 부호 없는 정규화된 B, G, R 채널과 8비트 패딩의 형식.
* DXGI_FORMAT_R10G10B10_XR_BIAS_A2_UNORM
  * 10비트 부호 없는 정규화된 RGB 채널과 2비트 부호 없는 정규화된 A 채널의 형식으로, XR 바이어스 사용.
* DXGI_FORMAT_B8G8R8A8_TYPELESS
  * 8비트 타입이 지정되지 않은 B, G, R, A 채널의 형식.
* DXGI_FORMAT_B8G8R8A8_UNORM_SRGB
  * 8비트 부호 없는 정규화된 B, G, R, A 채널의 형식으로, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_B8G8R8X8_TYPELESS
  * 8비트 타입이 지정되지 않은 B, G, R 채널과 8비트 패딩의 형식.
* DXGI_FORMAT_B8G8R8X8_UNORM_SRGB
  * 8비트 부호 없는 정규화된 B, G, R 채널과 8비트 패딩의 형식으로, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_BC6H_TYPELESS
  * 블록 압축 6형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC6H_UF16
  * 블록 압축 6형식으로, 16비트 부동 소수점 형식.
* DXGI_FORMAT_BC6H_SF16
  * 블록 압축 6형식으로, 부호 있는 16비트 부동 소수점 형식.
* DXGI_FORMAT_BC7_TYPELESS
  * 블록 압축 7형식으로, 타입이 지정되지 않은 형식.
* DXGI_FORMAT_BC7_UNORM
  * 블록 압축 7형식으로, 부호 없는 정규화된 형식.
* DXGI_FORMAT_BC7_UNORM_SRGB
  * 블록 압축 7형식으로, 부호 없는 정규화된 형식이며, sRGB 색 공간을 사용한다.
* DXGI_FORMAT_AYUV
  * 8비트 A, Y, U, V 채널의 형식.
* DXGI_FORMAT_Y410
  * 10비트 Y, U, V 채널과 2비트 A 채널의 형식.
* DXGI_FORMAT_Y416
  * 16비트 Y, U, V 채널과 16비트 A 채널의 형식.
* DXGI_FORMAT_NV12
  * 8비트 Y 채널과 8비트 2채널의 형식.
* DXGI_FORMAT_P010
  * 10비트 Y 채널과 10비트 2채널의 형식.
* DXGI_FORMAT_P016
  * 16비트 Y 채널과 16비트 2채널의 형식.
* DXGI_FORMAT_420_OPAQUE
  * 8비트 Y, U, V 채널의 형식으로, 투명도가 없는 형식.
* DXGI_FORMAT_YUY2
  * 8비트 Y, U, V 채널의 형식.
* DXGI_FORMAT_Y210
  * 10비트 Y, U, V 채널과 10비트 A 채널의 형식.
* DXGI_FORMAT_Y216
  * 16비트 Y, U, V 채널과 16비트 A 채널의 형식.
* DXGI_FORMAT_NV11
  * 8비트 Y 채널과 8비트 2채널의 형식.
* DXGI_FORMAT_AI44
  * 4비트 A와 4비트 I 채널의 형식.
* DXGI_FORMAT_IA44
  * 4비트 I와 4비트 A 채널의 형식.
* DXGI_FORMAT_P8
  * 8비트 색상 인덱스의 형식.
* DXGI_FORMAT_A8P8
  * 8비트 A 채널과 8비트 색상 인덱스의 형식.
* DXGI_FORMAT_B4G4R4A4_UNORM
  * 4비트 부호 없는 정규화된 B, G, R, A 채널의 형식.
* DXGI_FORMAT_P208
  * 8비트 2채널과 8비트 Y 채널의 형식.
* DXGI_FORMAT_V208
  * 8비트 2채널과 8비트 V 채널의 형식.
* DXGI_FORMAT_V408
  * 8비트 4채널의 형식.
* DXGI_FORMAT_SAMPLER_FEEDBACK_MIN_MIP_OPAQUE
  * 샘플러 피드백 최소 MIP 투명 형식.
* DXGI_FORMAT_SAMPLER_FEEDBACK_MIP_REGION_USED_OPAQUE
  * 샘플러 피드백 MIP 영역 사용 투명 형식.
