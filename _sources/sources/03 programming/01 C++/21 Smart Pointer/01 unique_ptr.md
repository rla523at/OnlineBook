# unique_ptr
C++에서 메모리를 잘못된 방식으로 관리하였을 때 발생하는 또 다른 문제점은 이미 해제된 메모리를 다시 해제하는 경우이다. 이런 경우를 `double free` 버그라고 한다.

다음 예시를 보자.
```cpp
Data* data = new Data();
Date* data2 = data;

// data 의 입장 : 사용 다 했으니 내가 소유했던 객체를 소멸시켜야지.
delete data;

// ...

// data2 의 입장 : 사용 다 했으니 내가 소유했던 객체를 소멸시켜야지.
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

> Reference  
> [modoocode-unique ptr](https://modoocode.com/229)  

## 함수의 인자
unique_ptr 객체를 함수의 인자로 전달하려고 하면 어떤 방법을 써야 할까?

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

## Move 
unique_ptr은 객체의 유일한 소유권을 보장하기 때문에 copy construct나 copy assign operation은 삭제되어있다.

하지만 move construct나 move assign operation은 "유일한 소유권을 다른 객체에게 이전"한다는 개념으로 사용이 가능하다. 다음 예시를 보자.
``` cpp
auto data_ptr = std::make_unique<Data>();
auto data_ptr2 = std::move(data_ptr);
```

이 때, move construct 이후의 소유권이 이전된 data_ptr를 `댕글링 포인터(dangling pointer)`라고하며 재 참조할 시에 정의되지 않은 동작을 할 수 있다. 따라서 소유권 이전은 dangling pointer를 절대 다시 참조 하지 않겠다는 확신 하에 이동해야 한다.

또한 move assign operation에 의해 다른 객체의 소유권을 가지게 되면 원래 소유하고 있던 객체는 소멸되게 된다. 
```cpp
auto data_ptr1 = std::make_unique<Data>();
auto data_ptr2 = std::make_unique<Data>();
data_ptr2 = std::move(data_ptr1); //data_ptr2가 가르키고 있던 객체는 소멸
```

따라서, unique_ptr 객체에 nullptr을 대입하면, 원래 소유하고 있떤 객체는 소멸되고 텅 빈 unique_ptr이 된다.
```cpp
std::unique_ptr<Data> a1 = std::make_unique<Data>();  
a1 = nullptr; // a1 = std::unique_ptr<A>();와 동일, a1이 가르키고 있던 객체는 소멸, a1은 텅빈 unique_ptr
```

## reset
unique_ptr은 소유하고 있던 객체를 다른 객체로 바꾸기 위한 reset method가 제공된다.

이 때, reset하면서 자신이 원래 소유하고 있던 객체는 소멸시킨다.
```cpp
std::unique_ptr<Data> a1 = std::make_unique<Data>();  
a1.reset(new Data()); // a1이 가르키고 있던 객체는 소멸!
```

## 포인터로 생성
unique_ptr은 생성자의 인자로 pointer를 전달할 수 있는데 이 경우에는 double free 버그를 발생시킬 수 있다.

```cpp
#include <iostream>
#include <utility>

struct A
{};

int main()
{
  A* a_ptr = new A();

  //double free bug
  std::unique_ptr<A> a_uptr1(a_ptr);
  std::unique_ptr<A> a_uptr2(a_ptr);
}
```

a_uptr1 객체가 소멸될 때, a_ptr이 가르키고 있는 동적으로 생성된 객체가 소멸된다.
그리고 a_uptr1 객체가 소멸될 때에도 a_ptr이 가르키고 있는 동적으로 생성된 객체를 소멸하려고 하는데 이미 소멸되었기 때문에 double free bug가 생긴다.

따라서 unique_ptr을 포인터로 생성하는것은 조심해야 한다.
