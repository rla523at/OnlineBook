# Message

<details> <summary> <h2 style="display:inline-block"> PostMessage vs SendMessage </h2></summary>

SendMessage 와 PostMessage 는 윈도우에 메시지를 전달하는 용도로 사용되는 함수이다. 이 때, 두 함수의 핵심적인 차이는 메시지 전달 방식과 동기/비동기적 동작에 있다.

1. **동기 vs 비동기**  
   - SendMessage: 동기적으로 동작한다. 함수를 호출한 스레드는 해당 메시지를 받은 window procedure 가 메시지를 처리할 때까지 대기한다. 즉, SendMessage 를 호출한 다음 메시지를 수신한 쪽의 처리 함수가 반환할 때까지 호출한 스레드는 Blocking 상태가 된다.  
   - PostMessage: 비동기적으로 동작한다. 메시지를 대상 윈도우의 메시지 큐에 단순히 등록해두고 PostMessage 함수는 즉시 반환된다. 메시지를 받은 측에서는 메시지 큐에서 메시지를 꺼내 순서에 따라 처리하므로, 호출한 스레드는 메시지 처리 완료를 기다리지 않고 바로 다음 코드로 진행할 수 있다.

2. **메시지 처리 순서와 방법**  
   - SendMessage: 메시지가 호출되면 대상 window procedure 는 곧바로 해당 메시지를 처리하게 된다. 처리 과정에서 UI가 잠시 멈추거나(Block) 다른 메시지 처리가 지연될 수 있으나, 호출한 쪽에서는 메시지 처리가 완료된 시점에 정확한 결과를 알 수 있다.  
   - PostMessage: 메시지를 메시지 큐에 넣어두기 때문에, 실제 메시지 처리는 Message Loop 에서 GetMessage 나 PeekMessage 로 해당 메시지를 꺼낼 때 수행됩니다. 따라서 호출 측에서 메시지 처리 완료 시점을 알 수 없고, 메시지를 받은 쪽에서 나중에 처리하게 된다.

> Reference   
> [learn.microsoft - nf-winuser-postmessagea](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-postmessagea)  
> [learn.microsoft - winuser-sendmessage](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-sendmessage)  

</details>
