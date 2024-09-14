# Miscellaneous

## Array
배열 선언 시 배열 크기를 따로 지정하지 않고 초기화를 진행하여도 초기화한 원소의 수에 맞는 고정크기 배열이 생성된다.

```cpp
int arr1[3] = {1,2,3};
int arr2[]  = {1,2,3};  //arr1과 동일
```


## Checking TypeName
Compile error message를 통해 void가 아닌 typename을 알아낼 수가 있다.

먼저 template 함수를 다음과 같이 정의하자.

```cpp
template <typename T> void type_checker(T& val)
{
	static_assert(std::is_void_v<T>, " ");
}
```

그 다음에 cpp파일을 compile하면 다음 예시처럼 error message에 type이 나온다.

```
instantiation of "void Mec::type_checker(T &) [with T=Mec::DataArchive]" at line 3657
```

## Iterator

> Reference   
> [blog](https://www.internalpointers.com/post/writing-custom-iterators-modern-cpp)


## bool은 왜 1비트가 아니라 1바이트(8비트)일까?
왜냐하면, 모든 C++ value는 주소값을 가질 수 있어야 하고, 주소는 1바이트를 차지하기 때문에 value의 최소 사이즈는 1바이트이다.

> REFERENCE   
> [STACKOVERFLOW](https://stackoverflow.com/questions/2064550/c-why-bool-is-8-bits-long)  


## 클래스 내부 또는 구조체 내부에서 익명 union 사용
클래스 또는 구조체 내부 익명 union이 있는 경우 일반적인 멤버 변수처럼 사용할 수 있기 때문에 오히려 익명으로 사용하는 것이 편리하다.

```cpp

#include <iostream>

struct A
{
  union
  {
    int number;
    char chracter;
  };
};

int main(void)
{
  A a;
  a.number = 10;
  a.chracter = 'a';

  return 0;
}

```

익명이 아니면 다음과 같이 사용해야 한다.

```cpp
#include <iostream>

struct A
{
  union B
  {
    int number;
    char chracter;
  } b;
};

int main(void)
{
  A a;

  a.b.number = 10;
  a.b.chracter = 'a';

  return 0;
}
```

