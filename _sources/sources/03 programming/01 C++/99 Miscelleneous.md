# Miscellaneous

## integer literal
Allows values of integer type to be used in expressions directly.

hex-literal   : 0x
binary-literal: 0b


> Reference   
> [cppreference - integer_literal](https://en.cppreference.com/w/cpp/language/integer_literal)  

## std::cout and char
std::cout 에 char 혹은 unsigned char 을 바로 전달하면 문자로 해석된다.

## std::set_terminate

프로그램이 정상적으로 예외를 처리할 수 없을 때 호출되는 종료 처리기(terminate handler)를 등록하는 함수이다.

예외를 던졌지만 적절한 예외 핸들러를 찾지 못하거나, 예외 처리 중 또 다른 예외가 발생한 경우 std::terminate() 함수가 호출이 되는데, 이 때 프로그램 종료 직전에 실행될 함수를 설정할 수 있다.

아래 코드를 빌드해서 생긴 .exe 파일을 실행하면 terminal 에 Unhandled exception 이 출력되는 걸 확인할 수 있다.

단, visual studio 에서 실행한 터미널에서는 확인할 수 없다.

```cpp
#include <cstdlib>
#include <exception>
#include <iostream>
 
int main()
{
    std::set_terminate([]()
    {
        std::cout << "Unhandled exception\n" << std::flush;
    });
    throw 1;
}
```


> Reference  
> [cppreference - set_terminate](https://en.cppreference.com/w/cpp/error/set_terminate)  
> [cppreference - terminate_handler](https://en.cppreference.com/w/cpp/error/terminate_handler)  

## bit 단위 연산자

### &
비트단위로 AND 연산을 한다.

| A | B | A & B |
|---|---|-------|
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

## Boolean 값으로 정규화

```
!!(value)
```

c++ 에서는 숫자 값이나 포인터 혹은 기타 표현식의 결과가 boolean 으로 사용될 때, 0 이 아닌 값은 true 로 0 은 false 로 간주된다.

이 때, 부정 연산자를 두번 연속으로 사용하면 `!!` 0 이 아닌 값은 항상 true 로 변환되며 0 인 경우 false 로 변환된다.

즉, boolean 값으로 표준화가 된다.

## GetLastError

Window 함수가 실패하면 GetLastError 함수에 의해 오류 코드가 반환된다.

오류에 대한 설명

https://learn.microsoft.com/ko-kr/windows/win32/debug/system-error-codes--1300-1699

## wcout, wprintf 한글 출력

```
setlocale(LC_ALL, "");
```

> Reference  
> [psychoria.tistory - wcout, wprintf 등에서 한글 출력 안될 때 해결법](https://psychoria.tistory.com/132)  

## vsprintf_s

### format specifiers

|specifier|expected arguement|representation|
|---|---|---|
|s|pointer to the array of character(char* or wchar_t*)|character string|
|d|signed integer|decimical representation|
|u|unsigned integer| decimical representation|

> Reference  
> [cppreference - vfprintf](https://en.cppreference.com/w/c/io/vfprintf)  

## regex

> Reference  
> [modoocode - 17 - 2. C++ 정규 표현식(<regex>) 라이브러리 소개](https://modoocode.com/303#google_vignette)  

### capture group

regex 와 일치하는 문자열 중에서 특정 부분을 얻고 싶다면 capture group 을 갖는 regex 를 사용하면 된다.

caputre group 은 `()`로 감싸서 만들 수 있다.

전화번호의 가운데 번호를 caputre group 으로 갖는 regex를 만드는 예시를 보자.

```
std::regex re(R"[01]{3}-(\d{3,4})-\d{4}")
```

### smatch

n == 0 이면 regular expression 과 일치하는 전체 문자열을 나타내는 std::sub_match 객체를 리턴한다.

0 < n < size() 이면 regular expression 과 일치하는 전체 문자열중에서 n 번째 captured marked subexpression 과 일치하는 문자열을 나타내는 std::sub_match 객체를 리턴한다.



> Reference  
> [cppreference - match_results/operator_at](https://en.cppreference.com/w/cpp/regex/match_results/operator_at)  

## Raw String Literal

Raw String Literal 은 다음과 같은 형태로 작성된다.

```
R"(...)"
```

Raw String Literal 의 경우 R 뒤에 오는 (...) 안에 있는 모든 문자는 그대로 문자열로 간주된다.

```cpp
const auto str1 = R"(.*_(.*)\.ref)"; 
const auto str2 = ".*_(.*)\\.ref";
// str1 == str2
```

Raw String Literal 의 경우 특수 문자들도 그대로 표현할 수 있어 특수 문자가 많은 문자열을 표현할 때 유용하다.


## fread

fread-is-reading-the-last-part-of-my-text-file-twice

> Reference
> [stackoverflow - fread-is-reading-the-last-part-of-my-text-file-twice](https://stackoverflow.com/questions/76246014/fread-is-reading-the-last-part-of-my-text-file-twice)
> [cppreference - fread](https://en.cppreference.com/w/c/io/fread)  
> [stackoverflow - why-does-fread-always-return-0](https://stackoverflow.com/questions/3594475/why-does-fread-always-return-0)  

## Array

<details>
<summary> 배열 크기 미지정 후 초기화 </summary>
배열 선언 시 배열 크기를 따로 지정하지 않고 초기화를 진행하여도 초기화한 원소의 수에 맞는 고정크기 배열이 생성된다.

```cpp
int arr1[3] = {1,2,3};
int arr2[]  = {1,2,3};  //arr1과 동일
```
</details>

<details>
<summary> Multi Dimensional Array memory layout </summary>
static 2d array 를 생성하면 memory 연속성이 보장된다.

```cpp
int arr[2][4] = {0,1,2,3,4,5,6,7};
```

위와 같은 변수가 있으면 memory layout 은 다음과 같다.

```
arr[0][0]  arr[0][1]  arr[0][2]  arr[0][3]  arr[1][0]  arr[1][1]  arr[1][2]  arr[1][3]
   [0]       [1]        [2]        [3]        [4]        [5]        [6]        [7]
```

즉 4개 짜리 배열 2개가 연속으로 있는 형태이며, 첫번째 4개 짜리 배열은 a[0] 두번째 4개 짜리 배열은 a[1] 이 된다.

> Reference  
> [cppreference - book/arrays](https://en.cppreference.com/book/arrays)  
</details>

<details>
<summary> std::fill 로 초기화 </summary>
std::fill 을 이용해서 배열을 초기화하려면 다음과 같이 해야 한다.

 ```cpp
char* arr[2][4];
std::fill(&arr[0][0], &arr[0][0]+8, nullptr);
```
	
&arr[0][0] 와 arr 이 주소가 같다고  다음과 같이 초기화 하면 오류가 발생한다.

```cpp
char* arr[2][4];
std::fill(arr, arr+8, nullptr);
```

왜냐하면 fill 함수가 instance 화가 되면 arr 의 타입은 char* 가 아니라 char[4] 배열을 가르키는 포인터 char(*)[4] 가 된다.

따라서, 배열을 nullptr 로 초기화 하려고 해서 오류가 발생하게 된다. 거기다가 char[4] 배열을 가르키는 포인터에다가 + 8 을 했음으로 out of range 문제도 있다.

단, 1차원 배열일 경우에는 두 방식 모두 정상동작한다.
 ```cpp
char* arr[2];
std::fill(&arr[0], &arr[0]+2, nullptr); // ok
std::fill(arr, arr+2, nullptr); // ok
```
</details>



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

