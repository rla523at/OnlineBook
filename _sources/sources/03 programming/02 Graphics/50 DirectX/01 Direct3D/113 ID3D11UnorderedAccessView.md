# ID3D11UnorderedAccessView
ID3D11UnorderedAccessView는 Direct3D 11에서 셰이더 프로그램이 리소스에 임의로 접근할 수 있게 하는 인터페이스다. 

ID3D11UnorderedAccessView의 주요 역할은 셰이더가 리소스에 임의로 접근하여 읽기 및 쓰기 작업을 수행할 수 있도록 하는 것이다. 

이 클래스는 ID3D11Device를 사용하여 생성된다. 이를 위해 D3D11_UNORDERED_ACCESS_VIEW_DESC 구조체를 사용하여 접근할 리소스의 속성을 정의하고, 해당 속성에 따라 ID3D11UnorderedAccessView를 초기화한다. 그런 다음, CreateUnorderedAccessView 메서드를 호출하여 ID3D11UnorderedAccessView 객체를 생성한다.

ID3D11UnorderedAccessView는 ID3D11DeviceContext를 통해 그래픽 또는 컴퓨팅 파이프라인에 바인딩된다. 이를 통해 셰이더가 해당 리소스에 접근할 수 있게 된다. 예를 들어, CSSetUnorderedAccessViews 메서드를 사용하여 컴퓨트 셰이더에 ID3D11UnorderedAccessView를 바인딩하거나, OMSetRenderTargetsAndUnorderedAccessViews 메서드를 사용하여 렌더 타겟과 함께 ID3D11UnorderedAccessView를 설정할 수 있다.

## D3D11_UNORDERED_ACCESS_VIEW_DESC 구조체
D3D11_UNORDERED_ACCESS_VIEW_DESC 구조체는 Direct3D 11에서 ID3D11UnorderedAccessView를 정의하기 위한 구조체이다. 

정의는 다음과 같다.
```cpp
typedef struct D3D11_UNORDERED_ACCESS_VIEW_DESC {
    DXGI_FORMAT Format;
    D3D11_UAV_DIMENSION ViewDimension;
    union {
        D3D11_BUFFER_UAV Buffer;
        D3D11_TEX1D_UAV Texture1D;
        D3D11_TEX1D_ARRAY_UAV Texture1DArray;
        D3D11_TEX2D_UAV Texture2D;
        D3D11_TEX2D_ARRAY_UAV Texture2DArray;
        D3D11_TEX3D_UAV Texture3D;
    };
} D3D11_UNORDERED_ACCESS_VIEW_DESC;
```
각각의 멤버변수는 다음과 같다.

* DXGI_FORMAT Format
  * 무순서 접근 뷰의 포맷을 지정한다.
  * 가능한 값
    * DXGI_FORMAT_R32G32B32A32_FLOAT: 128비트 RGBA 포맷
    * DXGI_FORMAT_R8G8B8A8_UNORM: 32비트 RGBA 포맷
  * 기본값은 DXGI_FORMAT_UNKNOWN이다.

* D3D11_UAV_DIMENSION ViewDimension
  * 무순서 접근 뷰의 차원을 지정한다.
  * 가능한 값
    * D3D11_UAV_DIMENSION_BUFFER: 버퍼 리소스 뷰
    * D3D11_UAV_DIMENSION_TEXTURE1D: 1D 텍스처 리소스 뷰
    * D3D11_UAV_DIMENSION_TEXTURE1DARRAY: 1D 텍스처 배열 리소스 뷰
    * D3D11_UAV_DIMENSION_TEXTURE2D: 2D 텍스처 리소스 뷰
    * D3D11_UAV_DIMENSION_TEXTURE2DARRAY: 2D 텍스처 배열 리소스 뷰
    * D3D11_UAV_DIMENSION_TEXTURE3D: 3D 텍스처 리소스 뷰
  * 기본값은 D3D11_UAV_DIMENSION_UNKNOWN이다.

