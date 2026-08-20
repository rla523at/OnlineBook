# Matrix Representation

## 한 줄 요약

Linear map의 matrix representation은 domain과 codomain의 ordered basis를 선택했을 때 input coordinate를 output coordinate로 보내는 matrix다.

## Motivation

Linear map $T:V\rightarrow W$는 abstract vector 사이에서 정의되므로, basis를 선택하기 전에는 entry를 가진 matrix가 아니다. 계산하려면 input vector를 domain basis의 coordinate column으로 바꾸고, output vector를 codomain basis의 coordinate column으로 바꾸어야 한다.

Domain basis를 $\beta$, codomain basis를 $\gamma$라고 하자. 원하는 matrix $A$는 모든 $v\in V$에 대해

$$
[T(v)]_\gamma
=
A[v]_\beta
$$

를 만족해야 한다. 이 식은 $A$가 vector 자체가 아니라 두 coordinate representation 사이에서 작용한다는 뜻이다.

## Vector Representation

$n$-dimensional vector space $V/\F$의 ordered basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

에 대해 vector

$$
v=\sum_{i=1}^n a_i\beta_i
$$

의 matrix representation은 coordinate column

$$
[v]_\beta
=
\begin{bmatrix}
a_1\\
\vdots\\
a_n
\end{bmatrix}
\in\F^n
$$

이다. [Coordinate](<06 Coordinate.md>)에서 본 것처럼 map $v\mapsto[v]_\beta$는 $V$와 $\F^n$ 사이의 vector space isomorphism이다.

## Matrix of a Linear Map

$\dim V=n$, $\dim W=m$이고 ordered bases를 각각

$$
\beta=(\beta_1,\ldots,\beta_n),
\qquad
\gamma=(\gamma_1,\ldots,\gamma_m)
$$

이라고 하자. 각 domain basis vector의 image를 $\gamma$로 표현하면 unique한 scalar $A_{ji}$가 존재하여

$$
T(\beta_i)
=
\sum_{j=1}^m A_{ji}\gamma_j.
$$

이 coefficient를 column으로 배열한 matrix

$$
[T]_\beta^\gamma
:=
\begin{bmatrix}
[T(\beta_1)]_\gamma
&
\cdots
&
[T(\beta_n)]_\gamma
\end{bmatrix}
\in\F^{m\times n}
$$

를 $T$의 domain basis $\beta$와 codomain basis $\gamma$에 대한 `matrix representation`이라고 한다. Lower index $\beta$는 input coordinate의 basis를, upper index $\gamma$는 output coordinate의 basis를 나타낸다. Matrix의 $i$번째 column은 $T(\beta_i)$의 $\gamma$-coordinate다.

## Fundamental Coordinate Equation

### 정리1

모든 $v\in V$에 대해

$$
\boxed{
[T(v)]_\gamma
=
[T]_\beta^\gamma[v]_\beta
}
$$

가 성립한다.

**Proof**

$$
v=\sum_{i=1}^n a_i\beta_i
$$

라고 하면

$$
\begin{aligned}
T(v)
&=
\sum_{i=1}^n a_iT(\beta_i)\\
&=
\sum_{i=1}^n a_i
\sum_{j=1}^m A_{ji}\gamma_j\\
&=
\sum_{j=1}^m
\left(
\sum_{i=1}^n A_{ji}a_i
\right)\gamma_j.
\end{aligned}
$$

따라서 output의 $j$번째 coordinate는 $\sum_iA_{ji}a_i$이고, 이를 column으로 모으면 matrix product

$$
[T(v)]_\gamma
=
[T]_\beta^\gamma[v]_\beta
$$

를 얻는다. $\qed$

## Example

Standard basis를 사용하는 $\R^2$에서

$$
T(x,y):=(x+2y,3x-y)
$$

라고 하자. Basis vector의 image는

$$
T(e_1)=
\begin{bmatrix}
1\\
3
\end{bmatrix},
\qquad
T(e_2)=
\begin{bmatrix}
2\\
-1
\end{bmatrix}
$$

이므로

$$
[T]_\epsilon^\epsilon
=
\begin{bmatrix}
1&2\\
3&-1
\end{bmatrix}.
$$

Column은 각각 한 input basis direction이 어디로 이동하는지를 기록한다.

