# CPU GPU Interaction
그래픽 프로그래밍에는 CPU 와 GPU 라는 두개의 프로세서가 작동하며, 병렬적으로 작동하나 때로는 동기화가 필요하다.

따라서, 최대의 성능을 내기 위해서는 동기화를 최소화시켜 두개의 프로세서가 계속해서 동작하게 만들어줘야 한다. 왜냐하면, 동기화는 하나의 processor 가 다른 processor 를 기다리게 만들 수 있기 때문이다.

## CommandQueue
GPU 가 실행할 command 의 목록을 가지고 있는 queue 이다.

CPU 는 Direct3D API 를 이용해서 command queue 에 command 를 제출한다. 이 때, 명심해야 할 점은 CPU 에서 command 를 command queue 에 제출했다고 해서 GPU 에서 바로 실행되는 것이 아니다. GPU 가 처리할 준비가 되어 command queue 에서 command 를 꺼내서 실행할 때 비로소 실행이 된다.

만약 command queue 가 비어있을 경우 GPU 는 idle 상태가 된다. 반면에 command queue 가 가득 차 있을 경우 CPU 는 idle 상태가 된다.

> Reference  
> {cite}`Luna` Chapter 4.2

### ID3D12CommandQueue
D3D12 에서 GPU 의 command queue 를 나타내는 interface 이다.

### ID3D12CommandQueue::ExecuteCommandLists 함수
command list 에 있는 commands 를 command queue 에 추가하는 함수이다.

command list 에 있는 command 는 첫 번 째 배열 요소부터 시작하여 순서대로 command queue 에 추가된다.

## Command List
CPU 가 GPU 에 command 를 제출하기 위해 사용하는 command 들의 list 이다.

D3D12 에서는 CPU 와 GPU 간의 효율적인 병렬 처리를 위해 command lists 를 사용하여 CPU 에서 여러 GPU 명령을 미리 작성하고 한 번에 전달하는 방식을 채택하였다. 이 떄, CPU 에서 미리 작성 된 command 들을 command list 라고 한다.

### ID3D12CommandList 인터페이스
command list 를 나타내는 인터페이스이다.

### ID3D12GraphicsCommandList 인터페이스
ID3D12CommandList interface 를 상속받은 interface 로 graphics 에 대한 command list 를 나타내는 interface 이다. 

ID3D12GraphicsCommandList 는 command list 에 command 를 추가하기 위한 다양한 함수를 제공한다.

이 떄, 주의할 점은 함수의 이름을 보면 마치 함수로 인해 command 가 바로 실행되는 것처럼 보이지만 실제로는 그저 command list 에 command 를 추가하는 역할만 한다.

> Reference  
> {cite}`Luna` p109

### ID3D12Device::CreateCommandList 함수
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

### ID3D12GraphicsCommandList::Close 함수
command 추가를 완료했음을 나타내는 함수이다.

ID3D12CommandQueue::ExecuteCommandLists 함수에 인자로 ID3D12GraphicsCommandList 를 넘겨주기 전에 반드시 Close 함수를 호출해야 한다.

> Reference  
> {cite}`Luna` p109

### ID3D12CommandList::Reset 함수
ID3D12CommandList::Reset 함수가 호출되면 command list 와 연관된 command allocator 의 상태가 어떻게 바뀌는지 설명해주세요.

만약 ID3D12CommandQueue::ExecuteCommandList 함수를 호출하였다면 ID3D12CommandList::Reset 를 호출하여 안전하게 command list 에 memory 를 새로운 command 들을 저장하는데 사용할 수 있다,

ID3D12CommandList::Reset 은 command list 를 방금 생성한 것과 동일한 상태로 만들어준다.

이로써 이전 command lists 에 할당된 memory 를 해제하고 새로운 memory 를 할당해주는 작업을 할 필요가 없이 allocator 를 재사용 할 수 있게 해준다.

(?) Reset 함수를 호출하더라도 command queue 에 있는 command 들에는 아무런 영향이 없다. 왜냐하면 command 들이 저장되어 있는 command allocator 에는 여전히 command queue 가 참조하는 command 가 memroy 에 존재하기 때문이다.

## Command Allocator
command list 에 기록될 command 가 실제로 저장될 memory 다.

ID3D12CommandQueue::ExecuteCommandLists 를 호출하면 command queue 는 인자로 전달된 command list 의 allocator 에 접근해 저장된 command 를 참조하게 된다. 따라서 GPU 에서 command 가 실행이 되는 동안에는 해당하는 command allocator 가 유지되어야 한다.

### ID3D12CommandAllocator 인터페이스
Command Allocator 를 나타내는 interface 이다.

