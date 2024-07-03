# ID3D11ShaderResourceView
ID3D11ShaderResourceView는 shader가 리소스를 읽을 수 있게 하는 인터페이스이다. 

이 인터페이스는 주로 텍스처와 같은 그래픽 리소스를 셰이더에 바인딩하는 데 사용된다.

shader에 리소스가 바인딩 되면 리소스를 셰이더에 사용할 수 있는 형태로 만들고 리소스의 특정 부분이나 특정 포맷을 shader가 접근할 수 있게 해준다.

## D3D11_SHADER_RESOURCE_VIEW_DESC 구조체
D3D11_SHADER_RESOURCE_VIEW_DESC 구조체는 Direct3D 11에서 셰이더 리소스 뷰를 기술하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_SHADER_RESOURCE_VIEW_DESC {
    DXGI_FORMAT Format;
    D3D11_SRV_DIMENSION ViewDimension;
    union {
        D3D11_BUFFER_SRV Buffer;
        D3D11_TEX1D_SRV Texture1D;
        D3D11_TEX1D_ARRAY_SRV Texture1DArray;
        D3D11_TEX2D_SRV Texture2D;
        D3D11_TEX2D_ARRAY_SRV Texture2DArray;
        D3D11_TEX2DMS_SRV Texture2DMS;
        D3D11_TEX2DMS_ARRAY_SRV Texture2DMSArray;
        D3D11_TEX3D_SRV Texture3D;
        D3D11_TEXCUBE_SRV TextureCube;
        D3D11_TEXCUBE_ARRAY_SRV TextureCubeArray;
        D3D11_BUFFEREX_SRV BufferEx;
    };
} D3D11_SHADER_RESOURCE_VIEW_DESC;
```
각각의 멤버변수는 다음과 같다.

* DXGI_FORMAT Format
  * 셰이더 리소스 뷰의 포맷을 지정한다.
  * 가능한 값
      * DXGI_FORMAT_R32G32B32A32_FLOAT: 128비트 RGBA 포맷
      * DXGI_FORMAT_R8G8B8A8_UNORM: 32비트 RGBA 포맷
  * 기본값은 DXGI_FORMAT_UNKNOWN이다.

* D3D11_SRV_DIMENSION ViewDimension
  * 셰이더 리소스 뷰의 차원을 지정한다.
  * 가능한 값
      * enum D3D_SRV_DIMENSION 값
  * 기본값은 D3D11_SRV_DIMENSION_UNKNOWN이다.

* union
  * 셰이더 리소스 뷰의 상세 설정을 위한 유니온이다. 각 타입에 따라 구조체를 사용한다.
  * 가능한 값과 각 값이 갖는 의미
    * D3D11_BUFFER_SRV Buffer
      * 버퍼 리소스에 대한 설정
    * D3D11_TEX1D_SRV Texture1D
      * 1D 텍스처 리소스에 대한 설정
    * D3D11_TEX1D_ARRAY_SRV Texture1DArray
      * 1D 텍스처 배열 리소스에 대한 설정
    * D3D11_TEX2D_SRV Texture2D
      * 2D 텍스처 리소스에 대한 설정
    * D3D11_TEX2D_ARRAY_SRV Texture2DArray
      * 2D 텍스처 배열 리소스에 대한 설정
    * D3D11_TEX2DMS_SRV Texture2DMS
      * 멀티샘플 2D 텍스처 리소스에 대한 설정
    * D3D11_TEX2DMS_ARRAY_SRV Texture2DMSArray
      * 멀티샘플 2D 텍스처 배열 리소스에 대한 설정
    * D3D11_TEX3D_SRV Texture3D
      * 3D 텍스처 리소스에 대한 설정
    * D3D11_TEXCUBE_SRV TextureCube
      * 큐브 텍스처 리소스에 대한 설정
    * D3D11_TEXCUBE_ARRAY_SRV TextureCubeArray
      * 큐브 텍스처 배열 리소스에 대한 설정
    * D3D11_BUFFEREX_SRV BufferEx
      * 확장된 버퍼 리소스에 대한 설정
  * 기본값은 없다. 각 ViewDimension에 따라 적절한 값을 설정해야 한다.

### D3D11_BUFFER_SRV
D3D11_BUFFER_SRV 구조체는 Direct3D 11에서 버퍼 리소스 뷰를 정의하기 위한 구조체이다. 이 구조체는 셰이더가 접근할 버퍼의 범위를 지정한다.

정의는 다음과 같다.

typedef struct D3D11_BUFFER_SRV {
    UINT FirstElement;
    UINT NumElements;
} D3D11_BUFFER_SRV;

각각의 멤버변수는 다음과 같다.

* UINT FirstElement
  * 뷰의 첫 번째 요소를 지정한다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 0이다.

* UINT NumElements
  * 뷰에 포함된 요소의 수를 지정한다.
  * 가능한 값
    * 1 이상의 정수 값
  * 기본값은 전체 버퍼 크기에서 FirstElement를 뺀 값이다.


### D3D11_BUFFEREX_SRV 구조체
D3D11_BUFFEREX_SRV 구조체는 Direct3D 11에서 확장된 버퍼 리소스 뷰를 정의하기 위한 구조체이다. 이 구조체는 버퍼의 범위를 지정하고, 원시 뷰를 허용하는 옵션을 제공한다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_BUFFEREX_SRV {
    UINT FirstElement;
    UINT NumElements;
    UINT Flags;
} D3D11_BUFFEREX_SRV;
```

각각의 멤버변수는 다음과 같다.

* UINT FirstElement
  * 뷰의 첫 번째 요소를 지정한다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 0이다.

* UINT NumElements
  * 뷰에 포함된 요소의 수를 지정한다.
  * 가능한 값
    * 1 이상의 정수 값
  * 기본값은 전체 버퍼 크기에서 FirstElement를 뺀 값이다.

* UINT Flags
  * 버퍼 뷰에 대한 옵션 플래그를 지정한다.
  * 가능한 값
    * D3D11_BUFFEREX_SRV_FLAG_RAW: 버퍼의 원시 뷰를 허용
  * 기본값은 0이다.

