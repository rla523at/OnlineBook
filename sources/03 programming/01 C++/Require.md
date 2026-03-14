# Require
## Require Clauses
require-clauses는 template argument의 제약조건을 특정하는 구문이다.

require-clauses는 `requires` keyword와 제약조건을 나타내는 expression으로 구성되어 있다.

```
requires {expression}
```

이 떄, expression은 contraint-expression이면서 primary expression이여야 한다.

그리고 expression끼리의 joint operator (&& or ||)을 사용할 수 있다.

```cpp
template <typename T>
int f(T) { return 1; };

template <typename T>
  requires std::is_same_v<T, int>
int f(T) { return 2; };

template <typename T>
  requires std::is_unsigned_v<T> && (sizeof(T) == 4)
int f(T) { return 3; };

template <typename T>
  requires std::is_same_v<T, float> || std::is_same_v<T, double>
int f(T) { return 4; };

TEST(concept, test4)
{
  EXPECT_EQ(f(char()), 1);
  EXPECT_EQ(f(int()), 2);
  EXPECT_EQ(f(unsigned int()), 3);
  EXPECT_EQ(f(unsigned long long()), 1);
  EXPECT_EQ(f(float()), 4);
  EXPECT_EQ(f(double()), 4);
}
```

### bad practices

#### 여러개를 만족시키는 경우
```cpp
template <typename T>
 requires true
int f(T) { return 1; };

template <typename T>
 requires std::is_same_v<T, int>
int f(T) { return 2; };

TEST(concept, test)
{
  EXPECT_EQ(f(int()), 2);
}

```
이 경우 requires를 모두 만족하기 때문에 어느 함수를 호출해야 될지 몰라 오류가 발생한다.


#### !expression
```cpp
//template <typename T>
//  requires !true
//int f(T) { std::cout << "f3\n"; };
```

expression끼리는 &&와 || 연산자만 가능하다.

#### not primary expression
```cpp
template <typename T>
bool is_float() { return std::is_same_v<T, float>; };

template <typename T>
  requires (is_float<T>())
void f(T) { std::cout << "f3\n"; };
```
함수의 return 값은 primary expression이 아님으로 오류가 발생한다.

이 떄, `()`로 감싸줌으로써 primary expression으로 바꿔주면 expression으로 사용할 수 있다.

> Reference  
> [cppreference - require clauses](https://en.cppreference.com/w/cpp/language/constraints#Requires_clauses)  
> [cppreference - primary expression](https://en.cppreference.com/w/cpp/language/expressions#Primary_expressions)  

## Requires Expression

요구조건 표현식

```cpp
// 요구 조건 표현식 
// parameter-list : 함수 선언의 파라미터와 같음
// requirement-seq : 단순 요구조건, 형식 요구조건, 복합 요구 조건, 중첩 요구 조건으로 구성된 순차열
requires ( parameter-list(optional) ) { requirement-seq }
```

### 단순 요구 조건
아래 예제와 같이 괄호에 묶여서 선언되어 사용됩니다.

```cpp
// Addable는 T타입에 대해서 a + b 표현식이 유효해야 합니다.
template<typename T>
concept Addable = requires(T a, T b) {
	a + b;
};

// Subtractable는 T타입에 대해서  a - b 표현식이 유효해야 합니다.
template<typename T>
concept Subtractable = requires(T a, T b) {
	a - b;
};

// Swappable은 T, U가 swap 표현식을 만족 해야 합니다.
template<typename T, typename U = T>
concept Swappable = requires(T&& t, U&& u)
{
    swap(std::forward<T>(t), std::forward<U>(u));
    swap(std::forward<U>(u), std::forward<T>(t));
};
```

### 형식 요구 조건

형식 요구 조건을 표현 할 때는 typename과 형식 이름을 사용합니다.

``` cpp
template<typename T>
using Ref = T&;

template<typename T>
concept RequirementType = requires {
    // T가 key_type을 가져야 합니다. 
    typename T::key_type;
    typename T::allocator_type;
    // Ref를 T로 인스턴스화 할 수 있어야 합니다.
    typename Ref<T>;
};

auto printValueType(RequirementType auto a) {
    cout << typeid(a).name() << endl;
}

int main()
{
    // printValueType(std::vector<int>{1, 2, 3});  // 관련 제약 조건이 충족되지 않습니다
    printValueType(std::map<int, int>{ {1, 2}, { 2, 3 }, { 3, 4 } });
}
```

### 복합 요구 조건

복합 요구 조건이 단순 요구 조건가 다른 점은 표현식에 {}(중괄호)를 감싸야 합니다.
noexcept 지정자와 반환 형식 요구조건을 추가로 붙일 수 있습니다. 

```cpp
// 복합 요구 조건은 다음의 형식을 만족해야 합니다.
{ expression } noexcept(optional) return-type-requirement(optional) ;		
```

```cpp
template<typename T>
concept C2 = requires(T x)
{
    // *x라는 표현식이 유효해야 합니다
    // T::inner라는 타입도 유효 해야 합니다. 
    // *x의 결과는 T::inner로 변환 가능해야합니다.
    {*x} -> std::convertible_to<typename T::inner>;
 
    // x + 1라는 표현식이 유효 해야 하며 그 결과가 int형식이어야 합니다.
    {x + 1} -> std::same_as<int>;
};
```

### 중첩 요구 조건

중첩 요구 조건은 requires안에 requires로 시작하는 표현식이 있는 것을 말합니다. 

``` cpp
template<class T>
concept Semiregular = DefaultConstructible<T> &&
    CopyConstructible<T> && Destructible<T> && CopyAssignable<T> &&
requires(T a, size_t n)
{  
    requires Same<T*, decltype(&a)>; // 중첩 요구 조건
    requires Same<T*, decltype(new T)>; // 중첩 요구 조건
    requires Same<T*, decltype(new T[n])>; // 중첩 요구 조건
    { a.~T() } noexcept; // 복합 요구 조건
    { delete new T }; // 복합 요구 조건
    { delete new T[n] }; // 복합 요구 조건
};
```

### Example
``` cpp
template <typename T>
concept Tuple = requires(T t) { t[0]; };
```

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/language/requires)  
> [blog](https://jungwoong.tistory.com/99)   
