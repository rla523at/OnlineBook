# Reference
Reference란 이미 존재하는 객체나 함수에 대한 `별칭(alias)`이다.

Reference가 다음과 같은 특징을 갖는다.
* reference는 alias지 객체가 아니다. 
  * 반드시 메모리를 차지하지 않을 수 있다.
    * [c++draft - dcl.ref#4](https://eel.is/c++draft/dcl.ref#4)
  * reference의 array도 없고 reference의 pointer도 없고 reference의 reference도 없다.
* reference는 alias이기 때문에 반드시 정의와 동시에 초기화 되어야 한다. 
* reference는 한번 정의가 되면 이후에는 다른 변수의 reference가 될 수 없다.
  * reference는 top-level cv-qualified가 불가능하다.
    * reference type은 두 계층으로 되어있다. reference to type. 이 때, 두번째 계층에서 cv-qualifier가 나타나는 것(reference to const type)은 가능하지만 첫번째 계층에서 cv-qualifier가 나타나는 것(const reference to type)은 정의되지 않는 표현식이다. 즉, const reference는 없는 개념이다. 왜냐하면 reference는 이미 const reference처럼 동작하기 때문이다.

```cpp
//int& a; //초기화 없는 정의 불가능

int a = 3;
int b = 4;
int& ra = a;
ra = b; // ra가 b의 alias가 되는게 아니라 a에 b의 값을 대입하는 것
```

그리고 C++에서는 다음과 같은 3개의 reference 타입을 제공한다.
* Lvalue reference
* Rvalue reference
* forwarding reference

> Reference  
> [cppreference - reference](https://en.cppreference.com/w/cpp/language/reference)  
> [c++draft - dcl.ref#4](https://eel.is/c++draft/dcl.ref#4)

## Lvalue Reference 
`좌측값 참조(Lvalue reference)`는 말 그대로 value category상 Lvalue인 객체들의 alias이다.


## Rvalue Reference
`우측값 참조(Rvalue reference)`는 C++ 11에서 추가된 문법으로 value category상 Rvalue인 객체들의 alias이다.

Rvalue Reference를 통해 함수의 input으로 Lvalue와 Rvalue를 구분할 수 있게 됨으로써 moved 특성이 있는 Rvalue가 input으로 들어온 경우에는 복사연산 대신 이동연산이 발생하도록 하여 불필요한 복사 문제를 해결할 수 있다.

위의 말을 이해하기 위해 다음 예시를 살펴보자.

class X 를 어떠한 리소스에 대한 포인터(예를 들어 m_pResource) 를 담고 있는 클래스라고 생각하자. 참고로 여기서 리소스 라고 말하는 것은, 생성 또는 복사, 소멸 하기에 많은 시간이 걸리는 거대한 어떤 무언가를 의미한다. 클래스 X 의 가장 좋은 예로 std::vector 를 들 수 있다.

```cpp
X foo();  // foo 는 X 타입의 객체를 리턴하는 함수이다!

X x;
x = foo();
```

만약 `operator=`가 복사대입연산자라고 한다면 다음 과정을 거친다.
1. x의 포인터가 가르키고 있는 리소스를 소멸한다.
2. (foo 가 리턴한) 임시 객체의 리소스를 복사 생성한다.
3. x의 포인터가 복사 생성된 리소스를 가리킨다.
4. 복사대입연산자가 끝나고 임시로 생성된 객체가 소멸되면서 임시 객체의 리소스 또한 소멸된다.

위와 같이 기존 객체에 임시생선된 객체를 복사대입 할 떄, 임시로 생성된 객체가 가르키고 있는 리소스의 대한 복사와 소멸은 필요가 없다는 걸 알 수 있다. 그냥 x가 가르키고 있떤 리소스만 소멸시킨 뒤 임시로 생성된 객체가 가지고 있는 리소스를 가르키는 포인터와 x의 리소스를 가르키는 포인터를 서로 교환만 해주면 된다. 어짜피 임시 객체는 향후에 다시 사용될 일이 없기 때문에 포인터가 어디를 가르켜도 아무런 문제가 생기지 않는다. 그리고 이 과정을 임시 객체의 리소스가 x로 이동되었다고 표현한다.


> Reference  
> [modoocode](https://modoocode.com/189)

### std::move
std::move는 C++11에서 추가된 함수중 하나로 input으로 들어온 객체의 Rvalue reference를 return하는 함수로 결론적으로 input 객체를 Rvalue로 casting 해주는 함수이다.

std::move를 사용하면 Lvalue를 Rvalue(Xvalue)로 casting 할 수 있어 프로그래머 판단하에 복사연산 대신 이동연산이 발생하도록 하여 불필요한 복사가 발생하지 않게 할 수 있다.

위의 말을 이해하기 위해 다음 예시를 살펴보자.

```cpp
template <class T>
void swap(T& a, T& b) {
  T tmp(a);
  a = b;
  b = tmp;
}

X a, b;
swap(a, b);
```
위와 같이 swap이 구현되어 있다고 하면, `tmp(a)`, `a=b`, `b=tmp`에서 총 3번 복사가 발생하게 된다. 하지만 이러한 복사가 굳이 필요 없다는걸 알 수 있다. 왜냐하면 a의 리소스를 tmp로 이동하고, b의 리소스를 a로 이동하고 마지막으로 tmp의 리소스를 a로 이동하기만 하면 된다. 

하지만 문제는 tmp, a와 b가 Rvalue가 아니여서 Rvalue를 input으로 받는 이동연산을 호출할 수 없다는 점이다.

C++ 표준의 첫 번째 십계명은 "우리 C++ 위원회는 C++ 프로그래머의 (때론 무모하더라도) 자유를 막는 규칙을 만들면 안된다 - The committee shall make no rule that prevents C++ programmers from shooting themselves in the foot 라고 명시되어 있다. 그래서 이러한 신념을 바탕으로 C++ 11 에서는 `std::move` 함수를 제공한다. std::move를 통해 다음과 같이 swap을 바꿀 수 있다.

```cpp
template <class T>
void swap(T& a, T& b) {
  T tmp(std::move(a));
  a = std::move(b);
  b = std::move(tmp);
}

X a, b;
swap(a, b);
```

> Reference  
> [modoocode](https://modoocode.com/189)

#### std::move의 구현
std::move의 구현은 다음과 같다.

```cpp
template <typename T>
constexpr std::remove_reference_t<T>&& move(T&& arg) noexcept {
    return static_cast<std::remove_reference_t<T>&&>(arg);
}
```

이 때, input이 `T&& arg`임으로 타입추론이 가능함으로 std::move는 template function임에도 불구하고 호출할 때 타입을 명시하지 않아도 된다.

```cpp
#include "gtest/gtest.h"

using namespace std;

struct A {};

void g(A& a) { cout << "좌측값 레퍼런스 호출" << endl; }
void g(const A& a) { cout << "좌측값 상수 레퍼런스 호출" << endl; }
void g(A&& a) { cout << "우측값 레퍼런스 호출" << endl; }
void g(const A&& a) { cout << "우측값 상수 레퍼런스 호출" << endl; }

TEST(test, test)
{
  A        a;
  const A  ca;
  A&       ra  = a;
  const A& cra = ca;

  g(move(a));   //우측값 레퍼런스 호출
  g(move(ca));  //우측값 상수 레퍼런스 호출
  g(move(ra));  //우측값 레퍼런스 호출
  g(move(cra)); //우측값 상수 레퍼런스 호출
}
```

## Forwarding Reference
`전달 레퍼런스(Forwarding references, Universal references)`는 C++11에 추가된 문법으로 std::forward와 함께 사용하여 함수의 input 인자들의 value category를 보존시켜줄 수 있는 특별한 종류의 reference이다.

forwarding references는 다음 두가지 경우이다.
* 템플릿 함수가 input으로 템플릿 parameter의 cv-unqualified rvalue reference를 사용하면 이는 forwarding references이다.
```cpp
template<class T>
int f(T&& x); // x is a forwarding reference
```
* brace-enclosed initializer list로 부터 추론되는게 아닌 auto&&는 forwarding references이다.
```cpp
auto&& vec = foo();       // foo() may be lvalue or rvalue, vec is a forwarding reference
for (auto&& x: f()){}     // x is a forwarding reference; this is a common way to use range for in generic code
    auto&& z = {1, 2, 3}; // *not* a forwarding reference (special case for initializer lists)
```

forwarding references와 std::forward를 사용해 함수 인자의 value category를 보존시킴으로써 `완벽한 전달(perfect forwarding)` 문제를 해결할 수 있다.

> Reference  
> [cppreference - Forwarding_references](https://en.cppreference.com/w/cpp/language/reference#Forwarding_references)  


### Perfect forwarding 문제
perfect forwardin 문제는 vector class의 emplace_back과 같이 input으로 받은 인자를 그대로 다른 함수에 전달해줘야 되는 경우 발생하는 문제다. 
* 기존의 templae argument deduction rule로는 Lvalue가 input으로 주어졌을 때, Lvalue reference로 Rvalue가 input으로 주어졌을 떄, Rvalue reference로 instatiation이 되지 않는다.
* instantiation이 원하는 대로 되어도 Rvalue Reference type의 함수의 인자는 Lvalue라는 점이다.

다음 예시를 보자.

```cpp
#include <iostream>
#include <vector>

template <typename T>
void wrapper(T& u) {
  g(u);
}

class A {};

void g(A& a) { std::cout << "좌측값 레퍼런스 호출" << std::endl; }
void g(const A& a) { std::cout << "좌측값 상수 레퍼런스 호출" << std::endl; }
void g(A&& a) { std::cout << "우측값 레퍼런스 호출" << std::endl; }
void g(const A&& a) { std::cout << "우측값 상수 레퍼런스 호출" << std::endl; }


int main() {
  A a;
  const A ca;

  wrapper(a);   // 기대값 : 좌측값 레퍼런스 호출
  wrapper(ca);  // 기대값 : 좌측값 상수 레퍼런스 호출
  wrapper(A()); // 기대값 : 우측값 레퍼런스 호출
}
```

이를 위해 기대하는 template function의 instantiation는 다음과 같다. 
```cpp
wrapper(a);   //expected instantiation result: wrapper(A& u)       
wrapper(ca);  //expected instantiation result: wrapper(const A& u) 
wrapper(A()); //expected instantiation result: wrapper(A&& u)      ``
```

하지만 막상 코드를 실행하면 기대와 다른 instantiation으로 Lvalue는 제대로 전달되지만 Rvalue는 제대로 전달이 되지 않고 compile error가 발생하는걸 알 수 있다.(왜 이렇게 instatiation 되는지 알려면 이 [페이지](https://rla523at.github.io/OnlineBook/sources/03%20programming/01%20C++/31%20Template.html#deduction-from-a-function-call)를 참고하면 된다.)

```cpp
template <typename T>
void wrapper(T& u) {
  g(u);
}

wrapper(a);     // instantiation result: wrapper(A& u)       
wrapper(ca);    // instantiation result: wrapper(const A& u) 
//wrapper(A()); // instantiation result: wrapper(A& u)      
```

함수의 parameter로 const Lvalue reference 타입을 사용해도 기대와 다른 instantiation으로 Rvalue가 제대로 전달이 되지 않고 엉뚱한 함수가 호출되는걸 알 수 있다.

```cpp
template <typename T>
void wrapper(const T& u) {
  g(u);
}

wrapper(a);   // instantiation result: wrapper(const A& u)       
wrapper(ca);  // instantiation result: wrapper(const A& u) 
wrapper(A()); // instantiation result: wrapper(const A& u)      
```

이런 문제를 해결하기 위해 forwaridng referencs는 [특별한 template argument deduction 규칙](https://rla523at.github.io/OnlineBook/sources/03%20programming/01%20C++/31%20Template.html#deduction-from-a-function-call)을 갖고 있어 함수의 어떤 input이 들어오더라도 그 input의 타입에 맞게 instantiation 된다.
```cpp
template <typename T>
void wrapper(T&& u) {
  g(u);
}

wrapper(a);   // instantiation result: wrapper(A& u)       
wrapper(ca);  // instantiation result: wrapper(const A& u) 
wrapper(A()); // instantiation result: wrapper(A&& u)      
```

하지만 목표한대로 instantiation 되더라도 Rvalue가 input으로 주어졌을 때 함수는 원래 의도한 바와는 다르게 동작한다. 왜냐하면 input이 Rvalue reference type을 주어지더라도 input은 Lvalue가 되기 때문이다. 왜냐하면 u는 "u"라는 변수명을 갖음으로써 향후에 접근가능성이 생김으로 moved 특성이 없기 때문이다. 따라서 `g(u)`는 좌측값 레퍼런스 호출이 될것이다. 

```cpp
template <typename T>
void wrapper(T&& u) {
  g(u);
}

wrapper(a);   // 좌측값 레퍼런스 호출
wrapper(ca);  // 좌측값 상수 레퍼런스 호출
wrapper(A()); // 좌측값 레퍼런스 호출
```

이를 해결하기 위해서 std::move를 사용한다고 하게 되면 Lvalue일 때 전부 우측값 레퍼런스가 호출이 되는 문제가 발생한다.
```cpp
template <typename T>
void wrapper(T&& u) {
  g(std::move(u));
}

wrapper(a);   // 우측값 레퍼런스 호출
wrapper(ca);  // 우측값 상수 레퍼런스 호출
wrapper(A()); // 우측값 레퍼런스 호출
```

위의 문제를 해결하기 위해 C++에서는 std::forward 함수를 제공한다.

std::forward는 template argument가 Lvalue reference가 되었을 때는, input으로 Lvalue reference type을 받고 그대로 Lvalue reference를 return하여 아무런 동작도 하지 않는것 처럼 동작한다. 하지만 template argument가 Type일 때는, input으로 Lvalue reference type을 받고 Rvalue reference를 return함으로써 마치 std::move처럼 동작하여 input을 Rvalue로 casting 해준다.

```cpp
template <typename T>
void wrapper(T&& u) {
  g(std::forward<T>(u));
}

wrapper(a);   // 좌측값 레퍼런스 호출
wrapper(ca);  // 좌측값 상수 레퍼런스 호출
wrapper(A()); // 우측값 레퍼런스 호출
```

그러면 std::forward 함수는 어떻게 이런 일을 할 수 있는걸까?

std::forward 함수의 정의는 다음과 같다.

```cpp
template <typename T>
constexpr T&& forward(std::remove_reference_t<T>& arg) noexcept {
    return static_cast<T&&>(arg);
}

```

이 떄, T=A&가 들어오는 경우 다음과 같이 instantiation 될 것이다.
```cpp
// if T=A&
A& forward(A& a) noexcept {  
  return static_cast<A&>(a); //reference collapsing rule (A&)&& --> A&
}
```

따라서 T=A&일 때, wrapper 함수에서 넘어온 인자를 g에 그대로 전달하게 된다.

이번에는 T=A가 들어오는 경우 다음과 같이 instantiation 될 것이다.
```cpp
// if T=A
A&& forward(A& a) noexcept { 
  return static_cast<A&&>(a);
}
```

따라서 T=A일 떄, wrapper 함수에서 넘어온 인자를 g에 g&& Rvalue reference 형태로 전달하게 된다.

참고로 std::forward의 input은 `std::remove_reference_t<T>& arg`이기 때문에 타입 추론이 불가능하다. 따라서 std::forward 함수를 호출할떄는 반드시 template argument를 주어야 한다.

> Reference  
> [cppreference - Forwarding_references](https://en.cppreference.com/w/cpp/language/reference#Forwarding_references)  
> [modoocode - Move문법과 완벽한 전달](https://modoocode.com/228)  

### Universal References
`보편적 레퍼런스(Universal reference)`라는 표현은 Scott Meyers가 cv-unqualified template parameter의 Rvalue reference을 설명하기 위해 사용한 표현이다.

하지만 C++ standard에서는 이를 위한 표현을 정의하지 않았고 나중에 forwarding reference라는 표현을 도입하였다. 따라서 C++ standard 용어는 forwarding reference이고 널리쓰이는 universal reference와 정확히 동일한 의미를 갖는다.

> Reference  
> [stackoverflow-is-there-a-difference-between-universal-references-and-forwarding-reference](https://stackoverflow.com/questions/39552272/is-there-a-difference-between-universal-references-and-forwarding-references)  


### Function References
함수 포인터처럼 함수 레퍼런스(function reference)를 만들 수 있다.

```cpp
void Foo()
{
}

int main()
{
    void(& func)() = Foo; // foo is function reference of FOO

    func(); //::Foo();
}
```

> Reference  
> [stackoverflow - function-references](https://stackoverflow.com/questions/480248/function-references)  