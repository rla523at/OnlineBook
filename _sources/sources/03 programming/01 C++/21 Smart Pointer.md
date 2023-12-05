# Smart Pointer
C++ 의 경우 한 번 동적으로 획득한 자원는 직접 해제해주지 않는 이상 프로그램이 종료되기 전 까지 영원히 남아있게 된다. 이로인해 `메모리 누수(memory leak)`가 발생할 수 있는데 memory leak이 발생하면 간단한 프로그램의 경우 크게 문제될 일이 없지만, 서버 처럼 장시간 작동하는 프로그램의 경우 시간이 지남에 따라 점점 사용하는 메모리 양의 늘어나서 결과적으로 시스템 메모리가 부족해 프로그램이 죽어버리는 상황이 발생할 수 있다. 

그래서 이런 memory leak 문제를 방지하기 위해 RAII 패턴을 따르며 pointer 역할을 하는 class인 `스마트 포인터(smart pointer)`가 나오게 되었다.

## RAII 패턴
언뜻 생각하면 memory leak의 해결 방법은 간단하다. 항상 잊지 말고 메모리 꼭 해제하면 된다. 하지만 프로그램의 크기가 커지면 메모리를 해제하는 위치가 애매한 경우가 많아서 놓치기 쉽다. 그리고 메모리를 해제하는 코드를 작성했다고 하더라도 다음 예시처럼 예상치 못한 동작으로 의도와 다르게 메모리가 해제되지 않는 경우도 있다. 

```cpp
#include <iostream>

class A {
 public:
  A() {
    data = new int[100];
    std::cout << "자원을 획득함!" << std::endl;
  }

  ~A() {
    std::cout << "자원을 해제함!" << std::endl;
    delete[] data;
  }

private:
  int *data;
};

void func() {
  throw 1;
}

void do_something() {
  A *pa = new A();
  func();
  delete pa;
}

int main() {
  try {
    do_something();
  } catch (int i) {
    std::cout << "예외 발생!" << std::endl;    
  }
}
```

원래 대로라면 do_something을 호출하면 메모리 획득과 해제가 되어야 하지만 중간에 function에서 exception이 발생했기 때문에 메모리 해제가 제대로 되지 않는다.

다양한 상황에서 memory leak 문제가 발생할 수 있기 때문에 비야네 스트로스트룹은 C++에서 자원을 관리하는 방법으로 `자원의 획득은 초기화(Resource Acquisition Is Initialization; RAII)`디자인 패턴을 제안했다. RAII 패턴은 스택에 생성된 객체를 통해 동적으로 할당한 자원을 관리하도록 하는 디자인 패턴이다.

함수가 종료될 때 스택내의 모든 객체들은 당연히 소멸자들이 호출되며, 예외가 발생해서 함수를 빠져나가더라도 그 함수의 스택에 정의되어 있는 모든 객체들은 stack unwinding에 의해 빠짐없이 소멸자가 호출되기 때문에 메모리를 해제가 제대로 되지 않는 문제를 해결할 수 있다.

그러면 RAII를 적용하기 위해 do_something 함수를 다시 보자. 현재 pa는 단순 pointer 이기 때문에 소멸자가 호출되지 않는다. 따라서, pointer의 역할을 하는 class가 필요하며 그 class의 소멸자에서는 자신이 가리키고 있는 데이터도 같이 delete 하게 해야 한다. 이와 같이 RAII를 지키는 똑똑한 pointer class를 `스마트 포인터(smart pointer)`라고 한다.

