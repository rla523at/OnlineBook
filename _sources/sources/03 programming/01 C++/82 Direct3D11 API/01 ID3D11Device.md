# ID3D11Device
ID3D11Device는 Direct3D 11 API의 인터페이스 중 하나로, COM(Component Object Model) 기반의 클래스다.

ID3D11Device 인터페이스는 GPU(Device)와 상호작용하는 데 필요한 다양한 리소스와 상태 객체를 생성하고 관리하는 역할을 한다.

* 리소스 생성
  * ID3D11Device 클래스는 Buffer 및 Texture를 생성한다.
* 리소스 관리
  * ID3D11Device 클래스는 Shader Resource View(SRV), Render Target View(RTV), Depth-Stencil View(DSV)등을 생성하여 Texture와 Buffer를 렌더링 파이프라인에 바인딩할 수 있게 한다.
  * 그래픽 파이프라인에서 리소스를 사용하려면, 해당 리소스를 적절한 뷰를 통해 파이프라인에 바인딩해야 한다. 이를 통해 GPU는 해당 리소스를 인식하고, 적절한 단계에서 사용할 수 있게 된다.

## CreateTexture2D 함수
ID3D11Device::CreateTexture2D 함수는 Direct3D 11에서 2D 텍스처를 생성하는 데 사용된다. 

이 함수는 주어진 D3D11_TEXTURE2D_DESC에 따라 2D 텍스처를 생성하고, 생성된 텍스처에 대한 포인터를 반환한다.

함수의 시그니쳐는 다음과 같다.

```cpp
HRESULT CreateTexture2D(
  const D3D11_TEXTURE2D_DESC *pDesc,
  const D3D11_SUBRESOURCE_DATA *pInitialData,
  ID3D11Texture2D **ppTexture2D
);
```

각 매개변수는 다음과 같다.
* const D3D11_TEXTURE2D_DESC* pDesc:
  * 생성할 텍스처의 특성을 정의하는 D3D11_TEXTURE2D_DESC 구조체에 대한 포인터다. 

* const D3D11_SUBRESOURCE_DATA* pInitialData:
  * 텍스처의 초기 데이터를 제공하는 D3D11_SUBRESOURCE_DATA 구조체에 대한 포인터다.
  * 각 서브리소스의 초기화 데이터를 포함하고 있으며, 이 매개변수가 NULL일 경우 텍스처는 초기 데이터 없이 생성된다.

* ID3D11Texture2D** ppTexture2D:
  * 생성된 텍스처 객체에 대한 포인터의 주소를 반환한다.


## CheckMultisampleQualityLevels 함수
CheckMultisampleQualityLevels 함수는 지정된 포맷 및 샘플 수에 대해 사용할 수 있는 멀티샘플링(Multi-Sample Anti-Aliasing; MSAA) 품질 수준의 수를 확인하는 함수다.

