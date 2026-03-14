# new

## new A vs new A()
new A와 new A()는 객체를 생성할 때 초기화 방식에서 차이가 있다.

또한, 이 차이는 POD(Plain Old Data) 타입인지, non-POD 타입인지에 따라 다르게 동작할 수 있다.

``` cpp
#include <iostream>
#include <memory>

class A{
public: int x;
}

int main(void) {
  std::unique_ptr<A> uptr;
  uptr.reset(new A);
  std::cout << uptr->x << "\n"; // 쓰레기 값

  uptr.reset(new A());
  std::cout << uptr->x << "\n"; // 0
}
```

### new A (default initialization)
new A 는 객체를 default initialization 하여 생성하게 된다.

따라서, A 가 POD 타입인 경우 new A는 메모리만 할당하고, 멤버 변수를 초기화하지 않아 멤버 변수는 불규칙한 값을 가질 수 있다.

그리고 A 가 non-POD 타입인 경우 new A 는 기본 생성자를 호출하여 객체를 생성한다.

### new A() (value initialization)
new A() 는 객체를 value initialization 하여 생성하게 된다.

따라서, A 가 POD 타입인 경우 new A()는 모든 멤버 변수를 0으로 초기화한다.

그리고 A 가 non-POD 타입인 경우 new A() 는 기본 생성자를 호출하여 객체를 생성한다.
