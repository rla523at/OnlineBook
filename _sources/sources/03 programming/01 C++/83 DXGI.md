# DirectX Graphics Infrastructure
`DirectX Graphics Infrastructure (DXGI)`는 DirectX API의 일부로, 그래픽 어댑터와 디스플레이를 관리하고 조작하는 데 필요한 기능을 제공하는 라이브러리다. DXGI는 Direct3D와 같은 그래픽 API와 함께 사용되어 그래픽 리소스, 디스플레이 모드, 스왑 체인 등의 관리를 단순화하는 역할을 한다.

## DXGI 역할
DXGI의 역할을 조금더 자세하게 살펴보자.

### 하드웨어 추상화
그래픽 어댑터와 디스플레이 장치 간의 추상화 계층을 제공하여 개발자가 하드웨어 세부 사항에 신경 쓰지 않고도 작업을 수행할 수 있게 한다. 이를 통해 다양한 하드웨어 환경에서 일관된 성능과 기능을 제공할 수 있다.

### 스왑체인 관리
프레임 버퍼를 관리하여 더블 버퍼링, 트리플 버퍼링 등의 기술을 지원하고, 스크린 티어링을 방지하는 데 도움을 준다. 스왑 체인은 두 개 이상의 프레임 버퍼를 사용하여 새로운 프레임을 준비하는 동안 기존 프레임을 디스플레이에 표시할 수 있게 한다.

### 디스플레이 모드 관리
디스플레이 해상도, 리프레시율 등의 설정을 관리하고 변경할 수 있게 한다. 이를 통해 다양한 디스플레이 환경에 맞게 동적으로 화면 설정을 조정할 수 있다

### 멀티 어댑터 지원
여러 개의 그래픽 어댑터를 지원하여, 다중 디스플레이 환경이나 고성능 컴퓨팅 환경에서 효율적인 자원 관리를 가능하게 한다. 이를 통해 복잡한 렌더링 작업을 여러 어댑터에 분산시켜 성능을 향상시킬 수 있다.

## DXGI 객체와 인터페이스
DXGI는 COM(Component Object Model) 기반의 객체인 인터페이스를 통해 다양한 기능을 제공한다. 주요 인터페이스는 다음과 같다:

### IDXGIDevice
Direct3D 디바이스와 DXGI 간의 상호작용을 관리하는 중요한 역할을 한다. 이 인터페이스는 Direct3D 디바이스가 DXGI 팩토리 및 어댑터와 통신할 수 있도록 하며, 이를 통해 디스플레이 설정 및 리소스 관리를 효율적으로 처리할 수 있다.

주요 역할 및 기능은 다음과 같다.

####  DXGI 어댑터와의 상호작용
IDXGIDevice는 Direct3D 디바이스와 DXGI 어댑터 간의 통신을 관리하는 역할을 한다. 

`GetAdapter` 메서드를 사용하여 IDXGIAdapter 인터페이스를 얻어 어댑터의 속성이나 모드를 쿼리하고 설정할 수 있게 한다. 이를 통해 디스플레이 설정을 조정하거나 성능을 최적화한다.

#### 리소스 공유
여러 디바이스 간의 리소스 공유를 가능하게 한다. 이는 복잡한 그래픽 애플리케이션에서 리소스를 효율적으로 관리할 수 있도록 돕는다. 리소스 공유는 멀티 GPU 설정이나 클러스터 컴퓨팅 환경에서 특히 중요하다.

IDXGIDevice를 사용하여 Direct3D 디바이스 간에 텍스처, 버퍼 등의 그래픽 리소스를 공유할 수 있다. 이를 통해 여러 디바이스가 동일한 리소스를 사용할 수 있게 되어 성능과 효율성을 높일 수 있다.

`QueryResourceResidency` 매서드를 사용하여 지정된 리소스가 로컬 비디오 메모리에 있는지, 공유 메모리에 있는지, 시스템 메모리에 있는지를 쿼리할 수 있다. 이를 통해 리소스의 위치와 접근 가능성을 확인할 수 있다.

#### GPU 스레드 우선 순위 관리
GPU 스레드의 우선 순위를 설정하고 쿼리할 수 있다. 이를 통해 GPU 작업의 우선 순위를 조정하여 특정 작업의 성능을 최적화할 수 있다.

`SetGPUThreadPriority` 매서드를 사용하여 GPU 스레드의 우선 순위를 설정할 수 있다. 이를 통해 중요한 작업의 우선 순위를 높이거나, 백그라운드 작업의 우선 순위를 낮출 수 있다.