멀티샘플링은 렌더링 품질을 향상시키기 위해 여러 샘플을 사용하여 픽셀의 가장자리 부드럽게 하는 기술이다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CheckMultisampleQualityLevels(
    DXGI_FORMAT Format,
    UINT SampleCount,
    UINT *pNumQualityLevels
);
```

각각의 매개변수는 다음과 같다.

* Format
  * DXGI_FORMAT 열거형으로, 확인할 멀티샘플링을 지원하는 텍스처 포맷을 지정한다. 
  * 예를 들어, DXGI_FORMAT_R8G8B8A8_UNORM은 일반적인 32비트 RGBA 포맷이다.
* SampleCount
  * 멀티샘플링에 사용할 샘플의 수를 지정한다. 
  * 이 값은 보통 1, 2, 4, 8, 16 등의 값을 가지며, 특정 하드웨어가 지원하는 샘플 수에 따라 다를 수 있다.
* pNumQualityLevels
  * 지정된 포맷과 샘플 수에 대해 사용 가능한 품질 수준의 수를 반환하는 포인터다. 
  * 이 값은 0부터 시작하며, 값이 1 이상이면 멀티샘플링을 사용할 수 있는 품질 수준이 있음을 의미한다.

### 멀티 샘플링 안티앨리어싱(MSAA) vs 슈퍼 샘플링 안티앨리어싱(SSAA)
MSAA는 각 픽셀을 여러 개의 서브픽셀로 나누고, 경계선에서만 다중 샘플링을 수행한다. 이는 주로 삼각형의 가장자리에서 앨리어싱을 줄이는 데 집중된다. 따라서 경계선 내부의 픽셀은 다중 샘플링을 하지 않으므로, 전체 화면에서 필요한 샘플 수를 줄인다.

SSAA는 화면을 고해상도로 렌더링한 다음, 결과를 다운샘플링하여 최종 이미지를 생성한다. 예를 들어, 2x SSAA는 화면을 원래 해상도의 두 배로 렌더링하고 이를 다시 원래 해상도로 축소한다. 따라서 화면의 모든 픽셀에 대해 다중 샘플링을 수행하여 모든 부분에서 앨리어싱을 줄인다.

| 특성                  | SSAA                       | MSAA                         |
|-----------------------|----------------------------|------------------------------|
|   샘플링 방식         | 모든 픽셀에 대해 다중 샘플링 | 경계선에서만 다중 샘플링    |
|   이미지 품질         | 매우 높음                   | 높음                         |
|   성능 비용           | 매우 높음                   | 낮음                         |
|   메모리 사용량       | 높음                        | 낮음                         |
|   적용 범위           | 전체 이미지                 | 경계선 주로                  |
|   복잡성              | 간단                        | 복잡                         |


## CreateRenderTargetView 함수
CreateRenderTargetView는 렌더 타겟 뷰를 생성하는 함수다. 이 함수는 ID3D11RenderTargetView 객체를 생성하여, 렌더링 파이프라인의 출력 병합 단계(OM, Output Merger)에 사용할 수 있게 한다.

```cpp
HRESULT CreateRenderTargetView(
  ID3D11Resource *pResource,
  const D3D11_RENDER_TARGET_VIEW_DESC *pDesc,
  ID3D11RenderTargetView **ppRTView
);
```

각각의 매개변수는 다음과 같다.

* pResource
  * 렌더 타겟 뷰를 생성할 리소스에 대한 포인터다.
  * 일반적으로 이는 텍스처(예: ID3D11Texture2D) 객체를 가리킨다.
* pDesc
  * 생성할 렌더 타겟 뷰의 속성을 정의하는 D3D11_RENDER_TARGET_VIEW_DESC 구조체에 대한 포인터다.
  * 이 구조체를 통해 포맷, 뷰의 첫 번째 배열 슬라이스 및 배열 슬라이스 수를 지정할 수 있다.
  * 이 값이 NULL인 경우, 리소스의 기본 뷰가 생성된다.
    * 기본 뷰를 사용하면 Direct3D가 해당 리소스에 대해 기본 설정을 사용하여 렌더 타겟 뷰를 생성한다.
    * 포맷: 리소스의 기본 포맷이 사용된다.
    * 차원: 리소스의 타입에 따라 자동으로 결정된다.
    * 슬라이스 및 크기: 리소스 전체를 대상으로 하는 뷰가 생성된다.
    * 기본 뷰를 사용하는 것은 간단한 설정을 요구하는 경우에 유용하며, 세부적인 제어가 필요하지 않을 때 편리하다.
* ppRTView
  * 생성된 렌더 타겟 뷰의 주소를 받을 포인터에 대한 이중 포인터다.
  * 성공적으로 생성되면, 이 포인터는 ID3D11RenderTargetView 인터페이스를 가리키게 된다.

## CreateRasterizerState 함수
CreateRasterizerState 함수는 Direct3D 11에서 ID3D11RasterizerState 객체를 생성하는 데 사용된다. 

ID3D11RasterizerState는 래스터화 단계의 동작을 제어하는 상태 객체이며 D3D11_RASTERIZER_DESC 구조체를 입력으로 받아, 해당 구조체의 설정에 따라 ID3D11RasterizerState 객체를 생성한다.

함수의 시그니쳐는 다음과 같다.
```cpp
HRESULT CreateRasterizerState(
  const D3D11_RASTERIZER_DESC *pRasterizerDesc,
  ID3D11RasterizerState **ppRasterizerState
);
```

각 매개변수는 다음과 같다.
* pRasterizerDesc
  * D3D11_RASTERIZER_DESC 구조체에 대한 포인터로, 래스터라이저 상태를 정의하는 설정을 포함한다.
* ppRasterizerState
  * 생성된 객체의 주소를 호출한 측에 전달하기 위해서는 역참조를 통해 생성된 ID3D11RasterizerState 객체의 주소를 받아야 함으로 역참조 했을 때, 객체의 주소를 가르키는 포인터가 되게 이중 포인터로 되어 있다.

의사 코드는 다음과 같다.
```cpp
// 새로운 ID3D11RasterizerState 객체 생성
ID3D11RasterizerState* pNewState = new ID3D11RasterizerState(pRasterizerDesc);

