# Forwarding References
vector class의 emplace_back과 같이 input으로 받은 인자를 그대로 다른 함수에 전달해줘야 되는 경우가 있다.

하지만 인자를 그대로 전달하는 일은 생각보다 쉬운 일이 아니다. 

이런 역할을 하는 함수를 구현하기 위해 함수의 parameter로 template parameter T를 사용하는 다음 예시를 보자.

```cpp
#include <iostream>
#include <vector>

template <typename T>
void wrapper(T u) {
  g(u);
}

class A {};

void g(A& a) { std::cout << "좌측값 레퍼런스 호출" << std::endl; }
void g(const A& a) { std::cout << "좌측값 상수 레퍼런스 호출" << std::endl; }
void g(A&& a) { std::cout << "우측값 레퍼런스 호출" << std::endl; }

int main() {
  A a;
  const A ca;

  wrapper(a);
  wrapper(ca);
  wrapper(A());
}
```

코드를 실행하면 전부 좌측값 레퍼런스 호출만 발생한다.

왜 이런 일이 발생하는지 한단계씩 살펴보자.

함수의 parameter로 reference type이 아닌 template parameter를 사용하면 C++의 [Deduction from a function call](https://en.cppreference.com/w/cpp/language/template_argument_deduction#Deduction_from_a_function_call) rule에 의해 argument가 cv-qualified type이면 top-level cv-qualifier를 무시하도록 조정한 다음에 T를 deduction한다. 
```cpp
//parameter: T --> is not a reference type
wrapper(a);   // Argument: a  --> T u = a --> deduced T = A
wrapper(ca);  // Argument: ca --> T u = ca & ca의 cv-qualifier를 무시함 --> deduced T = A
wrapper(A()); // Argument: A() --> T u = A() --> deduced T = A
```

결론적으로 전부 A로 추론되었기 때문에 좌측값 레퍼런스 호출만 발생하게 되는 것이다.

단순히 template parameter T를 함수 parameter로 써서는 전달하는 역할을 제대로 수행하지 못함으로 이번에는 함수의 parameter로 template parameter의 Lvalue reference 타입을 사용해보자. 

이 경우에는  deduction rule에 의해 추론할 떄 referenced type이 사용된다.
 
```cpp
template <typename T>
void wrapper(T& u) {
  g(u);
}

//...
//parameter: T& u --> reference type
wrapper(a);   // Argument: a   --> T& u = a   --> deduced T = A
wrapper(ca);  // Argument: ca  --> T& u = ca  --> deduced T = const A
wrapper(A()); // Argument: A() --> T& u = A() --> deduction fail!
```


이 떄, Lvalue reference는 PRvalue인 A()를 받을 수 없기 때문에 compile error가 발생한다. 
```cpp
  wrapper(a); //좌측값 레퍼런스 호출
  wrapper(ca); //좌측값 상수 레퍼런스 호출
  // wrapper(A()); // Compile Error
```

template parameter T의 Lvalue reference 타입을 함수 parameter로 써도 전달하는 역할을 제대로 수행하지 못함으로 이번에는 함수의 parameter로 template parameter의 const Lvalue reference 타입을 사용해보자. 

cv-qualified type이면서 referenced type이기 때문에 deduction rule에 의해 top-level cv-qualifieres가 무시되고 referenced type을 사용해서 T가 유추된다. 그리고 reference collapsing rule을 적용하면 다음과 같아진다.

```cpp
template <typename T>
void wrapper(const T& u) {
  g(u);
}

//...

  wrapper(a);   // T가 A&로 추론되서 (const A& & u) -> (const A& u)
  wrapper(ca);  // T가 A&로 추론되서 (const A& & u) -> (const A& u)
  wrapper(A()); // T가 A&&로 추론되서 (const A&& & u) -> (const A& u)
```
이 경우에도 전부 좌측값 상수 레퍼런스가 호출되기 때문에 Lvalue const reference도 해결책이 되지 못한다. 

따라서 Lvalue가 들어올 떄는 Lvalue reference처럼 Rvalue가 들어올 때는 Rvalue reference처럼 동작할 필요가 생긴다.

이를 위한 문법이 C++11에 추가된 `보편적 레퍼런스(Universal references, Forwarding references)`이다. forwarding references는 함수 인자의 value category를 보존시켜주는 특별한 종류의 reference이다.

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


std::forward 함수를 통해 value category를 보존하며 완벽하게 보존할 수 있다. Lvalue가 들어올때는 Lvalue refenre로 동작하고 Rvalue가 들어올떄는 Rvalue reference로 동작한다. 

그러면 universal reference는 어떻게 동작하는 것일까?

먼저 타입추론이 발생하는 곳에서 &&를 사용하면 universal reference가 된다. universal reference에서 


그리고 reference 겹침 규칙 

```cpp
template <typename T>
void wrapper(T&& u) { //T가 template parameter이기 때문에 타입추론이 발생한다.
  g(u);               //따라서 T&&는 T의 Rvalue reference가 아니라 Universal reference이다.
}
```


이 떄, reference의 reference가 발생하게 된다. reference의 reference는 정의되지 않은 표현식이기 때문에 이를 축약하여 정의된 표현식으로 만드는 규칙이 C++11에 도입되었으며 이를 `Reference collapsing` 규칙이라고 한다.

Reference collapsing 규칙은 다음과 같다.
```cpp
typedef int&  lref;
typedef int&& rref;
int n;
 
lref&  r1 = n; // type of r1 is int&
lref&& r2 = n; // type of r2 is int&
rref&  r3 = n; // type of r3 is int&
rref&& r4 = 1; // type of r4 is int&&
```

따라서 reference collapsing rule을 적용하면 다음과 같아진다.
```cpp
  wrapper(a);   // T가 A&로 추론 --> (A& & u) --> (A& u)
  wrapper(ca);  // T가 const A&로 추론 --> (const A& & u) --> (const A& u)
  wrapper(A()); // T가 A&&로 추론 --> (A&& & u) --> (A& u)
```

> Reference   
> [modoocode](https://modoocode.com/228)  
> [cppreference-forwarding references](https://en.cppreference.com/w/cpp/language/reference#Forwarding_references)  
> [cppreference-Deduction_from_a_function_call](https://en.cppreference.com/w/cpp/language/template_argument_deduction#Deduction_from_a_function_call)
> [cppreference-Implicit_instantiation](https://en.cppreference.com/w/cpp/language/function_template#Implicit_instantiation)
> [cppreference-Function_template](https://en.cppreference.com/w/cpp/language/function_template#Implicit_instantiation)
> [cppreference-template_argument_deduction](https://en.cppreference.com/w/cpp/language/template_argument_deduction)
 

### Universal References
Universal reference라는 표현은 Scott Meyers가 cv-unqualified template parameter의 Rvalue reference을 설명하기 위해 사용한 표현이다.

하지만 C++ standard에서는 이를 위한 표현을 정의하지 않았고 나중에 forwarding reference라는 표현을 도입하였다. 따라서 C++ standard 용어는 forwarding reference이고 널리쓰이는 universal reference와 정확히 동일한 의미를 갖는다.

> Reference  
> [stackoverflow-is-there-a-difference-between-universal-references-and-forwarding-reference](https://stackoverflow.com/questions/39552272/is-there-a-difference-between-universal-references-and-forwarding-references)  
> 



---

* movable이 무엇인가?
  * movable == 이동가능한 상태인가? == 현재 value가 가지고 있는 data가 이동해도 문제가 발생하지 않을 상태인가?
    * PRvalue는 향후에 사용이 불가능하기 때문에 PRvalue의 data가 이동해도 아무런 문제가 발생하지 않는다. 따라서 movable이다.
    * Lvalue는 향후에 사용이 가능하기 때문에 Lvalue의 data가 다른곳으로 이동하면 문제가 될 여지가 있다. 따라서 not movable이다.
  * swap 예제와 같이 경우에 따라서 move할 수 없는 lvalue를 move할 필요가 생긴다. --> lvalue를 rvalue reference로 casting하는 move 등장 -->   identity도 갖고 movable한 xvalue 등장! [blog](https://accu.org/journals/overload/27/150/knatten_2641/)
* identity가 무엇인가?
  * identity란 접근할 수 있는 주소를 가지고 있냐 못가지고 있냐의 차이이다.
    * Xvalue가 identity가 있지만, unary operator`&`로 접근이 안되는 이유는 `&`는 lvalue만 인자로 받기 때문이다. [what-does-it-mean-xvalue-has-identity](https://stackoverflow.com/questions/50783525/what-does-it-mean-xvalue-has-identity)
    * Xvalue의 identity를 보려면 rvalue to lvalue conversion을 해야 한다.[what-does-it-mean-xvalue-has-identity](https://stackoverflow.com/questions/50783525/what-does-it-mean-xvalue-has-identity)
    * `int&& r = f();` 처럼 prvalue를 return하는 함수도 identity가 있는거냐? 아니다! 지금 보는 identity는 Temporary materialization에 의해 생성된 xvalue인 temporary object의 identity를 보는 것이다.
  * [is-it-correct-to-say-that-xvalues-have-identity-and-are-movable](https://stackoverflow.com/questions/22430998/is-it-correct-to-say-that-xvalues-have-identity-and-are-movable)
  * [xvalue-has-identity-why-i-cant-cout-it-addreess](https://stackoverflow.com/questions/59332607/xvalue-has-identity-why-i-cant-cout-it-addreess)  
  * [xvalues-vs-prvalues-what-does-identity-property-add](https://stackoverflow.com/questions/45317763/xvalues-vs-prvalues-what-does-identity-property-add)
    * prvalue-to-xvalue conversion, "temporary materialization"
  * [clarifying-the-value-categories-of-expressions](https://stackoverflow.com/questions/69125113/clarifying-the-value-categories-of-expressions)
* lvalue, Xvalue, PRvalue의 예시들


* value category에 가장 기본(primary value categories)
  * lvalue
  * xvalue
  * prvalue
* identity가 있는 value category는 lvalue & xvalue --> glvalue
* movable한 value category는 xvalue & prvalue --> rvalue
* identity의 관점으로 value category를 나누면
  * identity가 있는 값 --> glvalue
  * identity가 없는 값 --> prvalue
* movable의 관점으로 value category를 나누면
  * movable한 값 --> rvalue
  * not movalbe한 값 --> lvalue
* movable 한지 안한지는 rvalue reference로 받아보면 알 수 있다.
  * rvalue reference로 binding이 된다 --> movable
  * rvalue reference로 binding이 안된다 --> not movable
  * rvalue refenrece란? [modoocode](https://modoocode.com/189#page-heading-6)  
* movable 할 수 없는 객체를 movable하게 만들어 주는게 std::move다.
* 이동이 가능한 값이란? [blog](https://aerocode.net/207)  
* glvalue와 prvalue만 정의가 되어 있고 나머지는 상대적으로 정의되어 있다. [stackoverflow](https://stackoverflow.com/questions/58703140/what-are-xvalues-in-c)
* 우측값 참조라 정의한 것들도 좌측값 혹은 우측값이 될 수 있다. 이를 판단하는 기준은, 만일 이름이 있다면 좌측값, 없다면 우측값이다. [modoocode](https://modoocode.com/189#page-heading-6)  
* Temporary materialization
  * A prvalue of any complete type T can be converted to an xvalue of the same type T. This conversion initializes a temporary object of type T from the prvalue by evaluating the prvalue with the temporary object as its result object, and produces an xvalue denoting the temporary object. [cppreference](https://en.cppreference.com/w/cpp/language/implicit_conversion#Temporary_materialization)
* Examples
  * [real-life-examples-of-xvalues-glvalues-and-prvalues](https://stackoverflow.com/questions/6609968/real-life-examples-of-xvalues-glvalues-and-prvalues)


> Reference
> [blog](https://dydtjr1128.github.io/cpp/2019/06/10/Cpp-values.html)  
> [what-do-they-mean-by-having-identity-but-not-movable-for-lvalue-in-c-11](https://stackoverflow.com/questions/44289402/what-do-they-mean-by-having-identity-but-not-movable-for-lvalue-in-c-11)  
> [meaning of indentity](https://stackoverflow.com/questions/53443234/whats-the-meaning-of-identity-in-the-definition-of-value-categories-in-c)  