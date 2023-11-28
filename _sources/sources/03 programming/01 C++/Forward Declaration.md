# Forward Declaration
## 예시
다음 예제 코드를 보자.
```cpp
//B.h

// forward declaration
class A;

class B
{
public:
    void func(A& a);
    A    func2(A& a);
}

//B.cpp
#include "A.h"
#include "B.h"
void B::func(A& a){ ... };
```
위와 같이 함수의 return 값이나 input 값으로 사용하는 경우 `전방선언(forward declaration)`만으로 충분하다. 전방선언을 사용한 경우 B.h에서 #include를 할 필요가 없어진다.

대신에 함수를 정의하는 B.cpp 파일에서는 #include A.h를 통해 A의 정의를 알아야 한다.

단, 객체로 변수를 가지고 있는 경우 전반선언으로 컴파일 의존성을 줄일 수 없다. 다음 예제 코드를 보자.
```cpp
//B.h
class A;
class B
{
public:
    void func(A& a);
private:
    A a;    //error!
}
```

이는 컴파일 시간에 A 객체의 크기를 알아야 B 객체의 크기를 결정하는데 A 객체의 정의부가 제공되어 있지 않기 때문이다.

### 주의사항
cpp에서 header file을 include할 때, header에 forward declaration을 확인하고 현재 cpp에서 전부 include 되어있는지 확인해야한다.

또한, header file이 include하고 있는 header file의 forward declaration도 반드시 확인해야 된다.