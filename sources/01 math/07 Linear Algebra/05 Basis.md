# Basis

## 한 줄 요약

Basis는 vector space 전체를 span하면서 linearly independent한 집합이며, 모든 vector에 unique한 coefficient 표현을 제공한다.

## Motivation

Generating set은 모든 vector를 만들 수 있지만 불필요한 vector를 포함할 수 있다. Linearly independent set은 중복이 없지만 공간 전체를 만들지 못할 수 있다. 두 조건을 동시에 만족하는 set을 선택하면 공간을 빠짐없이 표현하면서 coefficient의 중복도 제거할 수 있다. 이것이 basis가 필요한 이유다.

## Definition

Vector space $V/\F$의 subset $\beta\subseteq V$가

$$
\operatorname{span}(\beta)=V
$$

이고 linearly independent이면 $\beta$를 $V$의 `basis`라고 한다.

계산에서 vector의 순서가 필요할 때에는

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

처럼 `ordered basis`를 사용한다. Basis set이 같아도 순서가 달라지면 coordinate column의 entry 순서가 달라진다.

## Unique Representation

### 정리1

Subset $\beta\subseteq V$가 basis인 것과 모든 $v\in V$가 $\beta$의 vector들에 대한 finite linear combination으로 unique하게 표현되는 것은 동치다.

**Proof**

$\beta$가 basis이면 spanning property에 의해 표현이 존재한다. 두 표현에 등장하는 finite 개의 basis vector를 모두 모아 $\beta_1,\ldots,\beta_k$라고 하고, 한 표현에 나타나지 않는 vector의 coefficient는 $0_\F$로 두자. 그러면 두 표현을

$$
v=\sum_{i=1}^k a_i\beta_i
=
\sum_{i=1}^k b_i\beta_i
$$

가 있으면

$$
\sum_{i=1}^k(a_i-b_i)\beta_i=0_V.
$$

Linear independence에 의해 $a_i-b_i=0_\F$이므로 모든 coefficient가 같다.

반대로 모든 vector의 표현이 존재하므로 $\operatorname{span}(\beta)=V$다. 또한

$$
\sum_{i=1}^k a_i\beta_i=0_V
$$

라는 relation이 있으면 이는 $0_V$의 zero coefficients 표현과 같아야 한다. Uniqueness에 의해 모든 $a_i=0_\F$이므로 $\beta$는 linearly independent다. $\qed$

## Existence and Extension

### 정리2 (Basis extension theorem)

모든 linearly independent subset $S\subseteq V$는 $V$의 basis로 확장할 수 있다. 특히 모든 vector space는 basis를 갖는다.

**Proof**

$S$를 포함하는 linearly independent subset들의 collection을

$$
\mathcal P
:=
\{A\subseteq V\mid S\subseteq A,\ A\text{ is linearly independent}\}
$$

라고 하고 inclusion으로 order하자. $\mathcal P$의 chain $\mathcal C$에 대해 $\bigcup\mathcal C$를 생각하자. Union에서 생기는 linear relation은 finite 개의 vector만 사용한다. Chain이 totally ordered이므로 이 finite vector들을 모두 포함하는 한 member $A\in\mathcal C$가 존재한다. $A$가 linearly independent이므로 relation의 coefficient는 모두 zero다. 따라서 $\bigcup\mathcal C$도 linearly independent이고 chain의 upper bound다.

Zorn's lemma에 의해 $\mathcal P$에는 maximal element $M$이 존재한다. 만약 $v\notin\operatorname{span}(M)$인 vector가 있다면 $M\cup\{v\}$도 linearly independent이므로 maximality에 모순이다. 따라서 $\operatorname{span}(M)=V$이고 $M$은 $S$를 포함하는 basis다. $\qed$

이 정리는 Zorn's lemma, 따라서 axiom of choice에 의존한다. $S=\emptyset$로 두면 모든 vector space의 basis 존재를 얻는다. Zero vector space $\{0_V\}$의 basis는 empty set이다.

### 따름정리1

모든 generating set $G$는 basis인 subset을 포함한다.

$G$ 안의 maximal linearly independent subset을 $M$이라고 하자. Maximality에 의해 $G\subseteq\operatorname{span}(M)$이고, $G$가 $V$를 span하므로 $\operatorname{span}(M)=V$다. 따라서 $M\subseteq G$는 basis다.

## Dimension

모든 basis의 cardinality가 같다는 정리에 의해 vector space $V$의 `dimension`을 basis의 cardinality로 정의한다.

$$
\dim V:=|\beta|
$$

Finite basis를 가지면 finite-dimensional, 그렇지 않으면 infinite-dimensional vector space라고 한다. 특히

$$
\dim\{0_V\}=0.
$$

Finite-dimensional case에서 두 basis의 크기가 같은 이유는 Steinitz exchange lemma로 확인할 수 있다. 두 finite basis를 $\beta,\gamma$라고 하면 $\beta$는 linearly independent이고 $\gamma$는 generating set이므로

$$
|\beta|\le|\gamma|.
$$

역할을 바꾸면 $|\gamma|\le|\beta|$이므로 두 cardinality가 같다.

## Finite-Dimensional Consequences

$\dim V=n<\infty$라고 하자.

1. $n$개의 vector로 이루어진 generating set은 basis다.

   Generating set 안에서 basis를 추출할 수 있는데, 그 basis도 $n$개의 vector를 가져야 하므로 원래 set 전체가 basis다.

2. $n$개의 vector로 이루어진 linearly independent set은 basis다.

   Steinitz exchange lemma로 이 set을 임의의 basis의 vector들과 교환하면 추가로 남는 basis vector가 없으므로 이 set이 $V$를 span한다.

3. Subspace $U\le V$에 대해
   $$
   \dim U=\dim V
   \implies
   U=V.
   $$
   $U$의 basis는 $V$에서 linearly independent한 $n$개의 vector이므로 $V$의 basis이기도 하다.

## Basis and Direct Sum

Subspaces $W_1,\ldots,W_k\le V$에 대해

$$
V=W_1\oplus\cdots\oplus W_k
$$

이고 각 $W_i$의 basis가 $\beta_i$이면

$$
\beta_1\cup\cdots\cup\beta_k
$$

는 $V$의 basis다.

각 $v\in V$는 unique하게 $v=w_1+\cdots+w_k$, $w_i\in W_i$로 분해되고, 각 $w_i$는 $\beta_i$의 vector들로 unique하게 표현된다. 따라서 union은 $V$를 span한다. Union의 linear combination이 $0_V$이면 direct-sum decomposition의 uniqueness에 의해 각 $W_i$에 속하는 부분이 $0_V$이고, 다시 $\beta_i$의 linear independence에 의해 모든 coefficient가 zero다.

반대로 finite basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

를 두 부분으로 나누어

$$
U:=\operatorname{span}\{\beta_1,\ldots,\beta_r\},
\qquad
W:=\operatorname{span}\{\beta_{r+1},\ldots,\beta_n\}
$$

로 두면 basis coordinate의 existence와 uniqueness에 의해

$$
V=U\oplus W
$$

다.

## 관련 문서

- [Linearly Independent](<04 Linearly Independet.md>)
- [Coordinate](<06 Coordinate.md>)
- [Vector Space Isomorphism](<13 Vector Space Isomorphism.md>)
