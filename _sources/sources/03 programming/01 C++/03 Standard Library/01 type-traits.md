# type-traits
Template metaprogramming을 위한 library로 C++11부터 제공한다.

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/header/type_traits)

## Integral_constants

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/types/integral_constant)

### Possible implementation
``` cpp
template<class T, T v>
struct integral_constant {
    static constexpr T value = v;
    using value_type = T;
    using type = integral_constant; // using injected-class-name
    constexpr operator value_type() const noexcept { return value; }
    constexpr value_type operator()() const noexcept { return value; } // since c++14
};
```

### Specializations
다음 두가지 경우에 대해서 template specialization을 제공한다.

``` cpp
using true_type = std::integral_constant<bool, true>;
using false_type = std::integral_constant<bool, false>;
```

## is_same
> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/types/is_same)  

### Possible implementation
```cpp
template<class T, class U>
struct is_same : std::false_type {};
 
template<class T>
struct is_same<T, T> : std::true_type {};
```

### Alias(C++17)

```cpp 
template< class T, class U >
inline constexpr bool is_same_v = is_same<T, U>::value;
```

만약 T와 U가 다른 경우 is_same은 std::false_type을 상속받게 되고 value로 false를 갖게 된다.

따라서 `is_same_v = false`가 된다.

반대로 T와 U가 같은 경우 is_same은 std::true_type을 상속받게 되고 `is_same_v = true`가 된다.