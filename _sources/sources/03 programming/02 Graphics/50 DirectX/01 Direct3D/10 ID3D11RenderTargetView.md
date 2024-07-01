# ID3D11RenderTargetView
ID3D11RenderTargetView는 Direct3D 11에서 `렌더 타겟(render target)`을 나타내는 인터페이스다. 

이 클래스는 렌더링 파이프라인에서 출력 목표를 정의하고, 렌더링된 픽셀이 쓰여질 위치를 지정하는 역할을 한다. ID3D11RenderTargetView는 주로 화면에 렌더링 결과를 표시하거나, 텍스처에 렌더링 데이터를 저장하는 데 사용된다.

ID3D11RenderTargetView의 주요 역할은 render target을 설정하고 관리하는 것이다. render target은 렌더링된 픽셀이 저장되는 버퍼로 화면에 보여질 이미지를 저장하는 메모리 공간이다. render target으로는 주로 백 버퍼나 텍스처가 사용된다. 이 인터페이스를 통해 애플리케이션은 렌더링 출력을 특정 render target에 지정할 수 있다. 예를 들어, 화면에 직접 렌더링할 때는 백 버퍼를 render target으로 설정하고, 포스트 프로세싱 효과를 위해 텍스처를 render target으로 설정할 수 있다.

ID3D11RenderTargetView는 ID3D11Device를 사용하여 생성된다. 이를 위해 D3D11_RENDER_TARGET_VIEW_DESC 구조체를 사용하여 render target 뷰의 속성을 정의하고, 해당 속성에 따라 render target 뷰를 초기화한다. 이 구조체는 render target 뷰의 포맷, 뷰의 차원(예: 1D, 2D, 3D 텍스처), 그리고 사용될 리소스의 특정 부분을 정의한다.

또한, ID3D11RenderTargetView는 ID3D11DeviceContext를 통해 그래픽 파이프라인에 바인딩된다. 이를 통해 렌더링 작업이 수행되는 동안 특정 render target 뷰를 활성화하고, 렌더링 결과가 해당 뷰에 기록되도록 한다. 예를 들어, OMSetRenderTargets 메서드를 사용하여 render target 뷰를 설정하고, 이후의 드로우 콜이 지정된 render target에 출력되도록 할 수 있다.

이 인터페이스는 복수의 render target을 지원하므로, 복잡한 그래픽 효과를 구현하는 데 유용하다. 예를 들어, 다중 render target을 사용하여 여러 개의 출력 버퍼에 동시에 렌더링하거나, 디퍼드 셰이딩과 같은 고급 렌더링 기술을 구현할 수 있다.

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
