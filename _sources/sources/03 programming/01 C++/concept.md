# concept

C++20부터 지원하는 기능으로 concept는 template argument의 `이름있는 제약조건 집합(named set of requirements)`이다.

concept는 다음과 같은 형태로 정의한다.

```
template < template-parameter-list >
concept concept-name = constraint-expression;
```

A concept is a named set of requirements. The definition of a concept must appear at namespace scope.

The definition of a concept has the form




> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/language/constraints#Concepts)

## Require Clauses
require-clauses는 template argument의 제약조건을 특정하는 구문이다.

require-clauses는 `requires` keyword와 compile time bool type primary expression으로 구성되어 있다.

```
requires {compile time bool type primary expression}
```

자세한 사용법을 보기 위해 아래 예시 코드를 보자.

``` cpp
#include <concepts>

template <typename T>
constexpr bool is_meowable = true;

template <typename T>
constexpr bool is_purrable() {
  return true;
}

template <typename T>
concept is_quite = true;

template <typename T>
  requires is_meowable<T>
void f(T){};

template <typename T>
  requires(is_purrable<T>())
void g(T){};

template <typename T>
  requires is_quite<T>
void h(T){};

template <typename T>
  requires is_meowable<T> && (is_purrable<T>()) || is_quite<T>
void i(T){};

template <typename T>
void j(T)
  requires is_meowable<T>
{};

int main(void) {
  f(int());
  g(double());
  h(char());
  i(bool());
  j(short());
}

```


함수의 return 값은 primary expression이 아님으로 `()`로 감싸줌으로써 primary expression으로 바꾼 뒤 함수 `g`에서 처럼 require-clauses를 사용할 수 있다.

concept는 constexpr bool로 형변환 가능하기 때문에  함수 `h`에서 처럼 require-clauses를 사용할 수 있다.

bool type primary expression간의 연산의 결과도 bool type primary expression임으로 함수 `i`에서 처럼 require-clauses를 사용할 수 있다.

또한, require-clauses는 함수 선언 마지막에 위치할 수도 있음으로 함수 `j`에서 처럼 require-clauses를 사용할 수 있다.

> Reference  
> [cppreference - require clauses](https://en.cppreference.com/w/cpp/language/constraints#Requires_clauses)  
> [cppreference - primary expression](https://en.cppreference.com/w/cpp/language/expressions#Primary_expressions)  

## Requires Expression

### Example
``` cpp
template <typename T>
concept Tuple = requires(T t) { t[0]; };
```

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/language/requires)  

## 참고할만한 사이트

[kukuta blog](https://kukuta.tistory.com/252)