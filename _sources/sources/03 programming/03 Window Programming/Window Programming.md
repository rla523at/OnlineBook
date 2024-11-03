# Window Programming

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

## Universal Windows Platform
Windows Universal Windows Platform(UWP) 는 Windows 10과 11에서 실행되는 애플리케이션을 개발하기 위한 플랫폼이다.

UWP 앱은 다양한 장치(데스크탑, 태블릿, 스마트폰, Xbox, HoloLens 등)에서 동일한 코드 베이스로 동작하도록 설계되어 있다.

이 플랫폼은 개발자에게 모듈형 API 집합을 제공하여 여러 종류의 하드웨어와 OS 버전에 걸쳐 호환성을 유지한다.

## Windows Runtime 
윈도우즈 런타임(Windows Runtime; WinRT)은 Microsoft 가 Windows 8 에서 처음 도입한 애플리케이션 프로그래밍 인터페이스(API)이다. 

WinRT는 모던 윈도우 앱(Modern Windows Apps, Universal Windows Platform - UWP 앱)을 지원하기 위해 설계되었으며, 개발자들이 다양한 프로그래밍 언어를 사용해 동일한 API로 애플리케이션을 개발할 수 있도록 한다.

이 런타임은 COM (Component Object Model)을 기반으로 하며, C++, C#, Visual Basic, 그리고 JavaScript 같은 언어 간의 상호 운용성을 제공한다.

