# Direc3D 11 API

## ID3D11Device
ID3D11Device는 Direct3D 11 API의 인터페이스 중 하나로, COM(Component Object Model) 기반의 객체다.

ID3D11Device 인터페이스는 Direct3D 11 응용 프로그램의 핵심 컴포넌트 중 하나로, GPU(Device)를 관리하고 리소스를 생성하는 역할을 담당하며 그래픽스를 처리하는 데 필요한 다양한 기능과 메서드를 제공한다.

### CheckMultisampleQualityLevels
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

#### 멀티 샘플링 안티앨리어싱(MSAA) vs 슈퍼 샘플링 안티앨리어싱(SSAA)
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


### CreateRenderTargetView
CreateRenderTargetView는 렌더 타겟 뷰를 생성하는 메서드다. 이 메서드는 ID3D11RenderTargetView 객체를 생성하여, 렌더링 파이프라인의 출력 병합 단계(OM, Output Merger)에 사용할 수 있게 한다.

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

#### D3D11_RENDER_TARGET_VIEW_DESC 구조체
이 구조체는 렌더 타겟 뷰의 속성을 정의한다.

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
  * DXGI_FORMAT 열거형 값 중 하나를 사용한다.

* ViewDimension
  * D3D11_RTV_DIMENSION 열거형 값을 사용하여 뷰의 차원을 지정한다.
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

### CreateRasterizerState
CreateRasterizerState 메서드는 Direct3D 11에서 ID3D11RasterizerState 객체를 생성하는 데 사용된다. ID3D11RasterizerState는 폴리곤이 화면에 렌더링되는 방식을 제어하며, 이 상태 객체를 통해 다양한 렌더링 설정을 적용할 수 있다. 이 메서드는 D3D11_RASTERIZER_DESC 구조체를 입력으로 받아, 해당 구조체의 설정에 따라 ID3D11RasterizerState 객체를 생성한다.

메서드의 시그니쳐는 다음과 같다.
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
  * 생성된 래스터라이저 상태 객체의 주소를 받을 포인터에 대한 이중 포인터다.
  * 성공적으로 생성되면, 이 포인터는 ID3D11RasterizerState 인터페이스를 가리키게 된다.

#### D3D11_RASTERIZER_DESC
D3D11_RASTERIZER_DESC 구조체는 Direct3D 11에서 래스터라이저 상태를 정의하는 데 사용된다. 래스터라이저 상태는 폴리곤이 화면에 렌더링되는 방식을 제어하며, 컬링, 채움 모드, 깊이 바이어스 등의 다양한 설정을 포함한다.

구조체의 정의는 다음과 같다
```cpp
typedef struct D3D11_RASTERIZER_DESC {
    D3D11_FILL_MODE FillMode;
    D3D11_CULL_MODE CullMode;
    BOOL FrontCounterClockwise;
    INT DepthBias;
    FLOAT DepthBiasClamp;
    FLOAT SlopeScaledDepthBias;
    BOOL DepthClipEnable;
    BOOL ScissorEnable;
    BOOL MultisampleEnable;
    BOOL AntialiasedLineEnable;
} D3D11_RASTERIZER_DESC;
```

각 멤버변수는 다음과 같다.
* FillMode
  * D3D11_FILL_MODE 타입으로, 폴리곤의 내부를 채우는 방법을 지정한다.
  * 가능한 값:
    * D3D11_FILL_WIREFRAME: 폴리곤의 경계를 선으로 렌더링한다.
    * D3D11_FILL_SOLID: 폴리곤의 내부를 채워서 렌더링한다.

* CullMode
  * D3D11_CULL_MODE 타입으로, 렌더링하지 않을 폴리곤의 면을 지정한다.
  * 가능한 값:
    * D3D11_CULL_NONE: 모든 면을 렌더링한다.
    * D3D11_CULL_FRONT: 프론트 페이스를 렌더링하지 않는다.
    * D3D11_CULL_BACK: 백 페이스를 렌더링하지 않는다.

