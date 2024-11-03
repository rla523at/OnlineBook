# Message

## PostMessageA 함수
PostMessageA 함수는 지정된 window 에 비동기적으로 메시지를 전달하는 함수다. 

이 함수는 호출한 스레드가 메시지 처리를 기다리지 않고 즉시 반환되므로, 비동기 작업에서 유용하다. 

`PostMessageA`는 ANSI 버전이므로, ANSI 문자 인코딩을 사용하여 메시지를 전달한다.

함수의 시그니처는 다음과 같다.
```cpp
BOOL PostMessageA(
  [in, optional] HWND   hWnd,
  [in]           UINT   Msg,
  [in]           WPARAM wParam,
  [in]           LPARAM lParam
);
```

인자는 다음과 같다.
* HWND hWnd
  * 메시지를 받을 window 의 핸들이다.
  * HWND_BROADCAST 를 사용하면 시스템의 모든 최상위 window 에 메세지가 전달된다.
    * 단, 메세지는 자식 window 에는 전달되지 않는다.
  * NULL 를 사용하면 현재 스레드에 식별자로 설정된 dwThreadID 매개 변수를 사용하여 PostThreadMessage 에 대한 호출처럼 동작한다.

* UINT Msg
  * 전달할 메시지의 식별자이다. 

* WPARAM wParam
  * 메시지에 대한 추가 정보다.

* LPARAM lParam
  * 메시지에 대한 추가 정보다.

반환값은 다음과 같다.
* 성공 시
  * `TRUE`를 반환한다.

* 실패 시
  * `FALSE`를 반환하며, 오류 원인은 GetLastError()를 호출하여 확인할 수 있다.
  
> Reference  
> [learn.microsoft - nf-winuser-postmessagea](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-postmessagea)  
