# Orthogonal Map

## 한 줄 요약

Orthogonal map은 real inner product를 보존하는 linear map이며, 따라서 vector의 norm, distance, angle과 orthogonality를 모두 보존한다.

## Motivation

Inner product는 vector space에 length와 angle을 정한다. 그렇다면 linear map을 적용한 뒤에도 이러한 geometric quantities가 변하지 않으려면 어떤 조건이 필요할까?

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

가 성립하면 $T$를 `orthogonal map`이라고 한다. Complex inner product space에서 같은 조건을 만족하는 linear map은 `unitary map`이라고 한다. Real case의 matrix condition에는 transpose가, complex case에는 conjugate transpose가 나타난다.

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

이라고 하고 $A=[T]_\beta^\beta$라고 하자. $A$의 $i$번째 column을 $a_i$라고 쓰면 matrix representation의 정의에 의해

$$
a_i=[T\beta_i]_\beta
$$

이다. 즉, $a_i$는 $T\beta_i$의 coordinate column이다.

이제 $v,w\in V$의 coordinate column을 각각 $[v]_\beta=a$, $[w]_\beta=b$라고 하자. 그러면

$$
v=\sum_{k=1}^n a_k\beta_k,
\qquad
w=\sum_{\ell=1}^n b_\ell\beta_\ell.
$$

Basis $\beta$가 $B$에 대해 orthonormal이므로 $B(\beta_k,\beta_\ell)=\delta_{k\ell}$이다. 따라서

$$
\begin{aligned}
B(v,w)
&=\sum_{k=1}^n\sum_{\ell=1}^n
a_kb_\ell B(\beta_k,\beta_\ell)\\
&=\sum_{k=1}^n a_kb_k\\
&=a^{\mathsf T}b.
\end{aligned}
$$

즉, $B$를 dot product로 가정한 것이 아니라 $B$-orthonormal basis에서 $B$의 coordinate representation이 dot product와 같아진다. 특히 $v=T\beta_i$, $w=T\beta_j$로 두면

$$
B(T\beta_i,T\beta_j)=a_i^{\mathsf T}a_j.
$$

한편 $A=(a_1,\ldots,a_n)$이므로 $A^{\mathsf T}A$의 $(i,j)$ entry 역시 $a_i^{\mathsf T}a_j$다. 따라서

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

## Orthogonal Rows and Columns: Separating Length and Direction

### Motivation

Orthogonal matrix $Q$는

$$
Q^{\mathsf T}Q=I,
\qquad
QQ^{\mathsf T}=I
$$

를 모두 만족한다. 첫 번째 식은 $Q$의 column들이 orthonormal하다는 뜻이고, 두 번째 식은 row들이 orthonormal하다는 뜻이다. 따라서 orthogonal matrix 자체에서는 row와 column 중 어느 쪽을 기준으로 보아도 같은 결론을 얻는다.

하지만 서로 orthogonal하다는 조건만 남기고 각 vector의 norm이 $1$이라는 조건을 제거하면 row와 column은 서로 다른 조건이 된다. $M\in\R^{n\times n}$의 $i$번째 row를 $r_i$, $j$번째 column을 $c_j$라고 하자. Matrix multiplication의 entry를 계산하면

$$
(MM^{\mathsf T})_{ij}
=
r_i r_j^{\mathsf T}
=
\langle r_i,r_j\rangle
$$

이고

$$
(M^{\mathsf T}M)_{ij}
=
c_i^{\mathsf T}c_j
=
\langle c_i,c_j\rangle
$$

이다. 따라서 두 matrix는 서로 다른 Gram matrix다. 아래에서는 각 row와 column을 norm으로 나눌 수 있도록 $M$이 invertible한 square matrix인 경우를 다룬다. 이 조건에서는 모든 row와 column이 nonzero다.

| 기준 | 확인할 matrix | Diagonal일 때의 의미 | Length와 direction의 분리 |
| --- | --- | --- | --- |
| Row | $MM^{\mathsf T}$ | 서로 다른 row들이 orthogonal하다. | $M=SQ$ |
| Column | $M^{\mathsf T}M$ | 서로 다른 column들이 orthogonal하다. | $M=QD$ |

두 관점은 linear map을 해석할 때에도 역할이 다르다. Standard basis의 $j$번째 vector를 $e_j$라고 하면

$$
Me_j=c_j
$$

