# ID3D11Device
ID3D11Device는 Direct3D 11에서 그래픽 디바이스(GPU)를 나타내는 인터페이스다. 

이 클래스는 GPU와의 통신을 담당하며, 다양한 그래픽 리소스를 생성하고 관리하는 역할을 한다. 

ID3D11Device의 주요 역할은 그래픽 리소스와 관련된 작업을 수행하는 것이다. 이 클래스는 버퍼, 텍스처, 셰이더 등의 리소스를 생성하고 관리하며, 이러한 리소스를 GPU에 전달하여 렌더링 작업을 수행할 수 있도록 한다. 예를 들어, `ID3D11Buffer`를 생성하여 버텍스 버퍼나 인덱스 버퍼를 만들고, `ID3D11Texture2D`를 생성하여 2D 텍스처를 만든다.

또한, ID3D11Device는 셰이더 객체를 생성하고 관리한다. 셰이더는 그래픽 파이프라인에서 중요한 역할을 하며, ID3D11Device를 통해 버텍스 셰이더, 픽셀 셰이더 등의 셰이더를 생성할 수 있다. 이를 통해 다양한 그래픽 효과를 구현할 수 있다.

ID3D11Device는 그래픽 파이프라인 상태를 설정하고 관리하는 기능도 제공한다. 이는 그래픽 파이프라인에서 렌더링 작업을 수행하기 위해 필요한 다양한 설정을 관리하는 데 사용된다. 예를 들어, `렌더 타겟 뷰 (RenderTargetView;RTV)`, `깊이-스텐실 뷰(DepthStencilView;DSV)` 등을 설정하여 렌더링 결과를 특정 버퍼에 출력할 수 있다.

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

## CreateRenderTargetView 함수
CreateRenderTargetView는 렌더 타겟 뷰를 생성하는 함수다. 

이 함수는 ID3D11RenderTargetView 객체를 생성하여, 렌더링 파이프라인의 출력 병합 단계(OM, Output Merger)에 사용할 수 있게 한다.

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

## CreateShaderResourceView 함수
CreateShaderResourceView 함수는 ID3DShaderResourceView를 생성하는 함수다. 

ID3DShaderResourceView는 shader에서 텍스처나 버퍼와 같은 리소스를 읽을 수 있게 한다.

시그니처는 다음과 같다.
```cpp
HRESULT ID3D11Device::CreateShaderResourceView(
    ID3D11Resource *pResource,
    const D3D11_SHADER_RESOURCE_VIEW_DESC *pDesc,
    ID3D11ShaderResourceView **ppSRView
);
```
매개변수는 다음과 같다.

* ID3D11Resource* pResource
  * shader가 읽을 리소스에 대한 포인터
  * 사용 가능한 값
    * 유효한 ID3D11Resource 객체
  * 기본값: 없음

* const D3D11_SHADER_RESOURCE_VIEW_DESC* pDesc
  * 셰이더 리소스 뷰의 속성을 지정하는 구조체에 대한 포인터
  * 사용 가능한 값
    * NULL: 리소스의 기본 뷰를 생성한다.
    * 유효한 D3D11_SHADER_RESOURCE_VIEW_DESC 구조체
  * 기본값: NULL

* ID3D11ShaderResourceView** ppSRView
  * 생성된 셰이더 리소스 뷰 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 셰이더 리소스 뷰 객체를 반환하지 않는다.
    * ID3D11ShaderResourceView: 생성된 셰이더 리소스 뷰 객체를 반환한다.
  * 기본값: 없음

## CreateSamplerState 함수
CreateSamplerState 함수는 ID3D11Device 인터페이스의 멤버 함수로, ID3D11SamplerState 객체를 생성하는 함수다. 

ID3D11SamplerState 객체는 텍스처 샘플링 동작을 정의하며, 텍스처 필터링 모드와 경계 처리 방식을 설정한다.

시그니처는 다음과 같다.
```cpp
HRESULT ID3D11Device::CreateSamplerState(
    const D3D11_SAMPLER_DESC *pSamplerDesc,
    ID3D11SamplerState **ppSamplerState
);
```
매개변수는 다음과 같다.

* const D3D11_SAMPLER_DESC* pSamplerDesc
  * 샘플러 상태를 정의하는 구조체에 대한 포인터
  * 사용 가능한 값
    * 유효한 D3D11_SAMPLER_DESC 구조체
  * 기본값: 없음

