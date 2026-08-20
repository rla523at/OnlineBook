# Eigenvector, Eigenvalue and Eigenspace

## 한 줄 요약

Eigenvector는 linear map이 direction을 바꾸지 않고 scalar배만 하는 nonzero vector이며, eigenspace는 같은 eigenvalue에 대응하는 eigenvector들과 zero vector를 모은 subspace다.

## Motivation

$n$-dimensional vector space $V/\F$와 $T\in\operatorname{End}(V)$가 있다고 하자. Basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

에서 $T$의 matrix가 diagonal이 되려면 각 column이 대응하는 coordinate axis에만 nonzero entry를 가져야 한다. Matrix representation의 definition에 의해 이는

$$
T(\beta_i)=\lambda_i\beta_i
$$

인 것과 같다. 즉 각 basis vector의 direction은 유지되고 scalar $\lambda_i$만큼 scale되어야 한다.

이러한 vector들로 basis를 만들 수 있으면

$$
[T]_\beta^\beta
=
\begin{bmatrix}
\lambda_1&&0\\
&\ddots&\\
0&&\lambda_n
\end{bmatrix}
$$

가 되어 $T$의 작용을 direction별로 독립적으로 읽을 수 있다. 따라서 먼저 $T(v)$가 $v$와 parallel한 특별한 vector를 정의한다.

## Eigenvector and Eigenvalue

### Definition

Vector $v\in V$가

$$
v\ne0_V,
\qquad
T(v)=\lambda v
$$

를 만족하면 $v$를 $T$의 `eigenvector`, scalar $\lambda\in\F$를 그 eigenvector에 대응하는 `eigenvalue`라고 한다. Zero vector는 모든 scalar $\lambda$에 대해 $T(0_V)=\lambda0_V$를 만족하므로 direction을 구분하지 못한다. 그래서 eigenvector의 definition에서 반드시 제외한다. Scalar $\lambda$가 $T$의 eigenvalue라는 말은 이에 대응하는 nonzero eigenvector가 하나 이상 존재한다는 뜻이다.

## Eigenspace

Equation $T(v)=\lambda v$를 한쪽으로 모으면

$$
(T-\lambda id_V)(v)=0_V
$$

이다. 따라서 같은 $\lambda$에 대응하는 solution 전체는 kernel로 표현할 수 있다.

### Definition

$$
E_\lambda(T)
:=
\ker(T-\lambda id_V)
$$

를 $\lambda$에 대한 `eigenspace`라고 한다. Kernel은 subspace이므로 $E_\lambda(T)\le V$다. 이 subspace는 zero vector를 포함하며, $\lambda$가 eigenvalue일 때 그 nonzero vector들이 정확히 $\lambda$에 대응하는 eigenvector다.

$$
\lambda\text{ is an eigenvalue of }T
\iff
E_\lambda(T)\ne\{0_V\}.
$$

Eigenvector $v$에 대해 모든 nonzero scalar multiple $av$, $a\ne0_\F$도 같은 eigenvalue의 eigenvector다. 하지만 $a=0_\F$인 경우에는 zero vector이므로 eigenvector가 아니라 eigenspace의 element일 뿐이다.

## Finding Eigenvalues with a Matrix

$V$가 finite-dimensional이고 basis $\beta$에서

$$
A:=[T]_\beta^\beta
$$

라고 하자. Vector $v\ne0_V$에 대해

$$
T(v)=\lambda v
$$

인 것과

$$
(A-\lambda I)[v]_\beta=0
$$

이 nonzero solution을 갖는 것은 동치다.

### 정리1

$$
\lambda\text{ is an eigenvalue of }T
\iff
\det(A-\lambda I)=0
\iff
\chi_T(\lambda)=0.
$$

**Proof**

$\lambda$가 eigenvalue이면 어떤 $v\ne0_V$에 대해

$$
(A-\lambda I)[v]_\beta=0
$$

이고 $[v]_\beta\ne0$다. 따라서 $A-\lambda I$는 injective하지 않고 square matrix이므로 invertible하지 않다. 그러므로 determinant가 zero다.

반대로 $\det(A-\lambda I)=0$이면 $A-\lambda I$는 invertible하지 않으므로 homogeneous system에 nonzero solution $x$가 존재한다. Coordinate isomorphism에 의해 $x=[v]_\beta$인 nonzero vector $v$가 존재하고

$$
(A-\lambda I)[v]_\beta=0
$$

이므로 $T(v)=\lambda v$다.

Characteristic polynomial은 $\chi_T(t)=\det(tI-A)$이고

$$
\det(\lambda I-A)
=
(-1)^n\det(A-\lambda I)
$$

이므로 두 determinant가 zero인 조건은 같다. $\qed$

Characteristic polynomial은 basis가 바뀌어도 같지만, field $\F$ 안에 root가 없을 수 있다. 예를 들어 real plane의 nontrivial rotation은 real eigenvalue가 없을 수 있다.

