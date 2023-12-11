# Forwarding References
vector class의 emplace_back과 같이 input으로 받은 인자를 그대로 다른 함수에 전달해줘야 되는 경우가 있다.

하지만 인자를 그대로 전달하는 일은 생각보다 쉬운 일이 아니다. 다음 예시를 보자.

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

  wrapper(a);   
  wrapper(ca);  
  wrapper(A()); 
}
```

기대하는 template function의 instantiation는 다음과 같다. (왜 이렇게 instatiation 되는지 알려면 이 [페이지](https://rla523at.github.io/OnlineBook/sources/03%20programming/01%20C%2B%2B/31%20Template.html#Deduction-from-a-function-call)를 참고하면 된다.)
```cpp
wrapper(a);   //instantiation result: wrapper(T& u)       
wrapper(ca);  //instantiation result: wrapper(const T& u) 
wrapper(A()); //instantiation result: wrapper(T&& u)      ``
```

하지만 막상 코드를 실행하면 기대와 다른 instantiation으로 Lvalue는 제대로 전달되지만 Rvalue는 제대로 전달이 되지 않고 compile error가 발생하는걸 알 수 있다.

```cpp
template <typename T>
void wrapper(T& u) {
  g(u);
}

wrapper(a);     // instantiation result: wrapper(T& u)       
wrapper(ca);    // instantiation result: wrapper(const T& u) 
//wrapper(A()); // instantiation result: wrapper(T& u)      
```

함수의 parameter로 const Lvalue reference 타입을 사용해도 기대와 다른 instantiation으로 Rvalue가 제대로 전달이 되지 않고 엉뚱한 함수가 호출되는걸 알 수 있다.

```cpp
template <typename T>
void wrapper(const T& u) {
  g(u);
}

wrapper(a);   // instantiation result: wrapper(const T& u)       
wrapper(ca);  // instantiation result: wrapper(const T& u) 
wrapper(A()); // instantiation result: wrapper(const T& u)      
```

함수의 parameter로 Rvalue reference 타입을 사용해도 기대와 다른 instantiation으로 Lvalue가 제로 전달이 되지 않고 compile error가 발생할 것 임을 예상할 수 있다.

이런 문제를 해결하기 위해 C++11에 추가된 문법이 `보편적 레퍼런스(Forwarding references, Universal references)`이다. forwarding references는 함수 인자의 value category를 보존시켜주는 특별한 종류의 reference이다.

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

forwaridng referencs는 특별한 template argument deduction 규칙을 갖으며, 초기에 목표대로 instance화 된다.
```cpp
template <typename T>
void wrapper(T&& u) {
  g(u);
}

wrapper(a);   // instantiation result: wrapper(T& u)       
wrapper(ca);  // instantiation result: wrapper(const T& u) 
wrapper(A()); // instantiation result: wrapper(T&& u)      
```

하지만 목표한대로 instantiation 되더라도 함수는 원래 의도한 바와는 다르게 동작한다. 왜냐하면 T&& u가 되었을 떄, u는 Lvalue가 되기 때문이다. 따라서 `wrapper(A())`는 좌측값 레퍼런스 호출이 될것이다. 

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

따라서, 무조건 Rvalue reference로 바꿔주는 move가 아니라 어떤 함수가 있어서 T&&일 떄는 u를 Rvalue refenrece로 형변환 시켜주고 Lvalue reference type일 때는 아무런 변환도 하지 않아야 한다.

이를 위해 C++에서는 std::forward 함수를 제공한다.

```cpp
template <typename T>
void wrapper(T&& u) {
  g(std::forward(u));
}

wrapper(a);   // 좌측값 레퍼런스 호출
wrapper(ca);  // 좌측값 상수 레퍼런스 호출
wrapper(A()); // 우측값 레퍼런스 호출
```

이로써 `완벽한 전달(perfect forwarding)`을 하는 함수를 만들 수 있게 되었다.

> Reference
> [cppreference - Forwarding_references](https://en.cppreference.com/w/cpp/language/reference#Forwarding_references)  
> [modoocode - Move문법과 완벽한 전달](https://modoocode.com/228)  

## std::forward
std::forward 함수는 어떻게 이런 일을 할 수 있는지 알아보자.

std::forward 함수의 정의는 다음과 같다.

```cpp
template <class S>
S&& forward(typename std::remove_reference<S>::type& a) noexcept {
  return static_cast<S&&>(a);
}
```

이 떄, S=A&가 들어오는 경우 다음과 같이 instantiation 될 것이다.
```cpp
// if S=A&
A& forward(A& a) noexcept {  //reference collapsing rule (A&)&& --> A&
  return static_cast<A&>(a);
}
```

따라서 T=A&일 때, wrapper 함수에서 넘어온 인자를 g에 그대로 전달하게 된다.

이번에는 S=A가 들어오는 경우 다음과 같이 instantiation 될 것이다.
```cpp
// if S=A
A&& forward(A& a) noexcept { 
  return static_cast<A&&>(a);
}
```

따라서 T=A일 떄, wrapper 함수에서 넘어온 인자를 g에 g&& Rvalue reference 형태로 전달하게 된다.

> Reference  
> [modoocode - Move문법과 완벽한 전달](https://modoocode.com/228)  

## Universal References
Universal reference라는 표현은 Scott Meyers가 cv-unqualified template parameter의 Rvalue reference을 설명하기 위해 사용한 표현이다.

하지만 C++ standard에서는 이를 위한 표현을 정의하지 않았고 나중에 forwarding reference라는 표현을 도입하였다. 따라서 C++ standard 용어는 forwarding reference이고 널리쓰이는 universal reference와 정확히 동일한 의미를 갖는다.

> Reference  
> [stackoverflow-is-there-a-difference-between-universal-references-and-forwarding-reference](https://stackoverflow.com/questions/39552272/is-there-a-difference-between-universal-references-and-forwarding-references)  