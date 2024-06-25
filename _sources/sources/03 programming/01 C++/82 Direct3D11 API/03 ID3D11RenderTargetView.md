# ID3D11RenderTargetView
ID3D11RenderTargetView는 Direct3D 11 API에서 렌더링 타겟을 나타내는 객체다. 이 객체는 주로 렌더링 작업의 결과를 저장할 버퍼를 정의하는 데 사용된다.

## Render Target
`렌더링 타겟(Render Target)`은 그래픽스 프로그래밍에서 렌더링의 결과를 출력하는 `표면(surface)`을 말한다. 기본적으로, 렌더링 타겟은 `프레임 버퍼(Frame Buffer)`라고도 불리며, 화면에 보여질 이미지를 저장하는 메모리 공간이다.

### 렌더링 타겟의 역할
* 화면 출력
  * 렌더링 타겟은 최종적으로 화면에 출력될 이미지를 저장한다. 
  * 이는 디스플레이 장치로 전송되어 사용자가 볼 수 있는 형태로 나타난다.
* 임시 저장소
  * 렌더링 파이프라인의 중간 결과를 저장하기 위해 사용된다. 
  * 복잡한 효과나 다단계 렌더링에서 중간 결과를 저장해 다음 렌더링 단계에서 사용한다.
* 포스트 프로세싱
  * 화면에 출력되기 전에 여러 가지 후처리 효과(예: 블러, HDR)를 적용하기 위해 사용된다. 
  * 렌더링 타겟에 먼저 렌더링한 후, 그 결과를 사용해 후처리 효과를 적용한다.

### 렌더링 타겟의 종류
* 백 버퍼(Back Buffer)
  * 일반적으로 화면에 출력될 이미지를 저장하는 기본 렌더링 타겟이다.
  * 프론트 버퍼와 교체되어 화면에 최종적으로 출력된다.
* 텍스처 렌더링 타겟 
  * 특정 텍스처에 렌더링 결과를 저장할 수 있다.
  * 이는 쉐이더에서 사용되거나 후처리 효과를 적용하는 데 활용된다.
* 멀티 렌더 타겟(MRT, Multiple Render Targets) 
  * 동시에 여러 렌더 타겟에 렌더링 결과를 출력할 수 있다.
  * 이는 복잡한 셰이더 효과나 다양한 데이터 출력을 필요로 할 때 유용하다.

## D3D11_RENDER_TARGET_VIEW_DESC 구조체
D3D11_RENDER_TARGET_VIEW_DESC 구조체는 Direct3D 11에서 렌더 타겟 뷰를 정의하는 데 사용된다. 

이 구조체는 렌더 타겟 뷰의 형식, 뷰 차원, 및 특정 텍스처 레벨 또는 배열 슬라이스를 지정하는 다양한 속성을 포함하고 있다.

정의는 다음과 같다.

```cpp
typedef struct D3D11_RENDER_TARGET_VIEW_DESC {
    DXGI_FORMAT Format;
    D3D11_RTV_DIMENSION ViewDimension;
    union {
        D3D11_BUFFER_RTV Buffer;
        D3D11_TEX1D_RTV Texture1D;
        D3D11_TEX1D_ARRAY_RTV Texture1DArray;
        D3D11_TEX2D_RTV Texture2D;
        D3D11_TEX2D_ARRAY_RTV Texture2DArray;
        D3D11_TEX2DMS_RTV Texture2DMS;
        D3D11_TEX2DMS_ARRAY_RTV Texture2DMSArray;
        D3D11_TEX3D_RTV Texture3D;
    };
} D3D11_RENDER_TARGET_VIEW_DESC;
```

각각의 멤버변수는 다음과 같다.

* Format
  * 렌더 타겟 뷰의 데이터 포맷을 지정한다.
  * DXGI_FORMAT enum 값 중 하나를 사용한다.

* ViewDimension
  * D3D11_RTV_DIMENSION enum 값을 사용하여 뷰의 차원을 지정한다.

* union
  * D3D11_RTV_DIMENSION_BUFFER, 
    * D3D11_BUFFER_RTV 구조체로, 버퍼의 렌더 타겟 뷰를 정의한다.
    * FirstElement와 NumElements 멤버를 통해 버퍼의 시작 요소와 요소 수를 지정한다.
  * D3D11_RTV_DIMENSION_TEXTURE1D, 
    * D3D11_TEX1D_RTV 구조체로, 1D 텍스처의 렌더 타겟 뷰를 정의한다.
    * MipSlice 멤버를 통해 사용할 MIP 레벨을 지정한다.
  * D3D11_RTV_DIMENSION_TEXTURE1DARRAY
    * D3D11_TEX1D_ARRAY_RTV 구조체로, 1D 텍스처 배열의 렌더 타겟 뷰를 정의한다.
    * MipSlice, FirstArraySlice, ArraySize 멤버를 포함한다.
  * D3D11_RTV_DIMENSION_TEXTURE2D
    * D3D11_TEX2D_RTV 구조체로, 2D 텍스처의 렌더 타겟 뷰를 정의한다.
    * MipSlice 멤버를 통해 사용할 MIP 레벨을 지정한다.
  * D3D11_RTV_DIMENSION_TEXTURE2DARRAY
    * D3D11_TEX2D_ARRAY_RTV 구조체로, 2D 텍스처 배열의 렌더 타겟 뷰를 정의한다.
    * MipSlice, FirstArraySlice, ArraySize 멤버를 포함한다.
  * D3D11_RTV_DIMENSION_TEXTURE2DMS
    * D3D11_TEX2DMS_RTV 구조체로, 멀티샘플 2D 텍스처의 렌더 타겟 뷰를 정의한다.
    * 이 구조체에는 추가 멤버가 없다.
  * D3D11_RTV_DIMENSION_TEXTURE2DMSARRAY
    * D3D11_TEX2DMS_ARRAY_RTV 구조체로, 멀티샘플 2D 텍스처 배열의 렌더 타겟 뷰를 정의한다.
    * FirstArraySlice와 ArraySize 멤버를 포함한다.
  * D3D11_RTV_DIMENSION_TEXTURE3D 
    * D3D11_TEX3D_RTV 구조체로, 3D 텍스처의 렌더 타겟 뷰를 정의한다.
    * MipSlice와 FirstWSlice, WSize 멤버를 포함한다.

