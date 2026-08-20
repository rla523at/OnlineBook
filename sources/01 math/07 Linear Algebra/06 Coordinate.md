# Coordinate

## 한 줄 요약

Coordinate는 ordered basis를 선택했을 때 vector를 나타내는 unique scalar column이며, vector 자체가 아니라 basis에 의존하는 표현이다.

## Motivation

Abstract vector는 계산에 바로 사용할 scalar entry를 갖지 않을 수 있다. Basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

를 선택하면 모든 vector를 basis vector들의 unique linear combination으로 쓸 수 있다. 이때 coefficient만 column으로 모으면 abstract vector를 $\F^n$의 계산으로 옮길 수 있다.

Basis를 바꾸면 coefficient는 달라질 수 있지만 원래 vector는 바뀌지 않는다. 따라서 vector와 coordinate column을 구분해야 한다.

## Definition

$n$-dimensional vector space $V/\F$의 ordered basis를

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

이라고 하자. 모든 $v\in V$에는 unique한 scalar $a_1,\ldots,a_n\in\F$가 존재하여

$$
v=a_1\beta_1+\cdots+a_n\beta_n
$$

로 쓸 수 있다. 이 coefficient를 순서대로 모은 column

$$
[v]_\beta
:=
\begin{bmatrix}
a_1\\
\vdots\\
a_n
\end{bmatrix}
\in\F^n
$$

을 $v$의 basis $\beta$에 대한 `coordinate column`이라고 한다.

Existence는 $\beta$가 $V$를 span한다는 사실에서, uniqueness는 $\beta$가 linearly independent라는 사실에서 나온다. 실제로

$$
\sum_{i=1}^n a_i\beta_i
=
\sum_{i=1}^n b_i\beta_i
$$

이면

$$
\sum_{i=1}^n(a_i-b_i)\beta_i=0_V
$$

이고, linear independence에 의해 $a_i=b_i$다.

## Coordinate Map

Basis $\beta$가 정하는 `coordinate map`을

$$
C_\beta:V\rightarrow\F^n,
\qquad
C_\beta(v):=[v]_\beta
$$

로 정의한다.

### 정리1

$C_\beta$는 vector space isomorphism이다.

**Proof**

$$
u=\sum_{i=1}^n a_i\beta_i,
\qquad
v=\sum_{i=1}^n b_i\beta_i
$$

라고 하면 모든 $c,d\in\F$에 대해

$$
cu+dv
=
\sum_{i=1}^n(ca_i+db_i)\beta_i.
$$

따라서

$$
C_\beta(cu+dv)
=
cC_\beta(u)+dC_\beta(v)
$$

이므로 $C_\beta$는 linear하다.

$C_\beta(v)=0_{\F^n}$이면 $v=\sum_i0_\F\beta_i=0_V$이므로 injective다. 또한 임의의 column $(a_1,\ldots,a_n)^{\mathsf T}\in\F^n$은 vector $\sum_i a_i\beta_i$의 coordinate이므로 surjective다. 따라서 $C_\beta$는 isomorphism이다. $\qed$

Inverse coordinate map은 scalar column을 abstract vector로 복원한다.

$$
C_\beta^{-1}
\left(
\begin{bmatrix}
a_1\\
\vdots\\
a_n
\end{bmatrix}
\right)
=
\sum_{i=1}^n a_i\beta_i.
$$

## Coordinates Depend on the Basis

$\R^2$의 standard basis를 $\beta=(e_1,e_2)$라고 하고

$$
\gamma=(e_1+e_2,e_2)
$$

라고 하자. 같은 vector

$$
v=e_1+2e_2
$$

에 대해

$$
[v]_\beta
=
\begin{bmatrix}
1\\
2
\end{bmatrix}.
$$

한편

$$
v=1(e_1+e_2)+1e_2
$$

이므로

$$
[v]_\gamma
=
\begin{bmatrix}
1\\
1
\end{bmatrix}.
$$

두 column은 다르지만 둘 다 같은 $v$를 나타낸다. 어떤 basis의 coordinate인지 표시하지 않고 vector와 column을 동일시하면 이 차이를 놓치게 된다. 두 coordinate column 사이의 변환은 [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)에서 다룬다.

## 관련 문서

- [Basis](<05 Basis.md>)
- [Linear Map](<10 Linear Map.md>)
- [Matrix Representation](<20 Matrix Representation.md>)
- [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)
