# Minimal and Maximal Element
Poset $X$와 $X$의 subset $U$가 있다고 하자.

## Maximal Element
$u_M \in U$가 다음을 만족할 때, $u_M$을 $U$의 `maximal element`라고 한다.

$$ \forall u \in U, \quad  u_M \le u \implies u \le u_M $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Maximal_and_minimal_elements#Definition)

## Minimal Element
$u_m \in U$가 다음을 만족할 때, $u_m$을 $U$의 `minimal element`라고 한다.

$$ \forall u \in U, \quad  u \le u_m \implies u_m \le u $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Maximal_and_minimal_elements#Definition)

### 참고
Maximal element와 minimal element는 존재성과 유일성이 보장되지 않는다.

1. $\N$의 subset $S=[1,\infty)$는 maximal element를 갖지 않는다.
2. $S=\Set{s\in\Q |1\le s^2\le 2 }$는 maximal element를 갖지 않는다.
3. fence $S = \Set{ a_i,b_i |a_1 \le b_1 \ge a_2 \le b_2 \ge \cdots, a_i \neq b_i}$에서 모든 $a_i$는 mimal element이며 모든 $b_i$는 maximal element이다.