* ID3D11SamplerState** ppSamplerState
  * 생성된 샘플러 상태 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 샘플러 상태 객체를 반환하지 않는다.
    * ID3D11SamplerState: 생성된 샘플러 상태 객체를 반환한다.
  * 기본값: 없음

## CreateBuffer 함수
CreateBuffer 함수는 ID3D11Device 인터페이스의 멤버 함수로, 버퍼를 생성하는 함수다. 

버퍼는 정점 데이터, 인덱스 데이터, 상수 데이터 등을 저장하는데 사용된다.

시그니처는 다음과 같다.
```cpp
HRESULT ID3D11Device::CreateBuffer(
    const D3D11_BUFFER_DESC *pDesc,
    const D3D11_SUBRESOURCE_DATA *pInitialData,
    ID3D11Buffer **ppBuffer
);
```
매개변수는 다음과 같다.

* const D3D11_BUFFER_DESC* pDesc
  * 버퍼의 속성을 정의하는 구조체에 대한 포인터
  * 사용 가능한 값
    * 유효한 D3D11_BUFFER_DESC 구조체
  * 기본값: 없음

* const D3D11_SUBRESOURCE_DATA* pInitialData
  * 버퍼에 초기 데이터를 채우기 위한 구조체에 대한 포인터
  * 사용 가능한 값
    * NULL: 초기 데이터를 설정하지 않는다.
    * 유효한 D3D11_SUBRESOURCE_DATA 구조체
  * 기본값: NULL

* ID3D11Buffer** ppBuffer
  * 생성된 버퍼 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 버퍼 객체를 반환하지 않는다.
    * ID3D11Buffer: 생성된 버퍼 객체를 반환한다.
  * 기본값: 없음

## CreateBlendState 멤버함수
CreateBlendState 함수는 ID3D11Device 인터페이스의 멤버 함수로, 블렌드 상태 객체를 생성하는 함수다. 이 함수는 블렌딩(투명도 및 혼합) 동작을 정의하는데 사용된다.

시그니처는 다음과 같다.
```cpp
HRESULT ID3D11Device::CreateBlendState(
    const D3D11_BLEND_DESC *pBlendStateDesc,
    ID3D11BlendState **ppBlendState
);
```
매개변수는 다음과 같다.

* const D3D11_BLEND_DESC* pBlendStateDesc
  * 블렌드 상태를 정의하는 구조체에 대한 포인터
  * 사용 가능한 값
    * 유효한 D3D11_BLEND_DESC 구조체
  * 기본값: 없음

* ID3D11BlendState** ppBlendState
  * 생성된 블렌드 상태 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 블렌드 상태 객체를 반환하지 않음
    * 유효한 ID3D11BlendState 객체의 포인터
  * 기본값: 없음

## CreateUnorderedAccessView 멤버 함수
CreateUnorderedAccessView 함수는 ID3D11Device 인터페이스의 멤버 함수로, 언오더드 액세스 뷰를 생성하는 함수다. 

이 함수는 리소스에 대한 무순서 접근을 허용하는 뷰를 생성하며, 컴퓨트 셰이더 또는 픽셀 셰이더에서 사용될 수 있다.

시그니처는 다음과 같다.
```cpp
HRESULT ID3D11Device::CreateUnorderedAccessView(
    ID3D11Resource *pResource,
    const D3D11_UNORDERED_ACCESS_VIEW_DESC *pDesc,
    ID3D11UnorderedAccessView **ppUAView
);
```

매개변수는 다음과 같다.

* ID3D11Resource* pResource
  * 언오더드 액세스 뷰를 생성할 리소스
  * 사용 가능한 값
    * 유효한 ID3D11Resource 객체
  * 기본값: 없음

* const D3D11_UNORDERED_ACCESS_VIEW_DESC* pDesc
  * 언오더드 액세스 뷰의 속성을 지정하는 구조체에 대한 포인터
  * 사용 가능한 값
    * NULL: 리소스의 기본 뷰를 생성한다.
    * 유효한 D3D11_UNORDERED_ACCESS_VIEW_DESC 구조체
  * 기본값: NULL

