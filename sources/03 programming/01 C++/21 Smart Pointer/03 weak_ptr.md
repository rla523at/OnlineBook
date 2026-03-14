# weak_ptr Class
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

위 코드를 실행하면 소멸자가 호출 되지 않아 memory leak 문제가 발생한다. 왜 그런지 하나씩 살펴보자.

pa가 가르키는 동적으로 할당된 객체를 Data1, pb가 가르키는 동적으로 할당된 객체를 Data2라고 하고 함수가 끝나고 pa, pb순으로 소멸자가 호출된다고 하자. 

그러면 main이 끝나고 다음 그림과 같은 과정을 거친다.

```{figure} _image/2101.png
```

즉, 서로가 서로를 가르키는 circular reference가 발생한 경우 reference count가 0이 될 수가 없어서 Data의 소멸자가 호출될 수 없는 것이다. 이와 같이 circular reference에 의해 발생하는 memory leak 문제를 해결하기 위해 C++에서는 `weak_ptr` class를 제공한다.

그러면 weak_ptr이 어떻게 이 문제를 해결하는지 알아보자.

weak_pt는 일반 포인터와 shared_ptr 사이에 위치한 smart point로, smart point처럼 RAII 패턴을 따르면서 객체를 안전하게 참조할 수 있게 해주지만, shared_ptr와는 다르게 reference count를 늘리지 않는다. 

따라서 설사 어떤 객체를 weak_ptr 가 가리키고 있다고 하더라도, 다른 shared_ptr들이 가리키고 있지 않다면 소멸되게 된다. 

그럼으로 Data class의 other 멤버변수를 weak_ptr로 두면 refrence count가 늘지 않아 pa객체가 소멸될 때 Data1 객체가 소멸되고 pb객체가 소멸될 때 Data2 객체가 소멸되게 되서 문제가 해결된다. 

따라서, 위의 코드에서 member변수 other만 weak_ptr로 바꿔줄 경우 소멸자가 정상 호출되는 것을 확인할 수 있다.

이제, 다음 예시를 통해 weak_ptr을 어떻게 사용하는지 조금 더 자세하게 보자.

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

## 참고
### Destruction of control block
reference count가 0이 되면 shared_ptr이 가르키고 있는 객체를 소멸시킨다. 그렇다면 이 떄, control block 역시 소멸되어야 할까?

정답은 아니다!

왜냐하면 가리키는 shared_ptr은 0개지만 아직 weak_ptr 가 남아있는 경우 control block이 없으면 reference count가 0이라는 사실을 weak_ptr이 알 수 없기 때문이다. 따라서, control block는 reference count가 0일뿐만 아니라 객체를 가르키고 있는 weak_ptr 역시 0일 때 소멸되어야 한다. 

그럼으로 control block은 reference count와 더불어 `약한 참조 개수(weak count)`를 기록하게 된다.

### STL구현
* C++20 MSVC 기준
* _Ref_count_base class는 weak count를 나타내는 _Weaks를 갖는다.

> Reference  
> [modoocode-shared ptr](https://modoocode.com/252)  
