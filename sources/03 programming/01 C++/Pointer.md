# Pointer

## Pointer Type Hierarchy
`T* u`의 타입은 pointer to T이다. 

이 때, 타입은 2개의 계층으로 이루어져 있으며 첫번째 계층은 pointer 그리고 두번째 계층은 T이다.

그리고 C++에서는 cv-qualifier가 다른 계층에 각각 나타날 수 있다. 

예를 들어 `T* const u`의 타입은 const pointer to T이다. 즉, 여기서는 첫번째 계층에서 cv-qualifer가 나타난 경우이다.

그리고 `const T* u`의 타입은 pointer to const T이다. 즉, 여기서는 두번째 계층에 cv-qualifier가 나타난 경우이다.

그리고 reference는 top-level cv-qualifiers를 갖지 않는다.([standard](https://eel.is/c++draft/basic.type.qualifier), [cppreference](https://en.cppreference.com/w/cpp/language/reference))

> Reference  
> [Dan saks - Top-Level cv-Qualifiers in Function Parameters](http://www.dansaks.com/articles/2000-02%20Top-Level%20cv-Qualifiers%20in%20Function%20Parameters.pdf)  
> [stackoverflow - where-is-the-definition-of-top-level-cv-qualifiers-in-the-c11-standard](https://stackoverflow.com/questions/24676824/where-is-the-definition-of-top-level-cv-qualifiers-in-the-c11-standard)  
> [blog - references-dont-have-top-level-cv-qualifiers](https://blog.knatten.org/2023/03/17/references-dont-have-top-level-cv-qualifiers/)
> [standard - basic.type.qualifier](https://eel.is/c++draft/basic.type.qualifier)  
> [cpprefenrece - reference](https://en.cppreference.com/w/cpp/language/reference)


## Pointer as Function Input Argument
Pointer를 함수에 input으로 주었을 때, 함수 내부에 변화가 반영이 된다고 햇갈리지 않게 조심해야 한다.

```cpp
#include <iostream>

class A
{
public:
  int val;
};

void func(A* input_ptr)
{
  A* new_ptr     = new A();
  input_ptr      = new_ptr;
  input_ptr->val = 10;
}

int main(void)
{
  A* ptr = nullptr;
  func(ptr);

  std::cout << ptr->val << std::endl; // nullptr read access error!
  return 0;
}
```

위 예시의 경우에는 `func`이 호출된 이후에도 ptr은 계속 nullptr이기 때문에 읽기 위반 오류가 발생한다.

`func`에 인자로 주어진 ptr과 `func` 내부에 정의된 input_ptr은 다른 pointer이다.

`func` 내부에 정의된 input_ptr은 ptr을 복사해서 만든 다른 pointer일 뿐이다.

따라서, input_ptr에 new_ptr을 대입하여도, ptr은 여전히 nullptr이고 그래서 읽기 위반 오류가 발생하는 것이다.

이를 해결하려면 이중포인터나, 레퍼런스를 활용하면 된다.

```cpp
#include <iostream>

class A
{
public:
  int val;
};

void func_ref(A*& input_ptr)
{
  A* new_ptr     = new A();
  input_ptr      = new_ptr;
  input_ptr->val = 10;
}

void func_pptr(A** input_ptr)
{
  A* new_ptr        = new A();
  *input_ptr        = new_ptr;
  (*input_ptr)->val = 10;
}

int main(void)
{
  A* ptr = nullptr;
  func_ref(ptr);
  std::cout << ptr->val << std::endl;

  A* ptr2 = nullptr;
  func_pptr(&ptr2);
  std::cout << ptr2->val << std::endl;

  return 0;
}
```



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

## static constexpr pointer

class 내부에서 다음과 같이 정적멤버변수를 초기화 하려고 하면 오류가 발생한다.

```cpp
static constexpr wchar_t* class_name = L"MSGraphics";
```

이는 `L"MSGraphics"`의 타입은 `const wchar_t*`인데 `constexpr wchar_t*`의 타입은 `wchar_t* const`이기 때문이다.

따라서 이를 해결하기 위해서는 다음과 같이 초기화하면 된다.

```cpp
static constexpr const wchar_t* class_name = L"MSGraphics";
```

## Function Pointer

function pointer 는 아래와 같은 형태를 갖는다.
```
ReturnType (*PointerName)(InputType)
```

예를 들어 `int (*p)(int);` 는 int 를 input 으로 받아서 int 를 return 하는 함수를 가르키는 포인터 객체 p 를 정의한 것이다.

A pointer to function can be used as the left-hand operand of the function call operator, this invokes the pointed-to function:
```cpp
int f(int n)
{
    std::cout << n << '\n';
    return n * n;
}
 
int main()
{
    int (*p)(int) = f;
    int x = p(7);
}
```

> Reference  
> [cppreference - Pointers_to_functions](https://en.cppreference.com/w/cpp/language/pointer#Pointers_to_functions)  
