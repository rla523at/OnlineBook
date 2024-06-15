# DirectX

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


### 참고
Direct3D 11은 마이크로소프트의 DirectX API의 한 부분으로, 주로 게임 및 멀티미디어 응용 프로그램에서 사용된다. 

## ID3D11DeviceContext
ID3D11DeviceContext는 Direct3D 11 API의 인터페이스 중 하나로, COM(Component Object Model) 기반의 객체다.

ID3D11DeviceContext는 Direct3D 11의 핵심 인터페이스 중 하나로, 주로 렌더링 파이프라인을 관리하고 GPU에 명령을 전달하는 역할을 담당한다. 이 인터페이스는 그래픽스 및 컴퓨팅 작업을 수행하기 위한 다양한 메서드를 제공하며, 주로 드로우 호출, 리소스 관리, 파이프라인 상태 설정 등을 처리한다.

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

정의는 다음과 같다.

```cpp
typedef struct DXGI_SWAP_CHAIN_DESC {
    DXGI_MODE_DESC BufferDesc;
    DXGI_SAMPLE_DESC SampleDesc;
    DXGI_USAGE BufferUsage;
    UINT BufferCount;
    HWND OutputWindow;
    BOOL Windowed;
    DXGI_SWAP_EFFECT SwapEffect;
    UINT Flags;
} DXGI_SWAP_CHAIN_DESC;
```

각각의 멤버변수는 다음과 같다.

#### DXGI_MODE_DESC BufferDesc
DXGI_MODE_DESC 구조체는 디스플레이 모드를 설명하는 데 사용되는 구조체로, 해상도, 색상 형식, 리프레시율 등과 같은 디스플레이 속성을 정의한다. 

정의는 다음과 같다.

```cpp
typedef struct DXGI_MODE_DESC {
    UINT Width;
    UINT Height;
    DXGI_RATIONAL RefreshRate;
    DXGI_FORMAT Format;
    DXGI_MODE_SCANLINE_ORDER ScanlineOrdering;
    DXGI_MODE_SCALING Scaling;
} DXGI_MODE_DESC;
```

각각의 멤버 변수는 다음과 같다.
* UINT Width
  * 화면의 너비(픽셀 단위)
* UINT Height
  * 화면의 높이(픽셀 단위)
* DXGI_RATIONAL RefreshRate
  * 화면의 리프레시율(초당 화면 재생 빈도)
  * DXGI_RATIONAL 구조체는 분자와 분모로 구성된다.
    * RefreshRate.Numerator
    * RefreshRate.Denominator
* DXGI_FORMAT Format
  * 버퍼의 색상 형식
  * 일반적으로 사용되는 값은 DXGI_FORMAT_R8G8B8A8_UNORM (32비트 RGBA)이다
* DXGI_MODE_SCANLINE_ORDER ScanlineOrdering
  * 화면의 스캔라인 순서
  * 일반적으로 DXGI_MODE_SCANLINE_ORDER_UNSPECIFIED(스캔라인 순서가 지정되지 않음)을 사용한다.
* DXGI_MODE_SCALING Scaling;
  * 화면의 스케일링 방식
  * 일반적으로 DXGI_MODE_SCALING_UNSPECIFIED(스케일링 방식이 지정되지 않음)을 사용한다.

#### DXGI_SAMPLE_DESC SampleDesc
DXGI_SAMPLE_DESC 구조체는 Direct3D 11에서 멀티샘플링(anti-aliasing) 설정을 지정하는 데 사용된다. 

정의는 다음과 같다.
```cpp
typedef struct DXGI_SAMPLE_DESC {
    UINT Count;
    UINT Quality;
} DXGI_SAMPLE_DESC;
```

각각의 멤버 변수는 다음과 같다.
* UINT Count
  * 멀티샘플링의 샘플 수를 지정한다.
  * 값이 1이면 멀티샘플링이 비활성화되며, 값이 2, 4, 8 등의 값이면 해당 샘플 수로 멀티샘플링이 활성화된다.
  * 예를 들어 4x MSAA를 사용하려면 Count 값을 4로 설정한다.
* UINT Quality
  * 멀티샘플링의 품질 수준을 지정한다.
  * 품질 수준은 특정 샘플 수에 대해 지원되는 품질의 수를 나타낸다. 
  * 일반적으로 Quality 값은 0부터 시작하며, 지원되는 최대 품질 수준은 CheckMultisampleQualityLevels 함수에 의해 결정된다.
    * 품질 수준이 0이면 기본 품질 수준을 사용한다.
    * CheckMultisampleQualityLevels 함수에서 numQualityLevels를 얻었을 떄, 품질 수준이 numQualityLevels - 1이면 최고 품질 수준을 사용한다.


### ID3D11Device** ppDevice
생성된 디바이스 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.

### D3D_FEATURE_LEVEL* pFeatureLevel
생성된 디바이스의 기능 수준을 받을 포인터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 이 값을 무시한다.

### ID3D11DeviceContext** ppImmediateContext
생성된 디바이스 컨텍스트 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.