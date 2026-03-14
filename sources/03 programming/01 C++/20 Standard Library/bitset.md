# bitset

```
template< std::size_t N >
class bitset;
```

bitset 은 N bits 의 bit 시퀀스를 나타내는 클래스이다.

## bitset 객체 크기
N 값에 의존하여 바뀐다.

$N \in [1,32]$ 4 byte

$N \in [33,64]$ 8 byte

$N \in [64(m-1)+1, 64m]$ $8m$ btye, $m\in(2,3,4...)$

## bitset 객체에 char 전달하기
bitset 객체에 char 객체를 전달하면 생성자에 의해 char 에서 unsinged int 타입으로 형변환이 발생한다.

다행히도 이 형변환에서는 bit 상태가 바뀌지는 않는다.

```cpp
#include <iostream>

template <typename T>
void printBits(T c, const uint32_t num_print_bit){
  uint32_t bits = *reinterpret_cast<uint32_t>(&c);

  for (int i = numBit -1; i >= 0; --i)
    std::cout << ((bits>>i)&1);

  std::cout << std::endl;
}

int main(void){
  const char c = -1;
  const unsigned int ui = static_cast<unsigned int>(c);

  printBits(c,8);
  printBits(ui,8);
}
```

참고로 float 에서 unsigned int 로 형변환이 발생할 때는 bit 상태가 바뀐다.

```cpp
#include <iostream>

template <typename T>
void printBits(T c, const uint32_t num_print_bit){
  uint32_t bits = *reinterpret_cast<uint32_t>(&c);

  for (int i = numBit -1; i >= 0; --i)
    std::cout << ((bits>>i)&1);

  std::cout << std::endl;
}

int main(void){
  const float f = 1.0f;
  const unsigned int ui = static_cast<unsigned int>(f);

  printBits(f,32);
  printBits(ui,32);
}
```

그리고 bit 를 확인하기 위해서 uint32_t 로 보는 것은 convention 인 것 같다.
