# COM
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