이므로 $j$번째 column은 $j$번째 input coordinate 방향이 어디로 이동하는지를 나타낸다. 반면 $x\in\R^n$에 대해

$$
(Mx)_i=r_i x
$$

이므로 $i$번째 row는 input $x$로부터 $i$번째 output coordinate를 계산하는 방향을 나타낸다. 따라서 column orthogonality는 input coordinate 방향들의 image가 서로 orthogonal하다는 뜻이고, row orthogonality는 서로 다른 output coordinate를 계산하는 방향들이 orthogonal하다는 뜻이다.

예를 들어

$$
M=
\begin{pmatrix}
2 & 2\\
-1 & 1
\end{pmatrix}
$$

에 대해 직접 계산하면

$$
MM^{\mathsf T}
=
\begin{pmatrix}
8 & 0\\
0 & 2
\end{pmatrix},
\qquad
M^{\mathsf T}M
=
\begin{pmatrix}
5 & 3\\
3 & 5
\end{pmatrix}.
$$

따라서 이 $M$의 row들은 서로 orthogonal하지만 column들은 서로 orthogonal하지 않다. 서로 다른 row norm을 허용하면 row orthogonality만으로는 column orthogonality를 보장할 수 없음을 보여준다.

Row들이 nonzero이고 서로 orthogonal하면 각 row의 norm을

$$
s_i:=\lVert r_i\rVert_2
$$

라고 두고 $i$번째 row를 $s_i$로 나눌 수 있다. 이 length들을

$$
S:=\operatorname{diag}(s_1,\ldots,s_n)
$$

에 기록하면

$$
Q_r:=S^{-1}M,
\qquad
M=SQ_r
$$

이고 $Q_r$의 row들은 orthonormal하다. $Q_r$는 square matrix이므로 $Q_r\in O(n)$이다. 반대로 column들이 nonzero이고 서로 orthogonal하면

$$
d_j:=\lVert c_j\rVert_2,
\qquad
D:=\operatorname{diag}(d_1,\ldots,d_n)
$$

으로 두고

$$
Q_c:=MD^{-1},
\qquad
M=Q_cD
$$

로 쓸 수 있다. 이때 $Q_c$의 column들이 orthonormal하므로 $Q_c\in O(n)$이다. $Q_r$과 $Q_c$는 서로 다른 조건에서 얻는 matrix이므로 일반적으로 같은 matrix가 아니다.

두 식에서 diagonal matrix가 곱해지는 위치는 transformation의 적용 순서도 결정한다. Row 기준 분해에서는

$$
Mx=S(Q_rx)
$$

이므로 $Q_r$가 먼저 length와 angle을 보존하는 transformation을 하고, $S$가 각 output coordinate를 scaling한다. Matrix를 만드는 관점에서는 $Q_r$의 왼쪽에 $S$를 곱해 row들을 scaling한 것이다. Column 기준 분해에서는

$$
Mx=Q_c(Dx)
$$

이므로 $D$가 각 input coordinate를 먼저 scaling하고, 그다음 $Q_c$가 length와 angle을 보존하는 transformation을 한다. Matrix를 만드는 관점에서는 $Q_c$의 오른쪽에 $D$를 곱해 column들을 scaling한 것이다.

따라서 row 기준이 더 근본적인 것은 아니다. 문제에 $MM^{\mathsf T}$가 등장하는지 $M^{\mathsf T}M$이 등장하는지, 또는 output scaling과 input scaling 중 어느 해석이 필요한지에 따라 기준이 달라진다.

### $GL(n,\R)$

$GL(n,\R)$은 `general linear group`의 표기로, invertible한 $n\times n$ real matrix 전체의 집합이다.

$$
GL(n,\R)
:=
\{M\in\R^{n\times n}\mid M\text{ is invertible}\}
=
\{M\in\R^{n\times n}\mid\det M\ne 0\}.
$$

이 matrix들이 matrix multiplication에 대해 group을 이루기 때문에 이 이름을 쓴다. 아래 두 정리에서 중요한 점은 group 자체가 아니라 $M$이 invertible하다는 조건이다. 이 조건 덕분에 $M$의 모든 row와 column이 nonzero라서 각각의 norm으로 나눌 수 있다.

모든 diagonal entry가 positive인 diagonal matrix를 positive diagonal matrix라고 한다. 아래에서 $S$와 $D$의 diagonal entry는 nonzero row와 column의 norm이므로 모두 positive이고, 따라서 $S^{-1}$과 $D^{-1}$이 존재한다.

