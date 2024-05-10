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
capture는 `,`로 구분된 list로 lambda function body에서 접근 가능한 외부 변수들을 정의한다. 

이 때, 경우에 따라 capture-default `&`와 `=`로 시작할 수 있다.

capture-default로 `&`를 사용하는 경우 현재 automatic sotrage duration을 갖는 모든 변수들의 reference를 정의한다. 

```cpp
TEST(Lambda_Expression, capture1)
{
  int val = 10;

  auto f = [&]() { val = 20; };
  f();

  EXPECT_EQ(val, 20); // original val is changed to 20
}
```

그리고 capture-default로 `&`를 사용하는 경우, 이후에 이어지는 capture들은 `&`로 시작하면 안된다.

```cpp
TEST(Lambda_Expression, capture4)
{
  int val  = 10;
  int val2 = 20;

  //Error : If the capture-default is &, subsequent simple captures must not begin with &
  //auto g = [&, &val2]() { val = 20; };
  auto g = [&, val2]() { val = 20; };
  g();

  EXPECT_EQ(val, 20);
}
```

capture-default로 `[=]`로 쓰면 현재 automatic sotrage duration을 갖는 모든 변수들의 const-qualified copy를 정의한다. 단, specs 위치에 mutable을 사용할 경우 lambda expression body에서 copy한 객체를 수정할 수 있으며 non-cost member functions를 호출할 수 있다.

```cpp
TEST(Lambda_Expression, capture2)
{
  int val = 10;

  // compile error!
  // without mutable specifier cann't change val in the body
  // auto f = [=]() {
  //   val = 20; 
  // };
    
  auto f = [=]() mutable 
  {
    val = 20;
    EXPECT_EQ(val,20); // copied val is changed to 20
  };

  f();

  EXPECT_EQ(val, 10); // original val is still 10
}
```

그리고 capture-default로 `=`를 사용하는 경우, 이후에 이어지는 capture들은 `&`로 시작하거나 `*this` 혹은 `this`여야 한다.

만약 `*this`가 주어진 경우 객체가 복사가 된다. 

```cpp
TEST(Lambda_Expression, capture5)
{
  struct A {

    int val = 0;

    void func()
    {
      //capture the *this by copy
      auto f = [=, *this]() mutable {
        val = 3;
        EXPECT_EQ(val, 3); // val of copied (*this) is changed to 3
      };

      f();
    };
  };

  A a;
  a.func();

  EXPECT_EQ(a.val, 0); // val of original `a` is still 0
}

```


capture-default가 있으면 암시적으로 현재 객체(\*this)의 reference가 정의된다. 

`this`가 주어진 경우 C++ 20 이후에서는 `[=]`만 있는 것과 동일하게 취급된다.

```cpp
TEST(Lambda_Expression, capture6)
{
  struct A {

    int val = 0;

    void func()
    {
      // until C++20: Error: this when = is the default
      // since C++20: OK, same as [=]
      auto f = [=, this]() {
        val = 3;
        EXPECT_EQ(val, 3); // val of (*this) is changed to 3
      };

      f();
    };
  };

  A a;
  a.func();

  EXPECT_EQ(a.val, 3); // val of original `a` is changed to 3
}
```

단, 다음 외부 변수들의 경우 capture되지 않아도 접근 가능하다.
* non-local variables
* static or thread local storage duration variables
* reference that has been initialized with a constant expression
* variable is constexpr and has no mutable members

```cpp
TEST(Lambda_Expression, capture7)
{
  static int sval = 10;

  int val  = 10;
  auto f = [&val]() { val += sval; }; //static or thread local storage duration variables
  f();

  EXPECT_EQ(val, 20);
}

constexpr int gval = 10;

TEST(Lambda_Expression, capture8)
{
  constexpr const int& cref = gval;

  int  val = 10;
  auto f  = [&val]() { val += cref; }; // reference that has been initialized with a constant expression
  f();

  EXPECT_EQ(val, 20);
}

TEST(Lambda_Expression, capture9)
{
  constexpr int cval = 10;

  int  val = 10;
  auto f   = [&val]() { val += cval; }; //variable is constexpr and has no mutable members
  f();

  EXPECT_EQ(val, 20);
}
```

> Reference  
> [cppreference - lambda#Explanation](https://en.cppreference.com/w/cpp/language/lambda#Explanation)   
> [cppreference - lambda#Lambda_capture](https://en.cppreference.com/w/cpp/language/lambda#Lambda_capture)  
> [ezoeryou - 2019-07-09-deprecate-implicit-lambda-capture-of-this](https://ezoeryou.github.io/blog/article/2019-07-09-deprecate-implicit-lambda-capture-of-this.html)  
> [open-std - p0806r2](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0806r2.html)

### Params
Params는 closure의 operator()에 input으로 사용될 parameter list를 나타낸다.

> Reference  
> [cppreference - lambda](https://en.cppreference.com/w/cpp/language/lambda)  

### Trailing-type
Trailing type은 closure의 operator()에 return type을 명시하는 부분으로 `-> Type` 형태로 작성한다.

만약, trailing-type이 명시되어 있지 않은 경우에는 C++14 이후부터 `-> auto`로 작성된 걸로 간주하고 auto return type이 사용되며, 자동으로 return time이 추론된다.

> Reference  
> [cppreference - lambda](https://en.cppreference.com/w/cpp/language/lambda)   