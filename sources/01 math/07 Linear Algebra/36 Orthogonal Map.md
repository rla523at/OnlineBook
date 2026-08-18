# Orthogonal Map

## 한 줄 요약

Orthogonal map은 real inner product를 보존하는 linear map이며, 따라서 vector의 norm, distance, angle과 orthogonality를 모두 보존한다.

## Motivation

Inner product는 vector space에 length와 angle을 정한다. 그렇다면 coordinate를 바꾸거나 vector를 변환한 뒤에도 같은 geometry를 유지하려면 어떤 조건이 필요할까?

Real inner product space $V/\R$의 linear map $T:V\rightarrow V$가 모든 pair의 inner product를 보존하면

$$
B(Tv,Tw)=B(v,w)
$$

이고, norm과 angle은 inner product로부터 정의되므로 함께 보존된다. 이 조건은 $T$가 space를 늘이거나 찌그러뜨리지 않는다는 뜻이다. 이러한 linear map을 orthogonal map이라고 한다.

## Definition

Real inner product space $V/\R$와 $T\in\operatorname{End}(V)$가 있다고 하자. 모든 $v,w\in V$에 대해

$$
B(Tv,Tw)=B(v,w)
$$

가 성립하면 $T$를 `orthogonal map`이라고 한다.

Complex inner product space에서 같은 조건을 만족하는 linear map은 `unitary map`이라고 한다. Real case의 matrix condition에는 transpose가, complex case에는 conjugate transpose가 나타난다.

## Geometric Properties

### 정리1

Finite-dimensional real inner product space $V/\R$의 orthogonal map $T$는 다음을 만족한다.

1. Norm preservation:
   $$
   \lVert Tv\rVert=\lVert v\rVert.
   $$
2. Distance preservation:
   $$
   \lVert Tv-Tw\rVert=\lVert v-w\rVert.
   $$
3. Orthogonality preservation:
   $$
   v\perp w\iff Tv\perp Tw.
   $$
4. Angle preservation: nonzero $v,w$ 사이의 angle과 $Tv,Tw$ 사이의 angle이 같다.
5. $T$는 bijective다.

**Proof**

Inner product preservation에 의해

$$
\lVert Tv\rVert^2
=
B(Tv,Tv)
=
B(v,v)
=
\lVert v\rVert^2
$$

이므로 norm이 보존된다. $T$의 linearity를 함께 사용하면

$$
\lVert Tv-Tw\rVert
=
\lVert T(v-w)\rVert
=
\lVert v-w\rVert
$$

이므로 distance도 보존된다.

또한

$$
B(Tv,Tw)=B(v,w)
$$

이므로 한쪽 inner product가 $0_\R$인 것과 다른 쪽이 $0_\R$인 것은 동치다. 따라서 orthogonality가 보존된다. Nonzero vector의 angle 공식에 inner product와 norm의 보존을 대입하면

$$
\frac{B(Tv,Tw)}
{\lVert Tv\rVert\lVert Tw\rVert}
=
\frac{B(v,w)}
{\lVert v\rVert\lVert w\rVert}
$$

이므로 angle도 보존된다.

마지막으로 $Tv=0_V$이면

$$
\lVert v\rVert
=
\lVert Tv\rVert
=
0
$$

이므로 $v=0_V$다. 따라서 $\ker T=\{0_V\}$이고 $T$는 injective다. Domain과 codomain이 같은 finite dimension을 가지므로 $T$는 bijective다. $\qed$

## Matrix Representation

### Orthonormal basis

$n$-dimensional real inner product space $V/\R$의 orthonormal basis를

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

이라고 하고 $A=[T]_\beta^\beta$라고 하자. $A$의 $i$번째 column은 $T\beta_i$의 coordinate column이다. Basis가 orthonormal이므로

$$
B(T\beta_i,T\beta_j)
=
(A^{\mathsf T}A)_{ij}.
$$

따라서 $T$가 orthogonal map인 것과 다음 matrix equation은 동치다.

$$
A^{\mathsf T}A=I.
$$

$A$는 square matrix이고 $A^{\mathsf T}$가 left inverse이므로 invertible하다. 따라서

$$
A^{-1}=A^{\mathsf T},
\qquad
AA^{\mathsf T}=A^{\mathsf T}A=I.
$$

즉, orthonormal basis에서 orthogonal map의 matrix는 column과 row가 모두 orthonormal한 matrix다.

### Arbitrary basis

Basis $\beta$가 orthonormal이 아니면 [Gram Matrix](<31 Gram Matrix.md>) $G$가 basis vector 사이의 inner product를 기록한다. Coordinate column $a,b$에 대해

$$
B(v,w)=a^{\mathsf T}Gb
$$

이고, $T$를 적용한 coordinate는 각각 $Aa,Ab$다. 따라서 inner product preservation은 모든 $a,b$에 대해

