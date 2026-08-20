# Linearly Independent

## 한 줄 요약

Linearly independent set은 zero vector를 만드는 finite linear combination의 coefficient가 모두 zero일 때만 가능한, 즉 서로 중복되는 vector가 없는 집합이다.

## Motivation

Span은 주어진 vector들로 어떤 공간을 만들 수 있는지 알려 주지만, 표현의 중복은 구분하지 않는다. 예를 들어 $v_3=v_1+v_2$이면

$$
v_3=0v_1+0v_2+1v_3
$$

이면서

$$
v_3=1v_1+1v_2+0v_3
$$

이므로 같은 vector에 서로 다른 coefficient가 생긴다. Basis에서 coordinate를 unique하게 만들려면 이런 중복을 제거해야 한다. Linear independence가 그 조건이다.

## Definition

Vector space $V/\F$와 subset $S\subseteq V$가 있다고 하자. $S$에서 선택한 서로 다른 vector $v_1,\ldots,v_k$에 대해

$$
a_1v_1+\cdots+a_kv_k=0_V
\implies
a_1=\cdots=a_k=0_\F
$$

가 항상 성립하면 $S$를 `linearly independent set`이라고 한다.

이 정의는 finite set과 infinite set에 모두 적용된다. Infinite set의 경우에도 한 번에 사용하는 linear combination은 finite하므로, 모든 finite subset이 linearly independent인지 확인한다. Empty set은 nontrivial finite relation이 없으므로 linearly independent하다고 본다.

Linearly independent하지 않은 set을 `linearly dependent set`이라고 한다. 즉 서로 다른 $v_1,\ldots,v_k\in S$와 모두 zero는 아닌 coefficient $a_1,\ldots,a_k$가 존재하여

$$
a_1v_1+\cdots+a_kv_k=0_V
$$

를 만족한다.

## Dependence Means Redundancy

### 정리1

Subset $S\subseteq V$가 linearly dependent인 것과 어떤 $v\in S$가 나머지 vector들의 span에 속하는 것은 동치다.

$$
S\text{ is linearly dependent}
\iff
\exists v\in S
\quad
v\in\operatorname{span}(S\setminus\{v\}).
$$

**Proof**

$S$가 linearly dependent이면 nontrivial relation

$$
a_1v_1+\cdots+a_kv_k=0_V
$$

이 존재한다. Nonzero coefficient 하나를 $a_j\ne0_\F$로 고르면

$$
v_j
=
-\sum_{i\ne j}\frac{a_i}{a_j}v_i
$$

이므로 $v_j\in\operatorname{span}(S\setminus\{v_j\})$다.

반대로 $v\in\operatorname{span}(S\setminus\{v\})$이면 어떤 $v_1,\ldots,v_k\in S\setminus\{v\}$와 $a_i\in\F$에 대해

$$
v=a_1v_1+\cdots+a_kv_k
$$

다. 따라서

$$
1_\F v-a_1v_1-\cdots-a_kv_k=0_V
$$

는 nontrivial relation이므로 $S$는 linearly dependent다. $\qed$

이 정리에 의해 $0_V$를 포함하는 set은 linearly dependent다. 실제로 $1_\F0_V=0_V$가 nontrivial relation이다.

## Adding a New Direction

### 정리2

$S$가 linearly independent이고 $v\notin\operatorname{span}(S)$이면

$$
S\cup\{v\}
$$

도 linearly independent다.

**Proof**

$S$에서 선택한 $s_1,\ldots,s_k$와 scalar $a,a_1,\ldots,a_k$가

$$
av+\sum_{i=1}^k a_is_i=0_V
$$

를 만족한다고 하자. 만약 $a\ne0_\F$이면

$$
v=-\sum_{i=1}^k\frac{a_i}{a}s_i
$$

여서 $v\in\operatorname{span}(S)$가 되므로 전제와 모순이다. 따라서 $a=0_\F$이고, $S$의 linear independence에 의해 $a_1=\cdots=a_k=0_\F$다. $\qed$

## Steinitz Exchange Lemma

### 정리3

$X=\{x_1,\ldots,x_r\}$가 finite linearly independent set이고 $Y=\{y_1,\ldots,y_s\}$가 finite generating set of $V$이면

$$
r\le s
$$

이고, $Y$에서 $r$개의 vector를 제거한 뒤 $X$를 넣어도 $V$를 span하도록 선택할 수 있다. 즉 적절히 순서를 바꾸면

$$
\operatorname{span}
\{x_1,\ldots,x_r,y_{r+1},\ldots,y_s\}
=
V
$$

가 된다.

**Proof**

처음에는 $Y$가 $V$를 span한다. $x_1$을 $Y$의 linear combination으로 쓸 때 적어도 한 coefficient는 nonzero다. 해당 $y_j$를 그 식에서 풀어 쓰면 $y_j$를 $x_1$로 바꾸어도 나머지 vector들이 여전히 $V$를 span한다.

이 과정을 반복한다고 하자. $i$번째 단계 전에

$$
\{x_1,\ldots,x_{i-1},y_i,\ldots,y_s\}
$$

가 $V$를 span하도록 $Y$의 순서를 정했다고 하자. $x_i$를 이 spanning set의 linear combination으로 표현할 때 $y_i,\ldots,y_s$의 coefficient가 모두 zero일 수는 없다. 그렇다면 $x_i\in\operatorname{span}\{x_1,\ldots,x_{i-1}\}$가 되어 $X$의 linear independence와 모순이기 때문이다.

따라서 nonzero coefficient를 가진 remaining vector가 하나 이상 존재하므로 $i\le s$다. 그 vector를 $y_i$로 순서를 바꾸고 식에서 $y_i$를 풀면, $y_i$를 $x_i$로 교체한 집합도 $V$를 span한다. $r$번 반복하면 $r\le s$이고 원하는 spanning set을 얻는다. $\qed$

## Maximal Independent Subset

$M\subseteq S$가 linearly independent이고 $S$ 안에서 더 이상 vector를 추가해 linear independence를 유지할 수 없으면 $M$을 $S$의 maximal linearly independent subset이라고 한다. 이때

$$
S\subseteq\operatorname{span}(M).
$$

만약 $v\in S\setminus\operatorname{span}(M)$가 존재하면 정리2에 의해 $M\cup\{v\}$도 linearly independent여서 maximality와 모순이기 때문이다.

## 관련 문서

- [Span](<03 Span.md>)
- [Basis](<05 Basis.md>)
- [Coordinate](<06 Coordinate.md>)
