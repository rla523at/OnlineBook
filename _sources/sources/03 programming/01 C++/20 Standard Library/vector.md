# vector

## push_back vs emplace_back
push_back으로 생성하면 복사나 이동 연산이 발생하지만 emplace_back으로 생성하면 복사나 이동연산이 발생하지 않는다.

```cpp
#include <iostream>
#include <vector>

using namespace std;

class A {
public:
  A(const string& name) { cout << name + " Construct\n"; };
  A(const A& a) { cout << "Copy Construct\n"; };
  A(A&& a) { cout << "Move Construct\n"; };
  ~A(void) { cout << "Destruct\n"; };
};

int main(void)
{
  vector<A> vec;
  vec.reserve(2);

  cout << "push_back\n";
  vec.push_back(A("A1")); //Construct, Move Construct, Destruct

  cout << "\nemplace_back\n";
  vec.emplace_back("A2"); //Construct

  cout << "end\n";
}

```