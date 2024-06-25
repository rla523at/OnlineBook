# Miscellaneous

## COM
COM은 Component Object Model은 마이크로소프트가 개발한 인터페이스 기반의 객체지향 기술로, 소프트웨어 컴포넌트 간의 상호작용을 가능하게 한다. COM은 다양한 프로그래밍 언어와 환경에서 사용할 수 있는 바이너리 표준을 제공하여, 재사용 가능한 소프트웨어 컴포넌트를 쉽게 만들고 사용할 수 있게 한다.

COM은 인터페이스를 통해 객체와 상호작용한다. 인터페이스는 순수 가상 함수만을 포함하는 추상 클래스이다. 모든 COM 인터페이스는 IUnknown 인터페이스를 상속받으며 IUnknown 인터페이스는 참조 카운팅과 인터페이스 쿼리를 위한 세 가지 메서드(QueryInterface, AddRef, Release)를 정의한다. 

AddRef, Release 메서드는 COM 객체의 참조 카운팅을 관리하는데 사용된다. COM 객체는 참조 카운팅을 사용하여 객체의 생명 주기를 관리하는데 AddRef 메서드는 참조 카운트를 증가시키고, Release 메서드는 참조 카운트를 감소시킨다. 참조 카운트가 0이 되면 객체가 메모리에서 해제된다.

QueryInterface 메서드는 객체가 지원하는 다른 인터페이스로의 포인터를 요청하는 데 사용된다. 이 메서드는 인터페이스 식별자를 사용하여 요청된 인터페이스가 객체에서 지원되는지 확인하고, 지원되는 경우 해당 인터페이스 포인터를 반환한다.

COM 클래스의 간단한 예제는 다음과 같다.

```cpp
#include <Windows.h>
#include <combaseapi.h>

class MyCOMObject : public IUnknown {
private:
    LONG refCount;

public:
    MyCOMObject() : refCount(1) {}

    HRESULT __stdcall QueryInterface(REFIID riid, void **ppvObject) override {
        if (riid == IID_IUnknown) {
            *ppvObject = static_cast<IUnknown*>(this);
            AddRef();
            return S_OK;
        }
        *ppvObject = nullptr;
        return E_NOINTERFACE;
    }

    ULONG __stdcall AddRef() override {
        return InterlockedIncrement(&refCount);
    }

    ULONG __stdcall Release() override {
        ULONG count = InterlockedDecrement(&refCount);
        if (count == 0) {
            delete this;
        }
        return count;
    }
};
```

## ComPtr
ComPtr는 COM 객체를 관리하는 데 사용되는 스마트 포인터로, 참조 카운팅을 자동으로 관리하여 메모리 누수를 방지한다. Microsoft의 Windows Runtime Library (WRL) 또는 C++/WinRT와 함께 제공된다. ComPtr은 COM 객체 포인터의 수명 주기를 관리하여, 객체의 수동 해제와 참조 카운팅을 자동화한다.

### As
ComPtr는 As 메서드를 통해 쉽게 다른 인터페이스로의 포인터를 요청할 수 있는 메서드를 제공한다.
```cpp
    template<typename U>
    HRESULT As(_Inout_ Details::ComPtrRef<ComPtr<U>> p) const throw()
    {
        return ptr_->QueryInterface(__uuidof(U), p);
    }
```

다음 예제를 보자.

```cpp
#include <wrl.h>
using namespace Microsoft::WRL;

void UseComObject() {
    // ComPtr를 사용하여 COM 객체 생성 및 관리
    ComPtr<MyCOMObject> spMyCOMObject = Make<MyCOMObject>();

    // 다른 인터페이스로의 포인터 요청
    ComPtr<IUnknown> spUnknown;
    HRESULT hr = spMyCOMObject.As(&spUnknown);

    if (SUCCEEDED(hr)) {
        // spUnknown를 통해 IUnknown 인터페이스에 접근 가능
    }
    // spMyCOMObject와 spUnknown는 범위를 벗어날 때 자동으로 Release 호출
}
```

`HRESULT hr = spMyCOMObject.As(&spUnknown);`를 통해 spUnknown이 spMyCOMObject의 값을 가지게 된다. 즉, spUnknown은 spMyCOMObject이 가르키고 있는 주소값을 가지게 된다.

### Reset
Reset 메서드는 ComPtr이 현재 가리키고 있는 인터페이스 포인터를 해제하고, ComPtr을 초기화된 상태로 만드는 함수이다.

### GetAddressOf
GetAddressOf 메서드는 ComPtr 객체가 관리하는 포인터의 주소를 반환하는 함수이다.

함수의 정의는 다음과 같다.
```cpp
T** GetAddressOf() throw()
{
    return &ptr_;
}
```

T pointer의 주소를 반환해야 함으로, 리턴 타입은 T pointer의 pointer로 이중 포인터가 된다.

다음 예제를 보자.

```cpp
#include <iostream>
#include <wrl.h>
#include <d3d11.h>

using namespace Microsoft::WRL;

int main(void)
{
  ComPtr<ID3D11Device> spDevice;

  std::cout << spDevice.Get() << std::endl;
  std::cout << spDevice.GetAddressOf() << std::endl;

  return 0;
}
```

첫번째 출력에서는 ComPtr 객체가 관리하는 내부 포인터(ptr_)의 값이 출력된다. 초기 상태에서는 nullptr이므로 0000000000000000이 출력된다.

그리고 두번재 출력에서는 ComPtr 객체가 관리하는 내부 포인터(ptr_)의 주소가 출력된다.

