# Class
## 클래스 선언과 정의
일반적으로 객체(변수, 배열)나 함수가 가상 메모리 영역을 차지하는 것에 비하여 클래스는 오직 개념적인 구조로서만 존재할 뿐이다. 

즉, 클래스는 메모리 영역과는 전혀 상관이 없다.

아래의 예시 코드를 보자.

```cpp
//A.cpp TU1
int g_val;
class Test
{
public:
    int m_val;
}

//Main.cpp
extern int g_val;
class Test;

void main(void)
{
    g_val = 1;

    Test t;         //compile error
    t.m_val = 1;    //compile error
}
```

선언을 통해서 g_val이 int 전역 변수라는 것을 알고 있다. 따라서 컴파일러는 g_val이 4byte의 메모리 영역을 차지하고 있음을 알 수 있다. 이로써 컴파일 단계에서는 어셈블리로 특정 주소를 시작으로 4byte 영역을 1로 채우는 코드를 작성하면 되고 링크 단계에서는 특정 주소를 실제 g_val이 위치하는 주소로 변경하면 된다.

그러나 class의 경우에는 문제가 달라진다. Test 객체를 생성해야 하는데 Test의 크기를 알 수가 없다. Test의 크기를 알기 위해서는 정의가 필요하다. A.cpp에 Test의 정의가 있지만 이는 오직 TU1에서만 알 수 있다. 따라서 컴파일러는 여지없이 에러를 발생시키는 것이다.

컴파일러에게 필요한 것은 Test가 클래스라는 선언이 아니라 Test의 설계도인 정의이다. 결국 Main.cpp에도 Test를 정의해줘야 한다. 결국 클래스의 정의는 사용되는 모든 소스(cpp) 파일마다 정의되어야만 하기 때문에 일반 객체나 함수의 정의와는 다르게 중복을 허용한다.

클래스의 정의가 내용이 많을 경우 클래스를 사용하는 소스 파일마다 클래스의 정의를 똑같이 붙여 넣는 것은 분명히 비효율적인 방법이다. 그래서 클래스의 정의는 보통 헤더 파일에 작성하고, 클래스 내부의 멤버 함수의 정의 같은것은 소스 파일에 작성한다.

이렇게 클래스 정의가 중복될 수 있음으로 문제가 발생하기도 하는데 바로 중복되는 정의가 서로 다른 경우이다. 아래의 예제를 보자.

```cpp
//A.cpp
class Test
{
public:
    Test(const int val1, const int val2)
        : val1_(val1)
        , val2_(val2) {};

    int val2_;
    int val1_;
};

extern Test g_test;

int get_val1(void)
{
    return g_test.val1_;
}

//Main.cpp
class Test
{
public:
    Test(const int val1, const int val2)
        : val1_(val1)
        , val2_(val2) {};

    int val1_;
    int val2_;
};

Test g_test(1, 2);

int get_val1(void);

void main(void)
{
    std::cout << get_val1() << "\n"; // 2가 출력됨
}

```
코드를 살펴보면 1이 출력되야 될 것 같지만 실제 결과는 2가 출력이 된다. 

이를 이해해 보기 위해 먼저 A.cpp와 Main.cpp에 클래스 Test가 중복으로 정의되어 있지만 멤버 변수가 정의된 순서가 다르다는걸 알아야한다. 

Main.cpp에서 `g_test.val1_;`이 의미하는 것은 g_test의 메모리 영역 중에서 `첫 번째 멤버 변수`를 나타내지만  A.cpp에서 `g_test.val1_;`이 의미하는 것은 g_test의 메모리 영역 중에서 `두 번째 멤버 변수`를 나타낸다는 점에서 문제가 발생한다.

Main.cpp에서 `Test g_test(1, 2);`이 실행될 때는 이미 첫번째 4btye에는 1을 두번째 4byte에는 2를 작성하였다. 이후에 A.cpp에서 `g_test.val1_;`이 실행될 때 두번째 4byte 값을 읽기 때문에 결과로 2가 출력되는 것이다.

