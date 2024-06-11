# Pointer

## Pointer끼리 대입연산

Pointer끼리 대입연산을 하면 다른 포인터가 가르키는 주소값을 가지게 된다.

```cpp
#include <iostream>

class A
{};

class B : public A
{};

int main(void)
{
  B* ptr = new B();
  A* ptr2 = nullptr;
   
  std::cout << "ptr:" << ptr << "\n";   //ptr:000002C770D665B0
  std::cout << "ptr2:" << ptr2 << "\n"; //ptr2:0000000000000000

  ptr2 = static_cast<A*>(ptr);

  std::cout << "ptr:" << ptr << "\n";   //ptr:000002C770D665B0
  std::cout << "ptr2:" << ptr2 << "\n"; //ptr2:000002C770D665B0
}
```

