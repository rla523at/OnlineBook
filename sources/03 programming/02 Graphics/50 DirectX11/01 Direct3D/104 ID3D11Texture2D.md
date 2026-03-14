# ID3D11Texture2D
ID3D11Texture2D는 Direct3D 11에서 2D 텍스처를 나타내는 인터페이스다. 

이 클래스는 2차원 배열로 구성된 텍스처 데이터를 관리한다.

이 클래스는 ID3D11Device를 사용하여 생성된다. 이를 위해 D3D11_TEXTURE2D_DESC 구조체를 사용하여 텍스처의 속성을 정의하고, 해당 속성에 따라 텍스처를 초기화한다. 그런 다음, CreateTexture2D 메서드를 호출하여 텍스처 객체를 생성한다.

텍스처를 셰이더 리소스로 사용하려면 ID3D11ShaderResourceView를 생성하고, 이를 셰이더에 바인딩한다. 렌더 타겟으로 사용하려면 ID3D11RenderTargetView를 생성하여 렌더 타겟으로 설정할 수 있다. 또한, 깊이 스텐실 버퍼로 사용하기 위해 ID3D11DepthStencilView를 생성하여 깊이 스텐실 버퍼로 설정할 수 있다.

## D3D11_TEXTURE2D_DESC 구조체
D3D11_TEXTURE2D_DESC 구조체는 Direct3D 11에서 2D 텍스처를 생성할 때 사용되는 속성들을 정의한다. 

구조체의 정의는 다음과 같다.
```cpp
typedef struct D3D11_TEXTURE2D_DESC {
    UINT Width;
    UINT Height;
    UINT MipLevels;
    UINT ArraySize;
    DXGI_FORMAT Format;
    DXGI_SAMPLE_DESC SampleDesc;
    D3D11_USAGE Usage;
    UINT BindFlags;
    UINT CPUAccessFlags;
    UINT MiscFlags;
} D3D11_TEXTURE2D_DESC;
```

각 멤버 변수는 다음과 같다.

* UINT Width
  * 텍스처의 너비(픽셀 단위)를 지정한다.
  * 기본값은 없다.

* UINT Height
  * 텍스처의 높이(픽셀 단위)를 지정한다.
  * 기본값은 없다.

* UINT MipLevels
  * MIP 레벨의 수를 지정한다.
  * 기본값은 1이다.

* UINT ArraySize
  * 텍스처 배열의 크기를 지정한다.
  * 기본값은 1이다.

* DXGI_FORMAT Format
  * 텍스처의 형식을 지정한다.
  * 가능한 값
      * DXGI_FORMAT_R8G8B8A8_UNORM: 32비트 텍스처 포맷으로, 8비트씩 RGBA 값을 가진다.
      * DXGI_FORMAT_D24_UNORM_S8_UINT: 24비트 깊이 값과 8비트 스텐실 값을 가진다.
  * 기본값은 없다.

* DXGI_SAMPLE_DESC SampleDesc
  * 멀티샘플링의 개수와 품질 수준을 지정한다.
  * 가능한 값
      * Count: 멀티샘플링의 샘플 수를 지정한다. 기본값은 1이다.
      * Quality: 멀티샘플링의 품질 수준을 지정한다. 기본값은 0이다.

* D3D11_USAGE Usage
  * 텍스처의 사용 용도를 지정한다.
  * 기본값은 D3D11_USAGE_DEFAULT이다.

* UINT BindFlags
  * 텍스처를 바인딩할 파이프라인 단계를 지정한다.
  * D3D11_BIND_FLAG enum에 정의된 값으로 지정한다.
  * 기본값은 0이다.

* UINT CPUAccessFlags
  * CPU가 텍스처에 접근할 수 있는 권한을 지정한다.
  * 가능한 값
      * 0: CPU 접근 권한 없음.
      * enum D3D11_CPU_ACCESS_FLAG 값 
  * 기본값은 0이다.

* UINT MiscFlags
  * 기타 옵션 플래그를 지정한다.
  * 가능한 값
      * 0: 어떤 옵션도 사용하지 않겠다.
      * enum D3D11_RESOURCE_MISC_FLAG 값
  * 기본값은 0이다.

## 2D 이미지 데이터 저장
ID3D11Texture2D 객체는 2D 배열 형식으로 이미지 데이터를 저장한다.

ID3D11Texture2D 객체는 D3D11_TEXTURE2D_DESC에 정의된 방식으로 생성되며, D3D11_TEXTURE2D_DESC에 따라 이미지 데이터를 저장하는 방법이 다르다.

### ID3D11DeviceContext의 UpdateSubresource 함수
UpdateSubresource 함수는 D3D11_TEXTURE2D_DESC의 Usage 변수가 D3D11_USAGE_DEFAULT로 정의된 리소스를 업데이트 할 때 사용된다.

### ID3D11DeviceContext의 Map/UnMap 함수
Map/UnMap 함수는 D3D11_TEXTURE2D_DESC의 Usage 변수가 D3D11_USAGE_DYNAMIC/D3D11_USAGE_STAGING으로 정의된 리소스를 업데이트 할 때 사용된다.

## Tiling 
Texutre2D는 GPU 메모리에서 텍스처 데이터를 효율적으로 접근하고 저장하기 위해 타일 단위로 데이터를 관리하며 이를 타일링(Tiling)이라고 한다. 

각 타일은 일정한 크기의 픽셀 블록을 나타낸다(예: 8x8 또는 16x16 픽셀).

Tiling을 하는 이유는 보통 pixel 관련 연산을 할 때 주변 픽셀에 접근하는 경우가 많음으로 텍스처 데이터를 작은 타일 단위로 나누어 놓으면 필요한 데이터가 더 높은 확률로 캐시에 상주하게 되어 캐시 미스(cache miss)를 줄일 수 있으며, 주변 픽셀과의 데이터 지역성이 향상된다.

예를 들어, 256x256 텍스처 이미지를 16x16 타일로 나눈다고 가정하면, 각 타일은 16x16 픽셀을 포함하며 
전체 텍스처는 16x16 타일로 구성된다.

이 때, 픽셀 좌표 (32, 32)의 픽셀 데이터를 접근할 때, 이는 (2, 2) 타일 내에 위치하게 되며 타일 내 오프셋은 (32 % 16, 32 % 16) = (0, 0)이다.