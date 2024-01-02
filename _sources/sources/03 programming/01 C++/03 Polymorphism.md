# Polymorphism
`다형성(polymorphism)`이란 맥락에 따라서 다른 로직을 실행하는 능력으로 overloading과 overriding에 의해서 구현 가능하다.

overloading이란 이름은 같고 input은 다른 함수들을 정의하는 것이다.

```cpp
class Shape1;
class Shape2;
class Shape3;

//Polymorphism via overloading
double cal_area(const Shape1 shape);
double cal_area(const Shape2 shape);
double cal_area(const Shape3 shape);
```

overloading에 의해 맥락(어떤 input이 들어오느냐)에 따라 다른 로직이 실행되게 되는 polymorphism이 구현된다.

다음으로 overriding이란 base class의 virtual function을 derive class에서 재정의 하는 것이다.

```cpp
//Base.h
#include <iostream>
class Base
{
public:
    virtual void vfunc(void) { std::cout << "function in base\n"; };
};

//Derive.h
#include "Base.h"
class Derive : public Base
{
    void vfunc(void) override { std::cout << "function in derive\n"; };
};
```

overriding에 의해 virtual function이 dynamic binding이 되면 맥락(어떤 객체가 함수를 호출하냐)에 따라 다른 로직이 실행되게 되는 polymorphism이 구현됨과 동시에 의존성을 줄일 수 있다.

위의 문장을 이해하기 위해서는 다음 5가지를 알아야 한다.
* virtual function이 무엇인가?
* virtual function이 dynamic binding 된다는 건 무엇인가?
* virtual function이 dynamic binding 되면 왜 polymorphism이 구현되는가?
* virtual function이 dynamic binding 되면 왜 의존성이 줄어드는가?
* virtual function은 어떻게 dynamic bingding이 되는가?

이제, 하나씩 알아가 보자.

## 1. What is a Virtual Function?
virtual function이란 derive 클래스에서 재정의 될 것으로 기대하는 함수로  virtual keyword를 통해 나타낸다

## 2. What does it mean for a Dynamic Binding occur in Virtual Function?
virtual function이 dynamic binding 된다는 것은 여러 class에 정의되어 있는 virtual function을 호출할 때, 어떤 class의 virtual function을 호출할지가 run time에 결정되는 것이다. 

이를 이해하기 위해 다음 예시 코드를 보자.
```cpp
//Base.h
#include <iostream>
class Base
{
public:
    virtual void vfunc(void) { std::cout << "function in base\n"; };
};

//Derive.h
#include "Base.h"
class Derive : public Base
{
    void vfunc(void) override { std::cout << "function in derive\n"; };
};

//other.cpp
#include "Base.h"

void call_vfunc(const Base& base) { base.vfunc(); };

//main.cpp
#include "Derive.h"

void call_vfunc(const Base& base);

int main(void)
{
    Base b;
    call_vfunc(b);

    Derive d;
    call_vfunc(d);
}
```

call_vfunc을 보면 base는 Base class의 객체임으로 main문에서 call_vfunc의 input으로 어떤 객체가 들어오더라도 Base class의 vfunc이 호출될 것으로 보인다.

하지만 놀랍게도 Base class의 객체가 들어올때는 Base class의 vfunc을 Derive class의 객체가 들어올때는 Derive class의 vfunc 함수를 호출하는 것을 알 수 있다. 

즉, base 객체의 타입과 관계 없이 input으로 들어오는 객체의 실제 클래스의 함수가 호출이 되며 이는 `base.vfunc()` 코드에서 어떤 class의 함수를 호출할지가 run time에 어떤 input이 들어오는지에 따라 run time에 결정된다는 것이다.

이처럼 여러 class에 정의되어 있는 virtual function을 호출할 때, 어떤 class의 virtual function을 호출할지가 run time에 결정되는 경우를 virtual function이 dynamic binding 되었다고 한다.

## 3. Why Does Polymorphism Occur When a Virtual Function is Dynamically Bound?
virtual function이 dynamic binding 되면 맥락(호출하는 객체의 class)에 따라 다른 로직이 수행되는 polymorphism이 구현된다.

## 4. Why Does Dependency Decrease When Virtual Functions Are Dynamically Bound?
virtual function이 dynamic binding 되면 call_vfunc은 Derive class에 대해 compile time 의존성을 갖지 않아도 됨으로 의존성을 줄일 수 있는 것이다.

