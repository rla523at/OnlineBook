# concept

C++20부터 지원하는 기능으로 concept는 template argument의 `이름있는 제약조건 집합(named set of requirements)`이다.

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/language/constraints#Concepts)

## Concept 생성방법
concept는 다음과 같은 형태로 정의한다.

```cpp
template < template-parameter-list >
concept concept-name = constraint-expression;
```

constraint-expression는 컴파일 time에 bool 값을 돌려주는 호출 가능 객체이다.

constraint-expression의 예시들은 다음과 같다.

``` cpp
template <class _Ty>
concept always_true_concept = true;

template <class T>
concept is_char_concept1 = std::is_same_v<T, char>;

template <typename T>
constexpr bool is_char_func(void) { return false; };

template <>
constexpr bool is_char_func<char>(void) { return true; };

template <class _Ty>
concept is_char_concept2 = is_char_func<_Ty>();
```

### 참고1
constraint-expression의 논리 조합은 contraint_expression이다.

이 떄 사용가능한 논리 조합에는 논리 연산자( &&, ||, ! )가 있다.

``` cpp
template <class T>
concept is_vector_concept = std::is_same_v<T, std::vector<typename T::value_type>>;

template <class T>
concept is_int_concpet = std::is_same_v<T, int>;

template <class T>
concept is_vector_or_int_concept = is_vector_concept<T> || is_int_concpet<T>;

TEST(concept, test3)
{
  EXPECT_TRUE(is_vector_or_int_concept<int>);
  EXPECT_TRUE(is_vector_or_int_concept<std::vector<double>>);
  EXPECT_FALSE(is_vector_or_int_concept<unsigned int>);
}
```

### 참고2

concept는 constexpr bool로 형변환 가능하기 때문에 그 자체로 contraint_expression이 될 수 있다.

```cpp
// integral은 is_integral_v 컴파일 술어를 사용해서 정의합니다.
template <class _Ty>
concept integral = std::is_integral_v<_Ty>;

// unsigned_integral은 콘셉트인 integral, signed_integral을 사용해서 정의합니다.
template <class _Ty>
concept unsigned_integral = integral<_Ty> && !signed_integral<_Ty>;

// signed_integral는 콘셉트인 integral을 사용해서 정의합니다. 
// static_cast<_Ty>(-1) < static_cast<_Ty>(0) 컴파일 술어를 사용
template <class _Ty>
concept signed_integral = integral<_Ty> && static_cast<_Ty>(-1) < static_cast<_Ty>(0);
```
### 참고3
concept는 동일한 이름이고 parameter 수만 다르게 만들 수가 없다.

```cpp
template <typename T>
concept is_spanable = requires(T t)
{
  t;
};

template <typename T, typename U>
concept is_spanable = requires(T t, U u)
{
  t + u;
};

template <typename T>
  requires is_spanable<T>
bool g(T)
{
  return true;
}

template <typename T, typename U>
  requires is_spanable<T, U> // Error!
bool g(T, U)
{
  return true;
}


```


## Template Parameter

템플릿 매개변수에 콘셉트를 지정할 수 있다. 

아래 예제 코드를 보자.

```cpp
#include "gtest/gtest.h"
#include <concepts>

// 제약이 있는 템플릿 매개변수
template<std::integral T>
auto gcd(T a, T b) { return a; }

// 단축 함수 템플릿 - 제약 있는 형식 매개변수 
// 이 형태만 a와 b의 형식이 다를 수 있습니다.
std::integral auto gcd1(std::integral auto a, std::integral auto b) { return a; }

template<typename T>
struct Temp : public std::false_type {};

// 템플릿 특수화로 사용된 콘셉트
template<std::regular Reg>
struct Temp<Reg> : public std::true_type {};

// 가변 인수 템플릿에서 사용
template<std::integral... Args>
bool all(Args... args) { return (... && args); }

template<std::integral... Args>
bool any(Args... args) { return (... || args); }

template<std::integral... Args>
bool none(Args... args) { return !(... || args); }

TEST(concept, test1)
{
  EXPECT_TRUE(Temp<int>::value); // std:regular를 통한 특수화
  EXPECT_TRUE(any(5, true, false));
  EXPECT_FALSE(Temp<int&>::value); // 일반 템플릿 Test 구조체 호출   
  EXPECT_FALSE(all(5, true, false));
  EXPECT_FALSE(none(5, true, false));
}
```

> Reference  
> [blog](https://jungwoong.tistory.com/99)  