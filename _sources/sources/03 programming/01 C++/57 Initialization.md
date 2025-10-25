# initialization

> 참고  
> [initialization - cppreference](https://en.cppreference.com/w/cpp/language/initialization)

## Default Initialization

The effects of default-initialization are:

if T is a (possibly cv-qualified) non-POD(until C++11) class type, the constructors are considered and subjected to overload resolution against the empty argument list. The constructor selected (which is one of the default constructors) is called to provide the initial value for the new object;
if T is an array type, every element of the array is default-initialized;
otherwise, no initialization is performed (see notes).

> Reference
> [cppreference - default_initialization](https://en.cppreference.com/w/cpp/language/default_initialization)

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