## 5. How does Dynamic Binding occurs in a Virtual Function?
virtual function이 dynamic binding이 되는 이유는 virtual function을 갖는 class는 `가상 함수 테이블(virtual function table; vftable)`을 그리고 그 class의 객체는 `가상 함수 테이블 포인터(virtual function table pointer; vfptr)`라는 추가적인 구조를 가지고 있고 virtual function은 일반 함수와 다른 호출 방식을 가지며 vfptr은 객체의 타입과 관계없이 항상 실제 클래스의 vftable을 가리키도록 설계가 되어 있기 때문이다. 

위의 문장을 이해하기 위해서 virtual function에 대해 조금더 자세히 알아보자.

virtual function이란 derive 클래스에서 재정의 될 것으로 기대하는 함수로 C++에서는 virtual keyword를 통해 나타낸다. virtual function은 일반 함수와 함수 그 자체로서는 큰 차이 없이 메모리 코드 영역의 어딘가에 위치할 뿐이다. 하지만 virtual function은 vftable과 vfptr이라는 추가적인 구조를 가지고 있으며 함수의 호출 방식도 다르다.

### vftable
vftable은 virtual function을 가지고 있는 class마다 하나씩 존재하는 table로 class에 정의된 virtual function들의 시작 주소가 기록되어 있는 table이다.

컴파일러는 컴파일 시점에 소스 코드에 정의된 모든 클래스에 대해서 virtual function이 하나라도 있을 경우 그 클래스에 대한 vftable을 생성한다. 

#### 참고
컴파일러는 vftable을 생성할 때 함수마다 고유의 인덱스를 부여하고 이 인덱스를 이용해서 virtual function 호출을 처리한다. 예를 들어 가상함수 vfunc의 인덱스가 0번이라면, vfunc가 호출될 시 vfptr이 가리키는 vftable의 0번 인덱스에 접근하여 함수를 호출하도록 어셈블리 코드를 작성한다. 즉, virtual function는 오직 vfptr이 가리키는 vftable과 virtual function에 해당하는 인덱스만으로 주소를 찾아내서 호출될 수 있다.

### vfptr
vfptr은 말 그대로 vftable을 가르키는 pointer다. 

virtual function을 가지고 있는 클래스의 객체가 생성될 때, 컴파일러에 의해 객체의 시작 위치에 vfptr이 생성되고 생성자의 선처리 영역에서 vfptr이 vftable을 가르키도록 설정된다.

클래스의 생성자와 소멸자는 각각 선처리 영역과 후처리 영역을 가지고 있으며 이를 간단하게 정리하면 다음과 같다.

```
A(...)
[// 선처리 영역 시작
1. base 클래스의 생성자 호출
2. vfptr 설정
3. 클래스 타입 멤버의 생성자 호출
4. 기타 선처리..
]// 선처리 영역 끝
{// 생성자 블록 시작
}// 생성자 블록 끝

~A(...)
{// 소멸자 블록 시작    
1. vfptr 설정
}// 소멸자 블록 끝
[// 후처리 영역 시작
1. 클래스 타입 멤버의 소멸자 호출
2. 부모 클래스 소멸자 호출
3. 기타 후처리..
]// 후처리 영역 끝
```

#### 참고
vfptr은 복사 생성자나 연산자에 의해서 복사되지 않는다. 그렇다면 왜 C++에서는 복사되지 못하게 막아놓은 것일까? 그 이유를 아래 코드를 보면서 이해해보자.

``` cpp
class Base
{
public:
	virtual void vfunc(void) { };
};

class Derive : public Base
{
public:
	void vfunc(void) override { this->d_val_ = 5; };

private:
	int d_val_ = 3;
};

int main(void)
{
	Base b;
	Derive d;

	b = d;
	b.vfunc();
}
```

만약 `b = d;` 코드에서 vfptr이 복사가 일어난다고 생각해보자. 그러면 b의 vfptr은 Derive 클래스의 vftable을 가리키고 있다. 다음에 vfunc()를 호출했다면 멤버변수 d_val_에 5를 대입해야 하지만 문제가 있다. b 객체는 멤버변수 d_val_이 없다! 존재하지도 않는 멤버변수에 접근하는 것은 미정의 동작을 일으킬 수 밖에 없으며 컴파일러는 복사 생성자에서 절대 vfptr을 복사하지 않는다.

가끔 자식 클래스 객체에서 부모 클래스 객체로 단순 값 복사를 위해 memcpy를 사용하기도 하는데 memcpy는 강제로 vfptr을 복사하기 때문에 사용에 주의를 기울여야 한다.