여기서 알 수 있는 점은 클래스의 멤버 변수에 접근하는 것은 클래스 정의에 따라 해당 멤버 변수의 메모리 오프셋에 의해서 계산된 위치를 기준으로 한다는 점이다. 

즉, 멤버 변수의 이름은 오직 오프셋 계산을 위해서 사용되는 것이지, 그 자체가 하나의 고정된 영역을 나타내는 것은 아니라는 점이다.

보통은 이름만 같고 정의가 완전히 다른 경우에는 런타임 에러가 발생해서 어느 정도 문제를 빠르게 확인할 수 있을수도 있지만, 같은 라이브러리를 사용하지만 버전이 서로 다른 것이 혼합되어 있을 경우 클래스 정의가 유사하지만 약간 다른 부분이 가끔씩 문제를 일이클 경우에는 어디서 문제가 잘못되었는지 파악하기 어려울 수 있다.


> Reference  
> {cite}`FundamentalC++`

## 클래스의 멤버 함수의 링크 방식
클래스 멤버 함수의 링크 방식은 외부 링크 방식이다.

아래의 예제 코드를 통해 확인해보자

```cpp
// A.h
#include <iostream>

class A
{
public:
	void tell(void) const;
};

// A.cpp TU1
#include "A.h"

void A::tell(void) const
{
	std::cout << "I'm A\n";
}

// Main.cpp TU2
int main() {
	A a;
	a.tell();
}
```

TU2에서 TU1에 정의되어 있는 함수를 사용하였음으로 TU1에 정의된 A class의 tell 멤버 함수는 외부 링크 방식임을 알 수 있다.


## 익명 클래스 
익명 클래스(unnamed, anonymus class)는 이름이 주어지지 않은 클래스이다.

익명 클래스는 생성자는 갖을 수 없으나 소멸자는 갖을수 있으며 함수의 인자나 리턴 값으로 사용될 수 없다.

아래의 예시 코드를 보자.

```cpp
#include <iostream>

class //익명 클래스
{
public:
    void setData(int i)
    {
        this->i = i;
    }
    void print()
    {
        std::cout << "Value for i : " << this->i << std::endl;
    }

private:
    int i;       
} obj1;     //익명 클래스의 객체
            //obj1, obj2, ...; 와 같이 ,로 연결하여 여러개를 생성할 수 있음
  
int main()
{
    obj1.setData(10);   //익명 클래스의 객체의 저장 기간(stroage duration)과 링크 방식(linkage)??
    obj1.print();
    return 0;
}
```


