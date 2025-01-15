# CPU GPU Interaction
그래픽 프로그래밍에는 CPU 와 GPU 라는 두개의 프로세서가 작동하며, 병렬적으로 작동하나 때로는 동기화가 필요하다.

따라서, 최대의 성능을 내기 위해서는 동기화를 최소화시켜 두개의 프로세서가 계속해서 동작하게 만들어줘야 한다. 왜냐하면, 동기화는 하나의 processor 가 다른 processor 를 기다리게 만들 수 있기 때문이다.

## CPU GPU Synchronization
CPU 와 GPU 라는 두개의 processor 가 병렬적으로 동작하기 때문에 다양한 synchronization 문제가 발생할 수 있다.

예를 들어, resource R 이 있다고 하자. CPU 에서 R 에 그리고자 하는 정보 I1 을 저장하고 R 을 참고하여 rendering 을 하라는 draw command 를 command list 에 넣은 뒤 command queue 에 전달했다고 하자. command queue 에 전달한 것은 CPU 를 block 시키지 않음으로 CPU 는 바로 다음 동작으로 새로운 정보 I2 를 R 에 저장하는 순간 문제가 발생할 수 있다. 만약에 GPU 가 다른 작업 때문에 draw command 를 아직 실행하기 전인데 CPU 에서 새로운 정보 I2 를 R 에 저장하는 순간 draw command 를 실행할 때 R 에는 이미 I1 에 대한 정보가 없기 때문이다.

### Soluition1 : Flushing the Command Queue
한가지 해결방법은 GPU 가 draw command 를 완료할 떄 까지 CPU 를 강제로 기다리게 하는 fence 를 만드는 방법이다. D3D12 에서 fence 는 ID3D12Fence interface 를 통해 나타내어지며 D3D12 에서 fence 의 동작 방식을 이해하기 위해 아래 예제 코드를 보자.

```cpp
UINT64 mCurrentFence = 0;
void D3DApp::FlushCommandQueue()
{
 // 업데이트 될 Fence 값 
  mCurrentFence++;
  
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

`mCommandQueue->Signal(mFence.Get(), mCurrentFence)` 코드에서 Signal 함수에 의해 mFence 객체가 관리하는 값을 mCurrentFence 라는 값으로 업데이트하는 command 가 Command Queue 에 추가하게 된다. 이 떄, 이 command 를 fence point 라고 하면 GPU 가 fence point 이전에 command 를 전부 실행하고 fence point 에 도달하는 순간 mFence 객체가 관리하는 값이 업데이트 된다.

이 떄, GPU 가 아직 Signal command 를 실행하기 전에 CPU 가 if 문을 실행했다고 가정해보자.

그러면 mFence 객체가 관리하는 값이 아직 업데이트가 되지 않아 if 문이 true 가 되게 된다. 그러면 Event 객체를 만들고 SetEventOnCompletion 함수를 호출해 mFence 객체가 관리하는 값이 SetEventOnCompletion 함수에 인자로 주어진 값이 되게 되면 eventHanlde 이 나타내는 event 객체를 신호 상태로 만들어주게 한다. 그리고 WaitForSingleObject 함수를 호출하여 eventHandle 이 나타내는 evenet 객체가 신호상태가 될 떄 까지 현재 Thread 를 block 상태로 만들어준다.

이후에 GPU 가 Signal command 를 실행하게 되면 evenet 객체가 신호상태가 되며 현재 Thread 는 CloseHandle 함수를 호출하고 FLushCommandQueue 함수를 탈출하게 된다.

이 떄, while loop 로 mFence 객체가 관리하는 값을 매번 확인하는 방식으로 대기 상태를 구현할 수도 있지만 WaitForSingleObject 함수를 사용하는 이유는 while 문을 사용할 경우 현재 thread 가 mFence 객체가 관리하는 값을 매번 확인하면서 CPU 자원을 낭비하게 되기 때문이다.

### Solution2 : Frame Resource
첫번째 해결방법은 잘 동작하지만 성능 측면에서 문제가 있다.

매 frame 마다 마지막에 Flushing the Command Queue 를 실행하게 되면 CPU 는 GPU 가 command queue 에 있는 모든 command 를 완료할 떄 까지 idle 상태로 대기하게 된다. 또한 매 frame 의 시작점에는 command queue 에 아무런 command 가 없게 됨으로 GPU 는 CPU 가 command 를 추가할 때 까지 idle 상태로 대기하게 된다.

따라서 이를 해결하기 위해서는 CPU 가 매 frame 마다 수정해야 되는 resource 를 n 개 frame 에 필요한 resource 크기를 갖는 circular resource 로 만든 뒤에 GPU 가 i 번째 resource 를 활용하여 rendering 하고 있을 때, CPU 는 i+1 번째 resource 를 업데이트 하는 방식을 사용하면 된다. 이런 방식을 frame resource 라고 한다.

일반적으로 n = 3 을 사용한다.

> Reference  
> {cite}`Luna` chpater 7.1  