### Virtual function의 호출
virtual funtion의 함수 호출 과정을 알아보기 위해 아래의 어셈블리 코드를 살펴보자. 참고로 아래 어셈블리는 필요한 부분만 작성한 것이다.

```cpp
class Test
{
public:
    void func(void) const {};
    virtual void vfunc1(void) const {};
    virtual void vfunc2(void) const {};
};

int main(void)
{
    Test t;
    Test* pt = &t;

    pt->func();
    call Test::func        
    //비가상 멤버 함수의 경우 컴파일러는 직접 해당 함수의 메모리 주소를 call하는 어셈블리 코드를 작성한다.
    
    //dword ptr [name]
    //name이 레지스터명인 경우, 레지스터에 저장된 주소로 가서 저장된 데이터 4바이트를 읽어온다.
    //name이 변수명인 경우, 변수에 저장된 데이터 4바이트를 읽어온다.

    pt->vfunc1();
    mov eax,dword ptr [pt]  
    //pt 변수에 저장된 데이터 4바이트를 eax에 저장한다.
        //pt 변수는 t객체의 시작 주소가 저장되어 있다.
    //t객체의 시작 주소를 eax에 저장한다.
    
    mov edx,dword ptr [eax] 
    //eax에 저장된 주소로 가서 저장된 데이터 4바이트를 edx에 저장한다.
        //eax에는 t객체의 시작 주소가 저장되어 있다.
        //t객체의 시작 주소에는 vfptr의 값(vftable의 주소)이 저장되어 있다. 
    //vftable의 주소를 edx에 저장한다.

    mov eax,dword ptr [edx] 
    //edx에 저장된 주소로 가서 저장된 데이터 4바이트 eax에 저장한다.
        //edx에는 vftable의 시작 주소가 저장되어 있다.
        //vftable의 시작 주소에는 첫번째 가상함수(vfunc1)의 주소가 저장되어 있다.
    //vfunc1의 주소를 eax의 저장한다.

    call eax                
    //eax에 저장된 주소로 점프하여 실행한다.
        //eax에는 vfunc1의 주소가 저장되어 있다.
    //vfunc1 주소로 가서 함수를 실행한다.

    pt->vfunc2();
    mov eax,dword ptr [pt]
    mov edx,dword ptr [eax]
    mov eax,dword ptr [edx + 4]     
    //edx+4에 저장된 주소로 가서 저장된 데이터 4바이트 eax에 저장한다.
        //edx+4에는 두번째 가상함수(vfunc2)의 주소가 저장되어 있다.
    //vfunc2의 주소를 eax에 저장한다.
    call eax
}
```

정리하면 비가상 멤버함수는 컴파일러에 의해 바로 호출되는 반면 virtual function는 객체가 가지고 있는 vfptr를 이용해 vftable에 접근해 호출된다.

### Dynamic binding process
```cpp
//Base.h
#include <iostream>
class Base
{
public:
    virtual void vfunc(void) { std::cout << "function in base\n"; };
};

//Derive.h
#include "Base.h"
class Derive : public Base
{
    void vfunc(void) override { std::cout << "function in derive\n"; };
};

//other.cpp
#include "Base.h"

void call_vfunc(const Base& base) { base.vfunc(); };

//main.cpp
#include "Derive.h"

void call_vfunc(const Base& base);

int main(void)
{
    Base b;
    call_vfunc(b);

    Derive d;
    call_vfunc(d);
}
```

위의 예시코드를 가지고 한 단계씩 dynamic binding이 어떻게 발생하는지 보자.

먼저 컴파일러는 각 클래스에 virtual function이 있음을 확인하고 클래스 마다 vftable을 생성한다. Base 클래스의 vftable에는 항목 한 개가 있고 Base::vfunc의 주소가 들어간다. Derive 클래스의 vftable도 항목 한개가 있고 재정의한 Derive::vfunc의 주소가 들어간다. 

컴파일러에 의해 Derive 클래스의 d 객체의 맨 앞에는 vfptr이 생성되었으며 이후 생성자를 호출한다. Derive 클래스 생성자의 선처리 영역에서 Base 클래스의 생성자를 호출한다. Base 클래스의 생성자 호출의 선처리 영역에서 vfptr이 Base 클래스의 vftable을 가르키게 된다. 그리고 Base 클래스 생성자 호출이 끝나면 vfptr은 이제 Derive 클래스의 vftable을 가르키게 된다. 즉 여러 과정을 거쳐 결론적으로 vfptr이 Derive 클래스의 vftable을 가르키게 된다.

