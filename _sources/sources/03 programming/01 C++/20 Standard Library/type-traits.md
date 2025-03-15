# type-traits
Template metaprogramming을 위한 library로 C++11부터 제공한다.

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/header/type_traits)

## decay
주어진 타입에서 참조와 cv-한정자(const와 volatile)를 제거하고, 배열 및 함수 타입을 적절히 변환하여 "기본" 타입을 도출하는 데 사용한다. 이는 함수 템플릿 인수로 전달된 타입을 표준화하는 데 유용하다.

decay가 어떻게 이런 역할을 하는지 그 정의를 살펴보자.

```cpp
    _EXPORT_STD template <class _Ty>
    using decay_t = typename decay<_Ty>::type;

    _EXPORT_STD template <class _Ty>
    struct decay { // determines decayed version of _Ty
    using _Ty1 = remove_reference_t<_Ty>;
    using _Ty2 = typename _Select<is_function_v<_Ty1>>::template _Apply<add_pointer<_Ty1>, remove_cv<_Ty1>>;
    using type = typename _Select<is_array_v<_Ty1>>::template _Apply<add_pointer<remove_extent_t<_Ty1>>, _Ty2>::type;
    };

    template <bool>
    struct _Select { // Select between aliases that extract either their first or second parameter
        template <class _Ty1, class>
        using _Apply = _Ty1;
    };

    template <>
    struct _Select<false> {
        template <class, class _Ty2>
        using _Apply = _Ty2;
    };
```

먼저 decay의 _Ty1 타입은 주어진 argument의 reference를 제거한 타입이 된다.
다음으로 _Ty2 타입은 만약 _Ty1이 함수일 경우 add_pointer<_Ty1> 타입이 되고 아닐 경우 _Ty1에 cv qualifier를 제거한 타입이 된다.
마지막으로 type 타입은 만약 _Ty1이 배열일 경우 add_pointer<remove_extent_t<_Ty1>> 타입 내부에 정의된 type이 되고 아닐 경우 _Ty2 타입 내부에 정의된 type이 된다.

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