# Lambda Expression
Lambda expression이란 C++11에 추가된 기술로 한마디로 말하자면 이름 없는 함수를 만들어 내는 표현식이다. Lambda expression으로부터 런타임에 객체가 생성되며 이를 `클로져(closure)`라고 한다. 이 때, closure는 `함수 객체(Functor)`처럼 동작하며 그 동작은 lambda expression을 따른다.

> Reference  
> [modoocode - 씹어먹는 C++ 토막글 ② - 람다(lambda) 함수](https://modoocode.com/196)  
> [lunchballer - [C++] 클로져(Closures)가 그래서 무엇인가요](https://lunchballer.com/archives/284)  

## Structure of a Lambda Expression
```{figure} _image/2301.png
```

> Reference   
> [cppreference - lambda](https://en.cppreference.com/w/cpp/language/lambda)  
> [cppreference - lambda#Explanation](https://en.cppreference.com/w/cpp/language/lambda#Explanation)  

### Capture
capture는 `,`로 구분된 list로 lambda function body에서 접근 가능한 외부 변수들을 정의한다. 단, 다음 외부 변수들의 경우 capture되지 않아도 접근 가능하다.
* non-local variables
* static or thread local storage duration variables
* reference that has been initialized with a constant expression
* variable is constexpr and has no mutable members

capture를 `[&]`로 쓰면 현재 automatic sotrage duration을 갖는 모든 변수들의 reference를 정의한다. 

capture를 `[=]`로 쓰면 현재 automatic sotrage duration을 갖는 모든 변수들의 const-qualified copy를 정의한다. 단, sepcs 위치에 mutable을 사용할 경우 lambda expression body에서 copy한 객체를 수정할 수 있으며 non-cost member functions를 호출할 수 있다.

capture에 `[&]`나 `[=]`가 있으면 암시적으로 현재 객체(*this)의 reference가 정의된다. `[=]`가 있을 때, (*this)의 reference가 정의되는 것은 C++20 이후로 더이상 사용되지 않는다.

caputre의 동작을 보기 위해 다음 예시코드를 보자.
```cpp
TEST(Lambda_Expression, capture)
{
  struct A
  {
    int  val = 0;
    void func()
    {
      const int&       cref         = 10;
      constexpr int    cexpr        = 20;
      constexpr double cexpr_double = 20.0;

      // until C++20: current object(*this) is implicitly captured by reference
      // since C++20: The implicit capture of *this when the capture default is = is deprecated.
      // Despite being deprecated, there are currently no compile errors in MSVC.
      // reference that has been initialized with a constant expression can use without capture
      [=]() { val = cref; }();
      EXPECT_EQ(val, 1);

      // until C++20: Error: this when = is the default
      // since C++20: OK, same as [=]
      // variable is constexpr and has no mutable members can use without capture
      [=, this]() { val = cexpr; }();
      EXPECT_EQ(val, 2);

      // since C++17: captures the object (*this) by copy
      // To modify the object, the lambda must be mutable
      [=, *this]() mutable { val = cexpr_double; }();
      EXPECT_EQ(val, 2);
    }
  };

  A a;
  a.func();
}
```

> Reference  
> [cppreference - Lambda_capture](https://en.cppreference.com/w/cpp/language/lambda#Lambda_capture)  
> [ezoeryou - 2019-07-09-deprecate-implicit-lambda-capture-of-this](https://ezoeryou.github.io/blog/article/2019-07-09-deprecate-implicit-lambda-capture-of-this.html)  
> [open-std - p0806r2](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0806r2.html)