* FrontCounterClockwise
  * BOOL 타입으로, 프론트 페이스를 시계 반대 방향으로 정의할지 여부를 지정한다.
  * TRUE: 시계 반대 방향을 프론트 페이스로 간주한다.
  * FALSE: 시계 방향을 프론트 페이스로 간주한다.

* DepthBias
  * INT 타입으로, 깊이 값에 추가할 정적 깊이 바이어스를 지정한다.
  * 폴리곤의 깊이 값을 조절하여 Z-파이팅을 방지한다.

* DepthBiasClamp
  * FLOAT 타입으로, 깊이 바이어스에 대한 최대 절대값을 지정한다.
  * 바이어스가 이 값을 초과하지 않도록 클램핑한다.

* SlopeScaledDepthBias
  * FLOAT 타입으로, 폴리곤의 기울기에 따라 깊이 바이어스를 조정하는 스케일 값을 지정한다.
  * 기울기가 큰 폴리곤에서 깊이 바이어스를 증가시켜 Z-파이팅을 방지한다.

* DepthClipEnable
  * BOOL 타입으로, 깊이 클리핑을 활성화할지 여부를 지정한다.
  * TRUE: 깊이 클리핑을 활성화한다. (Z-값이 클립 공간 범위를 벗어나는 폴리곤은 잘린다)
  * FALSE: 깊이 클리핑을 비활성화한다.

* ScissorEnable
  * BOOL 타입으로, 가위 테스트를 활성화할지 여부를 지정한다.
  * TRUE: 가위 테스트를 활성화한다.
  * FALSE: 가위 테스트를 비활성화한다.

* MultisampleEnable
  * BOOL 타입으로, 멀티샘플링 안티앨리어싱(MSAA)을 활성화할지 여부를 지정한다.
  * TRUE: 멀티샘플링을 활성화한다.
  * FALSE: 멀티샘플링을 비활성화한다.

* AntialiasedLineEnable
  * BOOL 타입으로, 선에 대한 안티앨리어싱을 활성화할지 여부를 지정한다.
  * TRUE: 안티앨리어싱을 활성화한다.
  * FALSE: 안티앨리어싱을 비활성화한다.

## ID3D11Texture2D
ID3D11Texture2D는 Direct3D 11 API에서 2D 텍스처를 나타내는 인터페이스다. 텍스처는 이미지 데이터를 포함하는 리소스로, 3D 모델에 매핑하여 렌더링할 때 사용된다. 2D 텍스처는 게임 및 그래픽 애플리케이션에서 매우 흔하게 사용되며, 다양한 그래픽 효과를 구현하는 데 필수적이다.

ID3D11Texture2D 인터페이스는 ID3D11Resource 인터페이스를 상속받아, Direct3D 11 리소스와 관련된 모든 기능을 제공한다.


## ID3D11RenderTargetView
ID3D11RenderTargetView는 Direct3D 11 API에서 렌더링 타겟을 나타내는 객체다. 이 객체는 주로 렌더링 작업의 결과를 저장할 버퍼를 정의하는 데 사용된다.

### Render Target
`렌더링 타겟(Render Target)`은 그래픽스 프로그래밍에서 렌더링의 결과를 출력하는 `표면(surface)`을 말한다. 기본적으로, 렌더링 타겟은 `프레임 버퍼(Frame Buffer)`라고도 불리며, 화면에 보여질 이미지를 저장하는 메모리 공간이다.

#### 렌더링 타겟의 역할
* 화면 출력
  * 렌더링 타겟은 최종적으로 화면에 출력될 이미지를 저장한다. 
  * 이는 디스플레이 장치로 전송되어 사용자가 볼 수 있는 형태로 나타난다.
* 임시 저장소
  * 렌더링 파이프라인의 중간 결과를 저장하기 위해 사용된다. 
  * 복잡한 효과나 다단계 렌더링에서 중간 결과를 저장해 다음 렌더링 단계에서 사용한다.
* 포스트 프로세싱
  * 화면에 출력되기 전에 여러 가지 후처리 효과(예: 블러, HDR)를 적용하기 위해 사용된다. 
  * 렌더링 타겟에 먼저 렌더링한 후, 그 결과를 사용해 후처리 효과를 적용한다.