call_vfunc에 input으로 d 객체를 주었을 때, call_vfunction의 input 객체 base는 d 객체의 Base 클래스의 해당하는 영역뿐이다. 하지만 객체 d의 vfptr은 이미 Derive 클래스의 vftable을 가르키고 있기 때문에 vfptr을 이용하여 함수를 호출하더라도 원래 객체 d의 타입에 맞는 virtual function이 호출이 될 수 있다.

결론적으로  vfptr은 객체의 타입과 관계없이 항상 실제 클래스의 vftable을 가리키도록 설계가 되어 있기 때문에 dynamic binding이 가능하게 된다. 

## 참고
### C and Overloading
C는 overloading을 제공하지 않는다. 왜냐하면 컴파일러가 목적 코드를 생성할 떄 symbol을 함수 이름으로 생성하기 때문에 같은 이름이 있는 경우 어떤 함수를 호출할 지 구분할 수가 없게 되어 linking에 실패하기 때문이다.

그렇다면 C++에서는 어떻게 overloading을 제공하는 것일까? 정답은 `이름 맹글링(name mangling)`이다. C++에서는 목적 코드 생성시에 컴파일러가 함수의 이름을 바꾸는 것을 볼 수 있다. 이를 name mangling이라 하는데, 맹글링이라는 단어의 뜻이 원래 엉망진창으로 만들다 라는 의미다.

이렇게 name mangling을 하게 되면 원래의 함수 이름에 namespace 정보와 input 정보들이 추가된다. 따라서 같은 이름의 함수일 지라도, name mangling을 거치고 나면 다른 함수로 취급할 수 있게 되고 링킹을 성공적으로 수행할 수 있게 된다.

### Binding
`바인딩(binding)`이란 코드에 쓰인 이름 식별자들에 대해 값 또는 속성을 확정하여 변경할 수 없게 `묶는(bind)` 과정이다.

binding 시점에 따라 `정적 바인딩(static binding)`과 `동적 바인딩(dynamic binding)`으로 나뉘게 된다. static binding은 compile time에 binding이 일어나는 경우이고 dynamic binding은 run time에 binding이 일어나는 경우이다.

