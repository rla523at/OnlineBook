# Window Programming

## WNDCLASSEX 구조체
WNDCLASSEX 구조체는 Windows API에서 윈도우 클래스를 정의하는 데 사용되는 구조체로, 윈도우 클래스의 속성을 지정하여 그에 맞는 윈도우를 생성할 수 있게 한다.

각 멤버 변수는 윈도우 클래스의 특성을 지정하는 데 중요한 역할을 한다.

> REFERENCE  
> [MSDN - WNDCLASSEX](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/ns-winuser-wndclassexw)  

### cbSize
구조체의 크기(바이트)를 지정하는 변수다. 따라서, 항상 `sizeof(WNDCLASSEX)`로 설정해야 한다.

### style
윈도우 클래스의 스타일을 지정하는 변수다.

여러 스타일을 비트 OR 연산자로 결합하여 사용할 수 있다.

#### CS_CLASSDC
이 스타일은 클래스의 모든 창에서 공유할 하나의 `장치 컨텍스트(Device Context, DC)`를 할당할 때 사용한다.

장치 컨텍스트는 윈도우의 그리기 작업을 관리하는 객체로, 그래픽 객체와 그리기 속성(예: 펜, 브러시, 색상 등)을 포함한다. 애플리케이션은 장치 컨텍스트를 사용하여 윈도우의 클라이언트 영역에 텍스트, 도형, 비트맵 등을 그린다.

이 스타일을 사용하면 클래스의 모든 윈도우가 동일한 장치 컨텍스트를 공유하게 된다. 따라서 여러 윈도우가 개별적인 장치 컨텍스트를 가질 때보다 메모리 사용량이 줄어든다.

하지만 다른 윈도우의 그리기 작업이 현재 윈도우에 영향을 미칠 수 있다. 그래서 여러 윈도우가 동시에 그리기 작업을 수행하면 예상치 못한 결과가 발생할 수 있다. 

CS_CLASSDC는 일반적으로 권장되지 않으며, 특정 상황에서만 사용된다. 

### lpfnWndProc
윈도우 프로시저(윈도우 메세지를 처리할 함수)로 사용할 함수의 함수 포인터를 지정하는 변수다.

WNDCLASSEX 구조체에 원하는 속성을 지정한다음 RegisterClassEx 함수를 통해 윈도우 클래스를 등록한 뒤 CreateWindow 함수를 호출하면 등록된 윈도우 클래스를 기반으로 윈도우를 생성한다. 

이 떄, CreateWindow 함수는 내부적으로 CreateWindowExW를 호출하고, 여기서 생성된 윈도우에 대해 WNDCLASSEX 구조체에 지정된 lpfnWndProc 함수 포인터가 윈도우 프로시저로 설정된다.

윈도우가 생성되고 나면, 메시지 루프를 통해 메시지를 대기하고 처리하게 된다.  메세지 루프 로직에서 DispatchMessage 함수는 메시지를 해당 윈도우의 윈도우 프로시저로 전달한다. 따라서 이때, lpfnWndProc에 지정된 함수가 호출이 되게 된다. 참고로 lpfnWndProc은 윈도우 클래스와 연결된 모든 윈도우 인스턴스에 대해 호출된다.

### cbClsExtra
클래스 구조체에 추가로 할당할 바이트 수를 지정하는 변수다.

일반적으로 0으로 설정한다.

### cbWndExtra
윈도우 인스턴스에 추가로 할당할 바이트 수를 지정하는 변수다.

일반적으로 0으로 설정한다.

### hInstance
해당 윈도우 클래스가 속한 애플리케이션의 인스턴스 핸들을 지정하는 변수다. 

이를 통해 메시지가 올바른 윈도우 프로시저로 전달될 수 있다.

GetModuleHandle 함수에 NULL을 전달하면, 현재 실행 중인 프로세스의 인스턴스 핸들을 반환한다.

따라서 일반적으로 윈도우 클래스를 등록할 때 hInstance에 `GetModuleHandle(NULL)`값을 사용한다.

### hIcon
윈도우 클래스에 속한 모든 윈도우에 공통으로 사용될 아이콘을 지정하는 변수다.

이 아이콘은 주로 윈도우의 제목 표시줄, 작업 표시줄, 그리고 Alt+Tab 작업 전환 창에 표시된다.

만약 hIcon에 NULL 값을 주면, 윈도우는 기본 시스템 아이콘을 사용하게 된다.

### hCursor
윈도우 클래스에서 기본으로 사용할 커서를 지정하는 변수이다.

