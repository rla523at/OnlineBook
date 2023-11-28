# Perfact Forward + Universal Reference
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