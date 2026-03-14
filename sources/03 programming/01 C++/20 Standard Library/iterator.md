# Iterator

* https://en.cppreference.com/w/cpp/iterator
* Instead of being defined by specific types, each category of iterator is defined by the operations that can be performed on it. This definition means that any type that supports the necessary operations can be used as an iterator 
* If an iterator falls into one of these categories and also satisfies the requirements of LegacyOutputIterator, then it is called a mutable iterator and supports both input and output. Non-mutable iterators are called constant iterators.

## Iterator category
* https://en.cppreference.com/w/cpp/iterator/iterator_tags
* For every LegacyIterator type It, a typedef std::iterator_traits<It>::iterator_category must be defined to be an alias to one of these tag types, to indicate the most specific category that It is in.

## Minimum Input Iterator
* https://en.cppreference.com/w/cpp/iterator/input_iterator

## Minimum Foward Iterator
* https://en.cppreference.com/w/cpp/iterator/forward_iterator

### std::forward_iterator 와 default constructable
`std::forward_iterator<I>` 에는 다음과 같은 개념 제약이 있습니다 (C++20):

```cpp
template<class I>
concept forward_iterator =
    std::input_iterator<I> &&
    std::derived_from</*…*/<I>, std::forward_iterator_tag> &&
    std::incrementable<I> &&
    std::sentinel_for<I, I>;
```
여기서 **`std::incrementable<I>`** 는 다시

```cpp
template<class I>
concept incrementable =
    std::regular<I> &&
    std::weakly_incrementable<I> &&
    requires(I i) { { i++ } -> std::same_as<I>; };
```
를 요구하고, `std::regular<I>` 는

```cpp
template<class I>
concept regular =
    std::semiregular<I> &&
    std::equality_comparable<I>;
```

를, `std::semiregular<I>` 는

```cpp
template<class I>
concept semiregular =
    std::copyable<I> &&
    std::default_initializable<I>;
```

를 요구합니다.

즉,

1. `forward_iterator` → `incrementable`
2. `incrementable` → `regular`
3. `regular` → `semiregular`
4. `semiregular` → `default_initializable`

의 계단식 요구를 통해 **최종적으로 `I` 가 default-constructible** (`std::default_initializable<I>`) 이어야 함을 **간접적으로** 강제합니다.

또한, 개념의 의미 요구사항(semantic requirements)으로 “i 와 j 가 **value-initialized** 상태일 때 서로 비교가 가능하고, 같은 타입의 value-initialized 반복자는 모두 같아야 한다.” 는 규칙을 두고 있는데, value-initialization 역시 “`I{}`” 또는 “`I()`” 처럼 기본 생성(default construction) 할 수 있어야 의미가 통합니다.


## 참고할만한 블로그
> Reference   
> [blog](https://www.internalpointers.com/post/writing-custom-iterators-modern-cpp)