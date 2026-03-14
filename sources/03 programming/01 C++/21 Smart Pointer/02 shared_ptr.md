# shared_ptr Class
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

## make_shared
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

## enable_shared_from_this
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

## 참고
### Garbage collector
C++ 이후에 나온 많은 언어(Java 등등) 들은 대부분은 `가비지 컬렉터(Garbage Collector; GC)` 라 불리는 자원 청소기가 기본적으로 내장되어 있다. 이 GC의 역할은 프로그램 상에서 더 이상 쓰이지 않는 자원을 자동으로 해제해 주는 역할을 한다. 따라서 프로그래머들이 코드를 작성할 때, 자원을 해제하는 일에 대해 크게 신경 쓸 필요는 없다.

그렇다면 GC는 언제 자원을 해제해야 되는지 어떻게 판단할까? 

GC는 `도달성(Reachability)`이라는 개념을 적용한다. 객체에 유효한 레퍼런스가 있다면 Reachable로 구분되고, 유효한 레퍼런스가 없다면 Unreachable로 구분해버리고 자원을 해제한다. 즉, shared_ptr에서 refrence count로 객체 소멸 시점을 판단하는 것과 유사하다.

### STL 구현
* C++20 MSVC 기준
* _Ref_count_base class는 control block 개념을 구현한 class이다.
* _Ref_count_base class는 reference count를 나타내는 _Uses 멤버 변수를 갖는다.
* shared_ptr class은 _Ptr_base class를 상속받는다.
* _Ptr_base class는 _Ref_count_base class의 pointer인 _Rep 멤버 변수를 가지고 있다.
  * 따라서 _Rep은 control block을 가르키는 내부 pointer이다.