# ID3D11Texture2D
`ID3D11Texture2D`는 Direct3D 11에서 2D 텍스처를 나타내는 인터페이스다. 

이 클래스는 2차원 배열로 구성된 텍스처 데이터를 관리하며, 주로 텍스처 매핑, 렌더 타겟, 심지어 깊이 스텐실 버퍼 등 다양한 용도로 사용된다.

`ID3D11Texture2D`의 주요 역할은 2D 텍스처 리소스를 정의하고 관리하는 것이다. 이 클래스는 텍스처 데이터를 저장하고, 셰이더 프로그램에서 접근할 수 있도록 한다. 예를 들어, 게임 개발에서 캐릭터의 스킨이나 환경 텍스처를 저장하는 데 사용될 수 있다. 또한, 렌더 타겟이나 깊이 스텐실 버퍼로도 사용될 수 있어, 다양한 그래픽 연산에 활용된다.

이 클래스는 `ID3D11Device`를 사용하여 생성된다. 이를 위해 `D3D11_TEXTURE2D_DESC` 구조체를 사용하여 텍스처의 속성을 정의하고, 해당 속성에 따라 텍스처를 초기화한다. 이 구조체는 텍스처의 너비, 높이, MIP 맵 레벨 수, 포맷, 샘플링 설정, 사용 용도 등을 포함한다. 그런 다음, `CreateTexture2D` 메서드를 호출하여 텍스처 객체를 생성한다.

또한, `ID3D11Texture2D`는 다양한 그래픽 파이프라인 단계에서 사용될 수 있다. 텍스처는 셰이더 리소스로서 셰이더 프로그램에 바인딩되어 텍스처 매핑에 사용될 수 있으며, 렌더 타겟으로서 렌더링 결과를 저장하는 데 사용될 수도 있다. 이를 위해 `ID3D11DeviceContext`를 사용하여 텍스처를 적절한 파이프라인 단계에 바인딩한다.

예를 들어, 텍스처를 셰이더 리소스로 사용하려면 `ID3D11ShaderResourceView`를 생성하고, 이를 셰이더에 바인딩한다. 렌더 타겟으로 사용하려면 `ID3D11RenderTargetView`를 생성하여 렌더 타겟으로 설정할 수 있다. 또한, 깊이 스텐실 버퍼로 사용하기 위해 `ID3D11DepthStencilView`를 생성하여 깊이 스텐실 버퍼로 설정할 수 있다.

이 클래스는 2D 텍스처 데이터를 효율적으로 관리하고, 다양한 그래픽 효과를 구현하는 데 필수적인 역할을 한다. `ID3D11Texture2D`는 Direct3D 애플리케이션에서 고품질의 텍스처 매핑과 다양한 그래픽 연산을 지원하는 중요한 구성 요소다.

## 2D 이미지 데이터 저장
ID3D11Texture2D 객체는 2D 배열 형식으로 이미지 데이터를 저장한다.

ID3D11Texture2D 객체는 D3D11_TEXTURE2D_DESC에 정의된 방식으로 생성되며, D3D11_TEXTURE2D_DESC에 따라 이미지 데이터를 저장하는 방법이 다르다.

### ID3D11DeviceContext의 UpdateSubresource 함수
UpdateSubresource 함수는 D3D11_TEXTURE2D_DESC의 Usage 변수가 D3D11_USAGE_DEFAULT로 정의된 리소스를 업데이트 할 때 사용된다.

### ID3D11DeviceContext의 Map/UnMap 함수
Map/UnMap 함수는 D3D11_TEXTURE2D_DESC의 Usage 변수가 D3D11_USAGE_DYNAMIC으로 정의된 리소스를 업데이트 할 때 사용된다.




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

