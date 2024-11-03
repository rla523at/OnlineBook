# Message Loop
Message Loop 는 애플리케이션이 사용자 입력(키보드, 마우스) 및 시스템 메시지를 처리하는 루프이다. 

Message Loop 는 애플리케이션이 실행되는 동안 지속적으로 실행되며, 메시지를 받아들이고 처리하는 역할을 한다.

WIN 32 API 기반의 Message Loop 는 다음과 같은 구성 요소를 가지고 있다.

* 메시지 정보를 담는 구조체
* 메시지 큐에서 메시지를 가져오는 함수
* 키보드 메시지를 변환해서 텍스트 입력을 처리하는 함수
* 메시지를 윈도우 프로시저로 전달하는 함수

## 메시지 정보를 담는 구조체 
메시지 정보를 담는 구조체는 MSG 구조체로 Win32 API 에서 사용되는 구조체다.

MSG 구조체의 정의는 다음과 같다.

```cpp
typedef struct tagMSG {
  HWND   hwnd;      // 메시지를 수신할 윈도우의 핸들
  UINT   message;   // 메시지 식별자
  WPARAM wParam;    // 메시지의 추가 정보 (첫 번째 매개변수)
  LPARAM lParam;    // 메시지의 추가 정보 (두 번째 매개변수)
  DWORD  time;      // 메시지가 생성된 시간
  POINT  pt;        // 메시지가 발생한 커서 위치
#ifdef _MAC
  DWORD  lPrivate;  // 내부적으로 사용되는 값
#endif
} MSG, *PMSG;
```

멤버 변수는 다음과 같다.

* HWND hwnd  
  * 메시지가 전달될 윈도우의 핸들을 나타낸다.
  * 특정 윈도우로 전달되는 메시지가 아닌 경우 NULL로 설정될 수 있다.

* UINT message  
  * 처리할 메시지의 유형을 나타낸다

* WPARAM wParam  
  * 메시지에 대한 추가 정보를 제공하는 매개 변수다.

* LPARAM lParam  
  * 메시지에 대한 추가 정보를 제공하는 또 다른 매개 변수다.

* DWORD time  
  * 메시지가 생성된 시간을 밀리초 단위로 나타낸다. 

* POINT pt  
  * 메시지가 생성될 당시의 커서 위치를 화면 좌표로 나타낸다.
  * POINT 구조체는 x와 y 좌표를 포함한다.

