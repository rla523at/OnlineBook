# CPU GPU Interaction
그래픽 프로그래밍에는 CPU 와 GPU 라는 두개의 프로세서가 작동하며, 병렬적으로 작동하나 때로는 동기화가 필요하다.

따라서, 최대의 성능을 내기 위해서는 두개의 프로세서가 계속해서 동작하게 만들어줘야 한다. 이 때, 동기화는 하나의 processor 가 다른 processor 를 기다리게 만들 수 있기 때문에 동기화를 줄여야 한다.

GPU 는 command queue 를 가지고 있으며, CPU 는 Direct3D API 를 이용해서 command queue 에 command 를 제출한다.

이 때, 명심해야 할 점은 CPU 에서 command 를 command queue 에 제출했다고 해서 GPU 에서 바로 실행되는 것이 아니다. GPU 가 처리할 준비가 되어 command queue 에서 command 를 꺼내서 실행할 때 비로소 실행이 된다.

만약 command queue 가 비어있을 경우 GPU 는 idle 상태가 된다. 반면에 command queue 가 가득 차 있을 경우 CPU 는 어느 시점에 idle 상태가 된다.

> Reference  
> {cite}`Luna` Chapter 4.2


## ID3D12CommandQueue
D3D12 에서 command queue 를 나타내는 interface 이다.

## D3D12_COMMAND_QUEUE_DESC 구조체
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
> [learn.microsoft - ns-d3d12-d3d12_command_queue_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_command_queue_desc)    

## enum D3D12_COMMAND_LIST_TYPE
Command list의 유형을 정의하는 enum이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_COMMAND_LIST_TYPE
{
    D3D12_COMMAND_LIST_TYPE_DIRECT        = 0,
    D3D12_COMMAND_LIST_TYPE_BUNDLE        = 1,
    D3D12_COMMAND_LIST_TYPE_COMPUTE       = 2,
    D3D12_COMMAND_LIST_TYPE_COPY          = 3,
    D3D12_COMMAND_LIST_TYPE_VIDEO_DECODE  = 4,
    D3D12_COMMAND_LIST_TYPE_VIDEO_PROCESS = 5,
    D3D12_COMMAND_LIST_TYPE_VIDEO_ENCODE  = 6
} D3D12_COMMAND_LIST_TYPE;
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
> [learn.microsoft - ne-d3d12-d3d12_command_list_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_list_type)  

## enum D3D12_COMMAND_QUEUE_PRIORITY
Command queue의 우선순위를 정의하는 enum 이다.

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
> [learn.microsoft - ne-d3d12-d3d12_command_queue_priority](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_queue_priority)  

## enum D3D12_COMMAND_QUEUE_FLAGS
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
> [learn.microsoft - ne-d3d12-d3d12_command_queue_flags](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_queue_flags)  

## ID3D12Device::CreateCommandQueue 함수

## ID3D12CommandQueue::ExecuteCommandLists 함수
command list 에 있는 commands 를 command queue 에 추가하는 함수이다.

command list 에 있는 command 는 첫 번 째 배열 요소부터 시작하여 순서대로 command queue 에 추가된다.


## ID3D12CommandList 인터페이스

## ID3D12GraphicsCommandList 인터페이스
ID3D12CommandList interface 를 상속받은 interface 로 graphics 에 대한 command list 를 나타내는 interface 이다. 

ID3D12GraphicsCommandList 는 command list 에 command 를 추가하기 위한 다양한 함수를 제공한다.

이 떄, 주의할 점은 함수의 이름을 보면 마치 함수로 인해 command 가 바로 실행되는 것처럼 보이지만 실제로는 그저 command list 에 command 를 추가하는 역할만 한다.

> Reference  
> {cite}`Luna` p109