#### 주의
```cpp
std::cout << static_cast<void **>(&spDevice) << std::endl;
```

위 코드로도 ComPtr 객체가 관리하는 내부 포인터(ptr_)의 주소를 출력할 수 있다.

그러나 &spDevice의 결과인 ComPtrRef 객체는 내부적으로 void** 타입으로 형변환 할 때, pointer를 통해 `ReleaseAndGetAddressOf` 메서드를 호출하고 그 결과 ComPtr 객체가 관리하는 내부 포인터가 nullptr이 된다.

위의 예제에서는 애초에 nullptr이라 문제가 없지만, 아닌 경우에는 위와 같은 형변환을 조심해야 한다.


### operator&
ComPtr에는 ComPtrRef 객체를 return하는 주소변환 연산자가 정의되어 있다.

```cpp
    Details::ComPtrRef<ComPtr<T>> operator&() throw()
    {
        return Details::ComPtrRef<ComPtr<T>>(this);
    }
```

### InternalRelease
InternalRelease 메서드는 ComPtr이 관리하는 포인터가 가르키는 주소를 nullptr로 만들고 포인터가 원래 가르키던 주소를 return한다.

```cpp
    unsigned long InternalRelease() throw()
    {
        unsigned long ref = 0;
        T* temp = ptr_;

        if (temp != nullptr)
        {
            ptr_ = nullptr;
            ref = temp->Release();
        }

        return ref;
    }
```

### ReleaseAndGetAddressOf
ReleaseAndGetAddressOf 메서드는 ComPtr이 관리하는 포인터가 가르키는 주소를 nullptr로 만들고 포인터의 주소를 return한다.

```cpp
    T** ReleaseAndGetAddressOf() throw()
    {
        InternalRelease(); // ptr_ = nullptr이 된다.
        return &ptr_;
    }
```

## ComPtrRef
ComPtrRef에는 void**으로의 형변환 연산자가 정의되어 있다.

```cpp
return reinterpret_cast<void**>(this->ptr_->ReleaseAndGetAddressOf());
```

이는 내부적으로 ComPtr 객체의 `ReleaseAndGetAddressOf` 메서드를 호출한다.

> REFERENCE  
> [GITHUB - ComPtr](https://github.com/Microsoft/DirectXTK/wiki/ComPtr)  
> [MSDN - ComPtr class](https://learn.microsoft.com/en-us/cpp/cppcx/wrl/comptr-class?view=msvc-170)

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

## Message Loop
Message Loop(메시지 루프)는 애플리케이션이 사용자 입력(키보드, 마우스) 및 시스템 메시지를 처리하는 루프이다. 메시지 루프는 애플리케이션이 실행되는 동안 지속적으로 실행되며, 메시지를 받아들이고 처리하는 역할을 한다.

일반적인 메시지 루프는 다음과 같은 구성 요소를 가지고 있다.

* 메시지 정보를 담는 구조체
* 메시지 큐에서 메시지를 가져오는 함수
* 키보드 메시지를 변환해서 텍스트 입력을 처리하는 함수
* 메시지를 윈도우 프로시저로 전달하는 함수

### 메시지 정보를 담는 구조체
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


### 메시지 큐에서 메시지를 가져오는 함수
메시지 큐에서 메시지를 가져오는 함수로는 GetMessage와 PeekMessage 함수가 있다.

두 함수 모두 Windows 애플리케이션에서 메시지 루프를 구현하는 데 사용되지만, 동작 방식과 사용 목적에 따라 차이가 있다.

#### GetMessage
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

#### PeekMessage
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

### 키보드 메시지를 변환해서 텍스트 입력을 처리하는 함수
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

### 메시지를 윈도우 프로시저로 전달하는 함수
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

## Checking TypeName
Compile error message를 통해 void가 아닌 typename을 알아낼 수가 있다.

먼저 template 함수를 다음과 같이 정의하자.

```cpp
template <typename T> void type_checker(T& val)
{
	static_assert(std::is_void_v<T>, " ");
}
```

그 다음에 cpp파일을 compile하면 다음 예시처럼 error message에 type이 나온다.

```
instantiation of "void Mec::type_checker(T &) [with T=Mec::DataArchive]" at line 3657
```

## Iterator

> Reference   
> [blog](https://www.internalpointers.com/post/writing-custom-iterators-modern-cpp)


## bool은 왜 1비트가 아니라 1바이트(8비트)일까?
왜냐하면, 모든 C++ value는 주소값을 가질 수 있어야 하고, 주소는 1바이트를 차지하기 때문에 value의 최소 사이즈는 1바이트이다.

> REFERENCE   
> [STACKOVERFLOW](https://stackoverflow.com/questions/2064550/c-why-bool-is-8-bits-long)  


## 클래스 내부 또는 구조체 내부에서 익명 union 사용
클래스 또는 구조체 내부 익명 union이 있는 경우 일반적인 멤버 변수처럼 사용할 수 있기 때문에 오히려 익명으로 사용하는 것이 편리하다.

```cpp

#include <iostream>

struct A
{
  union
  {
    int number;
    char chracter;
  };
};

int main(void)
{
  A a;
  a.number = 10;
  a.chracter = 'a';

  return 0;
}

```

익명이 아니면 다음과 같이 사용해야 한다.

```cpp
#include <iostream>

struct A
{
  union B
  {
    int number;
    char chracter;
  } b;
};

int main(void)
{
  A a;

  a.b.number = 10;
  a.b.chracter = 'a';

  return 0;
}
```