> Reference  
> [learn.microsoft - intro-to-using-cpp-with-winrt](https://learn.microsoft.com/ko-kr/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)  

## GetSystemMetrics
GetSystemMetrics는 Windows API의 함수로, 운영 체제의 다양한 시스템 구성과 상태 정보를 조회할 수 있는 함수이다.

주로 화면 해상도, 시스템 요소의 크기, 사용자 인터페이스 설정과 관련된 정보를 가져올 때 사용하며 GetSystemMetrics에서 검색된 모든 차원은 픽셀 단위이다.

함수의 signature 는 다음과 같다.

```cpp
int GetSystemMetrics(
  [in] int nIndex
);
```

인자는 다음과 같다.
* int nIndex
  * 조회하려는 시스템 매트릭스(정보)의 유형을 지정한다.
 
반환값은 다음과 같다.
* 요청이 성공
  * 요청한 시스템 매트릭스의 값(정수)
* 요청이 실패
  * 0이 반환

> Reference  
> [learn.microsoft - nf-winuser-getsystemmetrics](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-getsystemmetrics)  

## UNREFERENCED_PARAMETER 매크로
UNREFERENCED_PARAMETER 는 Windows API 에서 사용되는 매크로로, 사용되지 않는 함수 매개변수를 처리할 때 컴파일러 경고를 없애기 위해 사용된다. 

winnt.h 에 정의되어 있으며 함수 시그니처를 유지해야 하지만 특정 인자가 사용되지 않는 경우 명시적으로 이를 나타낼 때 유용하다.

함수의 signature 는 다음과 같다.

```cpp
#include <windows.h>

void ExampleFunction(int param1, int param2) {
    UNREFERENCED_PARAMETER(param2);  // param2가 사용되지 않는 것을 명시

    // param1만 사용하는 코드
    std::cout << "param1: " << param1 << std::endl;
}
```

## OutputDebugStringA 함수
표시할 문자열을 디버거에 보내는 함수이다.

debugapi.h 에 정의되어 있으나 이 함수를 사용하려면 애플리케이션에 Windows.h 헤더를 포함해야 한다.

참고로, visual studio 에서 OutputDebugStringA 함수에 전달한 인자는 출력창의 디버그를 선택하면 출력되어 있는 걸 볼 수 있다.

함수의 signature 는 다음과 같다.

```cpp
void OutputDebugStringA(
  [in, optional] LPCSTR lpOutputString
);
```

매개변수는 다음과 같다.
* LPCSTR lpOutputString
  * 표시할 null로 종료된 문자열이다.

> Reference  
> [learn.microsoft - nf-debugapi-outputdebugstringa](https://learn.microsoft.com/ko-kr/windows/win32/api/debugapi/nf-debugapi-outputdebugstringa)  

## SetThreadAffinityMask 함수
SetThreadAffinityMask는 Windows API의 함수로, 특정 thread 를 지정된 CPU 코어(프로세서)에만 실행되도록 고정하는데 사용되는 함수다.

함수의 signature 는 다음과 같다.
```cpp
DWORD_PTR SetThreadAffinityMask(
  [in] HANDLE    hThread,
  [in] DWORD_PTR dwThreadAffinityMask
);
```

인자는 다음과 같다.
* HANDLE hThread
  * 고정될 스레드 핸들이다.
* DWORD_PTR dwThreadAffinityMask
  * 스레드를 고정할 CPU 코어(프로세서)를 지정하는 비트 마스크이다.
  * 예를 들어, 0x01은 첫 번째 코어, 0x02는 두 번째 코어를 의미한다.


> Reference  
> [learn.microsoft - nf-winbase-setthreadaffinitymask](https://learn.microsoft.com/ko-kr/windows/win32/api/winbase/nf-winbase-setthreadaffinitymask)   
> [jiniya.net - 스레드 고급 활용법: 우선순위, 친화도, TLS, APC](https://jiniya.net/wp/archives/7647/)  

## SetThreadPriority 함수
SetThreadPriority 함수는 Windows API 함수로, 특정 스레드의 우선순위(priority)를 설정한다. 

우선순위가 높은 스레드는 운영체제로부터 더 많은 CPU 시간을 할당받아 작업을 우선적으로 수행한다. 

이 함수는 멀티스레드 애플리케이션에서 스레드의 실행 순서를 조정하고, 특정 작업의 응답성을 향상시키는 데 사용된다.

함수의 signature 는 다음과 같다.
```cpp
BOOL SetThreadPriority(
  [in] HANDLE hThread,
  [in] int    nPriority
);
```

인자는 다음과 같다.
* HANDLE hThread
  * 우선순위를 설정할 스레드의 핸들이다.
* int nPriority
  * 설정할 우선순위 수준이다.
  * [여기](https://learn.microsoft.com/ko-kr/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadpriority)에서 사용가능한 값을 확인할 수 있다.
 
반환값은 다음과 같다.
* 성공
  * TRUE
*  실패
  * FALSE
  * 실패 원인에 대해서는 GetLastError()를 통해 확인할 수 있다.


> Reference  
> [learn.microsoft - nf-processthreadsapi-setthreadpriority](https://learn.microsoft.com/ko-kr/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadpriority)  
> [jiniya.net - 스레드 고급 활용법: 우선순위, 친화도, TLS, APC](https://jiniya.net/wp/archives/7647/)

## SetThreadIdealProcessorEx 함수  
SetThreadIdealProcessorEx 함수는 Windows API 의 함수로, 스레드의 이상적인(ideal) 프로세서를 설정한다. 

이상적인 프로세서는 해당 스레드가 주로 해당 코어에서 실행되도록 권장하는 것이며, 이로 캐시 효율성을 높이고 성능을 최적화할 수 있다. 

다만, 운영체제는 이 설정을 강제하지 않으며, 필요에 따라 스레드를 다른 코어에서 실행할 수 있습니다.

함수의 signature

```cpp
DWORD SetThreadIdealProcessorEx(
  [in]      HANDLE hThread,
  [in, out] PPROCESSOR_NUMBER lpIdealProcessor,
  [out]     PPROCESSOR_NUMBER lpPreviousIdealProcessor
);
```
인자는 다음과 같다.
* HANDLE hThread
  * 우선 실행할 프로세서를 설정할 스레드의 핸들이다
  * 이 핸들은 `CreateThread`나 `GetCurrentThread` 같은 함수로 가져올 수 있습니다.

* PPROCESSOR_NUMBER lpIdealProcessor 
  * 이상적인 프로세서를 지정할 포인터다.  
  * `PROCESSOR_NUMBER` 구조체는 다음과 같은 필드로 구성된다:
    - Group: 프로세서가 속한 프로세서 그룹 번호다.
    - Number: 프로세서 번호다.
    - Reserved: 예약된 필드다.

* PPROCESSOR_NUMBER lpPreviousIdealProcessor  
  * 이전에 설정된 이상적인 프로세서 정보를 저장할 포인터다.

반환값은 다음과 같다.
* 성공 시  
  * `ERROR_SUCCESS`를 반환한다.

* 실패 시  
  * `0`을 반환한다.
  * `GetLastError()`를 호출해 실패 원인을 확인할 수 있다.

> Reference  
> [learn.microsoft - nf-processthreadsapi-setthreadidealprocessorex](https://learn.microsoft.com/ko-kr/windows/win32/api/processthreadsapi/nf-processthreadsapi-setthreadidealprocessorex)  

## GlobalMemoryStatusEx 함수 
GlobalMemoryStatusEx 함수는 Windows API 의 함수로, 시스템 메모리의 현재 상태를 확인한다. 

이 함수는 물리적 메모리, 가상 메모리, 페이징 파일의 용량과 사용량 정보를 제공하며, 응용 프로그램이 실행 환경의 메모리 상황을 파악하고 메모리 부족 시 적절한 조치를 취할 수 있도록 돕는다.

함수의 signature는 다음과 같다.
```cpp
BOOL GlobalMemoryStatusEx(
  [out] LPMEMORYSTATUSEX lpBuffer
);
```

인자는 다음과 같다.
* LPMEMORYSTATUSEX lpBuffer  
  * 메모리 상태 정보를 저장할 `MEMORYSTATUSEX` 구조체의 포인터다.
  * 호출 전에 이 구조체의 `dwLength` 필드에 구조체의 크기를 설정해야 한다.

반환값은 다음과 같다.
* 성공 시  
  * 0이 아닌 값 반환한다.
* 실패 시  
  * 0을 반환하며, GetLastError 함수를 통해 실패 원인을 확인할 수 있다.

> Reference
> [learn.microsoft - nf-sysinfoapi-globalmemorystatusex](https://learn.microsoft.com/ko-kr/windows/win32/api/sysinfoapi/nf-sysinfoapi-globalmemorystatusex)

## GetACP 함수
GetACP 함수는 Windows API 의 함수로, 현재 시스템에서 사용되는 ANSI 코드 페이지(ACP)의 식별자를 반환한다. 

함수의 signature는 다음과 같다.
```cpp
UINT GetACP();
```

반환값은 다음과 같다.
* 성공 시  
  * 현재 사용 중인 ANSI 코드 페이지의 식별자를 반환한다.
  * 예를 들어, 1252는 서유럽 언어를 위한 코드 페이지다.
  * [식별자 목록](https://learn.microsoft.com/ko-kr/windows/win32/intl/code-page-identifiers)
* 실패 시  
  * 오류는 발생하지 않으며, 항상 유효한 코드 페이지 식별자를 반환한다.

## RegisterAppStateChangeNotification 함수
RegisterAppStateChangeNotification 함수는 Windows에서 애플리케이션의 상태 변화를 모니터링하고 해당 상태 변화에 대한 알림을 받을 수 있도록 콜백을 등록하는 함수이다.

함수의 시그니처는 다음과 같다.
```cpp
APICONTRACT ULONG RegisterAppStateChangeNotification(
  PAPPSTATE_CHANGE_ROUTINE Routine,
  PVOID                    Context,
  PAPPSTATE_REGISTRATION   *Registration
);
```

인자는 다음과 같다.
* PAPPSTATE_CHANGE_ROUTINE Routine
  * 앱이 일시 중단된 상태로 들어가거나 떠날 때 앱에 알리는 앱 정의 콜백 함수의 포인터이다.

* PVOID Context
  * 콜백 함수에 전달될 사용자 정의 데이터다. 필요하지 않으면 nullptr을 전달할 수 있다.

* PAPPSTATE_REGISTRATION *Registration
  * 등록된 알림 정보를 저장할 핸들 포인터다.
  * 함수가 성공하면 이 포인터는 등록 정보를 가리키게 된다.

반환값은 다음과 같다.
* 표준 Win32 상태 코드다.
  
> Reference  
> [learn.microsoft - nf-appnotify-registerappstatechangenotification](https://learn.microsoft.com/ko-kr/windows/win32/api/appnotify/nf-appnotify-registerappstatechangenotification)  

### PAPPSTATE_CHANGE_ROUTINE
앱이 일시 중단된 상태로 들어가거나 떠날 때 앱에 알리는 앱 정의 콜백 함수의 포인터이다.

콜백 함수는 다음과 같은 형태를 갖어야 한다.

```cpp
PAPPSTATE_CHANGE_ROUTINE PappstateChangeRoutine;

void PappstateChangeRoutine(
       BOOLEAN Quiesced,
  [in] PVOID Context
)
{...}
```

매개 변수는 다음과 같다.

* BOOLEAN Quiesced
  * 앱이 일시 중단된 상태로 들어가면 TRUE이고, 앱이 일시 중단된 상태를 벗어나는 경우 FALSE다.
* PVOID Context
  * 일시 중단 시 앱이 저장하고 다시 시작할 때 사용할 수 있는 데이터에 대한 포인터다.
  * 이 값은 RegisterAppStateChangeNotification 함수에서 제공한다.
  * 일반적으로 "this" 포인터다.

> Reference  
> [learn.microsoft - nc-appnotify-pappstate_change_routine](https://learn.microsoft.com/ko-kr/windows/win32/api/appnotify/nc-appnotify-pappstate_change_routine)

## SetWindowLongPtr & GetWindowLongPtr
```cpp
SetWindowLongPtr(hwnd, GWLP_USERDATA, reinterpret_cast<LONG_PTR>(sample.get());

// in WndProc function
{
  auto sample = reinterpret_cast<Sample*>(GetWindowLongPtr(hWnd, GWLP_USERDATA));
  //...
}
```

### SetWindowLongPtr
지정된 window 의 속성을 변경한다.

이 함수는 또한 추가 window 메모리의 지정된 오프셋에 값을 설정한다.

```cpp
LONG_PTR SetWindowLongPtrA(
  [in] HWND     hWnd,
  [in] int      nIndex,
  [in] LONG_PTR dwNewLong
);
```

> Reference  
> [learn.microsoft - nf-winuser-setwindowlongptra](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-setwindowlongptra)  

### GetWindowLongPtrA
지정된 창에 대한 정보를 검색한다. 

이 함수는 추가 window 메모리의 지정된 오프셋에 저장된 값을 검색한다.

```cpp
LONG_PTR GetWindowLongPtrA(
  [in] HWND hWnd,
  [in] int  nIndex
);
```

> Reference  
> [learn.microsoft - nf-winuser-getwindowlongptra](https://learn.microsoft.com/ko-kr/windows/win32/api/winuser/nf-winuser-getwindowlongptra)  

## GetACP 함수
`GetACP` 함수는 현재 시스템의 ANSI 코드 페이지( Active Code Page )를 가져오는 함수이다.

ANSI 코드 페이지는 시스템의 기본 문자 인코딩을 정의하며, 여러 문자 세트를 지원하기 위해 사용된다.

함수의 시그니처는 다음과 같다.
```cpp
UINT GetACP();
```

인자는 없다.

반환값은 다음과 같다.
* 성공 시
  * 현재 시스템의 ANSI 코드 페이지 식별자(UINT)를 반환한다.
  * 코드 페이지 식별자는 [여기](https://learn.microsoft.com/ko-kr/windows/win32/intl/code-page-identifiers) 를 참고하면 된다.

* 실패 시
  * 실패하는 경우는 거의 없으며, 반환값은 시스템의 현재 ANSI 코드 페이지로 지정된 값이다.

### 참고
GetACP 함수에 대응되는 시스템의 기본 코드 페이지 (Active Code Page, ACP )를 설정하는 직접적인 SetACP 함수는 Windows API에는 없다. 

Windows에서는 시스템 전체의 ACP를 변경하는 API를 제공하지 않으며, 기본 코드 페이지는 Windows의 로케일 설정에 따라 결정된다.

시스템 기본 코드 페이지를 변경하려면 Windows의 로케일 설정을 직접 수정해야 합니다:

제어판 → 시계 및 국가 → 국가 또는 지역을 엽니다.
관리자 옵션 탭에서 시스템 로캘 변경을 선택합니다.
원하는 로캘을 선택하고 시스템을 재부팅하여 적용합니다.

이 설정은 전체 시스템에 영향을 미치며, 특정 애플리케이션 내에서만 기본 코드 페이지를 변경하는 기능을 제공하지 않는다. 

따라서, 특정 코드 페이지를 사용하려면 애플리케이션 내부에서 SetConsoleOutputCP와 SetConsoleCP를 사용하거나, 코드 페이지 변환을 위한 함수(예: MultiByteToWideChar 및 WideCharToMultiByte)를 사용하는 것이 일반적이다.

