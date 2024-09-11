# ID3D12CommandQueue

ID3D12CommandQueue 는 GPU 가 실행할 CommandList 을 제출하는 역할을 한다. 

이 클래스는 ID3D12Device 를 사용하여 생성된다. 

명령 큐를 생성할 때는 D3D12_COMMAND_QUEUE_DESC 구조체를 사용하여 명령 큐의 특성(예: 우선순위, CommandList 유형 등)을 정의한다. 명령 큐의 유형은 그래픽 작업, 복사 작업, 컴퓨트 작업 등의 세 가지로 구분되며, 이를 통해 다양한 GPU 작업을 처리할 수 있다.

ExecuteCommandLists 메서드를 통해 준비된 CommandList을 명령 큐에 제출하고, GPU는 이를 실행하게 된다. 

또한, ID3D12CommandQueue는 GPU와 CPU 사이의 동기화를 관리하는데, 이를 위해 Signal과 Wait 메서드를 사용하여 GPU의 작업 완료 여부를 확인하거나 대기할 수 있다.

## D3D12_COMMAND_QUEUE_DESC 구조체

D3D12_COMMAND_QUEUE_DESC 구조체는 Direct3D 12에서 Command Queue 의 특성을 정의하기 위한 구조체이다. 

정의는 다음과 같다.

```cpp
typedef struct D3D12_COMMAND_QUEUE_DESC {
    D3D12_COMMAND_LIST_TYPE Type;
    INT Priority;
    D3D12_COMMAND_QUEUE_FLAGS Flags;
    UINT NodeMask;
} D3D12_COMMAND_QUEUE_DESC;
```

각각의 멤버변수는 다음과 같다.

* D3D12_COMMAND_LIST_TYPE Type
  * 명령 큐에서 실행할 명령 리스트의 유형을 지정한다.
  * enum D3D12_COMMAND_LIST_TYPE 값을 사용한다.
  * 기본값은 D3D12_COMMAND_LIST_TYPE_DIRECT이다.

* INT Priority
  * 명령 큐의 우선 순위를 지정한다. 우선 순위가 높을수록 더 자주 실행된다.
  * 가능한 값
    * D3D12_COMMAND_QUEUE_PRIORITY_NORMAL: 표준 우선 순위
    * D3D12_COMMAND_QUEUE_PRIORITY_HIGH: 높은 우선 순위
  * 기본값은 D3D12_COMMAND_QUEUE_PRIORITY_NORMAL이다.

* D3D12_COMMAND_QUEUE_FLAGS Flags
  * 명령 큐의 플래그를 지정한다.
  * 가능한 값
    * D3D12_COMMAND_QUEUE_FLAG_NONE: 특별한 플래그 없음
    * D3D12_COMMAND_QUEUE_FLAG_DISABLE_GPU_TIMEOUT: GPU 시간 제한 비활성화
  * 기본값은 D3D12_COMMAND_QUEUE_FLAG_NONE이다.

* UINT NodeMask
  * 명령 큐가 실행될 노드를 지정한다. 멀티 어댑터 환경에서 사용된다.
  * 가능한 값
    * 0: 단일 GPU 설정에서 기본값으로 사용
    * 멀티 어댑터 환경에서는 각 GPU를 식별하는 비트 마스크를 사용
  * 기본값은 0이다.