> Reference  
> [learn.microsoft - ns-winuser-msg](https://learn.microsoft.com/en-us/windows/win32/api/winuser/ns-winuser-msg)  

### 메세지 유형
* WM_CREATE
  * 애플리케이션이 CreateWindowEx 또는 CreateWindow 함수를 호출하면 해당 작업을 요청한 프로그램으로 전달되는 메시지이다.
  * 이 메시지는 윈도우를 생성하는 함수가 끝나기 전에 해당 프로그램의 메시지 큐에 전달된다.
  * 즉, 윈도우를 생성하는 함수에서 WM_CREATE 메시지를 해당 작업을 요청한 프로그램의 메시지 큐에 전달한다.
  * window procedure 에서는 이 메세지에 대한 동작으로 주로 window 를 생성한 후 수행되어야 할 초기화 코드를 작성한다.
 
> Reference  
> [blog.naver.com/tipsware - WM_CREATE 메시지에 대하여](https://blog.naver.com/tipsware/221123732393)  

* WM_COMMAND
  * 대화상자나 윈도우에 만들어진 버튼 같은 컨트롤을 누르거나 리소스에 등록된 단축키 (accelerator) 를 사용하거나 또는 메뉴에서 항목을 선택할 때 전송되는 메세지이다
  * 다양한 상황에서 발생하기 때문에 함께 전달되는 wParam 과 lParam 에 저장된 정보로 구분해야 한다.
    * lParam 에는 control 의 핸들 값이 저장된다.
    * 즉, 버튼과 같은 컨트롤을 누를 경우 lParam 에 control 의 HWND 값이 저장되어 있다.
    * 하지만 단축키나 메뉴처럼 컨트롤이 아닌 경우에는 lParam 에 NULL 이 저장된다.

> Reference
> [blog.naver.com/tipsware - WM_COMMAND 메시지에 대하여](https://m.blog.naver.com/tipsware/221551586185)  
> [learn.microsoft - wm-command](https://learn.microsoft.com/ko-kr/windows/win32/menurc/wm-command)  

* WM_MOVE
  * window 를 이동한 후에 발생하는 메세지이다.
 
> Reference  
> [learn.microsoft - wm-move](https://learn.microsoft.com/ko-kr/windows/win32/winmsg/wm-move)

* WM_SIZE
  * window 의 크기를 변경한 후에 발생하는 메세지이다.

> Reference  
> [learn.microsoft - wm-size](https://learn.microsoft.com/ko-kr/windows/win32/winmsg/wm-size)

* WM_TIMER
  * SetTimer 함수에 의해 생성된 타이머가 만료될 때마다 발생되는 메세지이다.
  * 이 메시지는 일정 간격으로 반복 작업을 수행하거나 지연 작업을 실행할 때 사용된다.
 
> Reference
> [learn.microsoft - wm-timer](https://learn.microsoft.com/en-us/windows/win32/winmsg/wm-timer)  

* WM_KILLFOCUS
  * window 가 focus 를 잃어 더 이상 키보드 입력을 받을 수 없게 될 때 발생하는 메시지다.

> Reference  
> [learn.microsoft - wm-killfocus](https://learn.microsoft.com/ko-kr/windows/win32/inputdev/wm-killfocus)  

* WM_SETFOCUS
  * window 가 focus 를 획득해 키보드 입력을 받을 수 있게 될 때 발생하는 메시지다.
 
> Reference  
> [learn.microsoft - wm-setfocus](https://learn.microsoft.com/ko-kr/windows/win32/inputdev/wm-setfocus)  

* WM_PAINT
  * window 가 화면에 클라이언트 영역을 다시 그려야 할 때 발생하는 메시지다.
    * windows 운영체제는 window 의 기본적인 동작을 관리하기 위해 일반적으로 window 의 특정 영역에만 그림을 그리도록 허용하며 이 영역을 client area 라고 한다.
    * 윈도우의 제목이 표시되는 캡션 (타티을바) 영역이나 윈도우의 테두리를 표시하는 영역은 기본적으로 운영체제가 관리한다.
  * 이 메시지는 시스템이 윈도우의 내용을 새로 그려야 할 필요가 있다고 판단하는 경우, 예를 들어 다른 윈도우가 해당 윈도우를 가렸다가 다시 보이게 되거나, 창 크기가 변경될 때 전송된다.
  * windows 운영체제는 무효화된 영역이 유효 영역으로 변경되는 경우, 응용 프로그램이 스스로 지워졌떤 그림을 복구할 수 있도록 복구가 필요한 영역의 정보를 담은 WM_PAINT 메시지를 해당 응용 프로그램에 전달한다.
  * hWnd
    * hwnd 는 운영체제의 커널 오브젝트를 사용하기 위해 필요한 장치이다.
    * 프로그램으로 만든 window 는 고유 식별 id 가 있고, 이런 식별 id 를 hwnd 로 관리한다.
    * window 는 운영체제가 관리하는 객체이기 때문에 메모리에 잡히기는 했지만 함부로 접근할 수 없다.
    * 따라서 hwnd 라는 id 를 통해서 윈도우를 간접적으로 다뤄야 한다.
  * Handle Device Context(HDC)
    * hdc 는 렌더링과 관련된 동작을 할 때 필요한 커널 오브젝트를 다룰 수 있게 해준다.
  * WM_PAINT 메세지에 대한 처리 작업을 중복해서 이루어지면 안되기 때문에 WM_PAINT 메세지를 직접 처리했다면 DefWindowProc 함수가 호출되지 않도록 처리해줘야 한다.
    * 일반적으로 return 0 을해서 WM_PAINT 를 직접 처리했다는 뜻으로 사용한다.
  * WM_PAINT 메시지는 메시지 테이블에서 자신의 상태 값이 1이면 메세지가 발생된다.
  * 그리고 BeginPaint 함수에서 WM_PAINT 메세지의 상태 값을 1에서 0으로 변경하는 작업을 해준다.
  * 하지만 WM_PAINT 를 처리하는 로직에서 BeginPaint 함수 대신에 GetDC 함수를 사용하면 메시지 테이블에서 WM_PAINT 메세지 상태가 1에서 0으로 변경되지 않는다. 따라서 WM_PAINT 메시지 처리가 완료 되더라도 계속해서 WM_PAINT 메세지가 발생하게 된다.
  

> Reference  
> [blog.naver.com/tipsware - WM_PAINT 메시지에 대하여](https://blog.naver.com/tipsware/221119932350)   
> [powerclabman.tistory - hWnd 윈도우 핸들, hDC](https://powerclabman.tistory.com/77)  
> [learn.microsoft - wm-paint](https://learn.microsoft.com/ko-kr/windows/win32/gdi/wm-paint)  

* WM_DESTROY
  * 윈도우가 파괴(destroy)될 때 전송되는 메시지다.
    * WM_CLOSE 메시지를 별도로 처리하지 않음녀 DefWindowProc 은 DestroyWindow 함수를 호출하여 윈도우를 파괴한다.
    * 이 때 WM_DESTROY 메세지가 발생한다.
    * 주로 열어 놓은 파일을 닫고 할당한 메모리를 해제하는 등의 정리작업을 한다.
  

> Reference   
> [learn.microsoft - wm-destroy](https://learn.microsoft.com/ko-kr/windows/win32/winmsg/wm-destroy)  
> [bstyle.tistory - WM_CLOSE,WM_DESTORY_WM_QUIT 차이점](https://bstyle.tistory.com/38)  

* WM_USER
  * 사용자 정의 메시지의 시작 값을 나타내는 상수이다.
  * 주로 특정 응용 프로그램이나 클래스에서만 사용하는 맞춤형 메시지를 정의할 때 사용된다.
  * 기본적으로 Windows는 많은 시스템 메시지를 정의하고 있는데, 이들은 특정 숫자 상수로 표현된다.
  * 시스템에서 제공하는 메시지만으로는 사용자 요구를 모두 만족시킬 수 없기 때문에 사용자가 직접 메시지를 만들어 추가하는 경우가 있다.
  * 이때 사용자 정의 메시지의 번호를 정하기 위해 WM_USER를 사용한다.
  * 사용자 정의 메세지의 번호를 정했다면 SendMessage 또는 PostMessage 함수를 사용하여 특정 윈도우에 보낼 수 있다.
  * 그리고 window procedure 에서 이 메시지를 식별하고 처리할 수 있다.

> Reference  
> [learn.microsoft - wm-user](https://learn.microsoft.com/ko-kr/windows/win32/winmsg/wm-user)  

## 메시지 큐에서 메시지를 가져오는 함수
Win32 API 에서 메시지 큐에서 메시지를 가져오는 함수로는 GetMessage와 PeekMessage 함수가 있다.

### GetMessage
GetMessage 함수는 메시지가 큐에 있을 때 해당 메시지를 반환하고, 메시지가 없으면 새로운 메시지가 도착할 때까지 대기한다.

GetMessage 함수의 시그니쳐는 다음과 같다.
```cpp
BOOL GetMessage(
  LPMSG lpMsg,
  HWND hWnd,
  UINT wMsgFilterMin,
  UINT wMsgFilterMax
);
```

각 매개 변수는 다음과 같다.

* lpMsg
  * 메시지를 받을 MSG 구조체에 대한 포인터다.
  * 이 구조체에 메시지 정보가 채워진다.
* hWnd
  * 메시지를 가져올 창의 핸들이다. 
  * 특정 창의 메시지만 가져오려면 그 창의 핸들을 지정하고, 모든 창의 메시지를 가져오려면 NULL을 지정한다.
* wMsgFilterMin
  * 처리할 메시지 범위의 최솟값이다.
  * 특정 메시지 범위를 지정하려면 해당 메시지의 시작 범위를 지정하고, 모든 메시지를 처리하려면 0을 지정한다.
* wMsgFilterMax
  * 처리할 메시지 범위의 최댓값이다. 
  * 특정 메시지 범위를 지정하려면 해당 메시지의 끝 범위를 지정하고, 모든 메시지를 처리하려면 0을 지정한다.

반환 값은 다음과 같다.
* TRUE
  * WM_QUIT 메시지를 제외한 모든 메시지를 성공적으로 가져왔을 때 반환된다.
* FALSE
  * WM_QUIT 메시지를 가져왔을 때 반환된다.

이를 이용한 간단한 message loop는 다음과 같다.

```cpp
MSG msg;
while (GetMessage(&msg, NULL, 0, 0)) {
    TranslateMessage(&msg);
    DispatchMessage(&msg);
}
```

### PeekMessage
PeekMessage 함수는 메시지 큐에 있는 메시지를 확인하고, 필요에 따라 해당 메시지를 제거하는 데 사용된다. 

PeekMessage는 메시지가 있을 때만 메시지를 처리하고, 메시지가 없으면 바로 FALSE를 반환하여 애플리케이션이 다른 작업을 계속할 수 있게 한다.

PeekMessage 함수의 시그니쳐는 다음과 같습니다.

```cpp
BOOL PeekMessage(
  LPMSG lpMsg,
  HWND hWnd,
  UINT wMsgFilterMin,
  UINT wMsgFilterMax,
  UINT wRemoveMsg
);
```
각 매개 변수는 다음과 같다.

* lpMsg
  * 메시지를 받을 MSG 구조체에 대한 포인터다.
* hWnd
  * 메시지를 검색할 창의 핸들이다. 
  * 특정 창의 메시지를 확인하려면 해당 창의 핸들을 전달하고, 모든 창의 메시지를 확인하려면 NULL을 전달한다.
* wMsgFilterMin
  * 처리할 메시지 범위의 최솟값이다.
  * 특정 메시지 범위를 지정하려면 해당 메시지의 시작 범위를 지정하고, 모든 메시지를 처리하려면 0을 지정한다.
* wMsgFilterMax
  * 처리할 메시지 범위의 최댓값이다. 
  * 특정 메시지 범위를 지정하려면 해당 메시지의 끝 범위를 지정하고, 모든 메시지를 처리하려면 0을 지정한다.
* wRemoveMsg
  * 메시지를 메시지 큐에서 제거할지 여부를 지정한다
  * PM_NOREMOVE: 메시지를 큐에서 제거하지 않고 확인만 한다.
  * PM_REMOVE: 메시지를 큐에서 제거한다.

반환 값은 메세지가 있으면 TRUE, 없으면 FALSE를 반환한다.

이런 반환 값 특성을 이용해 비동기적으로 메시지를 처리하려면 다음과 같이 하면 된다.

```cpp
MSG msg = {0};
while (WM_QUIT != msg.message) {
    if (PeekMessage(&msg, NULL, 0, 0, PM_REMOVE)) 
    {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
      // 메시지가 없을 때 수행할 작업
    }    
}
```

따라서, PeekMessage는 게임이나 애니메이션, 멀티미디어 애플리케이션 등에서 사용된다. 

예를 들어, 게임에서는 프레임을 계속해서 그려야 하므로 PeekMessage를 사용하여 메시지가 없는 동안에도 게임 로직을 처리할 수 있다.

## 키보드 메시지를 변환해서 텍스트 입력을 처리하는 함수
TranslateMessage 함수는 키보드 입력 메시지를 해석하여 텍스트 입력 정보를 갖는 새로운 메시지를 추가하는 역할을 한다.

WM_KEYDOWN 및 WM_KEYUP 조합은 WM_CHAR 또는 WM_DEADCHAR 메시지를 생성하고 WM_SYSKEYDOWN 및 WM_SYSKEYUP 조합은 WM_SYSCHAR 또는 WM_SYSDEADCHAR 메시지를 생성한다.

따라서, WM_CHAR, WM_SYSCHAR 등의 message 를 처리할 것이 아니라면 message loop 에서 TranslateMessage 를 호출할 필요가 없다.

함수의 시그니쳐는 다음과 같다.

```cpp
BOOL TranslateMessage(
  const MSG *lpMsg
);
```

사용자가 키보드를 통해 'A' 키를 눌렀을 때의 TranslateMessage 가 있는 경우 메시지 처리 과정을 보면 다음과 같다.

* 사용자가 'A' 키를 누른다.
* 시스템은 WM_KEYDOWN 메시지를 생성하여 메시지 큐에 넣는다.
* GetMessage 혹은 PeekMessage 함수로 이 메시지를 가져온다.
* TranslateMessage 함수가 WM_KEYDOWN 메시지를 받으면 WM_CHAR 메시지를 생성한다. 이 떄, WM_CHAR 메시지는 문자 'A'를 나타낸다.
* 생성된 WM_CHAR 메시지를 메시지 큐에 다시 넣는다.
* DispatchMessage 함수가 WM_CHAR 메시지를 가져와서 윈도우 프로시저로 전달한다.
* 윈도우 프로시저는 WM_CHAR 메시지를 처리하여 사용자 입력을 적절히 반영한다.

> Reference  
> [learn.microsoft - nf-winuser-translatemessage)](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-translatemessage)  
> [learn.microsoft - win32/inputdev/wm-char](https://learn.microsoft.com/ko-kr/windows/win32/inputdev/wm-char)  

## 메시지를 윈도우 프로시저로 전달하는 함수
DispatchMessage 함수는 Windows 애플리케이션에서 메시지 큐에 있는 메시지를 해당 윈도우의 윈도우 프로시저로 전달하는 역할을 한다. 

이 함수는 GetMessage나 PeekMessage 함수를 통해 얻은 메시지를 처리하는 데 사용된다.

함수의 시그니쳐는 다음과같다.

```cpp
LRESULT DispatchMessage(
  const MSG *lpMsg
);
```

매개변수 lpmsg는 처리할 메시지를 포함하는 MSG 구조체에 대한 포인터다. 

반환 값은 윈도우 프로시저가 반환하는 결과 값인데 대부분의 경우, 이 값은 무시된다.