> Reference    
> [lesslate - 정적 바인딩과 동적 바인딩](https://lesslate.github.io/cpp/%EB%8F%99%EC%A0%81-%EB%B0%94%EC%9D%B8%EB%94%A9-%EC%A0%95%EC%A0%81%EB%B0%94%EC%9D%B8%EB%94%A9/)  


### Overriding abstract class
다음 예시 코드를 보자.

```cpp
//Shape.h
class Shape {
    virtual double cal_area(void) const = 0;
};

//Shape_Impl.h
class Shape1 : public Shape {
    double cal_area(void) const override;
};
class Shape2 : public Shape {
    double cal_area(void) const override;
};
class Shape3 : public Shape {
    double cal_area(void) const override;
};

//other.cpp
#include <Shape.h>

int print_area(const Shape& shape){
    cout << shape.cal_area() << "\n";
}

```

dynamic binding 덕분에 print_area 함수는 compile time 의존성은 오직 abstract class인 Shape class에 대해서만 갖는다. 그리고 concrete class인 Shape1,2,3 class에 대해서는 run time 의존성만을 갖는다.

즉, compile time 의존성은 오직 상대적으로 변경에 안전한 abstract class에 두고 상대적으로 변경에 취약한 concrete class들에 대해서는 run time 의존성만 갖게 하여 의존성을 더욱 줄일 수 있다.


> Reference  
> [blog - why-is-polymorphism-important](https://wasabigeek.com/blog/why-is-polymorphism-important/)  


### virtual function이 아닌 base class의 함수
만약 virtual function이 아닌 base class의 함수를 redefing하면 어떻게 될지 다음 예시를 보자.

```cpp
#include <iostream>

class A {
public:
    void f(void) {std::cout <<"A";}
}

class B : public A {
public:
    void f(void) {std::cout << "B";}
}

int main(void){
    B b;
    b.f(); //"B"

    A& a_ref = b;
    a_ref.f(); //"A"
}
```
이 경우에는 overriding에 의한 polymorphism이 없기 때문에 `a_ref.f();`에서 "A"가 출력된다.

그렇다면 이름만 같고 인자가 다른 경우에는 어떻게 될지 다음 예시를 보자.
```cpp
#include <iostream>

class A {
public:
    void f(void) {std::cout <<"A";}
}

class B : public A {
public:
    void f(int) {std::cout << "B";}
}

int main(void){
    int i = 0;

    B b;
    b.f(i); //"B"
    //b.f(); // compile error!
}
```
B가 A를 상속받았음으로 B에는 아무 input도 받지 않는 f와 int를 input으로 받는 f가 모두 정의되어 있을 것 같지만 f라는 이름이 같기 때문에 A class에 정의되어 있는 아무 input도 받지 않는 f는 가려지게 되는 base class method hiding 문제가 발생한다.

이를 해결하기 위해서는 B class에 명시적으로 A class의 f함수가 있음을 다음과 같이 작성해야 한다.

```cpp
class B : public A {
public:
    using A::f;
    void f(int) {std::cout << "B";}
}
```
그러면 더이상 `b.f()`에서 compile error가 발생하지 않는다.

### Virtual destructor
먼저 아래 코드를 보자.

```cpp
#include <iostream>

class A_Base
{
public:
    ~A_Base(void) { std::cout << "A_Base destructor\n"; };
};

class A_Derive : public A_Base
{
public:
    ~A_Derive(void) { std::cout << "A_Derive destructor\n"; };
};

class B_Base
{
public:
    virtual ~B_Base(void) { std::cout << "B_Base destructor\n"; };
};

class B_Derive : public B_Base
{
public:
    ~B_Derive(void) override { std::cout << "B_Derive destructor\n"; };
};

int main(void)
{
    A_Base* ptr1 = new A_Derive;
    B_Base* ptr2 = new B_Derive;
    delete ptr1;
    delete ptr2;
}
```

코드 실행 결과를 보면 A_ 관련 클래스의 경우 Derive의 소멸자가 호출되지 않았지만 B_ 관련 클래스의 경우 정상적으로 Derive의 소멸자와 Base의 소멸자가 호출된 것을 알 수 있다. dynamic binding의 구현 원리를 배웠음으로 ptr2를 delete할 때 "vfptr과 virtual function 테이블을 이용해서 B_Derive 클래스의 소멸자를 호출해서 아무런 문제가 없구나" 라고 이해해 볼 수 있다. 

근데 곰곰히 생각해보면 뭔가 이상하다. 엄연히 소멸자는 상속되지 않는다. 상속되지 않기 때문에 재정의도 불가능하다. 상식적으로 함수의 이름만 봐도 다르지 않은가? 즉, Base 클래스의 소멸자와 Derive 클래스의 소멸자는 별 상관 없는 멤버 함수라고 보는 것이 더 합당하다. 그러나 소멸자가 virtual function로 지정될 때는 마치 소멸자가 상속되면서 재정의되는 함수처럼 여겨진다. override 키워드도 사용할 수 있다. C++는 어떤 원리로 이런 이상한 현상을 용인 하는 것일까? 이를 이해하기 위해서는 소멸자의 진정한 모습을 살펴 볼 필요가 있다.

#### 소멸자와 파괴자
먼저 아래의 어셈블리 코드를 보자. 참고로 아래 어셈블리는 필요한 부분만 작성한것이다.

```cpp

class A 
{
public:
    ~A(){};
};

int main(void)
{
    auto* ptr = new A;
    delete ptr;
    //call        A::'scalar deleting destructor'

    A a;    
    //call        A::~A
}

```

delete를 사용할 경우 scalar deleting destructor 함수를 호출하고 스택에서 객체가 해제될 경우에는 바로 A::~A를 호출한다. 즉, delete 연산은 직접적으로 소멸자를 호출하는 것이 아니라 파괴자(destructor)를 호출하고 그 안에서 A::~A를 호출함으로써 소멸자를 간접 호출한다. 파괴자는 클래스의 소멸자를 호출해준 뒤 바로 heap에 생성된 클래스 객체 메모리를 해제하는 역할을 한다.

소멸자를 영어로 쓰면 destructor이기 때문에 소멸자와 파괴자를 구분하는데 불명확한 부분이 있따. 그러나 객체의 소멸 과정을 명확하게 이해하기 위해서는 두 함수를 분리해서 이해할 필요가 있다. 소멸자는 메모리 해제를 하지 않는다. 소멸자는 객체가 메모리에서 해제되기 직전에 정리할 것이 있다면 정리를 하는 것이다. 파괴자는 소멸자를 먼저 호출해 준 후에 실제 메모리 영역을 힙으로부터 반환해주는 역할을 수행한다.

파괴자에 대해 조금더 자세히 알아보자. 컴파일러는 모든 클래스에 대하여 파괴자를 멤버 함수로서 암시적으로 추가하며, 각 파괴자는 자신이 소속된 클래스의 소멸자를 호출하도록 정의된다. 따라서 상속된 클래스 관계에서 본다면 자식 클래스의 파괴자는 부모 클래스의 파괴자를 상속받은 후에 재정의한 것으로 볼 수 있다. 이것이 C++에서 사용하는 트릭의 핵심이다.

소멸자에 virtual 키워드를 붙일 때 실제로는 파괴자를 virtual function로 지정하는 것이다. 따라서 실제 virtual function 테이블에 소멸자의 주소를 넣는 것이 아니라 소멸자를 내부적으로 호출하는 파괴자의 주소를 넣는 것이다. 그래서 부모 클래스의 소멸자와 자식 클래스의 소멸자가 이름이 다르더라도 실제 virtual function 테이블에 들어가는 파괴자의 이름은 같기 때문에 마치 소멸자들이 일반 멤버 함수가 virtual function에 의해 동작하듯이 동작할 수 있는 것이다.

결론적으로 delete ptr1을 할때는 A_Base 클래스의 파괴자를 바로 호출하지만 delete ptr2를 할 때는 vfptr과 virtual function 테이블을 이용해 B_Derive 클래스의 파괴자를 호출하여 B_Derive 소멸자와 B_Base 소멸자가 둘 다 정상적으로 호출 된다. 

### 생성자와 소멸자에서 virtual function 호출

먼저 아래 코드를 보자.

```cpp
#include <iostream>

class Base
{
public:
    Base(void) { vfunc(); };
    ~Base(void) { vfunc(); };
    virtual void vfunc(void) { std::cout << "function in base\n"; };
};

class Derive : public Base
{
    void vfunc(void) override { std::cout << "function in derive\n"; };
};

int main(void)
{
    Derive d;
}
```

위의 코드를 실행하면 "function in base" 문구만 2번 출력된다. virtual function의 성질에 의하면 virtual function는 실제 타입 클래스의 함수가 호출되어야 한다. 따라서 d 객체의 타입은 Derive임으로 Derive의 생성자에서 Base의 생성자를 호출하고 Base의 생성자에서 호출하는 vfunc는 Derive::vfunc가 되어야만 할 것 같다. 그러나 Base의 생성자 안에서 호출된 vfunc는 Base::vfunc이다. 이렇게 의도와는 다르게 동작하니 주의가 필요하다.

그렇다면 이제 왜 이렇게 작동하는지 한단계씩 살펴보자.

먼저 생성자를 살펴보자. Derive 클래스의 d 객체를 생성하기 위해 생성자를 호출한다. 생성자 블록이 시작하기 전에 Derive 클래스의 생성자의 선처리 영역이 시작된다. 가장 먼저 Base 클래스의 생성자가 호출된다. Base 클래스의 생성자의 선처리 영역에서는 더이상 부모 클래스가 없음으로 바로 vfptr이 Base 클래스의 vftable을 가르키게 한다. 기타 선처리가 끝나고 이제 생성자 블록안에 vfunc이 실행된다. 이때 vfptr은 Base 클래스의 vftable을 가르키고 있음으로 Base::vfunc이 실행된다. 

이제 소멸자를 살펴보자. Derive 클래스의 d 객체를 소멸시키기 위해 소멸자를 호출한다. d 객체의 암시적 소멸자가 호출되며 vfptr이 Derive 클래스의 vftable을 가르킨다. 그다음 후처리 영역이 시작되면서 Base 클래스의 소멸자를 호출한다. Base 클래스의 소멸자가 호출되면 이제 vfptr이 Base 클래스의 vftable을 가르키게 되고 그 이후에 vfunc이 호출 됨으로 Base::vfunc이 실행된다.

결론적으로 생성자나 소멸자에서 호출하는 virtual function는 비가상 멤버 함수처럼 호출되게 되어있다. 이를 활용할 수도 있겠지만 오해의 소지가 큼으로 의도를 명확히 표현하기 위해서는 주석을 충분히 써넣거나 범위 연산자를 사용해서 Base/Derive::vfunc 식으로 virtual function를 호출하는 것이 바람직하다.

> Reference  
> {cite}`FundamentalC++`

### Performance
> Reference
> [blog](https://johnnysswlab.com/the-true-price-of-virtual-functions-in-c/)


