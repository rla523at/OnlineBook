# initialization

> 참고  
> [initialization - cppreference](https://en.cppreference.com/w/cpp/language/initialization)

## Default Initialization
This is the initialization performed when an object is constructed with no initializer.

```cpp
T object;
new T
```

Default-initialization is performed in three situations
* when a variable with automatic, static, or thread-local storage duration is declared with no initializer;
* when an object with dynamic storage duration is created by a new-expression with no initializer;
* when a base class or a non-static data member is not mentioned in a constructor initializer list and that constructor is called.

### Effect
The effects of default-initialization are:
* if T is a (possibly cv-qualified) non-POD(until C++11) class type, the constructors are considered and subjected to overload resolution against the empty argument list. The constructor selected (which is one of the default constructors) is called to provide the initial value for the new object;
* if T is an array type, every element of the array is default-initialized;
* otherwise, no initialization is performed.

> Reference  
> [cppreference - default_initialization](https://en.cppreference.com/w/cpp/language/default_initialization)  

## Value Initialization
This is the initialization performed when an object is constructed with an empty initializer.

Syntex
| 구문                                        | 번호  | 비고          |
| ----------------------------------------- | --- | ----------- |
| `T()`                                     | (1) |             |
| `new T()`                                 | (2) |             |
| `Class::Class(...) : member()` `{ ... }`  | (3) |             |
| `T object {}`                             | (4) | since C++11 |
| `T {}`                                    | (5) | since C++11 |
| `new T {}`                                | (6) | since C++11 |
| `Class::Class(...) : member {}` `{ ... }` | (7) | since C++11 |


Value-initialization is performed in these situations:
* 1,5) when a nameless temporary object is created with the initializer consisting of an empty pair of parentheses or braces(since C++11);
* 2,6) when an object with dynamic storage duration is created by a new expression with the initializer consisting of an empty pair of parentheses or braces(since C++11);
* 3,7) when a non-static data member or a base class is initialized using a member initializer with an empty pair of parentheses or braces(since C++11);
* 4) when a named object (automatic, static, or thread-local) is declared with the initializer consisting of a pair of braces.

In all cases, if the empty pair of braces {} is used and T is an aggregate type, aggregate initialization is performed instead of value-initialization.


### Effect
The effects of value-initialization are:
* If T is a (possibly cv-qualified) class type:
  * If the default-initialization for T selects a constructor, and the constructor is not user-declared(until C++11)user-provided(since C++11), the object is first zero-initialized.
    * zero-initalized 된 후에 deafult-initialization 이 발생한다.
  * In any case, the object is default-initialized.
* Otherwise, if T is an array type, each element of the array is value-initialized.
* Otherwise, the object is zero-initialized.

### default-initialization VS value-initialization

```cpp
struct A
{
  int a;
};

int main(void)
{
  A a;
  A b{};
  return 0;
}
```

a 객체의 경우 default-initialization 되어 a.a 는 쓰레기 값이 들어있다.

하지만 b 객체의 경우 value-initialization 되어 0 으로 초기화 됨으로 b.a 는 0 이 들어가 있다.


> Reference  
> [cppreference - value_initialization](https://en.cppreference.com/w/cpp/language/value_initialization)  

## Zero Initialization

만약 T 가 scalar type 이라면 객체는 0 을 T 로 한 값으로 초기화 된다.
```cpp
T t = {}; // T t = T(0)
```


생성자가 없는 class 타입의 경우 non-static 멤버 변수들은 zero-initialized 된다.

```cpp
struct A
{
    int a, b, c;
};

A a = {}; // a.a = a.b = a.c = 0
```

```cpp
double f[3];   // zero-initialized to three 0.0's
```


> Reference  
> [cppreference - zero_initialization)](https://en.cppreference.com/w/cpp/language/zero_initialization)  


## Aggregate initialization
An aggregate is one of the following types:

* array types
* class types that has
  * no user-declared or inherited constructors (since C++20)
    * no user-declared constructors (until C++11)
    * no user-provided, inherited, or explicit constructors (since C++11) (until C++20)
  * no private or protected direct non-static data members  
  * no virtual base classes & no private or protected direct base classes (since C++17)
    * no base classes(until C++17)
  * no virtual member functions
  * no default member initializers (since C++11) (until C++14)

> Reference  
> [cppreference - aggregate_initialization](https://en.cppreference.com/w/cpp/language/aggregate_initialization)  

### Designated initializers
Since C++20, aggregate class only.

each designator must name a direct non-static data member of T

all designators used in the expression must appear in the same order as the data members of T
```cpp
struct A { int x; int y; int z; };
 
A a{.y = 2, .x = 1}; // error; designator order does not match declaration order
A b{.x = 1, .z = 2}; // ok, b.y initialized to 0
```


> Reference  
> [cppreference - Designated_initializers](https://en.cppreference.com/w/cpp/language/aggregate_initialization#Designated_initializers)