이 커서는 윈도우가 생성되고 마우스가 윈도우의 클라이언트 영역 위에 있을 때 기본적으로 표시된다.

hCursor 멤버 변수에는 커서 핸들(HCURSOR) 값을 대입해야 한다. 이 핸들은 보통 LoadCursor 함수나 LoadImage 함수를 사용하여 커서를 로드하여 얻는다.

만약, hCursor에 NULL 값을 주면, 윈도우 클래스는 기본 커서를 지정하지 않은 것으로 간주된다. 이 경우, 시스템은 기본 커서를 사용하지 않으며, 커서가 윈도우의 클라이언트 영역 위에 있을 때 커서가 보이지 않거나, 다른 설정된 커서를 사용할 수 있다.

커서가 표시되지 않으므로, 애플리케이션이 적절한 시점에 커서를 설정하거나 변경하는 로직을 추가로 구현해야 할 수 있다. 예를 들어, SetCursor 함수를 사용하여 커서를 수동으로 설정할 수 있다.

### hbrBackground
윈도우의 배경을 칠할 때 사용할 브러시를 지정하는 변수이다. 

이 브러시는 윈도우가 그려질 때 사용되며, 기본 배경 색상을 설정하는 데 사용된다.

hbrBackground에는 브러시 핸들(HBRUSH) 값을 대입해야 한다. 일반적으로 CreateSolidBrush 또는 CreatePatternBrush 같은 함수를 사용하여 브러시를 생성하거나, 미리 정의된 색상 값 또는 시스템 색상을 사용하는 경우가 많다.

만약, hbrBackground에 NULL 값을 주면 윈도우 클래스는 기본 배경 브러시를 사용하지 않는다. 이 경우 윈도우의 배경을 자동으로 칠하지 않음으로 클라이언트 영역에서 그리기를 요청할 때마다 자체 배경을 그려야 한다.

### lpszMenuName
윈도우 클래스에 사용할 메뉴의 이름을 지정하는 변수이다.

이 메뉴는 윈도우가 생성될 때 제목 표시줄 아래에 바로 표시된다.

만약 lpszMenuName에 NULL 값을 주면, 윈도우 클래스에 기본적으로 연결된 메뉴가 없다는 의미이다. 따라서 이 클래스에 속한 윈도우는 생성될 때 기본 메뉴를 가지지 않는다.

### lpszClassName
윈도우 클래스의 이름을 지정하는 변수이다.

이 이름은 윈도우 클래스를 식별하는 데 중요한 역할을 한다. 윈도우 클래스를 등록할 때 RegisterClassEx 함수에 의해 사용되고, CreateWindow 또는 CreateWindowEx 함수를 통해 윈도우를 생성할 때 해당 윈도우 클래스를 참조하는 데 사용된다.

윈도우 클래스를 식별하는 데 사용하는 이름임으로 같은 이름을 가진 여러 윈도우 클래스를 등록할 수 없다. 따라서 클래스 이름은 애플리케이션 내에서 고유해야 한다.

만약 lpszClassName에 NULL 값을 주면 RegisterClassEx 함수를 호출할 떄 오류 코드가 반환된다. 

### hIconSm
윈도우 클래스에 의해 생성된 윈도우의 작은 아이콘을 지정하는 변수이다. 

이 아이콘은 주로 윈도우의 작업 표시줄이나 제목 표시줄의 왼쪽 모서리에 표시된다.

hIconSm에는 작은 아이콘 핸들(HICON)을 설정해야 한다. 일반적으로 LoadIcon 또는 LoadImage 함수를 사용하여 아이콘을 로드하며 작은 아이콘은 일반적으로 크기가 16x16 픽셀이다.

만약 hIconSm에 NULL 값을 주면, 시스템은 작은 아이콘을 사용하지 않는다. 이 경우, 시스템은 기본 아이콘을 사용하거나, 큰 아이콘(hIcon)을 작은 크기로 축소하여 사용할 수 있다.


## RegisterClassEx 함수
RegisterClassEx 함수는 WNDCLASSEX 구조체에 정의된 윈도우 클래스를 등록하는 데 사용된다. 이 함수는 애플리케이션이 해당 클래스를 기반으로 윈도우를 생성할 수 있도록 윈도우 클래스를 시스템에 등록한다. RegisterClassEx는 RegisterClass 함수의 확장 버전으로, 추가적인 멤버와 기능을 제공한다.

RegisterClassEx 함수는 등록이 성공하면 반환값으로 고유한 클래스 원자(ATOM)를 반환한다. 이 값은 이후에 윈도우를 생성할 때 클래스 이름 대신 사용할 수 있다. 만약 등록이 실패하면, 0을 반환한다. 이 경우 GetLastError 함수를 호출하여 확장된 오류 정보를 얻을 수 있다.

