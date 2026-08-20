# Schur's Theorem

## 한 줄 요약

Schur's theorem은 characteristic polynomial이 split되는 finite-dimensional inner product space의 linear map이 어떤 orthonormal basis에서 upper triangular matrix로 표현됨을 보장한다.

## Motivation

Linear map의 matrix가 diagonal이면 각 basis direction이 서로 섞이지 않으므로 작용과 eigenvalue를 쉽게 읽을 수 있다. 하지만 모든 linear map이 diagonalizable한 것은 아니다. 예를 들어 eigenvector가 space 전체의 basis를 이루지 못하면 어떤 basis를 선택해도 diagonal matrix를 얻을 수 없다.

Upper triangular matrix는 diagonal matrix보다 약한 형태지만 다음 장점이 있다.

- Diagonal entry에서 eigenvalue를 읽을 수 있다.
- 앞쪽 basis vector들이 span하는 subspace가 map에 의해 보존된다.
- Determinant와 characteristic polynomial을 diagonal entry의 product로 계산할 수 있다.

따라서 diagonalization이 불가능하더라도 orthonormal basis를 유지하면서 upper triangular form까지 얻을 수 있는지 묻게 된다. Schur's theorem은 characteristic polynomial이 field $\F$에서 linear factor로 split되면 이것이 가능하다고 말한다.

Complex field에서는 fundamental theorem of algebra에 의해 모든 nonconstant polynomial이 split되므로 모든 complex square matrix에 Schur's theorem을 적용할 수 있다.

## Adjoint and an Invariant Orthogonal Complement

Schur's theorem의 induction에서는 dimension을 하나 줄이면서 restriction을 만들 수 있는 invariant subspace가 필요하다. Eigenvector $v$ of $T$를 선택하면 $\operatorname{span}\{v\}$는 $T$-invariant지만 그 orthogonal complement가 반드시 $T$-invariant인 것은 아니다.

이 문제는 $T$가 아니라 adjoint $T^*$의 eigenvector를 선택하면 해결된다. Finite-dimensional inner product space에서 adjoint는 [Riesz Representation Theorem](<43 Riesz Representation Theorem.md>)에 의해 존재하며 다음 관계를 만족한다.

$$
B(Tx,y)
=
B(x,T^*y).
$$

### 보조정리1

$T^*v=\overline\lambda v$인 nonzero vector $v$가 있다고 하자. 그러면

$$
\operatorname{span}\{v\}^\perp
$$

는 $T$-invariant subspace다.

**Proof**

$x\in\operatorname{span}\{v\}^\perp$이면 $B(x,v)=0_\F$다. 이 문서의 inner product는 두 번째 argument에 대해 conjugate linear하므로

$$
\begin{aligned}
B(Tx,v)
&=
B(x,T^*v)\\
&=
B(x,\overline\lambda v)\\
&=
\lambda B(x,v)\\
&=
0_\F.
\end{aligned}
$$

따라서 $Tx\in\operatorname{span}\{v\}^\perp$이고 이 subspace는 $T$-invariant다. $\qed$

Orthonormal basis에서 $T$의 matrix가 $A$이면 $T^*$의 matrix는 $A^{\mathsf *}$다. Characteristic polynomial을

$$
\chi_T(t):=\det(tI-A)
$$

로 정의하면

$$
\chi_{T^*}(t)
=
\overline{\chi_T(\overline t)}.
$$

따라서

$$
\chi_T(t)
=
\prod_{i=1}^n(t-\lambda_i)
$$

가 $\F$에서 split되면

$$
\chi_{T^*}(t)
=
\prod_{i=1}^n(t-\overline{\lambda_i})
$$

도 split된다. 즉 $T^*$에도 $\F$에 속하는 eigenvalue와 eigenvector가 존재한다.

## Schur's Theorem

### 정리1

$\F\in\{\R,\C\}$인 $n$-dimensional inner product space $V/\F$와 $T\in\operatorname{End}(V)$가 있다고 하자. Characteristic polynomial $\chi_T(t)$가 $\F$에서 split되면 $V$의 orthonormal basis $\beta$가 존재하여

$$
[T]_\beta^\beta
$$

가 upper triangular matrix가 된다.

**Proof**

Dimension $n$에 대한 induction을 사용한다. $n=1$이면 모든 $1\times1$ matrix가 upper triangular이므로 정리가 성립한다.

