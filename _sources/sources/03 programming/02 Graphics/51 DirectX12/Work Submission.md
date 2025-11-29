# Work Submission

## CommandQueue
GPU 가 실행할 command 의 목록을 가지고 있는 queue 이다.

CPU 는 Direct3D API 를 이용해서 command queue 에 command 를 제출한다. 이 때, 명심해야 할 점은 CPU 에서 command 를 command queue 에 제출했다고 해서 GPU 에서 바로 실행되는 것이 아니다. GPU 가 처리할 준비가 되어 command queue 에서 command 를 꺼내서 실행할 때 비로소 실행이 된다.

만약 command queue 가 비어있을 경우 GPU 는 idle 상태가 된다. 반면에 command queue 가 가득 차 있을 경우 CPU 는 idle 상태가 된다.

> Reference  
> {cite}`Luna` Chapter 4.2


<details> <summary> <h3 style="display:inline-block"> ID3D12CommandQueue 생성할 때 D3D12_COMMAND_LIST_TYPE 이 필요한 이유 </h3></summary>

 D3D12_COMMAND_QUEUE_DESC에 D3D12_COMMAND_LIST_TYPE을 명시하는 이유는 해당 큐가 어떤 종류의 명령 리스트를 처리할 것인지 명확히 정의하여, GPU의 적절한 하드웨어 엔진 할당, 최적화된 스케줄링, 그리고 효율적인 동기화 및 리소스 관리를 보장하기 위함입니다.
* 명령 리스트 유형에 따른 하드웨어 엔진 매핑
  * Direct, Compute, Copy:
  * 각각의 명령 리스트 타입은 GPU의 특정 하드웨어 엔진(그래픽, 컴퓨팅, 복사)에 대응합니다.
  * 예를 들어, Direct 타입은 그래픽 렌더링과 컴퓨팅 작업 모두를 처리할 수 있고, Copy 타입은 데이터 전송 작업에 특화되어 있습니다.
* 최적화된 스케줄링 및 실행 보장
  * 전용 큐 할당:
    * 큐를 생성할 때 명확한 명령 리스트 타입을 지정하면, 드라이버는 해당 큐에 올바른 하드웨어 리소스를 할당하고, 최적화된 방식으로 명령을 스케줄링할 수 있습니다.
  * 일관성 있는 명령 제출:
    * 커맨드 큐에 제출되는 명령 리스트는 지정된 타입과 일치해야 합니다. 이를 통해, 잘못된 유형의 명령이 큐에 들어오는 것을 방지하며, 예기치 않은 동작을 줄입니다.
* 동기화 및 리소스 관리의 용이성
  * 명시적 타입 지정:
    * 각 큐가 특정 작업에 최적화되어 있기 때문에, 동기화나 리소스 상태 관리 측면에서도 명확한 구분이 가능해집니다.
  * 병렬 처리의 이점:
    * 여러 타입의 큐를 분리하여 운영하면, 서로 다른 하드웨어 엔진을 활용한 병렬 작업이 가능해집니다.
    
</details>


<details> <summary> <h3 style="display:inline-block"> ID3D12CommandQueue::ExecuteCommandLists 함수 </h3></summary>

command list 에 있는 commands 를 command queue 에 추가하는 함수이다.
command list 에 있는 command 는 첫 번 째 배열 요소부터 시작하여 순서대로 command queue 에 추가된다.

</details>


<details> <summary> <h3 style="display:inline-block"> ID3D12CommandQueue::Signal 함수 </h3></summary>

command queue 에 Fence 가 관리하는 값을 특정 값으로 업데이트하라는 command 를 추가하는 함수이다.

> Reference   
> [learn.microsoft - d3d12-id3d12commandqueue-signal](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12commandqueue-signal)   
</details>


## Command Allocator
command list 에 기록될 command 가 실제로 저장될 memory 다.

ID3D12CommandQueue::ExecuteCommandLists 를 호출하면 command queue 는 인자로 전달된 command list 의 allocator 에 접근해 저장된 command 를 참조하게 된다. 따라서 GPU 에서 command 가 실행이 되는 동안에는 해당하는 command allocator 가 유지되어야 한다.

<details> <summary> <h3 style="display:inline-block"> ID3D12CommandAllocator 인터페이스 </h3></summary>
 
Command Allocator 를 나타내는 interface 이다.

</details>


<details> <summary> <h3 style="display:inline-block"> 생성 </h3></summary>

commnad allocator 는 ID3D12Device::CreateCommandAllocator 함수로 생성된다.
이 때, enum D3D12_COMMAND_LIST_TYPE 을 통해 앞으로 commnad allocator 에 어떤 타입의 command list 에 기록된 command 를 저장할지 결정한다.

> Reference  
> [learn.microsoft - id3d12device-createcommandallocator](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandallocator)  

Multiple Creation

하나의 allocator 로 여러개의 command lists 를 생성할 수 있다. 하지만, 여러개의 command lists 에 동시에 command 를 기록할 수는 없다. command 를 기록할 command list 를 제외한 다른 모든 command list 들은 closed 상태여야 한다. 만약, 그렇지 않을 경우 다음과 같은 오류가 발생하게 된다.
```
D3D12 ERROR: ID3D12CommandList::{Create,Reset}CommandList: The command allocator is currently in-use by another command list.
```

