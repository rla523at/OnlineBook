# Thread

Thread Stack Size
* https://learn.microsoft.com/en-us/windows/win32/procthread/thread-stack-size
* Each new thread or fiber receives its own stack space **consisting of both reserved and initially committed memory**.
* The reserved memory size represents the total stack allocation in virtual memory.

## _beginthreadex 
* https://learn.microsoft.com/en-us/cpp/c-runtime-library/reference/beginthread-beginthreadex?view=msvc-170
* STACK_SIZE_PARAM_IS_A_RESERVATION flag to use stack_size as the initial reserve size of the stack in bytes; if this flag isn't specified, stack_size specifies the commit size.

* https://learn.microsoft.com/en-us/windows/win32/procthread/creating-threads

Delegate
* https://www.codeproject.com/Articles/1170503/The-Impossibly-Fast-Cplusplus-Delegates-Fixed

WaitForMultipleObjectsEx
* https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitformultipleobjectsex
* bWaitAll
  * If this parameter is TRUE, the function returns when the state of all objects in the lpHandles array is set to signaled.
  *  If FALSE, the function returns when the state of any one of the objects is set to signaled. In the latter case, the return value indicates the object whose state caused the function to return.
    
ResetEvent
* https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-resetevent
* The ResetEvent function is used primarily for manual-reset event objects, which must be set explicitly to the nonsignaled state.
* Resetting an event that is already reset has no effect.

ManualReset Event
* 여러 스레드가 동시에 이 이벤트를 기다리고 있다면, 이벤트가 신호 상태가 될 때 모두 깨어날 수 있습니다. 
* https://learn.microsoft.com/ko-kr/windows/win32/api/synchapi/nf-synchapi-createeventa
  * If this parameter is FALSE, the function creates an auto-reset event object, and the system automatically resets the event state to nonsignaled after a single waiting thread has been released.

## WaitForSingleObjectEx

https://learn.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobjectex

Thread Handle 로 WiatForSingleObjectEx 함수를 호출할 때, 결과로 WAIT_ABANDONED 가 return 될 수 있는가?
* WAIT_ABANDONED는 뮤텍스 객체를 기다릴 때만 반환되는 값입니다.
* 스레드 핸들(thread handle)에 대해 WaitForSingleObjectEx(또는 WaitForSingleObject)를 호출하면 다음 중 하나만 반환됩니다. 
 * WAIT_OBJECT_0: 스레드가 종료되어 객체가 시그널 상태가 된 경우
 * WAIT_TIMEOUT: 지정한 시간 동안 아직 시그널 상태가 아닌 경우
 * WAIT_FAILED: 호출 자체에 실패한 경우 (GetLastError()로 상세 원인 확인)

### bAlertable
bAlertable을 TRUE로 주면, 대기 중에 I/O 완료 루틴(Completion Routine) 또는 사용자 APC(Asynchronous Procedure Call)를 처리할 수 있는 “Alertable 대기” 상태가 됩니다.

예시 1. ReadFileEx와 I/O 완료 루틴 처리
```cpp
// 1) Overlapped I/O 요청
OVERLAPPED ov = {};
ov.hEvent = CreateEvent(nullptr, FALSE, FALSE, nullptr);

ReadFileEx(
    hFile,            // 파일 핸들
    buffer,           // 버퍼
    bufferSize,       // 읽을 바이트 수
    &ov,              // Overlapped 구조체
    FileIOCompletion  // I/O 완료 시 호출될 콜백
);

// 2) Alertable 대기
DWORD result = WaitForSingleObjectEx(
    ov.hEvent,        // 이벤트 객체 핸들
    INFINITE,         // 무한 대기
    TRUE              // Alertable = TRUE: 대기 중에 콜백 실행 허용
);

// FileIOCompletion 콜백은 WaitForSingleObjectEx가 반환되기 전에 실행됨
```

* ReadFileEx → I/O 요청을 비동기로 발생시키고, 완료 시 FileIOCompletion이 큐에 들어감
* WaitForSingleObjectEx(..., TRUE)이 실행되면,
* I/O 완료 시점에 ov.hEvent가 시그널 상태가 되고
* 그 동시에 FileIOCompletion 콜백이 호출된 뒤에 함수가 반환된다

예시 2. QueueUserAPC로 사용자 APC 처리
```cpp
// 1) 타깃 스레드에 APC 큐잉
QueueUserAPC(
    MyAPCFunction,    // 실행할 함수 포인터
    hTargetThread,    // 대상 스레드 핸들
    customData        // MyAPCFunction에 전달할 인자
);

// 2) Alertable 대기
DWORD result = WaitForSingleObjectEx(
    hMyEvent,         // 스레드가 신호 대기할 이벤트
    INFINITE,
    TRUE              // TRUE로 주어야 MyAPCFunction이 호출됨
);

// result == WAIT_IO_COMPLETION 이면, 대기 중 APC가 실행된 것
```

* QueueUserAPC 호출만으로 “대상 스레드의 APC 큐”에 MyAPCFunction(pContext)가 등록됩니다.
* 스레드가 WaitForSingleObjectEx(hEvent, INFINITE, TRUE) 같은 Alertable 대기를 시작
* 큐에 등록된 APC가 하나라도 있으면 MyAPCFunction(pContext)를 호출
* 호출이 완료되면 WAIT_IO_COMPLETION을 반환
