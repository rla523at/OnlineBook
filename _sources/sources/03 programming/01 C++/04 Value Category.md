# Value Category
C++에서는 다음과 같은 value category를 갖는다.

```{figure} _image/0301.png
```

여기에서 Lvalue, Xvalue, PRvalue를 primary value category라고 한다. C++에 모든 value는 primary value cateogry중 하나이다.

다음으로 have identity 특성을 갖는 Lvalue와 Xvalue를 묶어서 GLvalue라고 부르고 can be moved 특성을 갖는 Xvalue와 PRvalue를 묶어서 Rvalue라고 부른다. 그리고 GLvalue와 Rvalue를 `mixed value category`라고 한다.

## Move
can be moved from에서 move의 가능여부는 value가 향후에 쓰일 일이 없어서 현재 가지고 있는 data가 이동해도 문제가 발생하지 않는가와 관련이 있고 `std::move`가 가능한가와는 전혀 관계가 없다.

예를 들어, value에 다시 접근이 불가능한 경우 data가 이동되도 문제가 없기 때문에 can be moved이며 다음과 같은 예시가 있다.
* Literal(1,true,nullptr)
* 레퍼런스가 아닌 타입을 return하는 함수에 의해 생기는 임시객체
* 후위연산자 `a++`에 의해 return되는 값
  * return되는 값은 증가되기 전 원래 값을 가지고 있는 임시객체이다.