$$
(Aa)^{\mathsf T}G(Ab)
=
a^{\mathsf T}Gb
$$

가 성립한다는 뜻이다. 이를 matrix equation으로 쓰면

$$
\boxed{A^{\mathsf T}GA=G}
$$

를 얻는다. Orthonormal basis에서는 $G=I$이므로 이 식이 $A^{\mathsf T}A=I$로 단순해진다.

Orthogonal map은 basis와 무관한 geometric property지만 $A^{\mathsf T}A=I$이라는 표현은 orthonormal basis를 선택했을 때만 그대로 성립한다.

## Orthogonal Matrix

Real square matrix $Q\in\R^{n\times n}$가

$$
Q^{\mathsf T}Q=I
$$

를 만족하면 $Q$를 `orthogonal matrix`라고 한다. Square matrix에서는 이 조건으로부터

$$
Q^{-1}=Q^{\mathsf T},
\qquad
QQ^{\mathsf T}=I
$$

도 따라온다.

Determinant를 취하면

$$
1
=
\det(Q^{\mathsf T}Q)
=
\det(Q)^2
$$

이므로

$$
\det Q\in\{-1,+1\}.
$$

Determinant가 $+1$인 orthogonal matrix는 orientation을 보존하고, determinant가 $-1$인 orthogonal matrix는 orientation을 뒤집는다.

## Orthogonal and Special Orthogonal Groups

$n\times n$ orthogonal matrix 전체의 집합을 `orthogonal group` $O(n)$이라고 한다.

$$
O(n)
:=
\{Q\in\R^{n\times n}\mid Q^{\mathsf T}Q=I\}.
$$

Determinant가 $+1$인 element만 모은 subgroup을 `special orthogonal group` $SO(n)$이라고 한다.

$$
SO(n)
:=
\{R\in O(n)\mid\det R=1\}.
$$

특히 $SO(3)$의 element는 3-dimensional orientation-preserving rotation을 나타낸다. 반면 $O(3)$에는 reflection과 rotation-reflection처럼 orientation을 뒤집는 transformation도 포함된다. 따라서 $Q^{\mathsf T}Q=I$만 확인하고 $Q$를 rotation matrix라고 부르면 determinant가 $-1$인 경우를 구분하지 못한다.

3-dimensional rotation의 geometric 의미는 [Rotation Matrix and SO(3)](<../08 Geometry/22 Rotation Matrix and SO(3).md>)에서 다룬다.

## Row Scaling Followed by an Orthogonal Map

### 정리2

$M\in GL(n,\R)$라고 하자. 다음 두 조건은 동치다.

1. $MM^{\mathsf T}$가 diagonal matrix다.
2. Positive diagonal matrix $S$와 orthogonal matrix $Q\in O(n)$가 존재해
   $$
   M=SQ
   $$
   로 표현된다.

**Proof**

$MM^{\mathsf T}$가 diagonal이라고 하자. 이 matrix의 diagonal entry는 $M$의 각 row의 squared norm이다. $M$의 $i$번째 row를 $r_i$라고 하자. $M$이 invertible이므로 모든 $r_i$가 nonzero이고, 다음 positive diagonal matrix를 정의할 수 있다.

$$
S
:=
\operatorname{diag}(s_1,\ldots,s_n),
\qquad
s_i:=\lVert r_i\rVert_2.
$$

서로 다른 row가 orthogonal하므로

$$
MM^{\mathsf T}=S^2.
$$

$Q:=S^{-1}M$이라고 두면

$$
QQ^{\mathsf T}
=
S^{-1}MM^{\mathsf T}S^{-1}
=
I.
$$

$Q$는 square matrix이므로 $QQ^{\mathsf T}=I$에서 $Q^{-1}=Q^{\mathsf T}$와 $Q^{\mathsf T}Q=I$가 따라온다. 따라서 $Q\in O(n)$이고 $M=SQ$다.

반대로 $M=SQ$, $S$가 positive diagonal, $Q\in O(n)$이면

$$
MM^{\mathsf T}
=
SQQ^{\mathsf T}S
=
S^2
$$

이므로 $MM^{\mathsf T}$는 diagonal이다. $\qed$

이 decomposition은 $M$의 서로 orthogonal한 row들을 먼저 unit length로 normalize하여 $Q$를 만들고, 각 row의 원래 length를 $S$에 기록한 것이다.

## 관련 문서

- [Gram Matrix](<31 Gram Matrix.md>)
- [Norm, Distance and Angle](<32 Norm Distance and Angle.md>)
- [Symmetric Matrix and Spectral Theorem](<39 Symmetric Matrix and Spectral Theorem.md>)
- [Rotation Matrix and SO(3)](<../08 Geometry/22 Rotation Matrix and SO(3).md>)
