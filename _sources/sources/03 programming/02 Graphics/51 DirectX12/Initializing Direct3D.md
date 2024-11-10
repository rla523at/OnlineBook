# Initializing Direct3D
* Load pipeLine
  * Device 만들기
  * Command Queue 만들기
  * Command Allocator 만들기
  * Swap Chain 만들기
  * RTV Descriptor Heap 만들기
  * RTV Descriptor 만들기

## Device
device 란 display adapter 를 나타내는 개념으로 graphics card 와 같은 hardware 일 수도 있고 WARP adapter 와 같은 software 일 수 있다.

Display adapter는 컴퓨터와 화면 표시 장치(모니터)를 연결하는 장치로써 화면 표시 장치에 정보를 표현할 수 있도록 그래픽을 처리하고 출력하는데 사용되는 장치를 말한다.

### Windows Advanced Rasterization Platform adapter
Windows Advanced Rasterization Platform(WARP) adapter 는 Direct3D 10, 11, 12에서 제공되는 소프트웨어 기반의 그래픽 어댑터이다.

하드웨어 GPU가 없거나, GPU 기능이 제한된 시스템에서 고성능 그래픽을 제공하기 위한 기술로 WARP는 CPU에서 그래픽 처리를 수행하며, 하드웨어 가속이 불가능할 때에도 Direct3D 기능을 사용할 수 있게 해준다.

### D3D12CreateDevice 함수
ID3D12Device 객체를 생성하는 함수이다. 

함수의 시그니처는 다음과 같다.
```cpp
HRESULT D3D12CreateDevice(
  [in, optional]  IUnknown          *pAdapter,
                  D3D_FEATURE_LEVEL MinimumFeatureLevel,
  [in]            REFIID            riid,
  [out, optional] void              **ppDevice
);
```

인자는 다음과 같다.
* IUnknown *pAdapter
  * 사용할 그래픽 어댑터를 지정하는 포인터이다. 
  * nullptr 을 전달하면 IDXGIFactory1::EnumAdapters 로 열거된 첫 번째 어댑터인 기본 어댑터가 선택된다.

* D3D_FEATURE_LEVEL MinimumFeatureLevel
  * 생성하려는 디바이스가 지원해야 하는 최소 기능 수준을 나타낸다. 

* REFIID riid
  * 요청하는 인터페이스의 식별자이다. 
  * ID3D12Device 인터페이스가 요청된다.

* void **ppDevice
  * 성공 시, 생성된 디바이스 객체를 받는 포인터다. 
  * 이 포인터는 함수가 성공적으로 완료되면, ID3D12Device와 같은 인터페이스를 가리키게 된다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * 실패 원인은 호환되지 않는 어댑터나 기능 수준 부족 등이 될 수 있으며, 적절한 진단을 위해 오류 코드를 확인해야 한다.

> Reference  
> [learn.microsoft - nf-d3d12-d3d12createdevice](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-d3d12createdevice)  

### D3D_FEATURE_LEVEL

정의는 다음과 같다.
```cpp
typedef enum D3D_FEATURE_LEVEL
{
    D3D_FEATURE_LEVEL_9_1  = 0x9100,
    D3D_FEATURE_LEVEL_9_2  = 0x9200,
    D3D_FEATURE_LEVEL_9_3  = 0x9300,
    D3D_FEATURE_LEVEL_10_0 = 0xa000,
    D3D_FEATURE_LEVEL_10_1 = 0xa100,
    D3D_FEATURE_LEVEL_11_0 = 0xb000,
    D3D_FEATURE_LEVEL_11_1 = 0xb100,
    D3D_FEATURE_LEVEL_12_0 = 0xc000,
    D3D_FEATURE_LEVEL_12_1 = 0xc100,
    D3D_FEATURE_LEVEL_12_2 = 0xc200
} D3D_FEATURE_LEVEL;
```

