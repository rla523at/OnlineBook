# Win32 API
Win32 API 또는 Windows API 는 Windows 및 하드웨어에 직접 액세스해야 하는 네이티브 C/C++ Windows 애플리케이션을 위한 original platform 이다.

.NET 및 WinRT(Windows 10용 UWP 앱용) 와 같은 managed runtime environment 에 의존하지 않고도 최고 수준의 개발 환경을 제공한다. 

Win32 API는 가장 높은 수준의 성능과 시스템 하드웨어에 대한 직접 액세스가 필요한 애플리케이션에 적합한 platform 이다.

> Reference  
> [learn.microsoft - win32/desktop-programming](https://learn.microsoft.com/ko-kr/windows/win32/desktop-programming)  

## CreateEventExA
CreateEventExA 함수는 지정된 이름을 가진 이벤트 객체를 생성하거나 열어주는 함수다. 

 SetEvent 함수로 이벤트를 신호 상태로 설정하거나, ResetEvent 함수로 신호가 아닌 상태로 재설정할 수 있다.

함수의 시그니처는 다음과 같다.
```cpp
HANDLE CreateEventExA(
  [in, optional] LPSECURITY_ATTRIBUTES lpEventAttributes,
  [in, optional] LPCSTR                lpName,
  [in]           DWORD                 dwFlags,
  [in]           DWORD                 dwDesiredAccess
);
```

인자는 다음과 같다.
* LPSECURITY_ATTRIBUTES lpEventAttributes
  * 이벤트 객체의 보안 속성을 지정하는 포인터다.
  * 기본 보안 설정을 사용하려면 NULL을 전달할 수 있다.

* LPCSTR lpName
  * 생성하거나 열 이벤트 객체의 이름을 지정하는 ANSI 문자열이다.
  * 이벤트 객체는 이름을 통해 다른 프로세스에서도 접근 가능하다.
  * 이름이 필요 없을 경우 NULL을 전달할 수 있다.

* DWORD dwFlags
  * 이벤트 객체의 초기 상태 및 자동 재설정 여부를 지정하는 플래그다.  

* DWORD dwDesiredAccess
  * 이벤트 객체에 대한 접근 권한을 지정한다.
  * 가능한 값은 [여기](https://learn.microsoft.com/ko-kr/windows/win32/sync/synchronization-object-security-and-access-rights)를 참고하면 된다.
  
반환값은 다음과 같다.
* 성공 시
  * 이벤트 객체의 핸들(HANDLE)을 반환한다.
  * 만약, 동일한 이름의 이벤트가 존재하는 경우, 이전에 정의된 이벤트의 handle 을 반환하고 GetLastError 에서는 `ERROR_ALREADY_EXISTS`를 반환한다.

* 실패 시
  * NULL을 반환한다.
  * 오류 원인은 `GetLastError()`를 호출하여 확인할 수 있다.
 
> Reference  
> [learn.microsoft - nf-synchapi-createeventexa](https://learn.microsoft.com/ko-kr/windows/win32/api/synchapi/nf-synchapi-createeventexa)

## RegisterAppStateChangeNotification 함수
RegisterAppStateChangeNotification 함수는 애플리케이션의 suspend state 를 모니터링하고 해당 상태 변경 시 알림을 받을 call back 함수를 등록하는 함수이다.

함수의 시그니처는 다음과 같다.
```cpp
APICONTRACT ULONG RegisterAppStateChangeNotification(
  [in]           PAPPSTATE_CHANGE_ROUTINE Routine,
  [in, optional] PVOID                    Context,
  [out]          PAPPSTATE_REGISTRATION   *Registration
);
```

인자는 다음과 같다.
* PAPPSTATE_CHANGE_ROUTINE Routine
  * 애플리케이션 상태가 변경될 때 호출될 콜백 함수의 포인터다.
  * 애플리케이션의 상태가 변경되면, 이 함수가 호출되어 상태에 따른 작업을 수행할 수 있다.

* PVOID Context
  * 콜백 함수에 전달될 사용자 정의 데이터이다.
  * 필요하지 않으면 `NULL`을 전달할 수 있다.

* PAPPSTATE_REGISTRATION *Registration
  * 등록된 알림 정보를 저장할 핸들 포인터이다.
  * 함수가 성공하면 이 포인터는 등록 정보를 가리키게 된다.

반환값은 다음과 같다.
* 성공 시
  * 0이 아닌 ULONG 값으로, 이 값은 등록된 알림의 핸들 역할을 한다.

* 실패 시
  * 0을 반환한다.
  * 실패 시 적절한 오류 코드를 분석하여 문제를 진단해야 한다.

> Reference  
> [learn.microsoft - nf-appnotify-registerappstatechangenotification](https://learn.microsoft.com/en-us/windows/win32/api/appnotify/nf-appnotify-registerappstatechangenotification)

### PAPPSTATE_CHANGE_ROUTINE callback function
어플리케이션의 suspend state 변경 시 호출 될 call back 함수의 포인터이다.

call back 함수는 다음과 같은 형태를 갖어야 한다.

```cpp
PAPPSTATE_CHANGE_ROUTINE PappstateChangeRoutine;

void PappstateChangeRoutine(
       BOOLEAN Quiesced,
  [in] PVOID Context
)
{...}
```

> Reference  
> [learn.microsoft - nc-appnotify-pappstate_change_routine](https://learn.microsoft.com/en-us/windows/win32/api/appnotify/nc-appnotify-pappstate_change_routine)  