* union
  * 무순서 접근 뷰의 상세 설정을 위한 유니온이다. 각 타입에 따라 구조체를 사용한다.
  * 가능한 값과 각 값이 갖는 의미
    * D3D11_BUFFER_UAV Buffer
      * 버퍼 리소스에 대한 설정
        * UINT FirstElement: 뷰의 첫 번째 요소를 지정
        * UINT NumElements: 뷰에 포함된 요소의 수를 지정
        * UINT Flags: 버퍼 뷰에 대한 플래그 (D3D11_BUFFER_UAV_FLAG_RAW 가능)
    * D3D11_TEX1D_UAV Texture1D
      * 1D 텍스처 리소스에 대한 설정
        * UINT MipSlice: 접근할 MIP 슬라이스를 지정
    * D3D11_TEX1D_ARRAY_UAV Texture1DArray
      * 1D 텍스처 배열 리소스에 대한 설정
        * UINT MipSlice: 접근할 MIP 슬라이스를 지정
        * UINT FirstArraySlice: 접근할 첫 번째 배열 슬라이스를 지정
        * UINT ArraySize: 배열의 크기를 지정
    * D3D11_TEX2D_UAV Texture2D
      * 2D 텍스처 리소스에 대한 설정
        * UINT MipSlice: 접근할 MIP 슬라이스를 지정
    * D3D11_TEX2D_ARRAY_UAV Texture2DArray
      * 2D 텍스처 배열 리소스에 대한 설정
        * UINT MipSlice: 접근할 MIP 슬라이스를 지정
        * UINT FirstArraySlice: 접근할 첫 번째 배열 슬라이스를 지정
        * UINT ArraySize: 배열의 크기를 지정
    * D3D11_TEX3D_UAV Texture3D
      * 3D 텍스처 리소스에 대한 설정
        * UINT MipSlice: 접근할 MIP 슬라이스를 지정
        * UINT FirstWSlice: 접근할 첫 번째 W 슬라이스를 지정
        * UINT WSize: W 슬라이스의 크기를 지정
  * 기본값은 없다. 각 ViewDimension에 따라 적절한 값을 설정해야 한다.

## GetResource 멤버 함수
GetResource 함수는 ID3D11UnorderedAccessView 인터페이스의 멤버 함수로, 언오더드 액세스 뷰(Unordered Access View)가 참조하는 리소스를 가져오는 함수다. 

이 함수는 해당 뷰와 연결된 기본 리소스에 대한 포인터를 반환한다.

시그니처는 다음과 같다.

```cpp
void ID3D11UnorderedAccessView::GetResource(
    ID3D11Resource **ppResource
);
```

매개변수는 다음과 같다.

* ID3D11Resource** ppResource
  * 언오더드 액세스 뷰가 참조하는 리소스를 받을 포인터의 주소
  * 사용 가능한 값
    * 유효한 ID3D11Resource 객체의 포인터를 받을 변수의 주소
  * 기본값: 없음

이 함수는 언오더드 액세스 뷰가 참조하는 리소스에 대한 포인터를 반환하며, 호출자는 반환된 리소스 포인터를 사용하여 해당 리소스에 접근할 수 있다. 이 함수가 호출되면 반환된 리소스의 참조 카운트가 증가하므로, 사용 후에는 반드시 Release 메서드를 호출하여 참조 카운트를 감소시켜야 한다.

## Typed UAV loads
UAV는 지원하는 format에 대해서만 만들 수 있다.

만약, DXGI_FORMAT_R32G32B32_FLOAT과 같이 지원하지 않는 format에 대해 UAV를 만들려고 하면 다음과 같은 오류가 발생한다.

```
X4596	typed UAV stores must write all declared components.	
```

UAV에서 지원하는 Format은 [learn.microsoft - windows/win32/direct3d12/typed-unordered-access-view-loads](https://learn.microsoft.com/en-us/windows/win32/direct3d12/typed-unordered-access-view-loads)에서 확인해볼 수 있다.