#### 렌더링 타겟의 종류
* 백 버퍼(Back Buffer)
  * 일반적으로 화면에 출력될 이미지를 저장하는 기본 렌더링 타겟이다.
  * 프론트 버퍼와 교체되어 화면에 최종적으로 출력된다.
* 텍스처 렌더링 타겟 
  * 특정 텍스처에 렌더링 결과를 저장할 수 있다.
  * 이는 쉐이더에서 사용되거나 후처리 효과를 적용하는 데 활용된다.
* 멀티 렌더 타겟(MRT, Multiple Render Targets) 
  * 동시에 여러 렌더 타겟에 렌더링 결과를 출력할 수 있다.
  * 이는 복잡한 셰이더 효과나 다양한 데이터 출력을 필요로 할 때 유용하다.

## ID3D11DeviceContext
ID3D11DeviceContext는 Direct3D 11 API의 인터페이스 중 하나로, COM(Component Object Model) 기반의 객체다.

ID3D11DeviceContext는 Direct3D 11의 핵심 인터페이스 중 하나로, 주로 렌더링 파이프라인을 관리하고 GPU에 명령을 전달하는 역할을 담당한다. 이 인터페이스는 그래픽스 및 컴퓨팅 작업을 수행하기 위한 다양한 메서드를 제공하며, 주로 드로우 호출, 리소스 관리, 파이프라인 상태 설정 등을 처리한다.

### RSSetViewports
RSSetViewports는 Direct3D 11에서 뷰포트를 설정하는 데 사용된다.

메서드의 시그니쳐는 다음과 같다.
```cpp
void RSSetViewports(
  UINT NumViewports,
  const D3D11_VIEWPORT *pViewports
);
```

매개변수는 다음과 같다.
* NumViewports
  * 설정할 뷰포트의 개수를 지정한다. 
  * 이 값은 0에서 D3D11_VIEWPORT_AND_SCISSORRECT_OBJECT_COUNT_PER_PIPELINE (16) 사이의 값을 가질 수 있다.
  * 최대 16개의 뷰포트를 동시에 설정할 수 있다.
* pViewports
  * 뷰포트 배열에 대한 포인터다. 
  * 각 뷰포트는 D3D11_VIEWPORT 구조체로 정의된다.
  * 이 배열의 길이는 NumViewports와 같아야 한다.

## D3D11_VIEWPORT
D3D11_VIEWPORT는 Direct3D 11에서 `뷰포트(Viewport)`를 정의하는 구조체다. 뷰포트는 렌더링된 이미지가 화면에 그려지는 영역을 지정하며, 3D 장면을 2D 화면 공간으로 변환할 때 사용된다. 뷰포트는 렌더링 파이프라인의 래스터라이저(Rasterizer) 단계에서 중요한 역할을 한다.

구조체의 정의는 다음과 같다.
```cpp
typedef struct D3D11_VIEWPORT {
    FLOAT TopLeftX;
    FLOAT TopLeftY;
    FLOAT Width;
    FLOAT Height;
    FLOAT MinDepth;
    FLOAT MaxDepth;
} D3D11_VIEWPORT;
```

멤버변수는 다음과 같다.
* TopLeftX
  * 뷰포트의 왼쪽 상단 코너의 X 좌표를 정의한다. (픽셀 단위)
* TopLeftY
  * 뷰포트의 왼쪽 상단 코너의 Y 좌표를 정의한다. (픽셀 단위) 
* Width
  * 뷰포트의 너비를 정의한다. (픽셀 단위)  
* Height
  * 뷰포트의 높이를 정의한다. (픽셀 단위)  
* MinDepth
  * 뷰포트의 최소 깊이 값을 정의한다. 일반적으로 0.0f로 설정된다.  
* MaxDepth
  * 뷰포트의 최대 깊이 값을 정의한다. 일반적으로 1.0f로 설정된다.

### Viewport
뷰포트는 렌더링된 이미지가 화면에 그려지는 영역이다.

## D3D11_RASTERIZER_DESC


## D3D11CreateDevice
D3D11CreateDevice 함수는 Direct3D 11 디바이스와 디바이스 컨텍스트를 생성하는 함수다. 