## The Representation Map Is an Isomorphism

Fixed bases $\beta,\gamma$에 대해

$$
\mathcal M_\beta^\gamma
:
L(V,W)\rightarrow\F^{m\times n},
\qquad
T\mapsto[T]_\beta^\gamma
$$

를 정의하자.

### 정리2

$\mathcal M_\beta^\gamma$는 vector space isomorphism이다.

**Proof**

$S,T\in L(V,W)$와 $a,b\in\F$에 대해 각 basis vector의 image를 계산하면

$$
[aS+bT]_\beta^\gamma
=
a[S]_\beta^\gamma+b[T]_\beta^\gamma
$$

이므로 representation map은 linear하다.

$[T]_\beta^\gamma=0$이면 모든 $i$에 대해 $T(\beta_i)=0_W$다. Linear map은 basis에서의 값으로 결정되므로 $T=0_{L(V,W)}$이고 representation map은 injective다.

반대로 임의의 $A\in\F^{m\times n}$가 주어졌다고 하자. Coordinate maps를 사용해

$$
T
:=
C_\gamma^{-1}\circ L_A\circ C_\beta,
\qquad
L_A(x):=Ax
$$

로 정의하면 $T$는 linear하고 모든 $v\in V$에 대해

$$
[T(v)]_\gamma=A[v]_\beta
$$

를 만족한다. 따라서 $[T]_\beta^\gamma=A$이고 representation map은 surjective다. $\qed$

즉 basis를 고정하면 abstract linear map과 $m\times n$ matrix가 일대일로 대응한다. Basis가 달라지면 같은 linear map에 대응하는 matrix도 달라질 수 있다.

## Composition Becomes Matrix Multiplication

Linear maps

$$
T:U\rightarrow V,
\qquad
S:V\rightarrow W
$$

와 ordered bases $\alpha,\beta,\gamma$가 각각 $U,V,W$에 주어졌다고 하자.

### 정리3

$$
\boxed{
[S\circ T]_\alpha^\gamma
=
[S]_\beta^\gamma[T]_\alpha^\beta
}
$$

가 성립한다.

**Proof**

모든 $u\in U$에 대해 fundamental coordinate equation을 두 번 적용하면

$$
\begin{aligned}
[(S\circ T)(u)]_\gamma
&=
[S]_\beta^\gamma[T(u)]_\beta\\
&=
[S]_\beta^\gamma[T]_\alpha^\beta[u]_\alpha.
\end{aligned}
$$

한편 definition에 의해

$$
[(S\circ T)(u)]_\gamma
=
[S\circ T]_\alpha^\gamma[u]_\alpha.
$$

두 matrix가 모든 coordinate column $[u]_\alpha\in\F^{\dim U}$에 같은 작용을 하므로 서로 같다. $\qed$

Identity map에 대해서는

$$
[id_V]_\beta^\beta=I_n
$$

이다. Isomorphism $T:V\rightarrow W$에 대해서는 $[T]_\beta^\gamma$가 invertible하고

$$
[T^{-1}]_\gamma^\beta
=
\left([T]_\beta^\gamma\right)^{-1}
$$

이다.

## Invariant Direct Sums and Block Matrices

$$
V=U\oplus W
$$

이고 두 subspace가 $T\in\operatorname{End}(V)$에 대해 invariant라고 하자. $U,W$의 ordered bases를 각각 $\beta,\gamma$라고 하고 이를 이어 붙인 $V$의 basis를

$$
\alpha=(\beta,\gamma)
$$

라고 하자.

$T(U)\subseteq U$이므로 $\beta$ vector의 image에는 $\gamma$ direction coordinate가 없고, $T(W)\subseteq W$이므로 $\gamma$ vector의 image에는 $\beta$ direction coordinate가 없다. 따라서

$$
[T]_\alpha^\alpha
=
\begin{bmatrix}
[T|_U]_\beta^\beta&0\\
0&[T|_W]_\gamma^\gamma
\end{bmatrix}.
$$

즉 invariant direct-sum decomposition은 linear map을 서로 독립적으로 작용하는 block들로 나눈다.

## 관련 문서

- [Coordinate](<06 Coordinate.md>)
- [Linear Map](<10 Linear Map.md>)
- [Vector Space Isomorphism](<13 Vector Space Isomorphism.md>)
- [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)
