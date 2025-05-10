# ID3D11ShaderResourceView
ID3D11ShaderResourceView는 Direct3D 11에서 shader가 리소스를 읽을 수 있게 하는 인터페이스다. 

이 클래스는 주로 텍스처와 같은 그래픽 리소스를 shader에 바인딩하는 데 사용된다. 이를 통해 shader는 다양한 그래픽 데이터를 샘플링하고 처리할 수 있다.

ID3D11ShaderResourceView의 주요 역할은 shader가 접근할 수 있는 리소스를 정의하고, 이를 그래픽 파이프라인에 바인딩하는 것이다. shader resource view는 텍스처, 버퍼 등의 리소스를 shader 코드에서 사용할 수 있도록 만들어준다. 예를 들어, 텍스처 매핑을 위해 2D 텍스처를 shader에 바인딩하거나, 복잡한 컴퓨팅 작업을 위해 구조화된 버퍼를 shader에 바인딩할 수 있다.

이 클래스는 ID3D11Device를 사용하여 생성된다. 이를 위해 D3D11_SHADER_RESOURCE_VIEW_DESC 구조체를 사용하여 shader resource view의 속성을 정의하고, 해당 속성에 따라 리소스 뷰를 초기화한다. 이 구조체는 리소스의 형식, 뷰의 차원(예: 1D, 2D, 3D 텍스처), 그리고 사용될 리소스의 특정 부분을 정의한다. 그런 다음, CreateShaderResourceView 메서드를 호출하여 shader resource view 객체를 생성한다.

ID3D11ShaderResourceView는 ID3D11DeviceContext를 통해 shader 스테이지에 바인딩된다. 이를 통해 shader는 지정된 리소스를 접근하고 사용할 수 있다. 예를 들어, PSSetShaderResources 메서드를 사용하여 픽셀 shader에 텍스처를 바인딩하거나, VSSetShaderResources 메서드를 사용하여 버텍스 shader에 리소스를 바인딩할 수 있다.

이 인터페이스는 다양한 그래픽 리소스를 shader에 제공함으로써, 복잡한 그래픽 효과와 연산을 shader 코드에서 구현할 수 있도록 한다. ID3D11ShaderResourceView는 Direct3D 애플리케이션에서 고급 그래픽 효과를 구현하는 데 필수적인 요소로, 텍스처 샘플링, 데이터 접근, 이미지 처리 등의 작업을 가능하게 한다.

## D3D11_SHADER_RESOURCE_VIEW_DESC 구조체
D3D11_SHADER_RESOURCE_VIEW_DESC 구조체는 Direct3D 11에서 shader resource view를 기술하기 위한 구조체이다.

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
  * shader resource view의 포맷을 지정한다.
  * 가능한 값
      * DXGI_FORMAT_R32G32B32A32_FLOAT: 128비트 RGBA 포맷
      * DXGI_FORMAT_R8G8B8A8_UNORM: 32비트 RGBA 포맷
  * 기본값은 DXGI_FORMAT_UNKNOWN이다.

* D3D11_SRV_DIMENSION ViewDimension
  * shader resource view의 차원을 지정한다.
  * 가능한 값
      * enum D3D_SRV_DIMENSION 값
  * 기본값은 D3D11_SRV_DIMENSION_UNKNOWN이다.

* union
  * shader resource view의 상세 설정을 위한 유니온이다. 각 타입에 따라 구조체를 사용한다.
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
D3D11_BUFFER_SRV 구조체는 Direct3D 11에서 버퍼 리소스 뷰를 정의하기 위한 구조체이다. 이 구조체는 shader가 접근할 버퍼의 범위를 지정한다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_BUFFER_SRV {
    UINT FirstElement;
    UINT NumElements;
} D3D11_BUFFER_SRV;
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

