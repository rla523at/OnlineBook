# Smart Pointer
C++ 의 경우 한 번 동적으로 획득한 자원은 직접 해제해주지 않는 이상 프로그램이 종료되기 전 까지 남아있게 된다. 이런 특성 때문에 사용하지 않는 메모리가 계속 할당되어 있는 `메모리 누수(memory leak)` 문제가 발생할 수 있다. memory leak이 발생하면 간단한 프로그램의 경우 크게 문제될 일은 없지만, 서버 처럼 장시간 작동하는 프로그램의 경우 시간이 지남에 따라 점점 사용하는 메모리 양의 늘어나서 결과적으로 시스템 메모리가 부족해 프로그램이 죽어버리는 상황이 발생할 수 있다. 

이와 같이 동적으로 할당한 자원을 해제하지 않아 발생하는 문제를 방지하기 위해 RAII 패턴을 따르며 pointer 역할을 하는 class인 `스마트 포인터(smart pointer)`가 나오게 되었다.

## 자원 해제의 어려움
언뜻 생각하면 동적으로 할당한 자원을 해제하지 않아 발생하는 문제의 해결 방법은 간단하다. 항상 잊지 말고 자원을 해제하면 된다. 

하지만 이 간단한 해결방법은 생각만큼 간단하지 않다. 프로그램의 크기가 커지면 자원을 해제하는 위치가 애매한 경우가 많아서 실수하기 쉽다. 또한 자원을 해제하는 코드를 작성했다고 하더라도 예상과 다른 실행 흐름으로 의도와 다르게 자원이 해제되지 않는 경우도 있다. 

다음 예시를 보자.

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

## RAII 패턴
개발자가 신경쓰지 않아도 항상 자원이 해제되도록 비야네 스트로스트룹은 C++에서 `자원의 획득은 초기화(Resource Acquisition Is Initialization; RAII)` 디자인 패턴을 제안했다. RAII 패턴은 스택에 생성된 객체를 통해 동적으로 할당한 자원을 관리하도록 하는 디자인 패턴이다. 다시 말해, 스택에 생성된 객체가 소멸됨에 따라 그 객체가 관리하는 동적으로 할당된 자원도 적절하게 해제 되게 디자인하는 것이다.

그렇다면 "왜 스택에 생성된 객체에게 자원 관리를 하도록 하였을까?" 그 이유는 스택에 생성된 객체는 항상 필요한 타이밍에 소멸되기 때문이다.

함수가 종료될 때 스택내의 모든 객체들은 당연히 소멸자들이 호출되며, 예외가 발생해서 함수를 빠져나가더라도 그 함수의 스택에 정의되어 있는 모든 객체들은 stack unwinding에 의해 빠짐없이 소멸자가 호출된다. 

따라서 위의 예시와 같은 문제도 RAII 패턴을 적용하면 해결할 수 있다. 그러면 RAII를 적용하기 위해 do_something 함수를 다시 보자. 현재 pa는 단순 pointer 이기 때문에 소멸자가 호출되지 않는다. 따라서, pointer의 역할을 하는 class가 필요하며 그 class의 소멸자에서는 자신이 가리키고 있는 데이터도 같이 delete 하게 해야 한다. 이와 같이 RAII를 지키는 똑똑한 pointer class를 `스마트 포인터(smart pointer)`라고 한다.

C++에서는 unique_ptr, shared_ptr, weak_ptr을 제공하고 있다.

> Reference   
> [modoocode-unique ptr](https://modoocode.com/229)  