> REFERENCE  
> [MSDN - RegisterClassEx](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-registerclassa?redirectedfrom=MSDN#return-value)

## AdjustWindowRect 함수
AdjustWindowRect 함수는 지정된 클라이언트 영역이 포함될 수 있도록 윈도우의 전체 크기를 조정하는 함수이다.

AdjustWindowRect 함수의 시그니쳐는 다음과 같다.
```cpp
BOOL AdjustWindowRect(
  LPRECT lpRect,
  DWORD dwStyle,
  BOOL bMenu
);
```

각 인자는 다음과 같다.
* lpRect 
  * 조정할 RECT 구조체에 대한 포인터다. 
  * 함수 호출 후, 이 RECT는 전체 윈도우 크기를 포함하게 된다.
* dwStyle
  * 윈도우 스타일을 지정하는 DWORD 값이다. 
  * WS_OVERLAPPEDWINDOW는 가장 일반적인 윈도우 스타일로, 제목 표시줄, 크기 조정 가능 경계선, 최소화/최대화 버튼 등을 포함한다.
* bMenu
  * 윈도우에 메뉴가 포함되어 있는지 여부를 지정하는 BOOL 값이다.

> REFERENCE  
> [MSDN - AdjustWindowRect 함수](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-adjustwindowrect)  
> [MSDN - 창 스타일](https://learn.microsoft.com/ko-kr/windows/win32/winmsg/window-styles)  

## CreateWindow 함수
CreateWindow 함수는 Windows 애플리케이션에서 새로운 윈도우를 생성하는 함수다. 이 함수는 윈도우의 여러 속성을 지정하고, 생성된 윈도우의 핸들을 반환한다. 이 핸들은 이후 윈도우 조작과 메시지 처리를 위해 사용된다.

CreateWindow 함수의 시그니쳐는 다음과 같다.
```cpp
HWND CreateWindow(
  LPCWSTR lpClassName,
  LPCWSTR lpWindowName,
  DWORD dwStyle,
  int x,
  int y,
  int nWidth,
  int nHeight,
  HWND hWndParent,
  HMENU hMenu,
  HINSTANCE hInstance,
  LPVOID lpParam
);
```

각 인자는 다음과 같다.
* lpClassName
  * 윈도우 생성에 사용할 윈도우의 클래스 이름을 지정한다. 
  * 이 클래스는 RegisterClass 또는 RegisterClassEx 함수를 사용하여 등록되어 있어야 한다.
* lpWindowName
  * 윈도우의 제목을 지정하는 문자열이다. 
  * 이 문자열은 윈도우의 제목 표시줄에 나타난다.
* dwStyle
  * 윈도우의 스타일을 지정하는 DWORD 값이다. 
  * 윈도우 스타일은 윈도우의 외관과 동작을 정의하며, 여러 스타일을 조합하여 사용할 수 있다. 
  * 예를 들어, WS_OVERLAPPEDWINDOW, WS_VISIBLE, WS_CHILD 등이 있다.
* x
  * 윈도우의 초기 위치(왼쪽 상단 모서리의 x 좌표)이다. 
  * CW_USEDEFAULT를 지정하면 시스템이 적절한 위치를 결정한다.
* y
  * 윈도우의 초기 위치(왼쪽 상단 모서리의 y 좌표)이다. 
  * CW_USEDEFAULT를 지정하면 시스템이 적절한 위치를 결정한다.
* nWidth
  * 윈도우의 너비이다. 
  * CW_USEDEFAULT를 지정하면 시스템이 적절한 너비를 결정한다.
* nHeight
  * 윈도우의 높이이다. 
  * CW_USEDEFAULT를 지정하면 시스템이 적절한 높이를 결정한다.
* hWndParent
  * 부모 윈도우의 핸들이다. 
  * 자식 윈도우를 생성할 때 사용하며, 최상위 윈도우를 생성할 경우 NULL로 지정한다.
* hMenu
  * 윈도우와 연결할 메뉴의 핸들이다. 
  * 최상위 윈도우에서는 메뉴의 핸들을 지정하고, 자식 윈도우에서는 컨트롤 ID를 지정한다. 
  * 메뉴가 없으면 NULL로 지정한다.
* hInstance
  * 윈도우를 생성하는 애플리케이션의 인스턴스 핸들이다.
* lpParam
  * 윈도우를 생성할 때 추가 데이터를 전달하는 용도로 사용된다.
  * 이 데이터는 주로 윈도우 프로시저가 초기화 과정에서 필요한 정보를 포함한다.
  * 윈도우가 생성되면, 시스템은 WM_CREATE 메시지를 윈도우 프로시저로 보낸다. 이 메시지의 lParam 매개변수는 CREATESTRUCT 구조체에 대한 포인터를 포함하며, 이 구조체의 lpCreateParams 멤버는 lpParam 인자의 값을 가리킨다.
  * NULL 값을 주면, 윈도우를 생성할 때 추가 데이터를 전달하지 않는다는 의미다.

## ShowWindow 함수
ShowWindow 함수는 지정된 윈도우의 표시 상태를 설정하는 데 사용하는 함수이다.

함수의 시그니쳐는 다음과 같다.
```cpp
BOOL ShowWindow(
  HWND hWnd,
  int nCmdShow
);
```

각 인자는 다음과 같다.
* hWnd
  * 표시 상태를 변경할 윈도우의 핸들
* nCmdShow
  * 윈도우의 표시 상태를 지정하는 정수 값 (int). 이 값은 윈도우가 어떻게 표시될지를 결정한다.
  * SW_HIDE: 윈도우를 숨긴다.
  * SW_SHOW: 윈도우를 보여준다. (윈도우를 활성화하고 표시)
  * SW_SHOWDEFAULT: STARTUPINFO 구조체의 wShowWindow 멤버에 지정된 값을 사용하여 윈도우를 보여준다.
  * SW_SHOWNORMAL: 윈도우를 정상적으로 표시한다. (SW_RESTORE와 동일)
  * SW_SHOWMINIMIZED: 윈도우를 최소화한다.
  * SW_SHOWMAXIMIZED: 윈도우를 최대화한다.
  * SW_SHOWNOACTIVATE: 윈도우를 활성화하지 않고 표시한다.
  * SW_RESTORE: 윈도우를 이전 크기와 위치로 복원한다.
  * SW_MINIMIZE: 윈도우를 최소화하고, 활성 윈도우를 유지한다.
  * SW_MAXIMIZE: 윈도우를 최대화한다.
  * SW_SHOWMINNOACTIVE: 윈도우를 활성화하지 않고 최소화한다.
  * SW_SHOWNA: 윈도우를 활성화하지 않고 표시한다.
  * SW_FORCEMINIMIZE: 윈도우를 최소화한다. 심지어 윈도우가 비활성화 상태여도 최소화한다.

반환 값은 TRUE이면 윈도우가 이전에 보였음을 의미하고 FALSE이면 윈도우가 이전에 숨겨졌음을 의미한다.

## UpdateWindow 함수
UpdateWindow 함수는 지정된 윈도우의 클라이언트 영역을 강제로 다시 그리도록 하는 함수이다. 이 함수는 WM_PAINT 메시지를 직접적으로 생성하고 처리하도록 하여, 윈도우가 즉시 다시 그려지게 한다. UpdateWindow는 클라이언트 영역이 무효화되어 다시 그려져야 할 때 사용된다.

## DestroyWindow 함수
DestroyWindow 함수는 지정된 창을 파괴하고, 해당 창과 연결된 모든 자원을 해제한다. 이 함수는 창이 삭제되도록 요청하고, 메시지 큐에서 모든 메시지를 제거하며, 창과 관련된 모든 시스템 리소스를 해제한다.

함수의 시그니쳐는 다음과 같다.
```cpp
BOOL DestroyWindow(
  HWND hWnd
);
```

## UnregisterClass 함수
UnregisterClass 함수는 윈도우 클래스의 등록을 해제하여, 해당 클래스가 더 이상 새로운 윈도우를 생성하는 데 사용되지 않도록 한다. 이 함수는 클래스와 관련된 모든 시스템 리소스를 해제하는 역할을 한다.

함수의 시그니쳐는 다음과 같다.
```cpp
BOOL UnregisterClass(
  LPCWSTR lpClassName, // 등록 해제할 클래스의 이름
  HINSTANCE hInstance  // 클래스가 등록된 애플리케이션의 인스턴스 핸들
);
```

참고로, UnregisterClass 함수는 특정 클래스에 대한 모든 윈도우가 닫혀 있을 때만 성공적으로 클래스를 등록 해제할 수 있다. 만약 해당 클래스를 사용하는 윈도우가 하나라도 열려 있으면 UnregisterClass 함수는 실패하게 된다.

따라서 DestroyWindow 함수를 호출한 뒤에 UnregisterClass를 호출해줘야 한다.