> Reference  
> [modoocode-unique ptr](https://modoocode.com/229)  

## unique_ptr Class
C++에서 메모리를 잘못된 방식으로 관리하였을 때 발생하는 또 다른 문제점은 이미 해제된 메모리를 다시 해제하는 경우이다. 이런 경우를 `double free` 버그라고 한다.

다음 예시를 보자.
```cpp
Data* data = new Data();
Date* data2 = data;

// data 의 입장 : 사용 다 했으니 소멸시켜야지.
delete data;

// ...

// data2 의 입장 : 나도 사용 다 했으니 소멸시켜야지
delete data2;
```
data2가 이미 해제된 메모리를 다시 해제하려하기 때문에 이럴 경우 메모리 오류가 나면서 프로그램이 죽게 된다. 

위와 같은 문제가 발생한 이유는 만들어진 객체의 소유권이 명확하지 않아서 이다. 객체는 하나이지만 객체를 가르키는 포인터가 2개이기 때문에 해제된 메모리를 또 해제하는 실수가 발생할 가능성이 높아진 것이다.

따라서 어떤 포인터에게 객체의 유일한 소유권을 부여해서 객체에 대한 메모리 관리를 완전히 맡긴다면, 위와 같이 같은 객체를 두 번 소멸시켜버리는 실수를 줄일 수 있을 것이다. 이를 위해 C++에서는 RAII 패턴을 따르면서 특정 객체에 유일한 소유권을 부여하여 double free 버그의 발생 가능성을 줄일 수 있는 `unique_ptr class`를 제공한다.

unique_ptr 객체는 유일한 소유권을 의미하기 때문에 복사와 관련된 함수들은 전부 삭제되어 있다. 다음 예시를 보자.

``` cpp
auto data_ptr = std::make_unique<Data>();
//auto data_ptr2 = data_ptr; //compile error  
```

unique_ptr를 복사할 수 있게 된다면, 특정 객체를 여러 개의 unique_ptr 들이 소유하게 되는 문제가 발생한다. 이는 원래 개발 의도와 맞지 않으며 RAII 패턴에 따라 unique_ptr들이 소멸될 때 전부 객체를 delete 하려 해서 앞서 말한 double free 버그가 발생하게 된다.

하지만 unique_ptr 객체도 이동 관련된 함수들은 사용이 가능하다. 이 때, 이동을 소유권을 이동시킨다는 개념으로 받아들이기 때문이다. 다음 예시를 보자.
``` cpp
auto data_ptr = std::make_unique<Data>();
auto data_ptr2 = std::move(data_ptr);
```

이 때, move method 이후의 소유권이 이전된 data_ptr를 `댕글링 포인터(dangling pointer)`라고하며 재 참조할 시에 정의되지 않은 동작을 할 수 있다. 따라서 소유권 이전은 dangling pointer를 절대 다시 참조 하지 않겠다는 확신 하에 이동해야 한다.

그러면 unique_ptr 객체를 함수의 인자로 전달하려고 하면 어떤 방법을 써야 할까?

먼저, 다음 예시와 같이 reference로 넘기는 경우를 생각해보자.
```cpp
void func(std::unique_ptr<Data>& uptr) { 
    // do soemthing with uptr
}

auto data_ptr = std::make_unique<Data>();
func(data_ptr);
```

위와 같이 레퍼런스로 unique_ptr을 전달했다면, func함수 내부의 uptr은 더이상 유일한 소유권을 의미하지 않는다. 물론 uptr은 레퍼런스 이기 때문에, func함수가 종료되도 uptr이 가리키고 있는 객체를 파괴하지는 않는다. 

하지만 data_ptr이 유일하게 소유하고 있던 객체는 적어도 func함수 내부에서는 uptr과 공동 소유가 되고 있다는 점이다. 즉, unique_ptr은 소유권을 의미한다는 원칙에 위배된다.

따라서, unique_ptr이 가르키고 있는 객체를 함수 내에서 사용하고 싶은 경우라면, raw pointer를 사용하는 것이 바람직하며 unique_ptr은 raw pointer를 return하는 get method를 제공한다.
``` cpp
void func(Data* ptr) { 
    // do soemthing with ptr
}

auto data_ptr = std::make_unique<Data>();
func(data_ptr.get());

```

> Reference  
> [modoocode-unique ptr](https://modoocode.com/229)  

## shared_ptr Class
프로그래밍을 하다 보면 하나의 객체를 여러 포인터가 소유해야하는 경우가 있다. 

이 경우에 사용할 수 있는 RAII를 따르는 smart pointer를 구현한다고 해보자. 이 때에는 객체를 소유하는 모든 smart pointer가 소멸자를 호출했을 때만 객체를 소멸시켜야 한다. 하지만 문제는 smart pointer가 언제 어떤 순서로 소멸되는지 알 수 없기 때문에 언제 객체를 소멸 시켜야 하는지 알 수 없다.

이를 해결하기 위해서는 좀더 스마트 한 포인터가 있어서, 특정 자원을 몇 개의 smart pointer가 가리키는지를 추적한 다음에 그 수가 0 이 되야만 비로소 객체를 소멸시키는 방식의 포인터가 필요하다. 이를 위해 C++에서는 특정 자원을 몇 개의 smart pointer가 가리키는지 추적이 가능한 `shared_ptr` class를 제공한다.

shared_ptr은 unique_ptr과 다르게 한 객체를 여러 포인터가 소유해야 하는 경우를 위한 class임으로 다음과 같이 복사가 잘 된다.
```cpp
auto data_ptr = std::make_shared<Data>();
auto data_ptr2 = data_ptr;
```

이 떄, shared_ptr은 자신이 가르키고 있는 객체를 가리키는 다른 shared_ptr이 몇개인지 알 수 있고 이를 `참조 개수(reference count)`라고 한다. reference count는 shared_ptr의 use_count 함수를 통해 알 수 있다.
```cpp
std::cout << data_ptr.use_count(); // 2
std::cout << data_ptr2.use_count(); // 2
```

shared_ptr이 제대로 동작하기 위해서는 같은 객체를 가르키는 shared_ptr끼리는 reference count가 몇인지 알아야하고 항상 같은 값을 갖어야 한다. 하지만 다음 예시를보면 이 일이 쉽지 않은 일임을 알 수가 있다.
```cpp
auto data_ptr = std::make_shared<Data>();
auto data_ptr2 = data_ptr;
auto data_ptr3 = data_ptr2;

std::cout << data_ptr.use_count(); // 3
std::cout << data_ptr2.use_count(); // 3
std::cout << data_ptr3.use_count(); // 3
```

만약에, shared_ptr 내부에 reference count를 저장한다면 data_ptr3를 생성할 떄, data_ptr2와 data_ptr3의 reference count를 올리는건 어떻게든 될 수 있지만 data_ptr의 reference count를 올리는건 어떻게 할 수가 없다.

하지만 실제로 data_ptr의 reference count도 3이 되는걸 알 수 있는데 어떻게 이런 마법을 부리고 있는건지 알아보자.

이 마법의 비밀은 `제어 블록(control block)`이란 추가적인 구조에 있다. control block이란 현재 객체를 가르키고 있는 shared_ptr이 몇개인지를 나타내는 reference count를 가지고 있는 객체이다. 

shared_ptr은 생성되고 처음으로 객체를 가르키는 경우 control block을 동적으로 할당한 뒤, 내부 pointer로 생성된 control block을 가르키고 control block의 reference count를 1로 만든다. 그리고 shared_ptr의 복사가 일어나는 경우 복사된 shared_ptr의 내부 pointer는 기존 shared_ptr의 control blcok을 가르키게 되며 control block에 접근하여 reference count를 늘린다. 반대로 소멸될 때는 control block에 접근해 줄이고 reference count를 줄이고 자신이 소멸될 때 reference count가 0이 되는 경우에 동적으로 할당된 객체를 소멸시킨다.

> Reference  
> [modoocode-shared ptr](https://modoocode.com/252)  

### make_shared
shared_ptr을 생성할 때, 원래 new 키워드를 사용하듯이 해도 된다.
```cpp
std::shared_ptr<Data> data_ptr = new Data();
```

하지만 이는 바람직한 shared_ptr의 생성 방법이 아니다. 왜냐하면 일단 Data를 생성할 떄 동적 할당이 한 번, 그 다음 shared_ptr 의 control block을 생성할 때 동적 할당이 한 번, 총 두 번의 동적 할당이 발생하기 때문이다.

동적 할당은 상당히 비싼 연산 임으로 어차피 동적 할당을 두 번 할 것 이라는 것을 알고 있다면, 아예 두 개 합친 크기로 한 번 할당 하는 것이 훨씬 빠르다.

그래서 make_shared 함수는 Data의 생성자의 인자들을 받아서 이를 통해 Data객체와 shared_ptr 의 control block까지 한 번에 동적 할당 한 후에 만들어진 shared_ptr을 리턴한다.

> Reference  
> [modoocode-shared ptr](https://modoocode.com/252)  
> [cppreference](https://en.cppreference.com/w/cpp/memory/shared_ptr#Implementation_notes)  

### enable_shared_from_this
shared_ptr은 인자로 pointer가 전달되면 마치 자기가 pointer가 가르키는 객체를 첫번째로 소유하는 shared_ptr인 것 마냥 행동해서 같은 객체에 여러개의 control block이 생길 수 있게 한다. 그리고 이는 잘못된 reference count와 double free 버그를 발생시킨다. 다음 예시 코드를 보자.

```cpp
Data* a = new Data();
std::shared_ptr<Data> p1 = std::shared_ptr<Data>(a);
std::shared_ptr<Data> p2 = std::shared_ptr<Data>(a);
```

이 경우에 p1도 p2도 모두 a가 가르키고 있는 Data객체를 자기가 첫번째로 소유한다고 판단해 control block이 각각 생기게 된다. 따라서 a를 가르키는 shared_ptr이 2개임에도 불구하고 각각 reference_count는 1이 되게 되고 p1이 소멸할 떄 이미 Data객체를 소멸시키기 때문에 p2가 소멸할 때 double free bug가 발생하게 된다.

따라서, shared_ptr을 생성할 때 인자로 pointer를 전달하면은 double free 버그가 발생할 수 있음으로 지양해야한다.

하지만, 객체 내부에서 자기 자신을 가리키는 shared_ptr를 만들어야 할 때는 어쩔 수 없이 인자로 pointer를 전달할 수 밖에 없다. 다음 예시 코드를 보자.

```cpp
#include <iostream>
#include <memory>

class Data {
public:
  Data() {
    data = new int[100];
    std::cout << "자원을 획득함!" << std::endl;
  }

  ~Data() {
    std::cout << "소멸자 호출!" << std::endl;
    delete[] data;
  }

  std::shared_ptr<Data> get_shared_ptr() { return std::shared_ptr<Data>(this); }

private:
  int *data;
};

int main() {
  std::shared_ptr<Data> pa1 = std::make_shared<Data>();
  std::shared_ptr<Data> pa2 = pa1->get_shared_ptr();

  std::cout << pa1.use_count() << std::endl;
  std::cout << pa2.use_count() << std::endl;
}
```
`get_shared_ptr`은 자기자신을 가르키는 shared_ptr을 return하는 함수이다. 따라서 다음과 같이 구현될 수 밖에 없다. 하지만 이번에도 위와 동일한 이유로 double free 버그가 발생하고 만다.

따라서 이러한 문제를 해결하기 위해 나온 class가 `std::enable_shared_from_this` class이다. this를 사용해서 shared_ptr을 만들고 싶은 클래스가 있다면, enable_shared_from_this를 상속 받으면 된다.

```cpp
#include <iostream>
#include <memory>

class Data : public std::enable_shared_from_this<Data> {
public:
  Data() {
    data = new int[100];
    std::cout << "자원을 획득함!" << std::endl;
  }

  ~Data() {
    std::cout << "소멸자 호출!" << std::endl;
    delete[] data;
  }

  std::shared_ptr<Data> get_shared_ptr() { return shared_from_this(); }

private:
  int *data;
};

int main() {
  std::shared_ptr<Data> pa1 = std::make_shared<Data>();
  std::shared_ptr<Data> pa2 = pa1->get_shared_ptr();

  std::cout << pa1.use_count() << std::endl;
  std::cout << pa2.use_count() << std::endl;
}
```
이 경우에는 제대로 동작하는 것을 볼 수 있다.

enable_shared_from_this 클래스에는 `shared_from_this`라는 멤버 함수가 정의되어 있는데, 이 함수는 이미 정의되어 있는 control block을 사용해서 shared_ptr을 만들어서 return 한다.따라서 같은 객체에 두 개의 다른 control block이 생성되는 일을 막을 수 있다.

한 가지 중요한 점은 shared_from_this가 잘 동작하기 위해서는 해당 객체를 가르키는 shared_ptr가 반드시 먼저 정의되어 있어야만 한다. 즉 shared_from_this는 있는 control block을 확인만 할 뿐, 없는 control block을 만들지는 않는다. 

따라서, 해당 객체를 가르키는 shared_ptr이 없는 경우 오류가 발생한다. 다음 예시를 보자.
```cpp
Data* a = new Data();
std::shared_ptr<Data> pa1 = a->get_shared_ptr(); // compile error!
```

> Reference  
> [modoocode-shared ptr](https://modoocode.com/252)  

### 참고
#### Garbage collector
C++ 이후에 나온 많은 언어(Java 등등) 들은 대부분은 `가비지 컬렉터(Garbage Collector; GC)` 라 불리는 자원 청소기가 기본적으로 내장되어 있다. 이 GC의 역할은 프로그램 상에서 더 이상 쓰이지 않는 자원을 자동으로 해제해 주는 역할을 한다. 따라서 프로그래머들이 코드를 작성할 때, 자원을 해제하는 일에 대해 크게 신경 쓸 필요는 없다.

그렇다면 GC는 언제 자원을 해제해야 되는지 어떻게 판단할까? 

GC는 `도달성(Reachability)`이라는 개념을 적용한다. 객체에 유효한 레퍼런스가 있다면 Reachable로 구분되고, 유효한 레퍼런스가 없다면 Unreachable로 구분해버리고 자원을 해제한다. 즉, shared_ptr에서 refrence count로 객체 소멸 시점을 판단하는 것과 유사하다.

#### STL 구현
* C++20 MSVC 기준
* _Ref_count_base class는 control block 개념을 구현한 class이다.
* _Ref_count_base class는 reference count를 나타내는 _Uses 멤버 변수를 갖는다.
* shared_ptr class은 _Ptr_base class를 상속받는다.
* _Ptr_base class는 _Ref_count_base class의 pointer인 _Rep 멤버 변수를 가지고 있다.
  * 따라서 _Rep은 control block을 가르키는 내부 pointer이다.

## weak_ptr Class
shared_ptr는 reference count가 0이 되면 가리키는 객체를 소멸시킨다. 그런데, 이런 방식때문에 shared_ptr을 멤버 변수로 가지고 있는 class의 두 객체가 shared_ptr로 서로가 서로를 가리키는 경우 reference count가 절대로 0이 될 수 없어서 객체 소멸이 불가능하게 될 수도 있다. 이를 `순환참조(circular reference)`라고 한다. 다음 예시를 보자.

```cpp
#include <iostream>
#include <memory>

class Data {
 public:
  Data() {
    data = new int[100];
    std::cout << "자원을 획득함!" << std::endl;
  }

  ~Data() {
    std::cout << "소멸자 호출!" << std::endl;
    delete[] data;
  }

  void set_other(std::shared_ptr<Data> o) { other = o; }

private:
  int *data;
  std::shared_ptr<Data> other;
};

int main() {
  std::shared_ptr<Data> pa = std::make_shared<Data>();
  std::shared_ptr<Data> pb = std::make_shared<Data>();
  pa->set_other(pb);
  pb->set_other(pa);
}
```

pa가 가르키는 동적으로 할당된 객체를 Data1, pb가 가르키는 동적으로 할당된 객체를 Data2라고 하고 함수가 끝나고 pa, pb순으로 소멸자가 호출된다고 하자. 그러면 main이 끝나고 다음 그림과 같은 과정을 거친다.

```{figure} _image/2101.png
```

즉, circular reference가 발생한 경우 reference count가 0이 될 수가 없어서 Data의 소멸자는 호출될 수 없어서 위의 코드는 memory leak이 발생한다. 이와 같이 circular reference에 의해 발생하는 memory leak 문제를 해결하기 위해 C++에서는 `weak_ptr` class를 제공한다.

그러면 weak_ptr이 어떻게 이 문제를 해결하는지 알아보자.

weak_pt 는 일반 포인터와 shared_ptr 사이에 위치한 smart point로, smart point처럼 RAII 패턴을 따르면서 객체를 안전하게 참조할 수 있게 해주지만, shared_ptr와는 다르게 reference count를 늘리지 않는다. 

따라서 설사 어떤 객체를 weak_ptr 가 가리키고 있다고 하더라도, 다른 shared_ptr들이 가리키고 있지 않다면 소멸되게 된다. 

그럼으로 Data class의 other 멤버변수를 weak_ptr로 두면 refrence count가 늘지 않아 pa객체가 소멸될 때 Data1 객체가 소멸되고 pb객체가 소멸될 때 Data2 객체가 소멸되게 되서 문제가 해결된다. 

이제, 다음 예시를 통해 weak_ptr을 어떻게 사용하는지 보자.

```cpp
#include <iostream>
#include <memory>
#include <vector>

class Data {
 public:
  Data() {
    data = new int[100];
    std::cout << "자원을 획득함!" << std::endl;
  }

  ~Data() {
    std::cout << "소멸자 호출!" << std::endl;
    delete[] data;
  }

  void set_other(std::weak_ptr<Data> o) { other = o; }
  void access(void) {std::cout << "접근 성공\n";};
  void acess_other(void) {
    std::shared_ptr<Data> o = other.lock();
    if (o) o->access();
    else   std::cout << "접근 실패, 이미 소멸 됨" << std::endl;
  }

private:
  int *data;
  std::weak_ptr<Data> other;
};

int main() {
  std::vector<std::shared_ptr<Data>> ptrs;
  ptrs.push_back(std::make_shared<Data>());
  ptrs.push_back(std::make_shared<Data>());

  ptrs[0]->set_other(ptrs[1]);
  ptrs[1]->set_other(ptrs[0]);

  std::cout << ptrs[0].use_count() << "\n"; // 1
  std::cout << ptrs[1].use_count() << "\n"; // 1

  ptrs[0]->acess_other();
  ptrs.pop_back();
  ptrs[0]->acess_other(); // 접근 실패
}
```
weak_ptr는 생성자로 shared_ptr나 다른 weak_ptr를 받는다. 따라서 set_other 함수에 shared_ptr을 전달하더라도 잘 동작한다. 

참고로 shared_ptr과는 다르게, 이미 control block이 만들어진 객체만이 의미를 가지기 때문에, 평범한 포인터 주소값으로 weak_ptr를 생성할 수 없다.

그리고 weak_ptr가 가르키고 있더라도 다른 shared_ptr이 가르키지 않으면 소멸되기 때문에 weak_ptr로는 원래 객체를 참조할 수 없고, 반드시 shared_ptr 로 변환해서 사용해야 한다. 따라서 acess_other 함수에서와 같이 weak_ptr에서 제공하는 lock 함수를 사용해서 객체에 접근해야 한다. 이 때 가리키고 있는 객체가 이미 소멸되었다면 빈 shared_ptr 로 변환되고, 아닐경우 해당 객체를 가리키는 shared_ptr로 변환된다.

### 참고
#### Destruction of control block
reference count가 0이 되면 shared_ptr이 가르키고 있는 객체를 소멸시킨다. 그렇다면 이 떄, control block 역시 소멸되어야 할까?

정답은 아니다!

왜냐하면 가리키는 shared_ptr은 0개지만 아직 weak_ptr 가 남아있는 경우 control block이 없으면 reference count가 0이라는 사실을 weak_ptr이 알 수 없기 때문이다. 따라서, control block는 reference count가 0일뿐만 아니라 객체를 가르키고 있는 weak_ptr 역시 0일 때 소멸되어야 한다. 

그럼으로 control block은 reference count와 더불어 `약한 참조 개수(weak count)`를 기록하게 된다.

#### STL구현
* C++20 MSVC 기준
* _Ref_count_base class는 weak count를 나타내는 _Weaks를 갖는다.

> Reference  
> [modoocode-shared ptr](https://modoocode.com/252)  