## ID3D12Device::CreateCommandList 함수
command list 을 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CreateCommandList(
  [in]           UINT                    nodeMask,
  [in]           D3D12_COMMAND_LIST_TYPE type,
  [in]           ID3D12CommandAllocator  *pCommandAllocator,
  [in, optional] ID3D12PipelineState     *pInitialState,
  [in]           REFIID                  riid,
  [out]          void                    **ppCommandList
);
```

인자는 다음과 같다.

- UINT nodeMask
  - 단일 GPU 환경에서는 0으로 설정한다. 
  - 다중 GPU 노드가 있는 경우, command list 을 생성할 노드를 식별하는 비트를 설정한다. 
  - 각 비트는 하나의 노드에 해당하며, 하나의 비트만 설정해야 한다.

- D3D12_COMMAND_LIST_TYPE type
  - 생성할 command list 의 유형을 지정한다. 
  - enum D3D12_COMMAND_LIST_TYPE 을 사용한다.

- ID3D12CommandAllocator *pCommandAllocator
  - 명령 목록을 생성할 때 사용할 명령 할당자에 대한 포인터다.

- ID3D12PipelineState *pInitialState
  - 선택적 매개변수로, 명령 목록의 초기 파이프라인 상태를 지정하는 파이프라인 상태 객체에 대한 포인터다. nullptr로 설정하면 런타임에서 기본 초기 파이프라인 상태를 설정한다.

- REFIID riid
  - 요청하는 인터페이스의 식별자다.

- void **ppCommandList
  - 성공 시, 생성된 ID3D12CommandList 또는 ID3D12GraphicsCommandList 인터페이스에 대한 포인터를 받을 포인터다.

반환값은 다음과 같다.

- 성공 시
  - S_OK를 반환한다.

- 실패 시
  - HRESULT 오류 코드를 반환한다. 예를 들어, 메모리가 부족한 경우 E_OUTOFMEMORY를 반환할 수 있다. 다른 가능한 반환 값은 Direct3D 12 반환 코드를 참고한다. 


> Reference   
> [learn.microsoft - nf-d3d12-id3d12device-createcommandlist](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandlist)  

### Multiple Creation 
여러개의 command lists 에 하나의 allocator 를 할당할 수 있다.

하지만 command 를 기록할 command list 를 제외한 다른 모든 command list 들은 closed 상태여야 한다.

만약, 그렇지 않을 경우 다음과 같은 오류가 발생하게 된다.

```
D3D12 ERROR: ID3D12CommandList::{Create,Reset}CommandList: The command allocator is currently in-use by another command list.
```

따라서, 하나의 command list 에 들어 있는 모든 command 들은 allocator 에 연속적으로 저장되게 된다. 

> Reference  
> {cite}`Luna` p111

## ID3D12GraphicsCommandList::Close 함수
command 추가를 완료했음을 나타내는 함수이다.

ID3D12CommandQueue::ExecuteCommandLists 함수에 인자로 ID3D12GraphicsCommandList 를 넘겨주기 전에 반드시 Close 함수를 호출해야 한다.

> Reference  
> {cite}`Luna` p109

## ID3D12CommandList::Reset 함수
만약 ID3D12CommandQueue::ExecuteCommandList 함수를 호출하였다면 ID3D12CommandList::Reset 를 호출하여 안전하게 command list 에 memory 를 새로운 command 들을 저장하는데 사용할 수 있다,

ID3D12CommandList::Reset 은 command list 를 방금 생성한 것과 동일한 상태로 만들어준다.

이로써 이전 command lists 에 할당된 memory 를 해제하고 새로운 memory 를 할당해주는 작업을 할 필요가 없이 allocator 를 재사용 할 수 있게 해준다.

(?) Reset 함수를 호출하더라도 command queue 에 있는 command 들에는 아무런 영향이 없다. 왜냐하면 command 들이 저장되어 있는 command allocator 에는 여전히 command queue 가 참조하는 command 가 memroy 에 존재하기 때문이다.

## ID3D12CommandAllocator 인터페이스
command list 와 연관되어 있는 memory 관련 interface 이다.

command 가 command list 에 추가된다는 것은 실제로 associated command allocator 에 저장된다는 것이다.

그리고 ID3D12CommandQueue::ExecuteCommandLists 를 통해 command list 가 전달되면 command queue 는 allocator 에 저장된 의 commands 을 참조하게 된다.

## ID3D12Device::CreateCommandAllocator 함수
command list 을 저장하는 데 사용할 Command Allocator를 생성하는 함수다. 

함수의 시그니처는 다음과 같다.
```cpp
HRESULT CreateCommandAllocator(
  [in]  D3D12_COMMAND_LIST_TYPE type,
        REFIID                  riid,
  [out] void                    **ppCommandAllocator
);
```

인자는 다음과 같다.
* D3D12_COMMAND_LIST_TYPE type
  * 생성할 커맨드 할로케이터의 유형을 지정한다. 
  * enum D3D12_COMMAND_LIST_TYPE 을 사용한다.

* REFIID riid
  * 요청하는 인터페이스의 식별자이다. 

* void **ppCommandAllocator
  * 성공 시, 생성된 ID3D12CommandAllocator 객체의 포인터를 받을 포인터다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * 실패 시 오류 코드를 통해 생성 실패 원인을 진단할 수 있다.

> Reference    
> [learn.microsoft - nf-d3d12-id3d12device-createcommandallocator](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandallocator)  


## enum D3D12_COMMAND_LIST_TYPE
Command list의 유형을 정의하는 enum이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_COMMAND_LIST_TYPE {
  D3D12_COMMAND_LIST_TYPE_DIRECT = 0,
  D3D12_COMMAND_LIST_TYPE_BUNDLE = 1,
  D3D12_COMMAND_LIST_TYPE_COMPUTE = 2,
  D3D12_COMMAND_LIST_TYPE_COPY = 3,
  D3D12_COMMAND_LIST_TYPE_VIDEO_DECODE = 4,
  D3D12_COMMAND_LIST_TYPE_VIDEO_PROCESS = 5,
  D3D12_COMMAND_LIST_TYPE_VIDEO_ENCODE,
  D3D12_COMMAND_LIST_TYPE_NONE
} ;
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
> [learn.microsoft - ne-d3d12-d3d12_command_list_type](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/ne-d3d12-d3d12_command_list_type)  

### Bundle
command list 를 만들 때, CPU overhead 가 발생할 수 있기 때문에 D3D12 에서는 bundle 에 command list 를 기록할 수 있는 최적화를 제공한다.

bundle 에 기록된 후에는 driver 는 rendering 중에 실행을 최적화하기 위해 bundle 에 기록된 command 를 전처리한다.

따라서, bundle 은 초기화시에 기록되어야 한다.

일반적으로 D3D12 API 는 매우 효율적이기 때문에 bundle 을 기본적으로 사용할 필요는 없으며, 프로파일링 결과 특정 command list 를 만드는 과정이 시간이 오래걸리는 경우에만 bundle 을 통한 최적화를 고려하면 된다.

> Reference  
> {cite}`Luna` p110  

