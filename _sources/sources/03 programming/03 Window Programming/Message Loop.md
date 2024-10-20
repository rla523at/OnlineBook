# Message Loop
Message Loop(메시지 루프)는 애플리케이션이 사용자 입력(키보드, 마우스) 및 시스템 메시지를 처리하는 루프이다. 메시지 루프는 애플리케이션이 실행되는 동안 지속적으로 실행되며, 메시지를 받아들이고 처리하는 역할을 한다.

일반적인 메시지 루프는 다음과 같은 구성 요소를 가지고 있다.

* 메시지 정보를 담는 구조체
* 메시지 큐에서 메시지를 가져오는 함수
* 키보드 메시지를 변환해서 텍스트 입력을 처리하는 함수
* 메시지를 윈도우 프로시저로 전달하는 함수

## 메시지 정보를 담는 구조체
메시지 정보를 담는 구조체는 MSG 구조체로 Windows API에서 사용되는 구조체다.

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

각 멤버 변수를 하나씩 살펴보자.

* hwnd
  * 타입: HWND
  * 설명: 메시지를 수신할 윈도우의 핸들이다. 이 핸들을 통해 어떤 윈도우가 메시지를 수신하는지 알 수 있다.
* message
  * 타입: UINT
  * 설명: 메시지 식별자이다. 메시지의 종류를 나타내며, WM_PAINT, WM_KEYDOWN 등의 값이 올 수 있다.
* wParam
  * 타입: WPARAM
  * 설명: 메시지의 추가 정보로 사용된다. 메시지에 따라 다양한 정보를 담을 수 있다.
  * 예: 키보드 메시지(WM_KEYDOWN)의 경우, 눌린 키의 코드 값이 들어 있다.
* lParam
  * 타입: LPARAM
  * 설명: 메시지의 추가 정보로 사용된다. 메시지에 따라 다양한 정보를 담을 수 있다.
  * 예: 마우스 메시지(WM_MOUSEMOVE)의 경우, 마우스 커서의 위치 좌표가 들어 있다.
* time
  * 타입: DWORD
  * 설명: 메시지가 생성된 시간을 밀리초 단위로 나타낸다. 시스템 시작 이후 경과된 시간을 기준으로 한다.
* pt
  * 타입: POINT
  * 설명: 메시지가 발생한 커서 위치를 나타낸다. POINT 구조체는 x와 y 좌표를 포함한다.
* lPrivate
  * 타입: DWORD
  * 설명: 내부적으로 사용되는 값으로, 애플리케이션에서 직접 사용할 필요는 없다. Windows 내부 구현에서 사용된다.


## 메시지 큐에서 메시지를 가져오는 함수
메시지 큐에서 메시지를 가져오는 함수로는 GetMessage와 PeekMessage 함수가 있다.

두 함수 모두 Windows 애플리케이션에서 메시지 루프를 구현하는 데 사용되지만, 동작 방식과 사용 목적에 따라 차이가 있다.

### GetMessage
GetMessage 함수는 Windows API에서 메시지 큐에서 메시지를 가져오는 함수다. 

이 함수는 메시지가 큐에 있을 때 해당 메시지를 반환하고, 메시지가 없으면 새로운 메시지가 도착할 때까지 대기한다.

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
PeekMessage 함수는 Windows 애플리케이션에서 메시지 큐에 있는 메시지를 확인하고, 필요에 따라 해당 메시지를 제거하는 데 사용된다. 

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

따라서, PeekMessage는 게임이나 애니메이션, 멀티미디어 애플리케이션 등에서 사용된다. 예를 들어, 게임에서는 프레임을 계속해서 그려야 하므로 PeekMessage를 사용하여 메시지가 없는 동안에도 게임 로직을 처리할 수 있다.

## 키보드 메시지를 변환해서 텍스트 입력을 처리하는 함수
TranslateMessage 함수는 Windows 애플리케이션에서 키보드 입력 메시지를 해석하여 문자 메시지로 변환하는 역할을 한다. 이 함수는 키보드 이벤트를 처리할 때, 키보드로 입력된 메시지를 더 높은 수준의 문자 메시지로 변환하여 애플리케이션이 더 쉽게 처리할 수 있도록 도와준다.

함수의 시그니쳐는 다음과 같다.

```cpp
BOOL TranslateMessage(
  const MSG *lpMsg
);
```

사용자가 키보드를 통해 'A' 키를 눌렀을 때의 메시지 처리 과정을 보면 다음과 같다.

* 사용자가 'A' 키를 누른다.
* 시스템은 WM_KEYDOWN 메시지를 생성하여 메시지 큐에 넣는다.
* GetMessage 혹은 PeekMessage 함수로 이 메시지를 가져온다.
* TranslateMessage 함수가 WM_KEYDOWN 메시지를 받아서 WM_CHAR 메시지로 변환한다. 이 떄, 변환된 WM_CHAR 메시지는 문자 'A'를 나타낸다.
* 변환된 WM_CHAR 메시지를 메시지 큐에 다시 넣는다.
* DispatchMessage 함수가 WM_CHAR 메시지를 가져와서 윈도우 프로시저로 전달한다.
* 윈도우 프로시저는 WM_CHAR 메시지를 처리하여 사용자 입력을 적절히 반영한다.

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