이제 dimension이 $n-1$ 이하인 경우 정리가 성립한다고 가정하고 $\dim V=n$이라고 하자. $\chi_T$가 split되므로 앞의 논의에 의해 $\chi_{T^*}$도 split된다. 따라서 $T^*$의 unit eigenvector $v$와 scalar $\overline\lambda\in\F$를 다음과 같이 선택할 수 있다.

$$
T^*v
=
\overline\lambda v,
\qquad
\lVert v\rVert=1.
$$

다음 subspace를 정의하자.

$$
W
:=
\operatorname{span}\{v\}^\perp.
$$

보조정리1에 의해 $W$는 $T$-invariant이고 $\dim W=n-1$이다. $W$의 basis를 먼저 놓고 마지막에 $v$를 놓으면 $T$의 matrix는 다음 block upper triangular form을 갖는다.

$$
[T]
=
\begin{bmatrix}
[T|_W] & b\\
0 & c
\end{bmatrix}.
$$

따라서 characteristic polynomial은

$$
\chi_T(t)
=
\chi_{T|_W}(t)(t-c)
$$

로 분해된다. $\chi_T$가 linear factor들로 split되므로 $\chi_{T|_W}$도 $\F$에서 split된다.

Induction hypothesis를 $T|_W$에 적용하면 $W$의 orthonormal basis

$$
\gamma=(\gamma_1,\ldots,\gamma_{n-1})
$$

가 존재하여

$$
[T|_W]_\gamma^\gamma
$$

가 upper triangular가 된다. $W\perp v$이고 $\lVert v\rVert=1$이므로

$$
\beta
:=
(\gamma_1,\ldots,\gamma_{n-1},v)
$$

는 $V$의 orthonormal basis다. 이 basis에서

$$
[T]_\beta^\beta
=
\begin{bmatrix}
[T|_W]_\gamma^\gamma & b\\
0 & c
\end{bmatrix}
$$

이고 오른쪽 matrix는 upper triangular다. 따라서 dimension $n$에서도 정리가 성립한다. $\qed$

## Complex Schur Decomposition

### 따름정리1

모든 complex square matrix $A\in\C^{n\times n}$에 대해 unitary matrix $U$와 upper triangular matrix $R$이 존재하여

$$
A
=
URU^{\mathsf *}
$$

로 쓸 수 있다.

**Proof**

Fundamental theorem of algebra에 의해 $\chi_A(t)$는 $\C$에서 split된다. Standard inner product가 주어진 $\C^n$에서 linear map $x\mapsto Ax$에 Schur's theorem을 적용하면 orthonormal basis를 column으로 갖는 unitary matrix $U$가 존재하여

$$
R
=
U^{\mathsf *}AU
$$

가 upper triangular가 된다. 양변을 정리하면 $A=URU^{\mathsf *}$다. $\qed$

Real matrix의 characteristic polynomial이 $\R$에서 split되지 않으면 real orthonormal basis만으로 triangular form을 만들 수 없을 수 있다. 이 경우 complex field로 확장하면 complex Schur decomposition을 적용할 수 있다. Real arithmetic만 유지하려면 $1\times1$과 $2\times2$ diagonal block을 갖는 real Schur form을 사용한다.

## Consequences

Upper triangular matrix의 characteristic polynomial은 diagonal entry로부터 바로 계산된다. $R=[T]_\beta^\beta$이고 diagonal entry가 $r_{11},\ldots,r_{nn}$이면

$$
\chi_T(t)
=
\det(tI-R)
=
\prod_{i=1}^n(t-r_{ii}).
$$

따라서 Schur form의 diagonal entry는 multiplicity를 포함한 $T$의 eigenvalue다.

Schur's theorem 자체는 upper triangular form만 보장한다. Off-diagonal entry가 모두 $0$이 되어 diagonal form을 얻으려면 normality나 symmetry 같은 추가 조건이 필요하다. Real symmetric matrix에 이 조건을 적용한 결과는 [Symmetric Matrix and Spectral Theorem](<40 Symmetric Matrix and Spectral Theorem.md>)에서 다룬다.

## 관련 문서

- [Orthogonal Complement and Orthogonal Projection](<35 Orthogonal Complement and Orthogonal Projection.md>)
- [Orthogonal Map](<37 Orthogonal Map.md>)
- [Symmetric Matrix and Spectral Theorem](<40 Symmetric Matrix and Spectral Theorem.md>)
- [Riesz Representation Theorem](<43 Riesz Representation Theorem.md>)