> Reference  
[geeks for geeks](https://www.geeksforgeeks.org/anonymous-classes-in-cpp/)

## 생성자

### Default move constructor
별도로 move constructor를 설정하지 않아도 member variable을 move로 옮기는 move constructor가 생성된다.

아래 코드를 확인해보자.

```cpp
#include <iostream>

class A {
 public:
  A(void) = default;
  A(const A& a) { std::cout << "copy constructor\n"; }
  A(A&& a) { std::cout << "move constructor\n"; }
};

class B {
 public:
  B(void) = default;

 private:
  A a;
  A b;
  A c;
};

int main() {
  B b;
  B c(std::move(b));
 //move constructor
 //move constructor
 //move constructor
```

Class `B`는 별도의  move constructor가 없지만 객체 `c`를 생성할 때, rvalue `b`를 넘겨줌으로써 default move constructor가 호출이 되고 멤버변수 `a,b,c`에 대해서 각각 move constructor를 호출하여 move constructor 문장이 3번 나오게 된다.

### explicit
explicit 키워드는 자신이 원하지 않은 형변환이 일어나지 않도록 제한하는 키워드이다.

아래 예제코드를 통해 생성자에 explicit이 있을때와 없을 때의 차이를 알아보자. 
```cpp
struct A
{
    A(int) { }      // converting constructor
    A(int, int) { } // converting constructor (C++11)
    operator bool() const { return true; }
};
 
struct B
{
    explicit B(int) { }
    explicit B(int, int) { }
    explicit operator bool() const { return true; }
};
 
int main()
{
    A a1 = 1;      // OK: copy-initialization selects A::A(int)
    A a2(2);       // OK: direct-initialization selects A::A(int)
    A a3 {4, 5};   // OK: direct-list-initialization selects A::A(int, int)
    A a4 = {4, 5}; // OK: copy-list-initialization selects A::A(int, int)
    A a5 = (A)1;   // OK: explicit cast performs static_cast
    if (a1) ;      // OK: A::operator bool()
    bool na1 = a1; // OK: copy-initialization selects A::operator bool()
    bool na2 = static_cast<bool>(a1); // OK: static_cast performs direct-initialization
 
//  B b1 = 1;      // error: copy-initialization does not consider B::B(int)
    B b2(2);       // OK: direct-initialization selects B::B(int)
    B b3 {4, 5};   // OK: direct-list-initialization selects B::B(int, int)
//  B b4 = {4, 5}; // error: copy-list-initialization does not consider B::B(int,int)
    B b5 = (B)1;   // OK: explicit cast performs static_cast
    if (b2) ;      // OK: B::operator bool()
//  bool nb1 = b2; // error: copy-initialization does not consider B::operator bool()
    bool nb2 = static_cast<bool>(b2); // OK: static_cast performs direct-initialization
}
```
> Reference  
> [explicit - cppreference](https://en.cppreference.com/w/cpp/language/explicit)


### copy list initialization
``` cpp
class A {
public:
  template <typename... Args> 
  A(Args... args) { std::cout << "calling constuctor\n"; }
  
  A(const A& a) { std::cout << "calling copy constructor\n"; }
};

int main(void)
{
  A a, b, c;
  A d = { a,b,c }; // A(A arg1, A arg2, A arg3) copy is occur here
}
```
### User define deduction guide
```cpp
//user-defined deduction guides
template <typename... Args>
EuclideanVector(Args... args)->EuclideanVector<sizeof...(Args)>;
EuclideanVector(const std::vector<double>& vec)->EuclideanVector<0>;
```


### Precautions

#### Virtual Functions
Pure virtual function can not use in `base class constructor` and virtual function call can be invoke unwanted behavior.

> Reference  
> [stackoverflow](https://stackoverflow.com/questions/8630160/call-to-pure-virtual-function-from-base-class-constructor)


#### variadic template with perfect forwarding
copy constructor doesn't work.
```cpp
class A
{
public:
	template <typename ... Args> A(Args&&... args)
		: values_(std::forward<Args>(args)...) {};

private:
	std::vector<double> values_;
};

int main(void) 
{
	std::vector<A> a(50);
	std::vector<A> b;

	b = a;

	std::cout << "Debug";
}
```

## 상속
### 부모클래스 매서드 숨김 문제
먼저 아래 예제코드를 보자.

``` cpp
#include <iostream>

class A
{
public:
	int test(void) const
	{
		return 1;
	}
};

class B : public A
{
public:
	int test(int a) const
	{
		return 2;
	}
};

int main(void)
{
	const B b;
	std::cout << b.test(); //Error!

	return 0;
}
```

B클래스는 A클래스를 상속하였음으로 인자를 받지 않는 test함수가 클래스 B에 존재하고 int를 인자로 받는 또 다른 test함수가 overloading 될 것으로 기대한다.

하지만 예제코드에서 알 수 있듯이, int를 인자로 받는 test함수가 정의가 되면 A 클래스에 있는 test 함수가 숨겨지고 에러가 발생한다.

기대와 다르게 클래스 A와 B사이에는 어떠한 overloading도 발생하지 않는다. 

그 이유는 overloading은 개념적으로 하나의 범위(scope)만을 살펴보기 때문이다. 

예제 코드를 보면 객체 b가 test 함수를 호출할 때 컴파일러는 B 클래스의 범위를 살펴본 뒤 int 타입을 인자로 받는 함수만을 찾게된다. 

A 클래스에 정의된 test함수는 A 클래스의 범위에 속하기 때문에 컴파일러가 overloading을 위해 하나의 범위만을 살펴본다는 기본 규칙에 의해 살펴보는 범위가 아니게 된다. 

따라서 컴파일러는 인자를 받지 않는 함수를 찾지 못해 에러를 발생시킨다. 

C++에서는 범위를 넘어서는 overloading을 지원하지 않으며 이 기본규칙은 상속관계에 있는 Base - Derived 클래스의 경우일지라도 일관되게 적용된다.

이 문제는 다음 방법으로 해결할 수 있다.
``` cpp
#include <iostream>

class A
{
public:
	int test(void) const
	{
		return 1;
	}
};

class B : public A
{
public:
	using A::test;
	int test(int a) const
	{
		return 2;
	}
};

int main(void)
{
	const B b;
	std::cout << b.test();

	return 0;
}
```

숨겨진 함수를 using을 사용한 선언(declaration)을 통해 드러낼 수 있다.


> Reference   
[stackoverflow](https://stackoverflow.com/questions/1896830/why-should-i-use-the-using-keyword-to-access-my-base-class-method)  
[CppFAQ](https://isocpp.org/wiki/faq/strange-inheritance#hiding-rule)

### Derive 클래스의 복사
다음 예제 코드를 보자
```cpp

#include <iostream>

class A
{
public:
	A(void) = default;
	A(const A& a) { std::cout << "A copy constructor\n"; };
	
	A& operator=(const A& a) { std::cout << "A copy operator\n"; return *this; };
};

class B : public A
{
public:
	B(void) = default;
	B(const B& b) { std::cout << "B copy constructor\n"; };             //wrong
	//B(const B& b) : A(b) { std::cout << "B copy constructor\n"; };    //right

	B& operator=(const B& other) { std::cout << "B copy operator\n"; return *this; };   //wrong
	//B& operator=(const B& other) { A::operator=(other); std::cout << "B copy operator\n"; return *this; }; //right
};

int main(void)
{
	B b1, b2;
	
	B b3 = b1;

	b2 = b1;

	return 0;
}

```

코드를 실행시킬경우 Parent 클래스와 관련된 복사는 발생하지 않는다. 

이런 문제를 해결하기 위해서 Derive 클래스의 복사생성자를 작성할 때는 initializer list를 사용해서 Parent 클래스의 복사생성자가 먼저 호출되게 해줘야 한다. 

또한 Derive 클래스의 복사 연산자를 구현할 때도 가장 먼저 Parent class의 복사 연산자가 먼저 호출되게 해줘야 한다. 

이러한 과정이 주석처리된 코드에 나타나있다.

### 부모 클래스의 protected member 함수
다음 예제 코드를 보자
```cpp

#include <iostream>

class A
{
protected:
    void test(void);
};

class B : public A
{
public:
    void test(void)
    {
        A a;
        //a.test(); //Error!
        this->test();
    }
};

```

protected acess는 `B` class가 가지고 있는 `A`class 객체에만 적용된다.

새롭게 생성한 `a` 객체는 


The protected access only applies to parent members of your own current object type. You don't get public access to the protected members of other objects of the parent type.

> Reference  
> [stackoverflow](https://stackoverflow.com/questions/24636234/access-to-protected-constructor-of-base-class)  

## Friend Class

### Across namespace
```cpp
//header1.h

//class ms::A; // doesn't work!

namespace ms {
	class A;
}

class B {
private:
	friend class ms::A;
private:
	double value = 3.14;
};

//header2.h
#include "header1.h"

namespace ms {
	class A
	{
	public:
		A() {
			B b;
			b.value = 5.14; // can access!
		};
	};
}

```

## Member variable
```cpp
struct X
{
	inline int val4;	// 정의한것	
						// 초기화되지 않은 전역 변수로 BSS 영역에 저장
						// 0으로 초기화됨								
};
```