* ID3D11UnorderedAccessView** ppUAView
  * 생성된 언오더드 액세스 뷰 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 언오더드 액세스 뷰 객체를 반환하지 않는다.
    * 유효한 ID3D11UnorderedAccessView 객체의 포인터
  * 기본값: 없음

### 리소스의 기본 뷰
CreateUnorderedAccessView 함수에서 pDesc 인자로 NULL이 주어지는 경우, Direct3D는 리소스의 기본 뷰를 생성한다. 

리소스의 기본 뷰가 설정되면, D3D11_UNORDERED_ACCESS_VIEW_DESC 구조체의 각 필드는 다음과 같은 값으로 설정된다.

* Format
  * 리소스의 기본 형식이 사용된다.
  * 예를 들어, 텍스처 리소스의 경우 텍스처의 형식이 사용된다. 버퍼 리소스의 경우, DXGI_FORMAT_R32_TYPELESS가 사용된다.

* ViewDimension
  * 리소스의 유형에 따라 뷰 차원이 자동으로 설정된다.
  * 예를 들어, 텍스처 2D 리소스의 경우 D3D11_UAV_DIMENSION_TEXTURE2D가 설정된다. 버퍼 리소스의 경우 D3D11_UAV_DIMENSION_BUFFER가 설정된다.

* union
  * union의 구체적인 필드는 리소스 유형에 따라 자동으로 설정된다.
  * 버퍼 리소스의 경우, D3D11_BUFFER_UAV 구조체가 설정된다.
  * 텍스처 리소스의 경우, D3D11_TEX*UAV 구조체가 설정된다.

#### 버퍼 리소스 (Buffer Resource)

* Format: DXGI_FORMAT_R32_TYPELESS
* ViewDimension: D3D11_UAV_DIMENSION_BUFFER
* D3D11_BUFFER_UAV 구조체 필드:
  * FirstElement: 0
  * NumElements: 버퍼의 요소 수
  * Flags: 0

#### 텍스처 1D 리소스 (Texture1D Resource)

* Format: 텍스처의 기본 형식
* ViewDimension: D3D11_UAV_DIMENSION_TEXTURE1D
* D3D11_TEX1D_UAV 구조체 필드:
  * MipSlice: 0

#### 텍스처 1D 배열 리소스 (Texture1D Array Resource)

* Format: 텍스처의 기본 형식
* ViewDimension: D3D11_UAV_DIMENSION_TEXTURE1DARRAY
* D3D11_TEX1D_ARRAY_UAV 구조체 필드:
  * MipSlice: 0
  * FirstArraySlice: 0
  * ArraySize: 텍스처 배열의 크기

#### 텍스처 2D 리소스 (Texture2D Resource)

* Format: 텍스처의 기본 형식
* ViewDimension: D3D11_UAV_DIMENSION_TEXTURE2D
* D3D11_TEX2D_UAV 구조체 필드:
  * MipSlice: 0

#### 텍스처 2D 배열 리소스 (Texture2D Array Resource)

* Format: 텍스처의 기본 형식
* ViewDimension: D3D11_UAV_DIMENSION_TEXTURE2DARRAY
* D3D11_TEX2D_ARRAY_UAV 구조체 필드:
  * MipSlice: 0
  * FirstArraySlice: 0
  * ArraySize: 텍스처 배열의 크기

#### 텍스처 3D 리소스 (Texture3D Resource)

* Format: 텍스처의 기본 형식
* ViewDimension: D3D11_UAV_DIMENSION_TEXTURE3D
* D3D11_TEX3D_UAV 구조체 필드:
  * MipSlice: 0
  * FirstWSlice: 0
  * WSize: 텍스처의 깊이

### Buffer와 리소스의 기본 뷰
pResource로 Structured가 아닌 D3D11_BUFFER가 주어지고 CreateUnorderedAccessView의 pDesc 인자로 nullptr을 전달하면 다음과 같은 오류가 발생한다.

```
A View of a non-Structured Buffer cannot be created using a NULL Desc. Default Desc parameters cannot be used, as a Format must be supplied
```

참고로, pResource로 D3D11_TEXTURE2D인 경우에는 아무런 오류가 발생하지 않는다.

오류를 해결하기 위해서는 D3D11_UNORDERED_ACCESS_VIEW_DESC 객체 생성해서 pDesc의 인자로 전달해줘야 한다.