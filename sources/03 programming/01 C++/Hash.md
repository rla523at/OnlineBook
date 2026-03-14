# Hash
해시(Hash)는 데이터를 고정된 크기의 값이나 키로 변환하는 것이다.

해시 함수(hash function)를 통해 원본 데이터를 특정한 크기의 해시 값(hash value)으로 변환한다.

## std::hash
std::hash 객체는 hash function 을 나타내는 객체로 operator() 메서드로 std::size_t 타입의 해시 값을 반환한다.

따라서, hash 값을 얻고 싶을 떄에는 std::hash 객체를 먼저 생성한 다음에 operator() 를 호출해야 한다.

참고로, std::size_t 는 일반적으로 64비트(64-bit) 또는 32비트(32-bit) 크기이며, 시스템의 주소 공간 크기에 따라 다르다.

MSVC 에서는 vcruntime.h 에 다음과 같이 정의되어 있다.

```cpp
#ifdef _WIN64
    typedef unsigned __int64 size_t;
#else
    typedef unsigned int size_t;
#endif
```

## boost::hash_combine
Boost 라이브러리에서 제공하는 함수로, 여러 개의 해시 값을 결합하여 하나의 해시 값을 생성할 때 사용된다.

구현은 다음과 같다.

```cpp
template <class T>
inline void hash_combine(std::size_t& seed, const T& value) {
    std::hash<T> hasher;
    seed ^= hasher(value) + 0x9e3779b9 + (seed << 6) + (seed >> 2);
}
```

0x9e3779b9는 해시를 더 고르게 분포시키기 위해 사용되는 상수이다.

이 상수는 골든 레이셔(Golden Ratio)를 기반으로 하여 해시의 충돌 가능성을 줄이는 역할을 한다.

(seed << 6)와 (seed >> 2)는 각각 비트를 왼쪽으로 6칸, 오른쪽으로 2칸 이동시키는 연산입니다. 이를 통해 기존 해시 값이 더 잘 섞이도록 한다.

> Reference  
> [boost.org/doc - hash.hpp](https://www.boost.org/doc/libs/1_51_0/boost/functional/hash/hash.hpp)  
