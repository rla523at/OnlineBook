# Span

## 한 줄 요약

Span은 주어진 vector들로 만들 수 있는 모든 finite linear combination의 집합이며, 그 vector들을 포함하는 가장 작은 subspace다.

## Motivation

Subset $S\subseteq V$가 주어졌을 때 $S$ 자체는 addition이나 scalar multiplication에 닫혀 있지 않을 수 있다. $S$를 포함하는 subspace를 만들려면 $S$의 vector들을 더하고 scalar를 곱해서 생기는 모든 vector를 함께 포함해야 한다. 이 과정을 더 반복해도 새로운 종류의 식은 필요하지 않으며, 결국 $S$의 finite linear combination 전체를 얻는다.

## Linear Combination and Span

Vector space $V/\F$와 subset $S\subseteq V$가 있다고 하자. $S$에서 선택한 vector $v_1,\ldots,v_k$와 scalar $a_1,\ldots,a_k\in\F$로 만든

$$
a_1v_1+\cdots+a_kv_k
$$

를 $S$의 vector들의 `linear combination`이라고 하고 $a_i$를 coefficient라고 한다.

$S$의 `span`을 다음과 같이 정의한다.

$$
\operatorname{span}(S)
:=
\left\{
\sum_{i=1}^{k}a_iv_i
\ \middle|\
k\in\N,
a_i\in\F,
v_i\in S
\right\}.
$$

Empty set에 대해서는

$$
\operatorname{span}(\emptyset):=\{0_V\}
$$

로 정의한다.

예를 들어 standard basis vector $e_1,e_2\in\R^3$에 대해

$$
\operatorname{span}\{e_1,e_2\}
=
\{(a,b,0)\mid a,b\in\R\}
$$

이므로 $xy$-plane 전체를 얻는다.

## Span Is the Smallest Subspace

### 정리1

$\operatorname{span}(S)$는 $S$를 포함하는 $V$의 가장 작은 subspace다. 즉,

1. $\operatorname{span}(S)\le V$,
2. $S\subseteq\operatorname{span}(S)$,
3. $S\subseteq U\le V$이면 $\operatorname{span}(S)\subseteq U$

가 성립한다.

**Proof**

먼저 $0_V\in\operatorname{span}(S)$다. Empty set인 경우에는 convention에 의해 성립하고, $S\ne\emptyset$이면 $s\in S$를 골라 $0_\F s=0_V$로 쓸 수 있다.

두 linear combination

$$
x=\sum_{i=1}^{k}a_iv_i,
\qquad
y=\sum_{j=1}^{\ell}b_jw_j
$$

와 scalar $c,d\in\F$에 대해

$$
cx+dy
=
\sum_{i=1}^{k}(ca_i)v_i
+
\sum_{j=1}^{\ell}(db_j)w_j
$$

도 $S$의 finite linear combination이다. Subspace test에 의해 $\operatorname{span}(S)\le V$다.

각 $s\in S$는 $s=1_\F s$로 쓸 수 있으므로 $S\subseteq\operatorname{span}(S)$다.

마지막으로 $S\subseteq U\le V$라고 하자. Subspace $U$는 addition과 scalar multiplication에 닫혀 있으므로 $S$의 모든 finite linear combination을 포함한다. 따라서 $\operatorname{span}(S)\subseteq U$다. $\qed$

이 정리에 의해 다음과 같이 쓸 수도 있다.

$$
\operatorname{span}(S)
=
\bigcap_{\substack{U\le V\\S\subseteq U}}U.
$$

## Generating Set

$$
\operatorname{span}(S)=V
$$

이면 $S$를 $V$의 `generating set` 또는 `spanning set`이라고 한다. 이는 $V$의 모든 vector를 $S$의 finite linear combination으로 표현할 수 있다는 뜻이다.

## Basic Consequences

Subsets $X,Y\subseteq V$에 대해 다음이 성립한다.

$$
X\subseteq Y
\implies
\operatorname{span}(X)\subseteq\operatorname{span}(Y),
$$

$$
\operatorname{span}(\operatorname{span}(S))
=
\operatorname{span}(S),
$$

$$
\operatorname{span}(V)=V.
$$

첫 번째 식은 $Y$의 linear combination이 허용하는 vector 선택에 $X$의 선택이 모두 포함되기 때문이다. 두 번째 식은 $\operatorname{span}(S)$가 이미 subspace여서 그 안의 vector들을 다시 linear combination해도 밖으로 나가지 않기 때문이다.

Span은 어떤 vector들을 사용할 수 있는지는 말하지만, 같은 vector를 나타내는 coefficient가 unique한지는 보장하지 않는다. 이 중복 여부는 [Linearly Independent](<04 Linearly Independet.md>)에서 다룬다.

## 관련 문서

- [Subspace](<02 Subspace.md>)
- [Linearly Independent](<04 Linearly Independet.md>)
- [Basis](<05 Basis.md>)