이 함수의 시그니쳐는 다음과 같다.
```cpp
HRESULT D3D11CreateDevice(
    IDXGIAdapter *pAdapter,
    D3D_DRIVER_TYPE DriverType,
    HMODULE Software,
    UINT Flags,
    const D3D_FEATURE_LEVEL *pFeatureLevels,
    UINT FeatureLevels,
    UINT SDKVersion,
    ID3D11Device **ppDevice,
    D3D_FEATURE_LEVEL *pFeatureLevel,
    ID3D11DeviceContext **ppImmediateContext
);
```

각각의 매개변수는 다음과 같다.

### IDXGIAdapter* pAdapter
사용할 DXGI 어댑터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 기본 어댑터가 사용되고 특정 GPU를 선택하려면 IDXGIAdapter 객체를 전달해야 한다.

### D3D_DRIVER_TYPE DriverType
디바이스의 드라이버 유형을 지정하기 위한 매개변수이다. 

사용 가능한 드라이버 유형은 다음과 같다:
* D3D_DRIVER_TYPE_HARDWARE
  * 하드웨어 가속을 사용하는 디바이스.
* D3D_DRIVER_TYPE_REFERENCE
  * 소프트웨어로 구현된 디바이스. 디버깅 및 테스트 목적으로 사용된다.
* D3D_DRIVER_TYPE_NULL
  * 출력이 없는 디바이스. 주로 성능 테스트에 사용된다.
* D3D_DRIVER_TYPE_SOFTWARE
  * 사용자 지정 소프트웨어 렌더러를 사용하는 디바이스.
* D3D_DRIVER_TYPE_WARP
  * WARP(WIndows Advanced Rasterization Platform) 소프트웨어 렌더러를 사용하는 디바이스.

### HMODULE Software
소프트웨어 드라이버의 경우 사용되는 DLL의 핸들을 지정하기 위한 매개변수이다.

DriverType이 D3D_DRIVER_TYPE_SOFTWARE인 경우 유효한 핸들이 필요하지만 그렇지 않으면 nullptr로 설정한다.

### UINT Flags
디바이스 생성 시 사용할 플래그를 지정하기 위한 매개변수이다. 

사용 가능한 주요 플래그는 다음과 같다:
* D3D11_CREATE_DEVICE_DEBUG
  * 디버그 디바이스를 생성하여 디버깅 정보를 활성화한다.
* D3D11_CREATE_DEVICE_SINGLETHREADED
  * 디바이스가 단일 스레드에서만 사용됨을 나타낸다.

만약 이 값에 0을 주는 경우 Direct3D 11 디바이스가 아무런 추가 디버그 기능이나 특수 모드를 사용하지 않고 기본 설정으로 생성된다.

### const D3D_FEATURE_LEVEL* pFeatureLevels
지원할 기능 수준을 나열한 배열을 지정하기 위한 매개변수이다. 

이 배열의 첫 번째 요소부터 적용이 시작됨으로 선호하는 기능 수준 순서로 배열을 만들어야 한다.

### UINT FeatureLevels
기능 수준 배열의 크기를 지정하기 위한 매개변수이다.

일반적으로 ARRAYSIZE(featureLevels)를 사용한다.

### UINT SDKVersion
SDK 버전을 지정하기 위한 매개변수이다. 

일반적으로 D3D11_SDK_VERSION을 사용한다.

### ID3D11Device** ppDevice
생성된 디바이스 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.

### D3D_FEATURE_LEVEL* pFeatureLevel
생성된 디바이스의 기능 수준을 받을 포인터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 이 값을 무시한다.

### ID3D11DeviceContext** ppImmediateContext
생성된 디바이스 컨텍스트 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.

## D3D11CreateDeviceAndSwapChain
D3D11CreateDeviceAndSwapChain 함수는 Direct3D 11 디바이스와 디바이스 컨텍스트 그리고 스왑 체인을 동시에 생성하는 함수다.

이 함수의 시그니처는 다음과 같다.