### ID3D12Device::CreateCommandAllocator 함수
ID3D12CommandAllocator 객체를 생성하는 함수다. 

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

> Reference    
> [learn.microsoft - nf-d3d12-id3d12device-createcommandallocator](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandallocator)  

### Multiple Creation 
하나의 allocator 로 여러개의 command lists 를 생성할 수 있다.

하지만, 여러개의 command lists 에 동시에 command 를 기록할 수는 없다. command 를 기록할 command list 를 제외한 다른 모든 command list 들은 closed 상태여야 한다. 

만약, 그렇지 않을 경우 다음과 같은 오류가 발생하게 된다.
```
D3D12 ERROR: ID3D12CommandList::{Create,Reset}CommandList: The command allocator is currently in-use by another command list.
```

이런 제약 조건 때문에 하나의 command list 에 들어 있는 모든 command 들은 allocator 에 연속적으로 저장되게 된다. 

> Reference  
> {cite}`Luna` p111


## CPU GPU Synchronization
CPU 와 GPU 라는 두개의 processor 가 병렬적으로 동작하기 때문에 다양한 synchronization 문제가 발생할 수 있다.

예를 들어, resource R 이 있다고 하자. CPU 에서 R 에 그리고자 하는 정보 I1 을 저장하고 R 을 참고하여 rendering 을 하라는 draw command 를 command list 에 넣은 뒤 command queue 에 전달했다고 하자. command queue 에 전달한 것은 CPU 를 block 시키지 않음으로 CPU 는 바로 다음 동작으로 새로운 정보 I2 를 R 에 저장하는 순간 문제가 발생할 수 있다. 만약에 GPU 가 다른 작업 때문에 draw command 를 아직 실행하기 전인데 CPU 에서 새로운 정보 I2 를 R 에 저장하는 순간 draw command 를 실행할 때 R 에는 이미 I1 에 대한 정보가 없기 때문이다.

### Soluition1 : Flushing the Command Queue
한가지 해결방법은 GPU 가 draw command 를 완료할 떄 까지 CPU 를 강제로 기다리게 하는 fence 를 만드는 방법이다. D3D12 에서 fence 는 ID3D12Fence interface 를 통해 나타내어지며 D3D12 에서 fence 의 동작 방식은 다음과 같다.

command queue 에 ID3D12Fence interface 의 값을 특정 값으로 업데이트하라는 command 를 추가한다. 이 command 를 fence point 라고 하면 GPU 가 fence point 이전에 command 를 전부 실행하고 fence point 에 도달하는 순간 ID3D12Fence interface 의 값이 업데이트 되는 방식이다.

아래 예제 코드를 보자.

```cpp
UINT64 mCurrentFence = 0;
void D3DApp::FlushCommandQueue()
{
 // 업데이트 될 Fence 값 
  mCurrentFence++;
 
 // Fence 를 mCurrentFence 라는 값으로 업데이트하는 command 를 Command Queue 에 추가
 ThrowIfFailed(mCommandQueue->Signal(mFence.Get(), mCurrentFence));
 
 // Fence 가 업데이트가 안되어 있을 경우 업데이트 될 떄 까지 기다리게 하는 코드
 if(mFence->GetCompletedValue() < mCurrentFence)
 {
 HANDLE eventHandle = CreateEventEx(nullptr, false, false, EVENT_ALL_ACCESS);
 
 ThrowIfFailed(mFence->SetEventOnCompletion(mCurrentFence, eventHandle));
 
 WaitForSingleObject(eventHandle, INFINITE);
 
 CloseHandle(eventHandle);
 }
}
```

### Solution2 : Frame Resource
첫번째 해결방법은 잘 동작하지만 성능 측면에서 문제가 있다.

매 frame 마다 마지막에 Flushing the Command Queue 를 실행하게 되면 CPU 는 GPU 가 rendering 을 완료할 떄 까지 idle 상태로 대기하게 된다. 또한 매 frame 의 시작점에는 command queue 에 아무런 command 가 없게 됨으로 GPU 는 CPU 가 command 를 추가할 때 까지 idle 상태로 대기하게 된다.

따라서 이를 해결하기 위해서는 CPU 가 매 frame 마다 수정해야 되는 resource 를 n 개 frame 에 필요한 resource 크기를 갖는 circular resource 로 만든 뒤에 GPU 가 i 번째 resource 를 활용하여 rendering 하고 있을 때, CPU 는 i+1 번째 resource 를 업데이트 하는 방식을 사용하면 된다. 이런 방식을 frame resource 라고 한다.

일반적으로 n = 3 을 사용한다.

> Reference  
> {cite}`Luna` chpater 7.1  
