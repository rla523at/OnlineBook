# Namespace
다음 예제 코드를 보자.

```c++
#include <iostream>

namespace A
{
	void func(void) { std::cout << "A namespace\n"; };
}

namespace B
{
	void func(void) { std::cout << "B namespace\n"; };
}

void func(void) { std::cout << "global namespace\n"; };

int main(void)
{	
	A::func();	//A namespace
	B::func();	//B namespace
	func();		//current namespace = global namespace
	::func();	//global namespace	
}
```

특정 namespace에 있는 함수를 호출하고 싶은 경우, `[namespace name]::[function name]`형태로 코드를 작성하면 된다.

만약, [namespace name]::없이 `[function name]`형태로 작성할 경우 current namespace에 있는 함수가 호출되며, 별다른 선언이 안되어 있는 경우 global namespace가 된다.

namespace만 없이 `::[function name]`형태로 코드를 작성할 경우 global namespace에 있는 함수가 호출된다.


## Using
매번 `[namespace name]::`을 적어주는 것이 귀찮을 수 있다.

이때는 using이라는 keyword를 사용하면 된다. 

### using
아래 예시 코드를 보자.

```cpp
namespace A
{
	void func(void) { std::cout << "A namespace\n"; };
}

namespace B
{
	void func(void) { std::cout << "B namespace\n"; };
}

void func(void) { std::cout << "global namespace\n"; };

int main(void)
{	
	func();			//global namespace

	using A::func;
	func();			//A namespace	
	::func();		//global namespace	
}
```

위와 같이 using keyword를 사용하는 경우 해당 이름의 함수를 current namespace로 가져온다.

따라서, using keyword를 사용한 후에는 current namespace에 있는 `func()`는 `A::func()`가 된다.

### using namepsace
아래 예시 코드를 보자.

```cpp
namespace A
{
	void func(void) { std::cout << "A namespace\n"; };
}

int main(void)
{	
	//func(); 	//Error! current namespace에 func() 함수가 없음!

	using namespace A;
	func();		//A::func()가 호출됨
}

int test(void)
{
	//func(); Error! current namespace is not A
}
```

위와 같이 using namespace keyword를 사용하면 current namespace를 A로 바꿔줄 수 있다.

using 선언의 범위는 대괄호 안으로 한정된다.

아래와 같이 using이 대괄호로 밖에 있는 경우, using 선언의 범위는 전체 TU로 확장된다.

```cpp
namespace A
{
	void func(void) { std::cout << "A namespace\n"; };
}

using namespace A;

int main(void)
{	
	func();	//A::func() 호출
}

int test(void)
{
	func(); //A::func() 호출
}
```

#### 주의사항
아래 예시 코드를 보자.
``` cpp
#include <iostream>

namespace A
{
	void func(void) { std::cout << "A namespace\n"; };
}

void func(void) { std::cout << "global namespace\n"; };

using namespace A;

int main(void)
{	
	//func();	// Error! function name 충돌!
}
```

using namespace keyword를 대괄호 밖에서 사용하는 경우, 위의 예시처럼 언제 name 충돌이 발생할지 모른다.

따라서, 간단한 코드가 아니라면 namespace를 다 적어주는것이 좋다.

> Reference  
> [Blog](https://nerdooit.github.io/2020/09/08/cpp_book_2.html)