```cpp
HRESULT D3D11CreateDeviceAndSwapChain(
    IDXGIAdapter* pAdapter,
    D3D_DRIVER_TYPE DriverType,
    HMODULE Software,
    UINT Flags,
    const D3D_FEATURE_LEVEL* pFeatureLevels,
    UINT FeatureLevels,
    UINT SDKVersion,
    const DXGI_SWAP_CHAIN_DESC* pSwapChainDesc,
    IDXGISwapChain** ppSwapChain,
    ID3D11Device** ppDevice,
    D3D_FEATURE_LEVEL* pFeatureLevel,
    ID3D11DeviceContext** ppImmediateContext
);
```
각각의 매개변수는 다음과 같다.

### IDXGIAdapter* pAdapter
사용할 DXGI 어댑터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 기본 어댑터가 사용되고 특정 GPU를 선택하려면 IDXGIAdapter 객체를 전달해야 한다.

### D3D_DRIVER_TYPE DriverType
디바이스의 드라이버 유형을 지정하기 위한 매개변수이다. 

사용 가능한 드라이버 유형은 다음과 같다:
* D3D_DRIVER_TYPE_HARDWARE
  * 하드웨어 가속을 사용하는 디바이스.
* D3D_DRIVER_TYPE_REFERENCE
  * 소프트웨어로 구현된 디바이스. 디버깅 및 테스트 목적으로 사용된다.
* D3D_DRIVER_TYPE_NULL
  * 출력이 없는 디바이스. 주로 성능 테스트에 사용된다.
* D3D_DRIVER_TYPE_SOFTWARE
  * 사용자 지정 소프트웨어 렌더러를 사용하는 디바이스.
* D3D_DRIVER_TYPE_WARP
  * WARP(WIndows Advanced Rasterization Platform) 소프트웨어 렌더러를 사용하는 디바이스.

### HMODULE Software
소프트웨어 드라이버의 경우 사용되는 DLL의 핸들을 지정하기 위한 매개변수이다.

DriverType이 D3D_DRIVER_TYPE_SOFTWARE인 경우 유효한 핸들이 필요하지만 그렇지 않으면 nullptr로 설정한다.

### UINT Flags
디바이스 생성 시 사용할 플래그를 지정하기 위한 매개변수이다. 

사용 가능한 주요 플래그는 다음과 같다:
* D3D11_CREATE_DEVICE_DEBUG
  * 디버그 디바이스를 생성하여 디버깅 정보를 활성화한다.
* D3D11_CREATE_DEVICE_SINGLETHREADED
  * 디바이스가 단일 스레드에서만 사용됨을 나타낸다.

만약 이 값에 0을 주는 경우 Direct3D 11 디바이스가 아무런 추가 디버그 기능이나 특수 모드를 사용하지 않고 기본 설정으로 생성된다.

### const D3D_FEATURE_LEVEL* pFeatureLevels
지원할 기능 수준을 나열한 배열을 지정하기 위한 매개변수이다. 

이 배열의 첫 번째 요소부터 적용이 시작됨으로 선호하는 기능 수준 순서로 배열을 만들어야 한다.

### UINT FeatureLevels
기능 수준 배열의 크기를 지정하기 위한 매개변수이다.

일반적으로 ARRAYSIZE(featureLevels)를 사용한다.

### UINT SDKVersion
SDK 버전을 지정하기 위한 매개변수이다. 

일반적으로 D3D11_SDK_VERSION을 사용한다.

### const DXGI_SWAP_CHAIN_DESC* pSwapChainDesc
스왑 체인을 생성하기 위해 필요한 DXGI_SWAP_CHAIN_DESC을 지정하기 위한 매개변수이다.

DXGI_SWAP_CHAIN_DESC 구조체는 DirectX Graphics Infrastructure (DXGI)에서 스왑 체인의 속성을 정의하며 스왑 체인 생성에 사용된다.

### ID3D11Device** ppDevice
생성된 디바이스 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.

### D3D_FEATURE_LEVEL* pFeatureLevel
생성된 디바이스의 기능 수준을 받을 포인터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 이 값을 무시한다.

### ID3D11DeviceContext** ppImmediateContext
생성된 디바이스 컨텍스트 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.