## D3D11_RTV_DIMENSION enum
D3D11_RTV_DIMENSION enum은 Render Target View(RTV)의 차원을 정의하는 enum이다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D11_RTV_DIMENSION
    {
        D3D11_RTV_DIMENSION_UNKNOWN            = 0,
        D3D11_RTV_DIMENSION_BUFFER             = 1,
        D3D11_RTV_DIMENSION_TEXTURE1D          = 2,
        D3D11_RTV_DIMENSION_TEXTURE1DARRAY     = 3,
        D3D11_RTV_DIMENSION_TEXTURE2D          = 4,
        D3D11_RTV_DIMENSION_TEXTURE2DARRAY     = 5,
        D3D11_RTV_DIMENSION_TEXTURE2DMS        = 6,
        D3D11_RTV_DIMENSION_TEXTURE2DMSARRAY   = 7,
        D3D11_RTV_DIMENSION_TEXTURE3D          = 8
    } 	D3D11_RTV_DIMENSION;
```

각 enum의 의미는 다음과 같다.
* D3D11_RTV_DIMENSION_UNKNOWN
  * 차원이 정의되지 않았다.
* D3D11_RTV_DIMENSION_BUFFER
  * Render Target이 버퍼로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE1D
  * Render Target이 1D 텍스처로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE1DARRAY
  * Render Target이 1D 텍스처 배열로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2D
  * Render Target이 2D 텍스처로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2DARRAY
  * Render Target이 2D 텍스처 배열로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2DMS
  * Render Target이 멀티샘플링된 2D 텍스처로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2DMSARRAY
  * Render Target이 멀티샘플링된 2D 텍스처 배열로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE3D
  * Render Target이 3D 텍스처로 구성된다.


## Render Target View에 저장된 데이터 읽기
다음 코드들로 Render Target View에 저장된 데이터를 읽을 수 있다.

```cpp
    // 1. 렌더 타겟 리소스 얻기
     ID3D11Resource* pRenderTargetResource = nullptr;
     renderTargetView->GetResource(&pRenderTargetResource);

     ID3D11Texture2D* pRenderTargetTexture = nullptr;
     pRenderTargetResource->QueryInterface(IID_PPV_ARGS(&pRenderTargetTexture));

    // 2. 임시 텍스처 생성
     D3D11_TEXTURE2D_DESC desc;
     pRenderTargetTexture->GetDesc(&desc);

     desc.Usage          = D3D11_USAGE_STAGING;
     desc.BindFlags      = 0;
     desc.CPUAccessFlags = D3D11_CPU_ACCESS_READ;
     desc.MiscFlags      = 0;

     ID3D11Texture2D* pStagingTexture = nullptr;
     device->CreateTexture2D(&desc, nullptr, &pStagingTexture);

    // 3. 렌더 타겟 텍스처를 스테이징 텍스처로 복사
     deviceContext->CopyResource(pStagingTexture, pRenderTargetTexture);

    // 4. 스테이징 텍스처에서 데이터 읽기
     D3D11_MAPPED_SUBRESOURCE mappedResource;
     HRESULT                  hr = deviceContext->Map(pStagingTexture, 0, D3D11_MAP_READ, 0, &mappedResource);
     if (SUCCEEDED(hr))
    {
       // 픽셀 데이터에 접근
       BYTE* pData    = static_cast<BYTE*>(mappedResource.pData);
       UINT  rowPitch = mappedResource.RowPitch;

      BYTE* pixel1 = pData + 100 * rowPitch + 0 * 4;
      BYTE* pixel2 = pData + 300 * rowPitch + 0 * 4;
      BYTE* pixel3 = pData + 500 * rowPitch + 0 * 4;
      BYTE* pixel4 = pData + 700 * rowPitch + 0 * 4;
      BYTE* pixel5 = pData + 900 * rowPitch + 0 * 4;

      deviceContext->Unmap(pStagingTexture, 0);
    }

    // 5. 자원 해제
     pRenderTargetResource->Release();
     pRenderTargetTexture->Release();
     pStagingTexture->Release();
```
