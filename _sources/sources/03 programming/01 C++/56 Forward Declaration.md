# Forward Declaration
## Compile Time Dependecy Reduction
다음 예제 코드를 보자.
```cpp
//B.h

// forward declaration
class A;

class B
{
public:
    void func(A& a);
    A    func2(A& a);
}

//B.cpp
#include "A.h"
#include "B.h"
void B::func(A& a){ ... };
```
위와 같이 함수의 return 값이나 input 값으로 사용하는 경우 `전방선언(forward declaration)`만으로 충분하다. 전방선언을 사용한 경우 B.h에서 #include를 할 필요가 없어진다.

대신에 함수를 정의하는 B.cpp 파일에서는 #include A.h를 통해 A의 정의를 알아야 한다.

단, 객체로 변수를 가지고 있는 경우 전반선언으로 컴파일 의존성을 줄일 수 없다. 다음 예제 코드를 보자.
```cpp
//B.h
class A;
class B
{
public:
    void func(A& a);
private:
    A a;    //error!
}
```

이는 컴파일 시간에 A 객체의 크기를 알아야 B 객체의 크기를 결정하는데 A 객체의 정의부가 제공되어 있지 않기 때문이다.

### 주의사항
cpp에서 header file을 include할 때, header에 forward declaration을 확인하고 현재 cpp에서 전부 include 되어있는지 확인해야한다.

또한, header file이 include하고 있는 header file의 forward declaration도 반드시 확인해야 된다.

## Pimpl with Unique_ptr
다음 예제 코드를 보자
```cpp
//A.h
class A{};

//B.h
#pragma once
#include <memory>

class A;

class B
{	
public:
    //B(void);
    //~B(void);
private:
	std::unique_ptr<A> a;
};

//B.cpp
#include "B.h"

#include "A.h"
//B::B(void)=default;
//B::~B(void)=default;

//main.cpp
#include "B.h"

int main(void)
{
	B b;
}
```

위 코드는 `memory.h`에서 incomplete type을 delete할 수 없다는 error가 발생한다.

error는 default constructor과 default destructor 때문에 발생한다.

### default constructor
`b`가 생성되는 순간을 생각해보자. `B`에 constructor가 없기 때문에 inline default constructor가 생길것이다. 그리고 inline default constructor에서는 `B`의 멤버 변수들의 default constructor를 호출할 것이다(`a = std::unique_ptr<A>();`). 하지만 class `A`는 forward declaration만 되어 있을 뿐, 그 정의를 알 수 없기 때문에 complie error가 발생한다.

따라서, B.h에 `B`의 생성자를 선언하고 B.cpp에 `B`의 생성자를 정의하면 `B`의 생성자가 호출될 때 `A`를 알고 있을 수 있게되고 default constructor 문제가 해결된다.

하지만 문제가 전부 해결된 것은 아니다.

### default destructor
이번에는 `b`가 소멸되는 순간을 생각해보자. `B`에 destructor가 없기 때문에 inline default destructor가 호출될 것이고 inline default destructor는 `B`의 멤버 변수들의 default destructor를 호출할 것이다.

memory에 있는 default destructor의 코드는 다음과 같다.
``` cpp
template<class _Ty>
struct default_delete
{	// default deleter for unique_ptr
	constexpr default_delete() noexcept = default;

	template<class _Ty2, enable_if_t<is_convertible_v<_Ty2 *, _Ty *>, int> = 0>
	default_delete(const default_delete<_Ty2>&) noexcept
	{	// construct from another default_delete
	}

	void operator()(_Ty * _Ptr) const noexcept
	{	// delete a pointer
		static_assert(0 < sizeof(_Ty), "can't delete an incomplete type");
		delete _Ptr;
	}
};
```

operator()를 통해 _Ty 타입의 pointer에 delete를 호출하는 역활을 하는데 이 때, _Ty가 incomplete type이 아니여야 한다.

하지만 default destructor가 호출될 때, class `A`는 forward declaration만 되어 있을 뿐, 그 정의를 알 수 없기 때문에 complie error가 발생한다.

따라서, B.h에 `B`의 소멸자를 선언하고 B.cpp에 `B`의 소멸자를 정의하면 `B`의 소멸자가 호출될 때 `A`를 알고 있을 수 있게되고 default destructor 문제가 해결된다.

> Reference  
> [blog1](https://ozt88.tistory.com/32)
> [blog2](https://gomgomi.tistory.com/5)
> [stackoverflow](https://stackoverflow.com/questions/42416776/pimpl-with-unique-ptr-why-do-i-have-to-move-definition-of-constructor-of-inter)