> Reference    
> [cppreference - prvalue](https://en.cppreference.com/w/cpp/language/value_category#prvalue)  
> [cppreference - xvalue](https://en.cppreference.com/w/cpp/language/value_category#xvalue)  

그리고 향후에 접근이 가능한 경우 data가 이동되면 문제가 발생할 수 있기 때문에 can not be moved from이 되며 다음과 같은 예시가 있다.
* the name of a variable, a function
* 전위연산자`++a`에 의해 return되는 값  
* lvalue 레퍼런스를 return하는 함수의 output
* string literal("abc")
  * [stackoverflow - why-string-literal-is-not-prvalue](https://stackoverflow.com/questions/41350090/why-string-literal-is-not-prvalue)  

> Reference  
> [cppreference - lvalue](https://en.cppreference.com/w/cpp/language/value_category#lvalue)



## Identity
identity란 접근할 수 있는 주소를 의미한다. 그래서 주소를 보기 위해 unary operator`&`를 이용할 수 있다. 따라서, Lvalue인 string literal은 `std::cout << &("abc");`이 된다.

하지만 Xvalue는 identity가 있지만 unary operator`&`로 직접 주소 보기가 안된다. 왜냐하면 `&`는 Lvalue만 인자로 받기 때문이다. 따라서 Xvalue의 identity를 보려면 Rvalue to Lvalue conversion을 해야 한다.

다음 예시를 보자.

```cpp
class A{};

A f(void) { return A();};

int main(void)
{
    A a;
    //cout << &std::move(a); compile error!
    A&& rref1 = std::move(a);   // Rvalue to Lvalue conversion
    cout << &rref1;             // print Xvalue adress    

    A&& rref2 = f();
    cout << &rref2;
}
```

이 때, rref2의 주소도 출력이 된다. 그렇다면 PRvalue의 주소를 출력한 것임으로 PRvalue도 identity를 갖는 것일까? 아니면 f가 return하는 값이 PRvalue가 아닌것일까? 정답은 둘다 아니다. rref2는 Temporary materialization에 의해 f가 return한 PRvalue로부터 생성된 Xvalue인 임시 객체의 identity를 보는 것이다. 

> Reference  
> [stackoverflow - what-does-it-mean-xvalue-has-identity](https://stackoverflow.com/questions/50783525/what-does-it-mean-xvalue-has-identity)  
> [cppreference-Temporary_materialization](https://en.cppreference.com/w/cpp/language/implicit_conversion#Temporary_materialization)

## History of Change of Value Category 
C++은 어쩌다가 이렇게 복잡한 value cateogry를 갖게 되었을까? C++도 처음부터 이렇게 복잡한 value category를 가지고 있었던 것은 아니였다. C++11 이전에는 Lvalue와 Rvalue라는 value category만 있었으며 identity가 있으면 Lvalue 없으면 Rvalue라는 간단한 구분 기준을 가지고 있었다.

하지만 `우측값 참조(rvalue reference)`와 std::move 연산이 들어오면서 더이상 identity만으로 구분이 안되는 상황이 발생하기 시작했다. 구체적인 상황을 보기 앞서 Rvalue reference와 std::move를 이해하기 위해 다음 상황을 가정하자.

X 를 어떠한 리소스에 대한 포인터(예를 들어 m_pResource) 를 담고 있는 클래스라고 생각하자. 참고로 여기서 리소스 라고 말하는 것은, 생성 또는 복사, 소멸 하기에 많은 시간이 걸리는 거대한 어떤 무언가를 의미한다. 클래스 X 의 가장 좋은 예로 std::vector 를 들 수 있다.

### Rvalue reference
다음 예시를 통해, Rvalue reference가 왜 필요한지 어떤역할을 하는지 알아보자.

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

하지만 위 과정은 단순히 생각해보아도, 임시로 생성된 객체 리소스를 굳이 복사할 필요가 없다. 그냥 임시로 생성된 객체가 가지고 있는 리소스를 가르키는 포인터와 x의 리소스를 가르키는 포인터를 서로 교환(swap)만 해주면 된다. 어짜피 임시 객체는 향후에 다시 사용될 일이 없기 때문에 포인터가 어디를 가르켜도 아무런 문제가 생기지 않는다. 그리고 이 과정을 임시 객체의 리소스가 x로 이동되었다고 표현합니다.

따라서 향후에 다시 사용될 일이 없는 Rvalue인 경우 복사연산 없이 교환만 하도록 구현하기 위해 Rvalue들을 나타낼 수 있는 Rvalue reference를 도입한다. 

> Reference  
> [modoocode](https://modoocode.com/189)

### std::move
이번에는 다음 예시를 통해 std::move가 왜 도입이 되었고 어떤 역할을 하는지 알아보자.

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

하지만 문제는 tmp, a와 b가 Rvalue가 아니여서 Rvalue를 input으로 받는 함수를 호출할 수 없다는 점이다.

C++ 표준의 첫 번째 십계명은 "우리 C++ 위원회는 C++ 프로그래머의 (때론 무모하더라도) 자유를 막는 규칙을 만들면 안된다 - The committee shall make no rule that prevents C++ programmers from shooting themselves in the foot 라고 명시되어 있다. 그래서 이러한 신념을 바탕으로 C++ 11 에서는 Lvalue를 Rvalue인 Rvalue reference로 바꿔주는 `std::move`연산을 제공한다. std::move를 통해 다음과 같이 swap을 바꿀 수 있다.

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

### Xvalue
이제, Rvalue reference와 std::move 연산이 어떻게 identity만으로 구분이 안되는 value를 만들어 내는지 보자.

Lvalue를 Rvalue reference로 casting하는 move의 결과는 identity를 갖으면서도 type casting을 통해 명시적으로 can be moved from의 성질을 부여한 value가 된다. 

이처럼 identity도 갖고 can be moved from한 변수를 Xvalue로 구분합니다. 

> Reference  
> [Blog](https://accu.org/journals/overload/27/150/knatten_2641/)







### 참고



* [is-it-correct-to-say-that-xvalues-have-identity-and-are-movable](https://stackoverflow.com/questions/22430998/is-it-correct-to-say-that-xvalues-have-identity-and-are-movable)
* [xvalue-has-identity-why-i-cant-cout-it-addreess](https://stackoverflow.com/questions/59332607/xvalue-has-identity-why-i-cant-cout-it-addreess)  
* [xvalues-vs-prvalues-what-does-identity-property-add](https://stackoverflow.com/questions/45317763/xvalues-vs-prvalues-what-does-identity-property-add)
* [clarifying-the-value-categories-of-expressions](https://stackoverflow.com/questions/69125113/clarifying-the-value-categories-of-expressions)
* [modoocode](https://modoocode.com/189#page-heading-6)  
* glvalue와 prvalue만 정의가 되어 있고 나머지는 상대적으로 정의되어 있다. [stackoverflow](https://stackoverflow.com/questions/58703140/what-are-xvalues-in-c)
* 우측값 참조라 정의한 것들도 좌측값 혹은 우측값이 될 수 있다. 이를 판단하는 기준은, 만일 이름이 있다면 좌측값, 없다면 우측값이다. [modoocode](https://modoocode.com/189#page-heading-6)  

* [real-life-examples-of-xvalues-glvalues-and-prvalues](https://stackoverflow.com/questions/6609968/real-life-examples-of-xvalues-glvalues-and-prvalues)
* [blog](https://dydtjr1128.github.io/cpp/2019/06/10/Cpp-values.html)  
* [what-do-they-mean-by-having-identity-but-not-movable-for-lvalue-in-c-11](https://stackoverflow.com/questions/44289402/what-do-they-mean-by-having-identity-but-not-movable-for-lvalue-in-c-11)  
* [meaning of indentity](https://stackoverflow.com/questions/53443234/whats-the-meaning-of-identity-in-the-definition-of-value-categories-in-c)  