## Eigenvectors for Distinct Eigenvalues

### 정리2

Pairwise distinct eigenvalues $\lambda_1,\ldots,\lambda_k$에 대응하는 eigenvectors $v_1,\ldots,v_k$는 linearly independent하다.

**Proof**

$k$에 대해 induction한다. $k=1$이면 $v_1\ne0_V$이므로 $\{v_1\}$은 linearly independent다.

이제 정리가 $k-1$개까지 성립한다고 가정하고

$$
\sum_{i=1}^k a_iv_i=0_V
$$

라고 하자. 양변에 $T-\lambda_k id_V$를 적용하면

$$
\sum_{i=1}^{k-1}
a_i(\lambda_i-\lambda_k)v_i
=
0_V.
$$

Induction hypothesis에 의해 $v_1,\ldots,v_{k-1}$은 linearly independent이므로

$$
a_i(\lambda_i-\lambda_k)=0_\F
\qquad
(i=1,\ldots,k-1).
$$

Eigenvalues가 서로 다르므로 $\lambda_i-\lambda_k\ne0_\F$이고 $a_1=\cdots=a_{k-1}=0_\F$다. 원래 relation에는 $a_kv_k=0_V$만 남고 $v_k\ne0_V$이므로 $a_k=0_\F$다. 따라서 모든 coefficient가 zero다. $\qed$

같은 eigenvalue에 대응하는 모든 eigenvector를 한꺼번에 모은 set은 일반적으로 linearly independent하지 않다. 예를 들어 $v$와 $2v$는 둘 다 같은 eigenspace의 eigenvector지만 서로 dependent하다. 대신 각 eigenspace에서 basis를 선택하고, 서로 다른 eigenspace의 basis들을 합치면 정리2에 의해 linearly independent한 set을 얻는다.

## Diagonalization

### 정리3

Finite-dimensional vector space $V$의 endomorphism $T$에 대해 다음 조건은 동치다.

1. 어떤 basis $\beta$에서 $[T]_\beta^\beta$가 diagonal이다.
2. $V$에는 $T$의 eigenvector들로 이루어진 basis가 존재한다.

**Proof**

$[T]_\beta^\beta$가 diagonal이고 diagonal entries가 $\lambda_i$이면 matrix의 $i$번째 column에 의해

$$
T(\beta_i)=\lambda_i\beta_i.
$$

Basis vector는 nonzero이므로 모두 eigenvector다.

반대로 eigenvector basis $\beta=(\beta_1,\ldots,\beta_n)$가 있고 $T(\beta_i)=\lambda_i\beta_i$이면 $i$번째 matrix column은 $\lambda_i e_i$다. 따라서 $[T]_\beta^\beta$는 diagonal이다. $\qed$

$T$가 가진 distinct eigenvalues를 $\lambda_1,\ldots,\lambda_r$라고 하면 diagonalizable condition을 다음처럼 쓸 수도 있다.

$$
V
=
E_{\lambda_1}(T)
\oplus\cdots\oplus
E_{\lambda_r}(T).
$$

즉 모든 eigenspace의 dimension 합이 $\dim V$에 도달해야 한다. Characteristic polynomial이 split되더라도 eigenvector가 충분하지 않으면 diagonalizable하지 않을 수 있다.

## Example: Reflection

$m\in\R$에 대해

$$
A
=
\frac{1}{1+m^2}
\begin{bmatrix}
1-m^2&2m\\
2m&m^2-1
\end{bmatrix}
$$

라고 하자. Direct calculation으로

$$
A
\begin{bmatrix}
1\\
m
\end{bmatrix}
=
\begin{bmatrix}
1\\
m
\end{bmatrix},
\qquad
A
\begin{bmatrix}
m\\
-1
\end{bmatrix}
=
-
\begin{bmatrix}
m\\
-1
\end{bmatrix}
$$

를 얻는다. 두 eigenvector는 linearly independent이므로

$$
\beta
=
\left(
\begin{bmatrix}1\\m\end{bmatrix},
\begin{bmatrix}m\\-1\end{bmatrix}
\right)
$$

는 $\R^2$의 basis이고

$$
[L_A]_\beta^\beta
=
\begin{bmatrix}
1&0\\
0&-1
\end{bmatrix}.
$$

이 map은 첫 번째 eigenvector direction은 유지하고 두 번째 direction의 sign을 뒤집는 reflection이다.

## 관련 문서

- [Kernel](<../02 Linear Maps and Isomorphisms/11 Kernel.md>)
- [Matrix Representation](<20 Matrix Representation.md>)
- [Linear Systems and Row Reduction](<../04 Linear Systems and Rank/21 Linear Systems and Row Reduction.md>)
- [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)
- [Symmetric Matrix and Spectral Theorem](<../08 Matrix Decompositions/40 Symmetric Matrix and Spectral Theorem.md>)