`GetGPUThreadPriority` 매서드를 사용하여 현재 설정된 GPU 스레드의 우선 순위를 쿼리할 수 있다. 이를 통해 현재 시스템의 스레드 우선 순위 설정을 확인하고, 필요에 따라 조정할 수 있다.

### IDXGIFactory
DXGI 객체와 리소스를 생성하는 팩토리 인터페이스다. 스왑 체인, 어댑터, 디바이스 등을 생성할 수 있다. 이를 통해 그래픽 어댑터와 출력 장치를 관리하고, 스왑 체인을 생성할 수 있다.

IDXGIFactory는 CreateDXGIFactory 함수를 이용하여 생성할 수 있다.

```cpp
ComPtr<IDXGIFactory> dxgiFactory;
HRESULT hr = CreateDXGIFactory(__uuidof(IDXGIFactory), &dxgiFactory);
```

주요 역할은 다음과 같다.

#### DXGI 객체 생성
IDXGIFactory는 다양한 DXGI 객체를 생성하는 기능을 제공한다. 이를 통해 그래픽 어댑터, 스왑 체인, 출력 장치 등을 생성하고 관리할 수 있다.

주요 생성 메서드로는 CreateSwapChain, CreateSoftwareAdapter등이 있다.

#### 그래픽 어댑터 나열
시스템에 설치된 그래픽 어댑터를 나열하고, 각 어댑터의 속성을 쿼리할 수 있다. 이를 통해 애플리케이션은 사용 가능한 그래픽 하드웨어를 파악하고, 적절한 어댑터를 선택할 수 있다.

설치된 그래픽 어댑터는 `EnumAdapters` 메서드로 얻을 수 있으며, 인덱스를 통해 각 어댑터를 쿼리할 수 있다. `EnumAdapters`의 시그니쳐는 다음과 같다.
```cpp
HRESULT EnumAdapters(
    UINT Adapter,
    IDXGIAdapter **ppAdapter
);
```

#### 디스플레이 모드 관리
어댑터에 연결된 출력 장치(모니터 등)의 디스플레이 모드를 쿼리하고 설정할 수 있다. 이를 통해 다양한 해상도와 리프레시율 설정을 관리할 수 있다.

#### 스왑 체인 관리
스왑 체인을 생성하고 관리하는 기능을 제공한다. 스왑 체인은 화면에 렌더링된 프레임을 표시하는 데 사용된다.

스왑 체인은 CreateSwapChain 메서드로 생성하며 시그니처는 다음과 같다.
```cpp
HRESULT CreateSwapChain(
    IUnknown *pDevice,
    DXGI_SWAP_CHAIN_DESC *pDesc,
    IDXGISwapChain **ppSwapChain
);
```

여기서 스왑 체인을 정의할 때 사용하는 DXGI_SWAP_CHAIN_DESC 구조체는 스왑 체인의 속성을 정의하는 데 사용된다.
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

##### DXGI_MODE_DESC BufferDesc
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
  * 스왑 체인의 백 버퍼의 너비(픽셀 단위)
  * 최종 렌더링된 이미지의 너비이다
* UINT Height
  * 스왑 체인의 백 버퍼의 높이(픽셀 단위)
  * 최종 렌더링된 이미지의 높이
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

##### DXGI_SAMPLE_DESC SampleDesc
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

##### DXGI_USAGE BufferUsage
버퍼의 사용 용도를 지정하는 플래그로, 주로 렌더 타겟 또는 셰이더 리소스로 사용된다.

이 값으로 DXGI_USAGE_RENDER_TARGET_OUTPUT을 사용하면 렌더 타겟으로 사용됨을 지정하며 DXGI_USAGE_SHADER_INPUT을 사용하면 셰이더 입력으로 사용을 지정할 수 있다.

##### UINT BufferCount
스왑 체인의 버퍼 개수를 지정한다.

2로 설정하여 하나의 프론트 버퍼와 하나의 백 버퍼를 사용하는 더블 버퍼링을 지정할 수 있고 3으로 설정하여 하나의 프론트 버퍼와 두 개의 백 버퍼를 사용하는 트리플 버퍼링을 사용할 수 있다.

##### HWND OutputWindow
렌더링 결과를 출력할 윈도우 핸들이다.

##### BOOL Windowed
창 모드인지 전체 화면 모드인지를 지정한다.

TRUE이면 창 모드로 설정되고 FALSE이면 전체 화면 모드로 설정된다.

##### DXGI_SWAP_EFFECT SwapEffect
스왑 체인이 버퍼를 교환하는 방식을 지정한다.

