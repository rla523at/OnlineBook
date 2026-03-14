# Window Class
Windows 프로그래밍에서 Window Class 는 window가 어떻게 동작하고 생김새가 어때야 되는지를 나타내는 개념이다.

각 창 클래스에는 같은 클래스의 모든 창이 공유하는 연결된 창 프로시저가 있다.

> Reference  
> [learn.microsoft - window-classes](https://learn.microsoft.com/en-us/windows/win32/winmsg/window-classes)

## WNDCLASSEXA 구조체
WNDCLASSEX 구조체는 Windows API 에서 window class 를 정의하는 데 사용되는 구조체다.

정의는 다음과 같다.

```cpp
typedef struct tagWNDCLASSEXA {
  UINT      cbSize;
  UINT      style;
  WNDPROC   lpfnWndProc;
  int       cbClsExtra;
  int       cbWndExtra;
  HINSTANCE hInstance;
  HICON     hIcon;
  HCURSOR   hCursor;
  HBRUSH    hbrBackground;
  LPCSTR    lpszMenuName;
  LPCSTR    lpszClassName;
  HICON     hIconSm;
} WNDCLASSEXA, *PWNDCLASSEXA, *NPWNDCLASSEXA, *LPWNDCLASSEXA;
```

멤버변수는 다음과 같다.

* UINT cbSize
  * 구조체의 크기를 나타낸다.
  * 이 값은 반드시 `sizeof(WNDCLASSEXA)`로 설정해야 한다.

* UINT style
  * 윈도우 클래스의 스타일을 지정하는 플래그 값이다.
  * [learn.microsoft - window-class-styles](https://learn.microsoft.com/ko-kr/windows/win32/winmsg/window-class-styles)  

* WNDPROC lpfnWndProc
  * 이 클래스의 모든 윈도우가 공유할 window procedure 에 대한 함수 포인터이다.

* int cbClsExtra
  * 클래스에 추가로 할당할 바이트 수를 지정한다.
  * 대부분의 경우 0으로 설정된다.

* int cbWndExtra
  * 윈도우 인스턴스에 추가로 할당할 바이트 수를 지정한다. 대부분의 경우 0으로 설정된다.

* HINSTANCE hInstance
  * 이 클래스가 속한 애플리케이션의 인스턴스 핸들을 나타낸다.
  * 애플리케이션의 실행 시 인스턴스를 구분하는 값이다.
  * GetModuleHandle 함수에 NULL을 전달하면, 현재 실행 중인 프로세스의 인스턴스 핸들을 반환한다.

* HICON hIcon
  * 윈도우에 표시할 기본 아이콘 핸들이다.
  * LoadIcon 함수를 사용해 아이콘을 로드하여 설정할 수 있다.
  * hIcon에 NULL 값을 주면, 윈도우는 기본 시스템 아이콘을 사용하게 된다.

* HCURSOR hCursor
  * 윈도우에 표시할 기본 커서 핸들이다.
  * LoadCursor 함수를 사용해 커서를 설정할 수 있다.
  *  NULL 인 경우 마우스가 애플리케이션의 window 로 이동할 때마다 애플리케이션에서 커서 셰이프를 명시적으로 설정해야 한다

* HBRUSH hbrBackground
  * 배경을 그리는 데 사용할 브러시에 대한 핸들이거나 색 값일 수 있다. 
  * NULL인 경우 애플리케이션은 클라이언트 영역에서 그리기를 요청할 때마다 자체 배경을 그려야한다.

* LPCSTR lpszMenuName
  * 클래스에서 사용할 메뉴의 리소스 이름을 지정하는 널 종료된 문자열이다.
  * 메뉴가 필요 없을 경우 NULL로 설정할 수 있다.

* LPCSTR lpszClassName
  * window class 의 이름을 나타내는 널 종료된 문자열이다.
  * 이 이름은 window 를 생성할 때 사용된다.

* HICON hIconSm
  * 윈도우의 작은 아이콘 핸들이다.
  * 이 아이콘은 작업 표시줄이나 Alt-Tab 창에서 사용된다.

> REFERENCE   
> [learn.microsoft - ns-winuser-wndclassexa](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/ns-winuser-wndclassexa)   

## Window Procedure
window class 에 속하는 모든 window 에서 보내거나 게시된 메세지를 처리하는 함수이다.

window procedure 은 window class 와 1:1 로 연결되어 있다.

window 의 모양과 동작은 메시지에 대한 window procedure 의 응답에 따라 달라진다.

> Reference  
> [learn.microsoft - window-procedures](https://learn.microsoft.com/en-us/windows/win32/winmsg/window-procedures)

### 호출과정
* 윈도우가 생성되고 나면, 메시지 루프를 통해 메시지를 대기하고 처리하게 된다.
* message loop 에서 DispatchMessage 함수는 메시지를 해당 window 의 window procedure 로 전달한다.

### DefWindowProc 함수
`DefWindowProc` 함수는 window procedure 에서 처리되지 않은 메시지를 default window procedure 로 전달하여 Windows가 기본 처리를 수행하도록 하는 함수이다. 

이 함수는 사용자 정의 window procedure 에서 호출되며, window 가 기본적으로 처리하는 메시지에 대해 표준 동작을 제공한다.

함수의 시그니처는 다음과 같다.
```cpp
LRESULT DefWindowProc(
  HWND   hWnd,
  UINT   Msg,
  WPARAM wParam,
  LPARAM lParam
);
```

인자는 다음과 같다.
* HWND hWnd
  * 메시지를 처리할 창의 핸들이다.

* UINT Msg
  * 처리할 메시지의 식별자다. 

* WPARAM wParam
  * 메시지에 대한 추가 정보로, 메시지에 따라 의미가 달라진다. 

* LPARAM lParam
  * 메시지에 대한 추가 정보로, 메시지에 따라 의미가 달라진다. 

반환값은 다음과 같다.
* 메시지에 대한 처리 결과를 반환하며, 반환값은 메시지와 상황에 따라 달라진다.

> Reference  
> [learn.microsoft - nf-winuser-defwindowprocw](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-defwindowprocw)  

## RegisterClassExA 함수
RegisterClassExA 함수는 WNDCLASSEX 구조체에 정의된 윈도우 클래스를 등록하는 데 사용된다. 이 함수는 애플리케이션이 해당 클래스를 기반으로 윈도우를 생성할 수 있도록 윈도우 클래스를 시스템에 등록한다. RegisterClassEx는 RegisterClass 함수의 확장 버전으로, 추가적인 멤버와 기능을 제공한다.

RegisterClassEx 함수는 등록이 성공하면 반환값으로 고유한 클래스 원자(ATOM)를 반환한다. 이 값은 이후에 윈도우를 생성할 때 클래스 이름 대신 사용할 수 있다. 만약 등록이 실패하면, 0을 반환한다. 이 경우 GetLastError 함수를 호출하여 확장된 오류 정보를 얻을 수 있다.



RegisterClassExA 함수는 window class 를 등록하여 해당 클래스를 사용하는 window 을 생성할 수 있도록 하는 함수이다. 

이 함수는 ANSI 버전이며, window 을 생성하는 데 필요한 정보를 포함하는 `WNDCLASSEXA` 구조체에 정의된 window class 를 등록한다.

함수의 시그니처는 다음과 같다.
```cpp
ATOM RegisterClassExA(
  [in] const WNDCLASSEXA *unnamedParam1
);
```

인자는 다음과 같다.
* const WNDCLASSEXA *unnamedParam1
  * 등록할 창 클래스에 대한 정보를 포함하는 `WNDCLASSEXA` 구조체의 포인터다.

반환값은 다음과 같다.
* 성공 시
  * 고유한 창 클래스 식별자(ATOM)를 반환한다.
  * 이 값을 사용하여 window 을 생성할 때 클래스 식별자로 사용할 수 있다.
    * 단, MAKEINTATOM 매크로와 함께 사용되어야 한다. 
    * [stackoverflow - why-doesnt-createwindoww-accept-an-atom-as-the-lpclassname](https://stackoverflow.com/questions/67202936/why-doesnt-createwindoww-accept-an-atom-as-the-lpclassname)  

* 실패 시
  * 0을 반환한다.
  * GetLastError()를 호출해 실패 원인을 확인할 수 있다. 

> REFERENCE  
> [learn.microsoft - nf-winuser-registerclassexa](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-registerclassexa)


## CreateWindow 함수
CreateWindowExA 함수는 확장된 창 스타일을 사용하여 새로운 윈도우 창을 생성하는 함수이다.

이 함수는 ANSI 버전이며, 애플리케이션이 window 을 만들고, 메시지를 처리하는 시작 지점이 된다.

함수의 시그니처는 다음과 같다.
```cpp
HWND CreateWindowExA(
  DWORD     dwExStyle,
  LPCSTR    lpClassName,
  LPCSTR    lpWindowName,
  DWORD     dwStyle,
  int       X,
  int       Y,
  int       nWidth,
  int       nHeight,
  HWND      hWndParent,
  HMENU     hMenu,
  HINSTANCE hInstance,
  LPVOID    lpParam
);
```

인자는 다음과 같다.
* DWORD dwExStyle
  * 창의 확장 스타일을 지정하는 값이다.
  * 0 일 경우 WS_EX_LTRREADING 이다.
  * [learn.microsoft - extended-window-styles](https://learn.microsoft.com/en-us/windows/win32/winmsg/extended-window-styles)  

* LPCSTR lpClassName
  * 창의 클래스 이름을 나타내는 문자열이거나, `RegisterClassExA`에서 반환된 ATOM 값을 사용할 수 있다.
  * ATOM 값을 사용할 경우 MAKEINTATOM 매크로와 함께 사용되어야 한다. 
    * [stackoverflow - why-doesnt-createwindoww-accept-an-atom-as-the-lpclassname](https://stackoverflow.com/questions/67202936/why-doesnt-createwindoww-accept-an-atom-as-the-lpclassname)  

* LPCSTR lpWindowName
  * 창의 제목 표시줄에 나타날 텍스트를 지정하는 문자열이다.

* DWORD dwStyle
  * 창의 스타일을 지정하는 값이다.
  * [learn.microsoft - window-styles](https://learn.microsoft.com/en-us/windows/win32/winmsg/window-styles)  

* int X
  * 창의 초기 위치를 지정하는 X 좌표이다.

* int Y
  * 창의 초기 위치를 지정하는 Y 좌표이다.

* int nWidth
  * 창의 초기 너비를 지정하는 값이다.

* int nHeight
  * 창의 초기 높이를 지정하는 값이다.

* HWND hWndParent
  * 부모 창의 핸들이다. 자식 창을 만들 때 사용되며, 부모 창이 없으면 NULL을 전달할 수 있다.

* HMENU hMenu
  * 창에 사용할 메뉴의 핸들이다. 자식 창일 경우 이 값은 창의 ID로 사용된다.

* HINSTANCE hInstance
  * 창을 생성하는 애플리케이션 인스턴스의 핸들이다.

* LPVOID lpParam
  * 창이 생성될 때 추가로 전달할 데이터를 지정하는 포인터이다. 이 데이터는 창의 `WM_CREATE` 메시지에서 참조할 수 있다.

반환값은 다음과 같다.
* 성공 시
  * 새로 생성된 창의 핸들(HWND)을 반환한다.

* 실패 시
  * NULL을 반환한다.
  * GetLastError()를 호출해 실패 원인을 확인할 수 있다.

> Reference  
> [learn.microsoft - nf-winuser-createwindowexa](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-createwindowexa)  