이런 제약 조건 때문에 하나의 command list 에 들어 있는 모든 command 들은 allocator 에 연속적으로 저장되게 된다. 

> Reference  
> {cite}`Luna` p111

</details>
 
<details> <summary> <h3 style="display:inline-block"> Reset </h3></summary>

 Reset 함수를 호출하지 않으면 command 가 계속 쌓여서, 프로세스 메모리가 끊임없이 증가한다.

> Reference   
> [learn.microsoft - id3d12commandallocator-reset](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12commandallocator-reset)  

</details>


## Command List
CPU 가 GPU 에 command 를 제출하기 위해 사용하는 command 들의 list 이다.

D3D12 에서는 CPU 와 GPU 간의 효율적인 병렬 처리를 위해 command lists 를 사용하여 CPU 에서 여러 GPU 명령을 미리 작성하고 한 번에 전달하는 방식을 채택하였다. 

이 떄, 주의할 점은 함수의 이름을 보면 마치 함수로 인해 command 가 바로 실행되는 것처럼 보이지만 실제로는 그저 command list 에 command 를 추가하는 역할만 한다.

<details> <summary> <h3 style="display:inline-block"> ID3D12CommandList 인터페이스 </h3></summary>
command list 를 나타내는 인터페이스이다.

ID3D12GraphicsCommandList 인터페이스는 ID3D12CommandList interface 를 상속받은 interface 로 graphics 에 대한 command list 를 나타내는 interface 이다. 

> Reference  
> {cite}`Luna` p109
</details>


<details> <summary> <h3 style="display:inline-block"> Command Allocator 없이 생성하기 </h3></summary>
ID3D12Device::CreateCommandList 함수를 사용해서 command list 를 생성하기 위해서는 command allocator 가 주어져야 한다. 하지만 ID3D12Device4::CreateCommandList1 함수를 사용하면 command allocator 가 주어지지 않아도 된다.

대신에 ID3D12Device4::CreateCommandList1 함수를 사용하면 close 상태의 command list 가 생성된다.

> Reference   
> [learn.microsoft - id3d12device-createcommandlist](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommandlist)  
> [learn.microsoft - id3d12device4-createcommandlist1](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device4-createcommandlist1)  
> [learn.microsoft - creating-command-lists](https://learn.microsoft.com/en-us/windows/win32/direct3d12/recording-command-lists-and-bundles#creating-command-lists)  

API 를 통해 객체를 생성할 때, enum D3D12_COMMAND_LIST_TYPE 을 통해 command list 의 타입을 결정한다. command list 의 타입은 direct command list, a bundle, a compute command list, or a copy command list 가 될 수 있다.

> Reference  
> [learn.microsoft - d3d12_command_list_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_command_list_type)  

</details>


<details> <summary> <h3 style="display:inline-block"> ID3D12GraphicsCommandList::Close 함수 </h3></summary>

command 추가를 완료했음을 나타내는 함수이다.

ID3D12CommandQueue::ExecuteCommandLists 함수에 인자로 ID3D12GraphicsCommandList 를 넘겨주기 전에 반드시 Close 함수를 호출해야 한다.

그리고 Close 함수를 호출한 command list 에 다시 command 를 기록하고 싶으면 반드시 Reset 함수를 호출해줘야 한다. 만약 Reset 함수를 호출하지 않고 command 를 기록하려고 한다면 다음 Debug Layrer 에서 다음 오류 메세지가 발생한다.
```
D3D12 ERROR: ID3D12GraphicsCommandList::*: This API cannot be called on a closed command list. [ EXECUTION ERROR #547: COMMAND_LIST_CLOSED]
```

> Reference  
> {cite}`Luna` p109  
> [learn.microsoft - id3d12graphicscommandlist-close](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-close)  

</details>


<details> <summary> <h3 style="display:inline-block"> ID3D12CommandList::Reset 함수 </h3></summary>

ID3D12CommandList::Reset 은 command list 를 방금 생성한 것과 동일한 상태로 만들어준다. 이로써 이전 command lists 에 할당된 memory 를 해제하고 새로운 memory 를 할당해주는 작업을 할 필요가 없이 allocator 를 재사용 할 수 있게 해준다.

함수의 인자로 주어지는 ID3D12CommandAllocator 객체를 생성할 떄 사용한 enum D3D12_COMMAND_LIST_TYPE 과 ID3D12CommandList 객체를 생성할 때 사용한 enum D3D12_COMMAND_LIST_TYPE 은 반드시 일치해야 한다.

Reset 함수를 호출하더라도 command queue 에 있는 command 들에는 아무런 영향이 없다. 왜냐하면 command 들이 저장되어 있는 command allocator 에는 여전히 command queue 가 참조하는 command 가 memroy 에 존재하기 때문이다.

ID3D12CommandList::Reset 은 호출하기 전에 반드시 ID3D12CommandList::Close 를 호출해줘야 한다. 그렇지 않으면 Debug Layer 에서 다음 오류 메세지가 발생한다.
```
D3D12 ERROR: ID3D12GraphicsCommandList::Reset: Reset fails because the command list was not closed. [ EXECUTION ERROR #544: COMMAND_LIST_OPEN]
```

> Reference  
> [learn.microsoft - nf-d3d12-id3d12graphicscommandlist-reset](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-reset)    
> [learn.microsoft - nf-d3d12-id3d12graphicscommandlist-reset#runtime-validation](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-reset#runtime-validation)  

</details>