### 정리2: Row 기준

$M\in GL(n,\R)$라고 하자. 다음 두 조건은 동치다.

1. $MM^{\mathsf T}$가 diagonal matrix다.
2. Positive diagonal matrix $S$와 orthogonal matrix $Q\in O(n)$가 존재해
   $$
   M=SQ
   $$
   로 표현된다.

**Proof**

$MM^{\mathsf T}$가 diagonal이라고 하자. $(MM^{\mathsf T})_{ij}=\langle r_i,r_j\rangle$이므로 서로 다른 row는 orthogonal하다. 다음 positive diagonal matrix를 정의하자.

$$
S
:=
\operatorname{diag}(s_1,\ldots,s_n),
\qquad
s_i:=\lVert r_i\rVert_2.
$$

$MM^{\mathsf T}$의 $i$번째 diagonal entry는 $\lVert r_i\rVert_2^2=s_i^2$이고 off-diagonal entry는 모두 $0$이므로

$$
MM^{\mathsf T}=S^2.
$$

$Q:=S^{-1}M$이라고 두면

$$
QQ^{\mathsf T}
=
S^{-1}MM^{\mathsf T}S^{-1}
=
S^{-1}S^2S^{-1}
=
I.
$$

$Q$는 square matrix이므로 $QQ^{\mathsf T}=I$에서 $Q^{-1}=Q^{\mathsf T}$이고, 이에 따라 $Q^{\mathsf T}Q=I$도 성립한다. 따라서 $Q\in O(n)$이고 $M=SQ$다.

반대로 $M=SQ$, $S$가 positive diagonal, $Q\in O(n)$이면

$$
MM^{\mathsf T}
=
SQQ^{\mathsf T}S
=
S^2
$$

이므로 $MM^{\mathsf T}$는 diagonal이다. $\qed$

### 정리3: Column 기준

$M\in GL(n,\R)$라고 하자. 다음 두 조건은 동치다.

1. $M^{\mathsf T}M$이 diagonal matrix다.
2. Orthogonal matrix $Q\in O(n)$와 positive diagonal matrix $D$가 존재해
   $$
   M=QD
   $$
   로 표현된다.

**Proof**

$M^{\mathsf T}M$이 diagonal이라고 하자. $(M^{\mathsf T}M)_{ij}=\langle c_i,c_j\rangle$이므로 서로 다른 column은 orthogonal하다. 다음 positive diagonal matrix를 정의하자.

$$
D
:=
\operatorname{diag}(d_1,\ldots,d_n),
\qquad
d_j:=\lVert c_j\rVert_2.
$$

$M^{\mathsf T}M$의 $j$번째 diagonal entry는 $\lVert c_j\rVert_2^2=d_j^2$이고 off-diagonal entry는 모두 $0$이므로

$$
M^{\mathsf T}M=D^2.
$$

$Q:=MD^{-1}$이라고 두면

$$
\begin{aligned}
Q^{\mathsf T}Q
&=
(MD^{-1})^{\mathsf T}(MD^{-1})\\
&=
D^{-1}M^{\mathsf T}MD^{-1}\\
&=
D^{-1}D^2D^{-1}\\
&=
I.
\end{aligned}
$$

따라서 $Q\in O(n)$이고 $M=QD$다.

반대로 $M=QD$, $Q\in O(n)$, $D$가 positive diagonal이면

$$
M^{\mathsf T}M
=
DQ^{\mathsf T}QD
=
D^2
$$

이므로 $M^{\mathsf T}M$은 diagonal이다. $\qed$

정리2와 정리3은 각각 row와 column을 normalize하는 대칭적인 결과다. 어느 한쪽이 다른 쪽을 일반적으로 함의하지 않는다. 특히 $M$ 자체가 orthogonal matrix이면 row와 column이 모두 orthonormal하므로 두 조건이 다시 일치한다.

## 관련 문서

- [Gram Matrix](<31 Gram Matrix.md>)
- [Norm, Distance and Angle](<32 Norm Distance and Angle.md>)
- [Four Fundamental Subspaces](<../06 Matrix Subspaces and Approximation/36 Four Fundamental Subspaces.md>)
- [Symmetric Matrix and Spectral Theorem](<../08 Matrix Decompositions/40 Symmetric Matrix and Spectral Theorem.md>)
- [Rotation Matrix and SO(3)](<../08 Geometry/22 Rotation Matrix and SO(3).md>)
