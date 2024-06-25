# Direct3D 11 API

## D3D11CreateDevice
D3D11CreateDevice 함수는 Direct3D 11 디바이스와 해당 디바이스의 즉시 컨텍스트를 생성하는 함수다. 이 함수는 Direct3D 애플리케이션의 기본 구성 요소로, 그래픽 하드웨어와 상호 작용할 수 있게 한다.

시그니처는 다음과 같다.
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
    ID3D11DeviceContext **ppImmediateContext
);
```

매개변수는 다음과 같다.

* IDXGIAdapter* pAdapter
  * 디바이스가 연결될 어댑터를 지정하는 포인터
  * 사용 가능한 값
    * NULL: 기본 어댑터를 사용한다.
    * IDXGIAdapter: 특정 어댑터를 지정한다.
  * 기본값: NULL

* D3D_DRIVER_TYPE DriverType
  * 디바이스 드라이버의 유형을 지정하는 열거형
  * 사용 가능한 값
    * D3D_DRIVER_TYPE_HARDWARE: 하드웨어 가속을 사용한다.
    * D3D_DRIVER_TYPE_REFERENCE: 소프트웨어 래스터라이저를 사용한다.
    * D3D_DRIVER_TYPE_NULL: 디바이스 없이 실행된다.
    * D3D_DRIVER_TYPE_SOFTWARE: 사용자가 제공하는 소프트웨어 드라이버를 사용한다.
    * D3D_DRIVER_TYPE_WARP: Microsoft의 고성능 소프트웨어 래스터라이저를 사용한다.
  * 기본값: 없음

* HMODULE Software
  * 사용자 지정 소프트웨어 래스터라이저 모듈의 핸들
  * 사용 가능한 값
    * NULL: 소프트웨어 드라이버를 사용하지 않는다.
    * HMODULE: 특정 소프트웨어 드라이버 모듈을 지정한다.
  * 기본값: NULL

* UINT Flags
  * 디바이스 생성에 대한 추가 옵션을 지정하는 플래그
  * D3D11_CREATE_DEVICE_FLAG enum 값을 사용한다.
  * 사용 가능한 값
    * 0: 기본 설정을 사용한다.
    * D3D11_CREATE_DEVICE_DEBUG: 디버그 레이어를 활성화한다.
    * D3D11_CREATE_DEVICE_SINGLETHREADED: 단일 스레드 모드를 사용한다.
  * 기본값: 0

* const D3D_FEATURE_LEVEL* pFeatureLevels
  * 지원하려는 기능 수준의 배열에 대한 포인터
  * 사용 가능한 값
    * D3D_FEATURE_LEVEL_11_0: Direct3D 11.0 기능 수준
    * D3D_FEATURE_LEVEL_10_1: Direct3D 10.1 기능 수준
    * D3D_FEATURE_LEVEL_10_0: Direct3D 10.0 기능 수준
    * D3D_FEATURE_LEVEL_9_3: Direct3D 9.3 기능 수준
    * D3D_FEATURE_LEVEL_9_2: Direct3D 9.2 기능 수준
    * D3D_FEATURE_LEVEL_9_1: Direct3D 9.1 기능 수준
  * 기본값: NULL (Direct3D 11.0을 기본으로 설정)

* UINT FeatureLevels
  * pFeatureLevels 배열의 크기
  * 사용 가능한 값
    * 0: pFeatureLevels가 NULL일 때 사용한다.
    * 1 이상의 정수: pFeatureLevels 배열의 요소 수
  * 기본값: 0

* UINT SDKVersion
  * Direct3D SDK의 버전을 지정
  * 사용 가능한 값
    * D3D11_SDK_VERSION: 현재 SDK 버전을 사용한다.
  * 기본값: D3D11_SDK_VERSION

* ID3D11Device** ppDevice
  * 생성된 디바이스 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 디바이스 객체를 반환하지 않는다.
    * ID3D11Device: 생성된 디바이스 객체를 반환한다.
  * 기본값: 없음

* ID3D11DeviceContext** ppImmediateContext
  * 생성된 즉시 컨텍스트 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 즉시 컨텍스트 객체를 반환하지 않는다.
    * ID3D11DeviceContext: 생성된 즉시 컨텍스트 객체를 반환한다.
  * 기본값: 없음

## D3D11_CREATE_DEVICE_FLAG enum
D3D11_CREATE_DEVICE_FLAG enum은 Direct3D 11 디바이스를 생성할 때 사용할 수 있는 플래그를 정의하는 열거형이다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D11_CREATE_DEVICE_FLAG
    {
        D3D11_CREATE_DEVICE_SINGLETHREADED          = 0x1,
        D3D11_CREATE_DEVICE_DEBUG                   = 0x2,
        D3D11_CREATE_DEVICE_SWITCH_TO_REF           = 0x4,
        D3D11_CREATE_DEVICE_PREVENT_INTERNAL_THREADING_OPTIMIZATIONS = 0x8,
        D3D11_CREATE_DEVICE_BGRA_SUPPORT            = 0x20,
        D3D11_CREATE_DEVICE_DEBUGGABLE              = 0x40,
        D3D11_CREATE_DEVICE_PREVENT_ALTERING_LAYER_SETTINGS_FROM_REGISTRY = 0x80,
        D3D11_CREATE_DEVICE_DISABLE_GPU_TIMEOUT     = 0x100,
        D3D11_CREATE_DEVICE_VIDEO_SUPPORT           = 0x800
    } 	D3D11_CREATE_DEVICE_FLAG;
```

각 enum의 의미는 다음과 같다.

* D3D11_CREATE_DEVICE_SINGLETHREADED
  * 디바이스가 단일 스레드에서만 호출됨을 나타낸다.
* D3D11_CREATE_DEVICE_DEBUG
  * 디버그 디바이스를 생성하여 디버깅 정보를 제공한다.
* D3D11_CREATE_DEVICE_SWITCH_TO_REF
  * 하드웨어 디바이스를 사용할 수 없을 때 참조 래스터라이저로 전환한다.
* D3D11_CREATE_DEVICE_PREVENT_INTERNAL_THREADING_OPTIMIZATIONS
  * 내부 스레딩 최적화를 방지한다.
* D3D11_CREATE_DEVICE_BGRA_SUPPORT
  * BGRA 포맷을 지원한다.
* D3D11_CREATE_DEVICE_DEBUGGABLE
  * 디바이스가 디버깅 가능한 상태로 생성된다.
* D3D11_CREATE_DEVICE_PREVENT_ALTERING_LAYER_SETTINGS_FROM_REGISTRY
  * 레지스트리에서 레이어 설정 변경을 방지한다.
* D3D11_CREATE_DEVICE_DISABLE_GPU_TIMEOUT
  * GPU 타임아웃을 비활성화한다.
* D3D11_CREATE_DEVICE_VIDEO_SUPPORT
  * 비디오 응용 프로그램에 필요한 기능을 지원한다.

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