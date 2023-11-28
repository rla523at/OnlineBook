# Static
## static 전역 변수
class scope 외부에 있는 static 변수는 동일한 TU라는 특정 범위에서만 사용 가능한 전역 변수이다.

만약 static 변수를 헤더파일에 정의하고 여러 cpp에서 헤더파일을 포함할 경우 각 TU마다 고유의 static 변수가 생기며 하나의 TU에서 생긴 변화는 다른 TU에 영향을 주지 않는다.

아래의 예시를 통해 확인해 보자

```cpp
//Common.h
static int g_static_value = 3;

//source1.cpp TU1
#include <iostream>

#include "Common.h"

void modify1(void)
{
	g_static_value = 1;
}

void tell1(void)
{
	std::cout << g_static_value << "\n";
}

//source2.cpp TU2
#include <iostream>

#include "Common.h"

void modify2(void)
{
	g_static_value = 2;
}

void tell2(void)
{
	std::cout << g_static_value << "\n";
}

//main.cpp TU3
void modify1(void);
void tell1(void);
void modify2(void);
void tell2(void);

int main(void)
{
	modify1();  //TU1의 g_static_value는 1로 바뀜
	tell1();    //1 출력
    tell2();	//TU2의 g_static_value는 3임 >> 3 출력
}
```


### static과 extern
extern은 '외부의'라는 뜻을, static은 '정적인, 고정된'이란 뜻을 나타낸다. static 이란 영역 내부에 고정된 어떤 것을 나타내서 특정 범위에서만 사용할 수 있는 것을 나타내고, extern은 영역 외부에 존재하기 때문에 영역 내/외부 모두에서 사용할 수 있음을 나타낸다.

extern과 static이외에 local도 있는데 local이란 개념은 말 그대로 '지역'이라는 의미로서 지역 객체를 나타낼 때 사용하며, 지역 객체란 함수 안에서 정의되어 함수 안에서만 사용 될 수 잇는 객체를 의미한다.

결국 객체는 local과 global로 나누어질 수 있으며, local은 지역 객체이고, global의 경우 모든 범위에서 사용될 수 있는 전역 객체와 한정된 사용 범위를 가지는 정적 객체로 나누어짐을 알 수 있다.

static과 extern을 구분하기 위해 아래의 예제를 보자.

```cpp
//A.cpp
extern int g_val = 0; //extern을 생략할 수 있음
static int s_val = 0;

namespace ms
{
	void func(void)
	{
		std::cout << "wow\n";
	}
}

//Main.cpp
extern int g_val;
extern int s_val;

namespace ms
{
    extern void func(void); //extern을 생략할 수 있음
}

void main(void)
{
    std::cout << g_val << "\n";
    std::cout << s_val << "\n"; // link error 발생
    ms::func();
}
```
s_val은 정적 변수이기 때문에 내부링크 방식을 사용하고 A.cpp의 TU에서만 사용할 수 있으며 다른 TU에서는 사용할 수 없다. 하지만 g_val과 func은 외부링크 방식을 사용함으로 다른 TU에서 사용할 수 있다.

> Reference  
> {cite}`FundamentalC++`

## Static Data Members
class 내부에서 static keyword를 사용하는 경우, 저장 방식 지정자를 나타낼때와는 별개로 class 객체와는 무관한 멤버 함수나 변수가 됨을 의미한다.

static data member들은 어떤 객체와도 연관되어 있지 않으며 클래스의 객체가 정의 되기 전에 존재한다. 

static data member는 static storage duration을 가지고 있으며 프로그램 전체를 통틀어 단 하나만 존재한다. 

단, static data member가 thread local인 경우 thread local storage duration을 갖으며 각 쓰레드마다 하나씩 존재한다.

namespace scope을 갖는 클래스의 static data member는 클래스가 외부 링크 방식을 갖을 경우 외부 링크 방식을 갖는다. 

local 클래스(함수 내부에 정의된 클래스) 그리고 unnamed 클래스, unnamed 클래스에 속한 멤버 클래스등은 static data member를 갖지 못한다. 