// 생성된 객체의 주소를 호출한 측에 전달
*ppRasterizerState = pNewState;
```

## CreateDepthStencilView 함수
CreateDepthStencilView 함수는 ID3D11DepthStencilView를 생성하는 데 사용되는 함수다. 

이 함수는 주어진 텍스처 리소스를 기반으로 ID3D11DepthStencilView를를 생성하며, 이 뷰를 통해 깊이 및 스텐실 테스트를 수행할 수 있다.

함수의 시그니쳐는 다음과 같다.
```cpp
HRESULT CreateDepthStencilView(
  ID3D11Resource *pResource,
  const D3D11_DEPTH_STENCIL_VIEW_DESC *pDesc,
  ID3D11DepthStencilView **ppDepthStencilView
);
```

각 매개변수는 다음과 같다.
* pResource:
  * ID3D11Resource* 타입의 포인터로, 깊이-스텐실 뷰를 생성할 텍스처 리소스를 가리킨다.
  * 일반적으로 ID3D11Texture2D 타입의 텍스처 리소스를 사용한다.
* pDesc:
  * D3D11_DEPTH_STENCIL_VIEW_DESC 구조체에 대한 포인터로, 깊이-스텐실 뷰의 속성을 정의한다.
  * 이 구조체는 뷰의 형식, 차원, MIP 슬라이스 등을 지정한다.
  * 이 매개변수가 NULL일 경우, 기본 속성으로 뷰가 생성된다.
* ppDepthStencilView:
  * 생성된 ID3D11DepthStencilView 객체에 대한 포인터의 주소를 반환한다.
  * 이 이중 포인터를 통해 함수가 성공적으로 뷰를 생성했을 때, 호출한 측에서 이 뷰를 사용할 수 있다.

## CreateInputLayout 멤버 함수
CreateInputLayout 함수는 Input Lyaout을 생성하는 함수다. 

Input Lyaout은 셰이더가 입력 데이터를 어떻게 해석할지를 정의하며, 주로 정점 셰이더에 사용된다.

시그니처는 다음과 같다.
```cpp
HRESULT ID3D11Device::CreateInputLayout(
    const D3D11_INPUT_ELEMENT_DESC *pInputElementDescs,
    UINT NumElements,
    const void *pShaderBytecodeWithInputSignature,
    SIZE_T BytecodeLength,
    ID3D11InputLayout **ppInputLayout
);
```
매개변수는 다음과 같다.

* const D3D11_INPUT_ELEMENT_DESC* pInputElementDescs
  * 입력 요소의 배열에 대한 포인터
  * 사용 가능한 값
    * 유효한 D3D11_INPUT_ELEMENT_DESC 구조체 배열의 포인터
  * 기본값: 없음

* UINT NumElements
  * pInputElementDescs 배열의 요소 수
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* const void* pShaderBytecodeWithInputSignature
  * 셰이더 바이트 코드의 포인터
  * 사용 가능한 값
    * 유효한 셰이더 바이트 코드의 포인터
  * 기본값: 없음

* SIZE_T BytecodeLength
  * pShaderBytecodeWithInputSignature의 바이트 코드 길이
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* ID3D11InputLayout** ppInputLayout
  * 생성된 Input Lyaout 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 Input Lyaout 객체를 반환하지 않는다.
    * ID3D11InputLayout: 생성된 Input Lyaout 객체를 반환한다.
  * 기본값: 없음
