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