가능한 값들은 다음과 같다.
* DXGI_SWAP_EFFECT_DISCARD
  * 가장 최근 프레임을 디스플레이에 전환하고 이전 프레임을 버린다.
* DXGI_SWAP_EFFECT_SEQUENTIAL
  * 버퍼를 순차적으로 교환한다.
* DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL
  * 플립 모델을 사용하여 버퍼를 순차적으로 교환한다.
* DXGI_SWAP_EFFECT_FLIP_DISCARD
  * 플립 모델을 사용하고 이전 내용을 버린다.

> REFERENCE  
> [MSDN - ne-dxgi-dxgi_swap_effect](https://learn.microsoft.com/ko-kr/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_effect)

##### UINT Flags
스왑 체인의 동작을 제어하는 플래그이다.

가능한 값들은 다음과 같다.
* DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH
  * 전체 화면 모드 전환을 허용한다.
* DXGI_SWAP_CHAIN_FLAG_GDI_COMPATIBLE



### IDXGIAdapter
그래픽 어댑터를 나타내는 인터페이스로, 어댑터의 속성이나 모드를 쿼리하고 설정할 수 있다. 이를 통해 애플리케이션은 그래픽 카드의 메모리, 성능, 지원하는 디스플레이 모드 등의 정보를 얻을 수 있다.

주요 역할은 다음과 같다.

#### 그래픽 어댑터 정보 제공
IDXGIAdapter는 시스템에 설치된 그래픽 어댑터의 정보를 제공한다. 이를 통해 애플리케이션은 사용 가능한 그래픽 하드웨어의 성능과 특성을 파악할 수 있다.

`GetDesc` 메서드를 이용하여 어댑터에 관한 정보를 담은 DXGI_ADAPTER_DESC 구조체를 얻을 수 있다.
```cpp
HRESULT GetDesc(DXGI_ADAPTER_DESC *pDesc);
```

DXGI_ADAPTER_DESC 구조체는 다음과 같이 정의되어 있다.
```cpp
typedef struct DXGI_ADAPTER_DESC {
    WCHAR Description[128];
    UINT VendorId;
    UINT DeviceId;
    UINT SubSysId;
    UINT Revision;
    SIZE_T DedicatedVideoMemory;
    SIZE_T DedicatedSystemMemory;
    SIZE_T SharedSystemMemory;
    LUID AdapterLuid;
} DXGI_ADAPTER_DESC;
```

각각의 멤버 변수는 다음과 같은 의미를 갖는다.

* Description
  * 어댑터의 이름을 나타낸다.
* VendorId
  * 벤더 ID를 나타낸다.
* DeviceId
  * 디바이스 ID를 나타낸다.
* SubSysId
  * 서브 시스템 ID를 나타낸다.
* Revision
  * 리비전 번호를 나타낸다.
* DedicatedVideoMemory
  * 전용 비디오 메모리 크기를 나타낸다.
* DedicatedSystemMemory
  * 전용 시스템 메모리 크기를 나타낸다.
* SharedSystemMemory
  * 공유 시스템 메모리 크기를 나타낸다.
* AdapterLuid
  * 어댑터의 로컬 고유 식별자(LUID)를 나타낸다.

#### 출력 관리
어댑터에 연결된 디스플레이 출력 장치(모니터 등)에 대한 정보를 제공하고, 이를 관리할 수 있다.

`EnumOutputs` 메서드를 이용하여 어댑터에 연결된 출력 장치를 나열하고 이를 통해 여러 출력 장치(모니터 등)를 관리할 수 있다.

```cpp
HRESULT EnumOutputs(UINT Output, IDXGIOutput **ppOutput);
```

### IDXGISwapChain
IDXGISwapChain는 프레임 버퍼를 관리하고 렌더링된 이미지를 화면에 표시하는 역할을 한다. IDXGISwapChain는 Direct3D 디바이스와 함께 사용되며, 주로 게임이나 그래픽 애플리케이션에서 화면에 그래픽을 출력하기 위해 사용된다.

주요 역할은 다음과 같다.

#### 프레임 버퍼 관리
스왑 체인은 하나 이상의 백 버퍼와 하나의 프론트 버퍼로 구성된다. 프론트 버퍼는 현재 화면에 표시되는 버퍼이고, 백 버퍼는 다음 프레임을 렌더링할 버퍼다. 렌더링이 완료되면 백 버퍼와 프론트 버퍼를 교환(swap)하여 화면에 새로운 프레임을 표시한다.

이 때, 하나의 프론트 버퍼와 하나의 백 버퍼를 사용하여 렌더링이 완료되면 두 버퍼를 교환하는 방식을 더블 버퍼링이라고 한다.
그리고 프론트 버퍼와 두 개의 백 버퍼를 사용하여 더 부드러운 애니메이션과 입력 지연을 줄이는 방식을 트리플 버퍼링이라고 한다.

이처럼, 스왑 체인은 두 개 이상의 프레임 버퍼를 관리하여, 하나의 프레임을 디스플레이에 표시하는 동안 다른 프레임을 준비할 수 있게 한다. 이를 통해 화면의 깜빡임을 줄이고 부드러운 애니메이션을 제공할 수 있다.

프레임 버퍼 관리와 관련해 주요 메서드들은 다음과 같다.

##### ResizeBuffers
스왑 체인의 버퍼 크기, 개수 또는 포맷을 변경할 수 있다.
```cpp
HRESULT ResizeBuffers(
    UINT BufferCount,
    UINT Width,
    UINT Height,
    DXGI_FORMAT NewFormat,
    UINT SwapChainFlags
);
```

이 때, 각 매개 변수는 다음과 같다.
* BufferCount
  * 새로 생성할 버퍼의 개수.
* Width
  * 버퍼의 새로운 너비.
* Height
  * 버퍼의 새로운 높이.
* NewFormat
  * 버퍼의 새로운 포맷.
* SwapChainFlags
  * 추가적인 스왑 체인 옵션.

##### ResizeTarget
스왑 체인의 출력 창 크기를 변경한다.
```cpp
HRESULT ResizeTarget(
    const DXGI_MODE_DESC *pNewTargetParameters
);
```

이 때, 각 매개 변수는 다음과 같다.
* pNewTargetParameters
  * 새로운 출력 창 크기를 지정하는 DXGI_MODE_DESC 구조체.

##### GetBuffer
스왑 체인의 백 버퍼에 대한 포인터를 얻는다.

```cpp
HRESULT GetBuffer(
    UINT Buffer,
    REFIID riid,
    void **ppSurface
);
```

이 떄, 각 매게 변수는 다음과 같다.
* Buffer
  * 가져올 버퍼의 인덱스.
* riid
  * 요청할 인터페이스의 식별자.
* ppSurface
  * 요청된 인터페이스 포인터를 받는 변수.

#### 화면갱신
`Present` 메서드를 통해 새로운 프레임을 화면에 표시할 수 있다. 이 메서드는 현재 백 버퍼에 있는 내용을 프론트 버퍼로 전환하여 화면에 표시한다.
```cpp
HRESULT Present(
    UINT SyncInterval,
    UINT Flags
);
```

이 때, 각 매개 변수는 다음과 같다.
* SyncInterval
  * 화면 갱신 빈도와 동기화할지 여부를 지정한다. 0은 가능한 빨리 전환하고, 1 이상은 수직 동기화 간격을 나타낸다.
* Flags
  * 추가적인 프레젠트 옵션을 지정한다.

#### 전체 화면 모드 전환
`SetFullscreenState` 메서드를 통해 전체 화면 모드와 창 모드 간의 전환을 관리할 수 있다. 이를 통해 게임이나 멀티미디어 애플리케이션에서 유연하게 화면 모드를 전환할 수 있다.
```cpp
HRESULT SetFullscreenState(
    BOOL Fullscreen,
    IDXGIOutput *pTarget
);
```

이 때, 각 매개 변수는 다음과 같다.
* Fullscreen
  * 전체 화면 모드로 전환할지 여부를 지정한다.
* pTarget
  * 전체 화면 모드에서 사용할 출력 장치.


### IDXGIOutput
디스플레이 출력을 나타내는 인터페이스로, 모니터와 같은 출력 장치에 대한 정보와 제어를 제공한다. 연결된 모니터의 해상도, 리프레시율 등의 정보를 얻고 설정할 수 있다.


## DXGI 객체 계층 구조
DXGI는 그래픽 어댑터, 출력 장치, 스왑 체인 등의 관리와 상호작용을 위한 다양한 인터페이스를 제공하며, 이들 인터페이스는 계층 구조를 형성한다.

IDXGIFactory의 자식 객체는 IDXGIAdapter와 IDXGISwapChain이며, IDXGIAdapter의 자식 객체는 IDXGIOutput이다.

이를 그림으로 나타내면 다음과 같다.

```
IDXGIFactory
|-- IDXGIAdapter
|   |-- IDXGIOutput
|
|-- IDXGISwapChain
```