> Reference  
[cppreference](https://en.cppreference.com/w/cpp/language/static)

### static data member

```cpp
class X
{
public:
	static int val1;
	//static int val2 = 1;	// compile error!
};

//.cpp
int X::val = 1;	
```

`static int val1;`은 static member변수를 선언만 하고 정의를 하지 않은것이다.

따라서, 추가적으로 정의해주지 않으면 변수의 정의를 찾지 못해 link error가 발생한다.

이를 해결하기 위해 일반적으로, 아래와같이 .cpp 파일에서 변수를 정의해주며 이 때는 static keyword를 사용하지 않는다.
```cpp
//.cpp
int X::val = 1;	
```

만약, 아래와같이 .h 파일에서 정의할 경우, 여러 .cpp 파일에서 이 .h를 inclue할 경우 OCR위반이 발생한다.
```cpp
//.h
int X::val = 1;	
```

참고로, 아래와 같이 static member 변수를 in-class initialization 시도하면 compile error가 발생한다.
```cpp
//static int val2 = 1;				// compile error!
```

> Reference  
[cppreference](https://en.cppreference.com/w/cpp/language/static)

### Const static data member
static data member는 const로 선언될 수 있다.

만약 const static data member가 integral type이거나 enumeration type인 경우 class 내부에서 정의 및 초기화 될 수 있다.

```cpp
//.h
class X
{
public:
    const static int n = 1;
    const static int m{2}; // since C++11
    const static int k;
};
//.cpp
const int X::k = 3;
```

`X::k`를 보면 알 수 있듯이, const static data member는 class 내부에서 반드시 정의 및 초기화가 이뤄저야 되는것은 아니다.

> Reference  
[cppreference](https://en.cppreference.com/w/cpp/language/static)

### Constexpr static data member(since C++11)
LiterType의 static data member는 constexpr로 선언될 수 있다.

Constexpr static data member는 class 내부에서 정의 및 초기화 되어야 한다.

```cpp
class X
{
    constexpr static int arr[] = { 1, 2, 3 };        	// OK
    constexpr static std::complex<double> n = {1,2}; 	// OK
	//constexpr static int k; 							// compile Error
};
```

`X::k`를 보면 알 수 있듯이, constexpr static data member는 반드시 class 내부에서 정의 및 초기화가 이뤄저야 한다.


> Reference  
[cppreference](https://en.cppreference.com/w/cpp/language/static)  
> [stackoverflow - redeclaration](https://stackoverflow.com/questions/45019980/redefinitions-of-constexpr-static-data-members-are-allowed-now-but-not-inline)

### Inline static data member(since C++17)
static data member는 lnline으로 선언될 수 있다.

Inline static data member의 경우, 다음과 같이 class 내부에서 정의 및 초기화 될 수 있다.

```cpp
class X
{
public:
    inline static int n = 1;
};
```

이 경우에는 class 외부에 따로 정의를 할 필요가 없다.

> Reference  
[cppreference](https://en.cppreference.com/w/cpp/language/static)

### In const member function
static data member는 const 함수의 보호를 받지 못한다.

아래 예시 코드를 보자.

```cpp

#include <iostream>

class A
{
public:
	void func(void) const
	{
		this->static_val = 5; 
		std::cout << this->static_val << "\n";
	}

private:
	static inline int static_val = 3;
};
```

static_val은 member variable로 취급되지 않기 때문에 const member function에서도 수정이 가능하다.

따라서, 이와 같은 맥락때문에 static data member는 mutable 또한 될 수 없다.

> Reference  
[cppreference](https://en.cppreference.com/w/cpp/language/static)


---



## static 멤버 함수

### static 멤버 함수의 정의와 static 키워드
```cpp
class Foobar {
 public:
  static void do_something();
};

static void Foobar::do_something() {} // Compile Error!

int main() {
  Foobar::do_something();
}
```

위의 코드를 보면 static 멤버 함수를 정의하는 부분에 여기에 스토리지 클래스를 지정할 수 없다는 에러 문구가 뜬다. 이를 해결하기 위해서는 static 문구만 지워주면 되는데 왜 이렇게 작동하는지 알아보자.

static 키워드는 C++에서 다양한 의미로 사용되는데 위의 코드에서는 2가지 다른 방식으로 사용되고 있다.

멤버 함수의 입장에서 static 키워드는 "이 멤버 함수는 별도의 객체가 필요하지 않으며 기본적으로 class scope에 있는 함수일 뿐이다" 라는 의미를 갖는다.

하지만 함수의 정의 입장에서 static 키워드는 "이 함수는 이 TU에서만 정적으로 사용될 것이고 다른 TU에서는 사용할 수 없다." 라는 의미를 갖는다.

컴파일러는 함수의 정의 입장에서 저 구문을 분석하게 되고 이 부분이 바로 C++에서 허용하지 않는 부분이다. 왜냐하면 이는 혼란을 야기할 수 있기 때문이다. 예를 들어 여러개의 다른 TU에서 Foobar class의 정의를 포함한 뒤 static 멤버 함수를 다르게 정의할 경우, linking error는 발생하지 않고 같은 멤버 함수가 서로 다른 공간에서 서로 다르게 행동할 수 있게 된다.

이를 해결 하기 위해서는 static 멤버 함수를 정의할 때 static 키워드를 제거하면 된다. 컴파일러는 이미 do_something이라는 함수가 static 멤버 함수라는 것을 알고 있기 때문에 static 키워드가 없어도 아무런 문제가 되지 않는다.

> Reference   
[stack over flow](https://stackoverflow.com/questions/31305717/member-function-with-static-linkage)  