각 기능 수준마다 어떤 기능을 지원하는지는 [여기](https://learn.microsoft.com/ko-kr/windows/win32/direct3d11/overviews-direct3d-11-devices-downlevel-intro)를 참고하면 된다.

> Reference  
> [learn.microsoft - d3d_feature_level](https://learn.microsoft.com/ko-kr/windows/win32/api/d3dcommon/ne-d3dcommon-d3d_feature_level)  

### ID3D12Device 인터페이스
device 를 나타내는 interface 다.

## Command Queue
GPU 가 실행할 command 의 목록을 가지고 있는 queue 이다.

참고로, 실제 command 의 data 는 command allocator 가 가지고 있으며, command queue 는 allocator 에 저장된 data 를 참고하여 GPU 에서 command 가 실행되도록 한다.

### ID3D12CommandQueue
D3D12 에서 GPU 의 command queue 를 나타내는 interface 이다.

### ID3D12Device::CreateCommandQueue 함수
ID3D12CommandQueue 객체를 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CreateCommandQueue(
  const D3D12_COMMAND_QUEUE_DESC *pDesc,
  REFIID                         riid,
  void                           **ppCommandQueue
);
```

인자는 다음과 같다.

- const D3D12_COMMAND_QUEUE_DESC *pDesc
  - 생성할 command queue 의 속성을 지정하는 D3D12_COMMAND_QUEUE_DESC 구조체에 대한 포인터다. 

- REFIID riid
  - 요청하는 인터페이스의 식별자다. 
  - 일반적으로 __uuidof(ID3D12CommandQueue)를 사용하여 얻는다.

- void **ppCommandQueue
  - 성공 시, 생성된 ID3D12CommandQueue 객체의 포인터를 받을 포인터다.

반환값은 다음과 같다.

- 성공 시
  - S_OK를 반환한다.

- 실패 시
  - HRESULT 오류 코드를 반환한다. 

> Reference   
> [learn.microsoft - id3d12device-createcommandqueue](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandqueue)  

### D3D12_COMMAND_QUEUE_DESC 구조체
Command Queue 의 특성을 정의하기 위한 구조체이다. 

정의는 다음과 같다.

```cpp
typedef struct D3D12_COMMAND_QUEUE_DESC {
  D3D12_COMMAND_LIST_TYPE   Type;
  INT                       Priority;
  D3D12_COMMAND_QUEUE_FLAGS Flags;
  UINT                      NodeMask;
} D3D12_COMMAND_QUEUE_DESC;
```

각각의 멤버변수는 다음과 같다.

* D3D12_COMMAND_LIST_TYPE Type
  * 명령 큐에서 실행할 명령 리스트의 유형을 지정한다.
  * enum D3D12_COMMAND_LIST_TYPE 값을 사용한다.

* INT Priority
  * 명령 큐의 우선 순위를 지정한다. 
  * 우선 순위가 높을수록 더 자주 실행된다.
  * enum D3D12_COMMAND_QUEUE_PRIORITY 의 값을 사용할 수 있다.

* D3D12_COMMAND_QUEUE_FLAGS Flags
  * 명령 큐의 플래그를 지정한다.
  * enum D3D12_COMMAND_QUEUE_PRIORITY 값을 사용할 수 있다.

* UINT NodeMask
  * 명령 큐가 실행될 노드를 지정한다. 
  * 0은 단일 GPU 설정에서 기본값으로 사용된다.

> Reference  
> [learn.microsoft - d3d12_command_queue_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_command_queue_desc)    

### enum D3D12_COMMAND_LIST_TYPE
Command list의 유형을 정의하는 enum이다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D12_COMMAND_LIST_TYPE
    {
        D3D12_COMMAND_LIST_TYPE_DIRECT	= 0,
        D3D12_COMMAND_LIST_TYPE_BUNDLE	= 1,
        D3D12_COMMAND_LIST_TYPE_COMPUTE	= 2,
        D3D12_COMMAND_LIST_TYPE_COPY	= 3,
        D3D12_COMMAND_LIST_TYPE_VIDEO_DECODE	= 4,
        D3D12_COMMAND_LIST_TYPE_VIDEO_PROCESS	= 5,
        D3D12_COMMAND_LIST_TYPE_VIDEO_ENCODE	= 6,
        D3D12_COMMAND_LIST_TYPE_NONE	= -1
    } 	D3D12_COMMAND_LIST_TYPE;
```

각 enum의 의미는 다음과 같다.

* D3D12_COMMAND_LIST_TYPE_DIRECT
  * 그래픽 및 컴퓨트 명령을 모두 포함할 수 있는 기본 명령 리스트 타입이다.
  * 렌더링, 리소스 복사 등 다양한 작업에 사용된다.
  * GPU의 그래픽 파이프라인을 직접 제어한다.
* D3D12_COMMAND_LIST_TYPE_BUNDLE
  * 재사용 가능한 명령 집합을 만들기 위한 번들 타입의 명령 리스트이다.
  * 다른 명령 리스트에서 호출되어 작업을 수행한다.
  * 주로 반복되는 명령 시퀀스를 최적화하기 위해 사용된다.
* D3D12_COMMAND_LIST_TYPE_COMPUTE
  * 컴퓨트 셰이더 작업에 특화된 명령 리스트이다.
  * 그래픽 렌더링 명령을 포함하지 않으며, 계산 작업에 집중한다.
* D3D12_COMMAND_LIST_TYPE_COPY
  * 리소스 간의 데이터 복사 작업에 최적화된 명령 리스트이다.
  * 복사 명령만을 포함하며, 그래픽이나 컴퓨트 작업은 포함하지 않는다.
* D3D12_COMMAND_LIST_TYPE_VIDEO_DECODE
  * 비디오 디코딩 작업에 사용되는 명령 리스트이다.
  * 비디오 스트림을 디코딩하여 프레임을 추출하는 데 사용된다.
* D3D12_COMMAND_LIST_TYPE_VIDEO_PROCESS
  * 비디오 프로세싱 작업에 사용되는 명령 리스트이다.
  * 비디오 프레임의 색상 보정, 필터 적용 등 후처리에 사용된다.
* D3D12_COMMAND_LIST_TYPE_VIDEO_ENCODE
  * 비디오 인코딩 작업에 사용되는 명령 리스트이다.
  * 프레임을 인코딩하여 비디오 스트림을 생성하는 데 사용된다.

> Reference  
> [learn.microsoft - d3d12_command_list_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_list_type)  

#### Bundle
command list 를 만들 때, CPU overhead 가 발생할 수 있기 때문에 D3D12 에서는 bundle 에 command list 를 기록할 수 있는 최적화를 제공한다.

bundle 에 기록된 후에는 driver 는 rendering 중에 실행을 최적화하기 위해 bundle 에 기록된 command 를 전처리한다.

따라서, bundle 은 초기화시에 기록되어야 한다.

일반적으로 D3D12 API 는 매우 효율적이기 때문에 bundle 을 기본적으로 사용할 필요는 없으며, 프로파일링 결과 특정 command list 를 만드는 과정이 시간이 오래걸리는 경우에만 bundle 을 통한 최적화를 고려하면 된다.

> Reference  
> {cite}`Luna` p110  

### enum D3D12_COMMAND_QUEUE_PRIORITY
Command queue 의 우선순위를 정의하는 enum 이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_COMMAND_QUEUE_PRIORITY
{
    D3D12_COMMAND_QUEUE_PRIORITY_NORMAL          = 0,
    D3D12_COMMAND_QUEUE_PRIORITY_HIGH            = 100,
    D3D12_COMMAND_QUEUE_PRIORITY_GLOBAL_REALTIME = 10000
} ;
```

각 enum의 의미는 다음과 같다.

* D3D12_COMMAND_QUEUE_PRIORITY_NORMAL
  * 기본 우선순위 수준이다.
  * 일반적인 렌더링 및 컴퓨트 작업에 적합하다.

* D3D12_COMMAND_QUEUE_PRIORITY_HIGH
  * 높은 우선순위 수준이다.
  * 중요한 작업의 처리 시간을 단축하기 위해 사용된다.
  * 동일한 우선순위의 다른 큐보다 먼저 스케줄링된다.

* D3D12_COMMAND_QUEUE_PRIORITY_GLOBAL_REALTIME
  * 글로벌 실시간 우선순위 수준이다.
  * 가장 높은 우선순위를 가지며, 최소한의 지연 시간이 필요한 실시간 응용 프로그램에서 사용된다.
  * 시스템 성능에 영향을 미칠 수 있으므로 주의해서 사용해야 한다.

> Reference   
> [learn.microsoft - d3d12_command_queue_priority](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_queue_priority)  

### enum D3D12_COMMAND_QUEUE_FLAGS
Command queue의 플래그를 정의하는 enum 이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_COMMAND_QUEUE_FLAGS
{
    D3D12_COMMAND_QUEUE_FLAG_NONE                = 0,
    D3D12_COMMAND_QUEUE_FLAG_DISABLE_GPU_TIMEOUT = 0x1
} D3D12_COMMAND_QUEUE_FLAGS;
```

각 enum의 의미는 다음과 같다.

* D3D12_COMMAND_QUEUE_FLAG_NONE
  * 기본 설정으로, 특별한 플래그가 없다.
  * GPU 시간 제한이 활성화되어 있어 장시간 실행되는 셰이더나 작업이 시간 제한에 걸릴 수 있다.

* D3D12_COMMAND_QUEUE_FLAG_DISABLE_GPU_TIMEOUT
  * GPU의 시간 제한을 비활성화한다.
  * 장시간 실행되는 컴퓨트 셰이더나 작업을 수행할 때 사용된다.
  * 시스템 안정성에 영향을 줄 수 있으므로 사용에 주의해야 한다.

> Reference   
> [learn.microsoft - d3d12_command_queue_flags](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_queue_flags)  

## Swap Chain
Swap Chain 은 화면에 렌더링된 이미지를 표시하기 위해 사용하는 버퍼들의 집합이다. 

버퍼들은 디스플레이에 출력되고 있는 front buffer 와 앞으로 출력될 데이터를 준비하고 있는 back buffer 로 구분되며, 이들을 교환하여 화면 전환을 구현한다.

### NBuffering
N 버퍼링은 N 개의 buffer 를 사용하여 swap chain 을 구성하는 방식이다.

트리플 버퍼링의 사용 예시를 보자.

버퍼 A, B, C가 있다고 가정하고 초기 상태가 다음과 같다고 가정하자.
* 버퍼 A: 디스플레이가 출력중인 frame 
* 버퍼 B: GPU가 디스플레이가 출력할 데이터를 렌더링중인 frame
* 버퍼 C: CPU가 새로운 프레임 데이터를 준비하고 있는 frame

다음 프레임에서는 swap chain 에서 flip 및 present 후 다음 상태가 된다.
* 버퍼 A: CPU가 새로운 프레임 데이터를 준비하고 있는 frame
* 버퍼 B: 디스플레이가 출력중인 frame
* 버퍼 C: GPU가 디스플레이가 출력할 데이터를 렌더링중인 frame

이런 과정을 반복하면서 다음과 같은 순서로 작업이 순환된다
* 디스플레이는 준비된 버퍼에서 프레임을 출력.
* CPU는 출력되지 않는 버퍼에서 새로운 프레임 데이터를 준비.
* GPU는 CPU가 준비한 버퍼에서 프레임을 렌더링.

### Flipping
버퍼 교환 시, back buffer 의 내용을 front buffer 로 복사하는 대신, 두 버퍼의 포인터를 교환하는 방식을 flipping 이라고 한다. 

이 방법은 하드웨어적으로 매우 빠르게 수행되며, 효율적인 화면 전환을 가능하게 합니다. 

### IDXGISwapChain1 VS IDXGISwapChain3 
IDXGISwapChain1과 IDXGISwapChain3는 모두 DXGI의 스왑 체인 인터페이스이지만, DirectX의 발전에 따라 기능적 차이가 있다. IDXGISwapChain1은 DirectX 11.1에서 도입된 기본 스왑 체인 인터페이스이고, IDXGISwapChain3는 DirectX 12에서 추가된 기능을 포함하여 성능 최적화가 된 확장 인터페이스다.

| 기능                          | IDXGISwapChain1                                | IDXGISwapChain3                                 |
|-------------------------------|------------------------------------------------|------------------------------------------------|
| 지원 버전                     | DirectX 11.1 이상                              | DirectX 12 이상                                |
| **현재 백 버퍼 인덱스 조회**  | 미지원                                        | GetCurrentBackBufferIndex 메서드 지원        |
| **프레임 동기화 최적화**       | 제한적                                        | DirectX 12 커맨드 큐와의 효율적인 연동 가능     |
| **사용 가능한 플래그**         | DirectX 11.1 수준                              | DirectX 12에서 추가된 새로운 플래그 지원       |
| **다중 백 버퍼 관리**         | 제한적                                        | 효율적인 백 버퍼 동기화와 관리 기능 제공        |

참고로, IDXGISwapChain3 는 직접 생성할 수 없다. 

IDXGISwapChain3 인터페이스는 CreateSwapChainForHwnd 와 같은 DXGI 팩토리 메서드에서 반환되는 IDXGISwapChain1 을 통해 업그레이드(캐스팅)하여 사용하는 인터페이스다. DXGI 팩토리 메서드들은 기본적으로 IDXGISwapChain1 또는 IDXGISwapChain 타입의 객체를 반환하므로, 스왑 체인 생성 후 QueryInterface 또는 As 메서드를 통해 IDXGISwapChain3로 변환해야 한다.

### DXGI_SWAP_CHAIN_DESC1 구조체
swap chain 의 특성을 정의하는 데 사용된다.

구조체의 정의는 다음과 같다.
```cpp
typedef struct DXGI_SWAP_CHAIN_DESC1 {
    UINT             Width;
    UINT             Height;
    DXGI_FORMAT      Format;
    BOOL             Stereo;
    DXGI_SAMPLE_DESC SampleDesc;
    DXGI_USAGE       BufferUsage;
    UINT             BufferCount;
    DXGI_SCALING     Scaling;
    DXGI_SWAP_EFFECT SwapEffect;
    DXGI_ALPHA_MODE  AlphaMode;
    UINT             Flags;
} DXGI_SWAP_CHAIN_DESC1;
```

각 멤버 변수는 다음과 같다.

* UINT Width  
  * 백 버퍼의 너비를 지정한다.

* UINT Height  
  * 백 버퍼의 높이를 지정한다.

* DXGI_FORMAT Format  
  * 백 버퍼의 픽셀 형식을 지정한다.

* BOOL Stereo  
  * 스왑 체인이 스테레오(3D) 모드를 지원하는지 여부를 지정한다.
  * TRUE로 설정하면 스테레오 모드를 활성화한다.

* DXGI_SAMPLE_DESC SampleDesc  
  * 멀티샘플링(안티앨리어싱) 매개변수를 지정하는 구조체이다.
  * Count와 Quality를 설정하여 멀티샘플링 수준을 결정한다.

* DXGI_USAGE BufferUsage  
  * 백 버퍼의 사용 방법을 지정하는 플래그이다.
  * DXGI_USAGE_RENDER_TARGET_OUTPUT은 렌더 타겟으로 사용됨을 나타낸다.

* UINT BufferCount  
  * 스왑 체인에서 사용할 버퍼의 수를 지정한다.
  * 일반적으로 더블 버퍼링은 2, 트리플 버퍼링은 3으로 설정한다.

* DXGI_SCALING Scaling  
  * 백 버퍼의 크기가 출력 창과 다를 때의 스케일링 방법을 지정한다.
  * DXGI_SCALING_STRETCH는 백 버퍼를 출력 창 크기에 맞게 늘린다.

* DXGI_SWAP_EFFECT SwapEffect  
  * 스왑 체인의 스왑 효과를 지정한다.
  * DXGI_SWAP_EFFECT_FLIP_DISCARD는 플립 모델을 사용하며, 프레젠테이션 후 백 버퍼의 내용을 폐기한다.

* DXGI_ALPHA_MODE AlphaMode  
  * 백 버퍼의 알파 모드(투명도)를 지정한다.
  * DXGI_ALPHA_MODE_IGNORE는 알파 채널을 무시함을 나타낸다.

* UINT Flags  
  * 스왑 체인의 추가 옵션을 지정하는 플래그이다.
  * NULL 을 전달하면 어떤 플래그도 설정되지 않는다.

### DXGI_UAGE 매크로
백 버퍼의 사용 방법을 지정하는 플래그다.

정의는 다음과 같다
```cpp
#define DXGI_CPU_ACCESS_NONE    ( 0 )
#define DXGI_CPU_ACCESS_DYNAMIC    ( 1 )
#define DXGI_CPU_ACCESS_READ_WRITE    ( 2 )
#define DXGI_CPU_ACCESS_SCRATCH    ( 3 )
#define DXGI_CPU_ACCESS_FIELD        15
#define DXGI_USAGE_SHADER_INPUT             ( 1L << (0 + 4) )
#define DXGI_USAGE_RENDER_TARGET_OUTPUT     ( 1L << (1 + 4) )
#define DXGI_USAGE_BACK_BUFFER              ( 1L << (2 + 4) )
#define DXGI_USAGE_SHARED                   ( 1L << (3 + 4) )
#define DXGI_USAGE_READ_ONLY                ( 1L << (4 + 4) )
#define DXGI_USAGE_DISCARD_ON_PRESENT       ( 1L << (5 + 4) )
#define DXGI_USAGE_UNORDERED_ACCESS         ( 1L << (6 + 4) )
typedef UINT DXGI_USAGE;
```

| 상수                      | 값              | 설명                                                                                           |
|---------------------------|-----------------|------------------------------------------------------------------------------------------------|
| DXGI_USAGE_BACK_BUFFER    | 1L << (2 + 4)   | 표면 또는 리소스는 백 버퍼로 사용된다. 스왑 체인 생성 시 전달 필요 없음. IDXGIResource::GetUsage를 호출하여 리소스가 스왑 체인에 속하는지 확인 가능. |
| DXGI_USAGE_DISCARD_ON_PRESENT | 1L << (5 + 4) | 내부 전용 플래그다.                                                                        |
| DXGI_USAGE_READ_ONLY      | 1L << (4 + 4)   | 읽기 전용으로 Surface 또는 리소스를 사용한다.                                                |
| DXGI_USAGE_RENDER_TARGET_OUTPUT | 1L << (1 + 4) | 표면 또는 리소스를 출력 렌더링 대상으로 사용한다.                                            |
| DXGI_USAGE_SHADER_INPUT   | 1L << (0 + 4)   | 셰이더에 대한 입력으로 표면 또는 리소스를 사용한다.                                          |
| DXGI_USAGE_SHARED         | 1L << (3 + 4)   | 표면 또는 리소스를 공유한다.                                                                 |
| DXGI_USAGE_UNORDERED_ACCESS | 1L << (6 + 4) | 순서가 지정되지 않은 액세스에 Surface 또는 리소스를 사용한다.                                 |

> Reference  
> [learn.microsoft - dxgi-usage](https://learn.microsoft.com/ko-kr/windows/win32/direct3ddxgi/dxgi-usage)  

### enum DXGI_SCALING
스케일링 모드를 정의하는 enum이다. 

정의는 다음과 같다.

```cpp
typedef enum DXGI_SCALING
{
    DXGI_SCALING_STRETCH           = 0,
    DXGI_SCALING_NONE              = 1,
    DXGI_SCALING_ASPECT_RATIO_STRETCH = 2
} DXGI_SCALING;
```

각 enum의 의미는 다음과 같다.

- DXGI_SCALING_STRETCH
  - 콘텐츠를 화면 전체에 맞춰 늘려서 표시한다.
  - 원본 해상도와 출력 해상도가 다를 경우, 비율이 유지되지 않아 왜곡될 수 있다.

- DXGI_SCALING_NONE
  - 스케일링을 하지 않고 원본 해상도 그대로 출력한다.
  - 해상도 차이가 있는 경우, 화면 일부가 비어 있을 수 있다.

- DXGI_SCALING_ASPECT_RATIO_STRETCH
  - 화면의 가로 세로 비율을 유지하면서 콘텐츠를 가능한 크게 늘려서 표시한다.
  - 해상도가 다르더라도 원본 비율을 유지하여 왜곡을 최소화한다.

> Reference  
> [learn.microsoft - dxgi_scaling](https://learn.microsoft.com/ko-kr/windows/win32/api/dxgi1_2/ne-dxgi1_2-dxgi_scaling)  

### enum DXGI_SWAP_EFFECT
swap chain 의 swap effect 를 정의하는 enum이다. 

swap chain 에서 프레임을 교환하는 방식에 따라 화면에 출력되는 내용이 달라지며, 각각의 swap effect 는 메모리 및 성능에 영향을 줄 수 있다.

정의는 다음과 같다.

```cpp
typedef enum DXGI_SWAP_EFFECT
{
    DXGI_SWAP_EFFECT_DISCARD        = 0,
    DXGI_SWAP_EFFECT_SEQUENTIAL     = 1,
    DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL = 3,
    DXGI_SWAP_EFFECT_FLIP_DISCARD   = 4
} DXGI_SWAP_EFFECT;
```

각 enum의 의미는 다음과 같다.

- DXGI_SWAP_EFFECT_DISCARD
  - swap chain 에서 프레임을 교환할 때 이전 프레임의 내용을 버린다.
  - 각 프레임이 독립적으로 렌더링되며, 메모리 사용이 효율적이다.
  - 일반적으로 풀스크린 응용 프로그램에서 사용된다.

- DXGI_SWAP_EFFECT_SEQUENTIAL
  - swap chain 에서 프레임을 순차적으로 교환한다.
  - 이전 프레임의 내용이 보존되므로 창 모드 애플리케이션에서 흔히 사용된다.

- DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL
  - 프레임을 교환할 때 플립 방식으로 순차적으로 전환한다.
  - DXGI_SWAP_EFFECT_SEQUENTIAL과 비슷하지만, Direct3D 11 이상에서 더 나은 성능을 제공하며 최신 그래픽 API에서 권장된다.

- DXGI_SWAP_EFFECT_FLIP_DISCARD
  - 프레임을 플립 방식으로 전환하며 이전 프레임의 내용을 버린다.
  - Direct3D 12 및 최신 Direct3D 11 응용 프로그램에서 권장되는 설정으로, 메모리와 성능 효율이 좋다.

> Reference  
> [learn.microsoft - dxgi_swap_effect](https://learn.microsoft.com/ko-kr/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_effect)  

### enum DXGI_ALPHA_MODE
swap chain 에서 알파 채널을 처리하는 방식을 정의하는 enum이다. 

정의는 다음과 같다.
```cpp
typedef enum DXGI_ALPHA_MODE
{
    DXGI_ALPHA_MODE_UNSPECIFIED       = 0,
    DXGI_ALPHA_MODE_PREMULTIPLIED     = 1,
    DXGI_ALPHA_MODE_STRAIGHT          = 2,
    DXGI_ALPHA_MODE_IGNORE            = 3,
    DXGI_ALPHA_MODE_FORCE_DWORD       = 0xFFFFFFFF
} DXGI_ALPHA_MODE;
```

각 enum의 의미는 다음과 같다.

- DXGI_ALPHA_MODE_UNSPECIFIED
  - 알파 모드가 지정되지 않았다.
  - 시스템이 기본 설정을 사용하도록 하며, 특정한 알파 블렌딩이 필요하지 않은 경우에 사용된다.

- DXGI_ALPHA_MODE_PREMULTIPLIED
  - 알파가 미리 곱해진 방식이다.
  - 각 색상 값이 알파 값에 미리 곱해진 상태로 렌더링되며, 투명도 블렌딩 시 색상 및 알파 값이 함께 계산된다.
  - 대부분의 투명도 효과에서 사용되는 일반적인 설정이다.

- DXGI_ALPHA_MODE_STRAIGHT
  - 알파가 미리 곱해지지 않은 방식이다.
  - 색상 값과 알파 값이 별도로 저장되어 있으며, 블렌딩 시 별도로 알파 값을 곱해 계산한다.
  - 투명도와 색상을 독립적으로 조정할 수 있어 정밀한 블렌딩이 필요할 때 사용된다.

- DXGI_ALPHA_MODE_IGNORE
  - 알파 채널을 무시한다.
  - 색상 값만 사용되며, 알파 값이 없는 상태로 처리된다. 투명도를 적용하지 않는 경우에 적합하다.

- DXGI_ALPHA_MODE_FORCE_DWORD
  - 강제로 32비트 값으로 설정하여 enum 크기를 DWORD로 강제 지정한다.
  - DXGI의 호환성을 위한 예약 값으로, 직접적으로 사용되지 않는다.

> Reference   
> [learn.microsoft - dxgi_alpha_mode](https://learn.microsoft.com/ko-kr/windows/win32/api/dxgi1_2/ne-dxgi1_2-dxgi_alpha_mode)  

### enum DXGI_SWAP_CHAIN_FLAG
swap chain 의 플래그를 정의하는 enum이다. 

각 플래그는 swap chain 의 동작 방식을 제어하며, 성능 및 기능적 특성에 영향을 줄 수 있다.

정의는 다음과 같다.

```cpp
typedef enum DXGI_SWAP_CHAIN_FLAG
{
    DXGI_SWAP_CHAIN_FLAG_NONE                        = 0,
    DXGI_SWAP_CHAIN_FLAG_NONPREROTATED               = 1,
    DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH           = 2,
    DXGI_SWAP_CHAIN_FLAG_GDI_COMPATIBLE              = 4,
    DXGI_SWAP_CHAIN_FLAG_RESTRICTED_CONTENT          = 8,
    DXGI_SWAP_CHAIN_FLAG_RESTRICT_SHARED_RESOURCE_DRIVER = 16,
    DXGI_SWAP_CHAIN_FLAG_DISPLAY_ONLY                = 32,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT = 64,
    DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER            = 128,
    DXGI_SWAP_CHAIN_FLAG_FULLSCREEN_VIDEO            = 256,
    DXGI_SWAP_CHAIN_FLAG_YUV_VIDEO                   = 512,
    DXGI_SWAP_CHAIN_FLAG_HW_PROTECTED                = 1024,
    DXGI_SWAP_CHAIN_FLAG_ALLOW_TEARING               = 2048
} DXGI_SWAP_CHAIN_FLAG;
```

각 enum의 의미는 다음과 같다.

- DXGI_SWAP_CHAIN_FLAG_NONE
  - 기본 설정으로, 특별한 플래그가 없다.

- DXGI_SWAP_CHAIN_FLAG_NONPREROTATED
  - swap chain 의 이미지를 회전하지 않은 상태로 유지한다.
  - 회전이 필요한 경우 디스플레이 드라이버가 아닌 응용 프로그램이 직접 회전 작업을 처리해야 한다.

- DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH
  - 풀스크린 모드와 창 모드 간 전환을 허용한다.
  - 응용 프로그램이 창 모드에서 풀스크린 모드로 전환할 때 디스플레이 해상도가 변경될 수 있다.

- DXGI_SWAP_CHAIN_FLAG_GDI_COMPATIBLE
  - GDI(Windows Graphics Device Interface)와 호환되도록 설정한다.
  - GDI를 사용하여 swap chain 에 직접 그리기를 할 수 있다.

- DXGI_SWAP_CHAIN_FLAG_RESTRICTED_CONTENT
  - 제한된 콘텐츠를 표시하기 위한 swap chain 을 생성할 때 사용된다.
  - 저작권 보호가 필요한 콘텐츠에 대해 보호된 출력 모드를 지원한다.

- DXGI_SWAP_CHAIN_FLAG_RESTRICT_SHARED_RESOURCE_DRIVER
  - swap chain 의 리소스가 공유되었을 때 일부 드라이버 기능을 제한한다.

- DXGI_SWAP_CHAIN_FLAG_DISPLAY_ONLY
  - 디스플레이 전용으로 설정하며, GPU에 의한 렌더링이 아닌 디스플레이 업데이트만 수행한다.

- DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
  - 프레임 지연을 제어할 수 있는 대기 가능 객체를 사용하여 프레임 렌더링 속도를 관리한다.
  - 프레임을 대기 상태로 유지하거나 조정할 수 있어, 타이밍 제어가 필요한 응용 프로그램에서 사용된다.

- DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER
  - 응용 프로그램을 포어그라운드 레이어에 배치한다.
  - 다른 창보다 상위 레이어에 배치되는 특수 용도에 사용된다.

- DXGI_SWAP_CHAIN_FLAG_FULLSCREEN_VIDEO
  - 비디오를 풀스크린 모드로 표시할 때 사용한다.
  - 비디오 콘텐츠에 최적화된 설정이다.

- DXGI_SWAP_CHAIN_FLAG_YUV_VIDEO
  - YUV 색상 공간을 사용하는 비디오 콘텐츠에 최적화된 설정이다.
  - 색상 변환 없이 YUV 형식의 콘텐츠를 출력할 수 있다.

- DXGI_SWAP_CHAIN_FLAG_HW_PROTECTED
  - 하드웨어 보호된 콘텐츠를 표시하기 위한 플래그이다.
  - 저작권 보호가 필요한 콘텐츠의 보안을 강화한다.

- DXGI_SWAP_CHAIN_FLAG_ALLOW_TEARING
  - 화면 찢김(테어링)을 허용하는 플래그이다.
  - 일반적으로 수직 동기화(VSync)가 비활성화된 경우에 사용되며, 게임이나 실시간 그래픽 응용 프로그램에서 프레임 속도를 최적화할 때 유용하다.


> Reference   
> [learn.microsoft - dxgi_swap_chain_flag](https://learn.microsoft.com/ko-kr/windows/win32/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)  

### IDXGIFactory2::CreateSwapChainForHwnd 함수
지정된 HWND 핸들과 연결된 swap chain 을 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CreateSwapChainForHwnd(
  [in]           IUnknown                              *pDevice,
  [in]           HWND                                  hWnd,
  [in]           const DXGI_SWAP_CHAIN_DESC1           *pDesc,
  [in, optional] const DXGI_SWAP_CHAIN_FULLSCREEN_DESC *pFullscreenDesc,
  [in, optional] IDXGIOutput                           *pRestrictToOutput,
  [out]          IDXGISwapChain1                       **ppSwapChain
);
```

인자는 다음과 같다.

- IUnknown *pDevice
  - Direct3D 11 및 이전 버전의 Direct3D의 경우, 스왑 체인용 Direct3D 디바이스에 대한 포인터다. 
  - Direct3D 12의 경우, direct command queue 에 대한 포인터다.
  - 이 매개변수는 NULL일 수 없다.

- HWND hWnd
  - 생성할 스왑 체인과 연결된 창의 핸들이다. 
  - 이 매개변수는 NULL일 수 없다.

- const DXGI_SWAP_CHAIN_DESC1 *pDesc
  - 스왑 체인의 속성을 지정하는 DXGI_SWAP_CHAIN_DESC1 구조체에 대한 포인터다. 
  - 이 매개변수는 NULL일 수 없다.

- const DXGI_SWAP_CHAIN_FULLSCREEN_DESC *pFullscreenDesc
  - 전체 화면 스왑 체인의 속성을 지정하는 DXGI_SWAP_CHAIN_FULLSCREEN_DESC 구조체에 대한 포인터다. 
  - 전체 화면 스왑 체인을 생성하려면 이 매개변수를 설정하고, 창 모드 스왑 체인을 생성하려면 NULL로 설정한다.

- IDXGIOutput *pRestrictToOutput
  - 콘텐츠를 제한할 출력 대상의 IDXGIOutput 인터페이스에 대한 포인터다. 
  - 콘텐츠를 특정 출력으로 제한하려면 이 매개변수를 설정하고, 그렇지 않으면 NULL로 설정한다.

- IDXGISwapChain1 **ppSwapChain
  - 생성된 IDXGISwapChain1 인터페이스에 대한 포인터를 받을 포인터다. 
  - 이 매개변수는 NULL일 수 없다.

반환값은 다음과 같다.

- 성공 시
  - S_OK를 반환한다.

- 실패 시
  - HRESULT 오류 코드를 반환한다. 예를 들어, 메모리가 부족한 경우 E_OUTOFMEMORY를 반환할 수 있다. 호출 애플리케이션이 잘못된 데이터를 제공한 경우 DXGI_ERROR_INVALID_CALL을 반환할 수 있다. 

> Reference  
> [learn.microsoft - idxgifactory2-createswapchainforhwnd](https://learn.microsoft.com/ko-kr/windows/win32/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforhwnd)  

## CommandAllocator
command 가 실제로 저장될 memory 다.

### ID3D12Device::CreateCommandAllocator 함수
ID3D12CommandAllocator 객체를 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CreateCommandAllocator(
  D3D12_COMMAND_LIST_TYPE type,
  REFIID                  riid,
  void                    **ppCommandAllocator
);
```

인자는 다음과 같다.

- D3D12_COMMAND_LIST_TYPE type
  - 생성할 명령 할당자의 유형을 지정한다. 

- REFIID riid
  - 요청하는 인터페이스의 식별자다. 
  - __uuidof(ID3D12CommandAllocator)를 사용하여 얻는다.

- void **ppCommandAllocator
  - 성공 시, 생성된 ID3D12CommandAllocator 객체의 포인터를 받을 포인터다.

반환값은 다음과 같다.

- 성공 시
  - S_OK를 반환한다.

- 실패 시
  - HRESULT 오류 코드를 반환한다.

> Reference  
> [learn.microsoft - id3d12device-createcommandallocator](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandallocator)  