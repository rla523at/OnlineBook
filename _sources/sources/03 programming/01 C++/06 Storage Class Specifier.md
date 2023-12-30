# Storage Class Specifier
`저장 방식 지정자(storage class specifier)`는 심볼들의 `저장 기간(Storage duration)`과 `링크 방식(Linkage)`이라는 두 가지 중요한 정보들을 지정하는 값으로 C++에서는 총 5가지가 있다.

|storage class specifier|storage duration|linkage|
|:---:|---|---|
|(no specifier)|automatic storage duration|-|
|static|static or thread storage duration|internal linkage|
|extern|static or thread storage duration|external linkage|
|thread_local|thread storage duration|-|
|mutable|-|-|

참고로, 이전에는 auto 와 register 지정자들도 있었는데 auto는 C++11에서 type deduction을 나타내는 명칭으로 쓰이고 no specifier로 바뀌었으며 register C++17에서 사라졌다. 

> Reference  
> [cppreference - storage_duration](https://en.cppreference.com/w/cpp/language/storage_duration)  

## Storage Duration
프로그램에서의 모든 객체들의 경우 반드시 아래 넷 중에 한 가지 방식의 storage duration을 가지게 된다.

> Reference  
> [cppreference - storage_duration](https://en.cppreference.com/w/cpp/language/storage_duration)  


### Automatic storage duration
여기에 해당하는 객체들은 보통 {} 안에 정의된 객체들로 코드 블록을 빠져나가게 되면 자동으로 소멸하게 된다. static, extern, thread_local로 지정된 객체들 이외의 모든 지역 객체들이 automatic storage duration을 가지게 된다. 쉽게 말해 우리가 흔히 생각하는 지역 변수들이 여기에 해당된다.

```cpp
int func() 
{
  int a;            //자동 저장 기간
  SomeObject x;     //자동 저장 기간

  {
    std::string s;  //자동 저장 기간
  }
}
```

### static storage duration
static 저장 기간에 해당하는 객체들은 프로그램이 시작할 때 할당 되고, 프로그램이 끝날 때 소멸되며 유일하게 존재한다. 예를 들어서 지역 변수의 경우 만일 여러 쓰레드에서 같은 함수를 실행한다면 같은 지역 변수의 복사본들이 여러 군데 존재하겠지만 static 객체들은 이 경우에도 유일하게 존재한다.

보통 함수 밖에 정의된 것들이나 (즉 namespace 단위에서 정의된 것들) static 혹은 extern 으로 정의된 객체들이 static 저장 기간을 가진다. 

참고로 static keyword와 static storage duration을 구분해야 한다. static keyword가 붙은 객체들이 static storage duration인것은 맞지만, 다른 방식으로 정의된 것들도 static 저장 기간을 가질 수 있다.

```cpp
int a;              // 전역 변수, static 저장 기간
namespace ss 
{
    int b;          // static 저장 기간
}

extern int a;       // static 저장 기간
int func() 
{
  static int x;     // static 저장 기간
}
```

### thread storage duration
`쓰레드(thread)` 저장 기간에 해당하는 객체들은 thread가 시작할 때 할당 되고, thread가 종료될 때 소멸되며 각 thread들이 해당 객체들의 복사본들을 가진다. 

thread_local로 선언된 객체들이 이 thread 저장 기간을 가질 수 있다.

```cpp
#include <iostream>
#include <thread>

thread_local int i = 0;

void g() { std::cout << i; }

void threadFunc(int init) {
  i = init;
  g();
}

int main() {
  std::thread t1(threadFunc, 1);
  std::thread t2(threadFunc, 2);
  std::thread t3(threadFunc, 3);

  t1.join();
  t2.join();
  t3.join();

  std::cout << i;   //main thread는 항상 0
}
```

위 예제를 살펴보자. 몇 번 실행하다보면 1230, 2130, 3120등과 같은 결과를 볼 수 있다. 그 이유는 thread_local 로 정의된 i가 각 쓰레드에 유일하게 존재하기 때문이다. 마치 정의는 전역 변수인 것 처럼 되어 있지만, 실제로는 각 쓰레드에 하나씩 복사본이 존재하게 되고, 각 쓰레드 안에서 해당 i를 전역 변수인것마냥 참조할 수 있다.

### dynamic storage duration
dynamic storage duration의 경우 동적 할당 함수를 통해서 할당되고 해제되는 객체들을 의미 한다. 대표적으로 new와 delete로 정의되는 객체들 이다. 이러한 저장 방식은 나중에 링커에서 해당 변수나 함수들을 배치시에 어디에 배치할 지 중요한 정보로 사용된다.

## Linkage
`링크 방식(Linkage)`는 어떤 이름이 어디에서 사용될 수 있는지 지정하는 것이다. 앞선 저장 방식이 객체들에게만 해당되는 내용이였다면 linakge의 경우 C++의 모든 객체, 함수, 클래스, 템플릿, 이름 공간 등등을 지칭하는 이름들에 적용되는 내용이다. 

C++에선 다음 linakge들을 제공한다.
* No linkage
* Internal linkage
* External linkage

> Reference  
> [cppreference - storage_duration#Linkage](https://en.cppreference.com/w/cpp/language/storage_duration#Linkage)

### No linkage
링크 방식이 지정되지 않은 경우 동일한 블록 스코프 ({}) scope 안에서 사용할 수 있다.

block scope에서 다음과 같이 선언된 경우 no linkage가 된다.
* static keyword 여부와 관계없이 extern으로 선언되지 않은 변수
* local class와 그 member 함수
* block scope내에서 선언된 typedefs, enumerations, enumerators

```cpp
{ int a = 3; }  //변수 a는 {} 안에 링크 방식이 없는 상태로 정의
a;              //scope 밖에서 a를 참조할 수 없어서 오류 
```

> Reference  
> [cppreference - storage_duration#No_linkage](https://en.cppreference.com/w/cpp/language/storage_duration#No_linkage)

### Internal linkage
Internal linkage를 갖을 경우 같은 TU안에서 사용 할 수 있다. 

namespace scope에서 다음과 같이 성너된 경우 internal linkage가 된다.
* static으로 정의된 함수, 변수, 템플릿 함수, 템플릿 변수
* non-volatile const-qualified type의 non-template 변수
  * 단, 다음의 경우에는 internal linkage를 갖지 않는다.
    * inline 변수
    * 이전에 명시적으로 extern으로 선언된 변수
    * 이전에 internal linkage가 아닌 다른 linkage를 갖게 선언된 변수
* `익명의 이름 공간(unnamed namespaces)`에 정의된 함수나 변수
  * unnamed namespace에서 extern으로 선언이 되더라도 internal linkage를 갖는다.

```cpp
const int a = 3;    // internal linkage, static const int a와 동일한 의미
namespace 
{
int b;              // internal linkage, sttatic int b와 동일한 의미
}
```

> Reference  
> [cppreference - storage_duration#Internal_linkage](https://en.cppreference.com/w/cpp/language/storage_duration#Internal_linkage)  
> [stackoverflow - define-constant-variables-in-c-header](https://stackoverflow.com/questions/12042549/define-constant-variables-in-c-header)

### External linkage
`외부 링크 방식(external linkage)`을 갖을 경우 다른 TU에서 사용 가능하다.

참고로 외부 링크 방식으로 정의된 개체들에 `언어 링크 방식(language linkage)`을 정의할 수 있어서, 다른 언어 (C 와 C++) 사이에서 함수를 공유하는 것이 가능하다.

no linkage나 internal linkage를 갖는 경우를 제외하면 나머지는 모두 external linkage으로 정의된다. 참고로, 블록 스코프 안에 정의된 변수를 external linkage으로 선언하고 싶다면 extern 키워드를 사용하면 된다. 하지만 unnamed namespace에서 extern 키워드를 사용하더라도 internal linkage를 갖는다.

언어 링크 방식을 선언하고 싶다면 아래와 같이 하면 된다.

``` cpp
extern "C" int func();  // C 및 C++ 에서 사용할 수 있는 함수

extern "C++" int func2();   // C++ 에서만 사용할 수 있는 함수
                            // 기본적으로 C++ 의 모든 함수들에 extern "C++" 이 숨어 있다고 보면 된다.

int func2();                // 위와 동일
```

> Reference  
> [cppreference - storage_duration#External_linkage](https://en.cppreference.com/w/cpp/language/storage_duration#External_linkage)  