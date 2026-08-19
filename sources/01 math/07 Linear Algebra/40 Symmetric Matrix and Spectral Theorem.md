# Symmetric Matrix and Spectral Theorem

## 한 줄 요약

Real symmetric matrix는 real eigenvalue와 orthonormal eigenvector basis를 가지며, 따라서 orthogonal coordinate change로 diagonalize할 수 있다.

## Motivation

일반 square matrix는 real eigenvalue를 갖지 않을 수 있고, eigenvector들이 space의 basis를 이루지 못할 수도 있다. 따라서 arbitrary linear map을 eigenvector basis에서 diagonal matrix로 표현할 수 있다고 기대할 수는 없다.

반면 real symmetric matrix는 transpose를 취해도 변하지 않는다.

$$
S^{\mathsf T}=S.
$$

이 식은 standard inner product에 대해 $S$가 자신의 adjoint와 같다는 뜻이다. Adjoint와의 compatibility는 서로 다른 eigenspace를 orthogonal하게 만들고, [Schur's Theorem](<39 Schur's Theorem.md>)의 upper triangular form에서 off-diagonal entry를 모두 제거한다. 그 결과 symmetric matrix는 orthonormal eigenvector basis를 갖는다.

이 성질은 arbitrary real matrix $A$로 만든 $A^{\mathsf T}A$에 적용할 수 있다. $A^{\mathsf T}A$는 항상 symmetric이고 positive semidefinite이므로, 그 eigenvalue와 eigenvector에서 singular value decomposition을 구성할 수 있다.

## Symmetric Matrix

### Definition

Real square matrix $S\in\R^{n\times n}$가

$$
S^{\mathsf T}=S
$$

를 만족하면 $S$를 `symmetric matrix`라고 한다.

Standard inner product를 사용하는 $\R^n$에서 이 조건은 모든 $x,y\in\R^n$에 대해

$$
B(Sx,y)=B(x,Sy)
$$

가 성립한다는 뜻이다. 즉 matrix $S$가 나타내는 linear map의 adjoint도 $S$다.

### 정리1 (Eigenvalue properties)

Real symmetric matrix의 eigenvalue는 모두 real number다. 또한 서로 다른 eigenvalue에 속하는 eigenvector들은 orthogonal하다.

**Proof**

Eigenvalue가 real임을 보이기 위해 $S$를 $\C^n$에 작용하는 matrix로 생각하자. $Sv=\lambda v$, $v\ne0$라고 하면 $S^{\mathsf *}=S$이므로

$$
B(Sv,v)=B(v,Sv).
$$

첫 번째 argument는 linear이고 두 번째 argument는 conjugate linear이므로

$$
\lambda B(v,v)
=
\overline\lambda B(v,v).
$$

$B(v,v)>0$이므로 $\lambda=\overline\lambda$, 즉 $\lambda\in\R$다.

이제

$$
Sv=\lambda v,
\qquad
Sw=\mu w,
\qquad
\lambda\ne\mu
$$

라고 하자. Symmetry에 의해

$$
\lambda B(v,w)
=
B(Sv,w)
=
B(v,Sw)
=
\mu B(v,w).
$$

따라서 $(\lambda-\mu)B(v,w)=0$이고 $\lambda\ne\mu$이므로 $B(v,w)=0$이다. 즉 $v\perp w$다. $\qed$

Repeated eigenvalue가 있으면 해당 eigenspace 안의 basis는 하나로 정해지지 않는다. 그러나 [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)를 적용하면 각 eigenspace에서 orthonormal basis를 선택할 수 있다.

## Spectral Theorem

### 정리2 (Real spectral theorem)

모든 real symmetric matrix $S\in\R^{n\times n}$에 대해 orthogonal matrix $Q\in O(n)$와 real diagonal matrix $\Lambda$가 존재하여

$$
S
=
Q\Lambda Q^{\mathsf T}
$$

가 성립한다.

$Q$의 column은 $S$의 orthonormal eigenvector이고, $\Lambda$의 diagonal entry는 대응하는 eigenvalue다.

**Proof**

먼저 $S$를 complex matrix로 생각하자. Complex Schur decomposition에 의해 unitary matrix $U$와 upper triangular matrix $R$이 존재하여

$$
S
=
URU^{\mathsf *}
$$

로 쓸 수 있다. 따라서

$$
R
=
U^{\mathsf *}SU.
$$

$S^{\mathsf *}=S$이므로

$$
R^{\mathsf *}
=
U^{\mathsf *}S^{\mathsf *}U
=
R.
$$

즉 $R$은 upper triangular이면서 Hermitian이다. Upper triangular matrix의 diagonal 아래 entry는 모두 $0$이고, Hermitian condition은 diagonal 위 entry가 대응하는 아래 entry의 complex conjugate임을 요구한다. 따라서 diagonal 위 entry도 모두 $0$이고 $R$은 real diagonal matrix다.

이로써 $\C^n$에는 $S$의 orthonormal eigenvector basis가 존재하고 eigenvalue는 모두 real임을 알 수 있다. $\lambda\in\R$에 대해 $S-\lambda I$는 real matrix이므로 complex solution $z=x+iy$가

$$
(S-\lambda I)z=0
$$

을 만족하면 real vector $x,y$도 각각 같은 equation을 만족한다. 따라서 각 complex eigenspace는 해당 real eigenspace의 complexification이며 real eigenvector들로 span된다.

서로 다른 eigenspace는 정리1에 의해 orthogonal하다. 각 real eigenspace에서 orthonormal basis를 선택해 합치면 $\R^n$의 orthonormal eigenvector basis

$$
(q_1,\ldots,q_n)
$$

를 얻는다. 이 vector들을 column으로 갖는 matrix를 $Q$, 대응하는 eigenvalue를 diagonal에 갖는 matrix를 $\Lambda$라고 하면

$$
SQ=Q\Lambda.
$$

$Q$가 orthogonal이므로 $Q^{-1}=Q^{\mathsf T}$이고

$$
S=Q\Lambda Q^{\mathsf T}
$$

를 얻는다. $\qed$

Spectral theorem은 symmetric matrix가 적절한 orthonormal coordinate system에서 각 coordinate를 eigenvalue만큼 독립적으로 scale하는 map임을 뜻한다.

## Positive Semidefinite Matrix

### Definition

Real symmetric matrix $S$가 모든 $x\in\R^n$에 대해

$$
x^{\mathsf T}Sx\ge0
$$

을 만족하면 $S$를 `positive semidefinite matrix`라고 한다. $x\ne0$일 때 항상 strict inequality가 성립하면 `positive definite matrix`라고 한다.

### 정리3

Real symmetric matrix $S$에 대해 다음이 성립한다.

$$
S\text{ is positive semidefinite}
\iff
\text{every eigenvalue of }S\text{ is nonnegative}.
$$

또한

$$
S\text{ is positive definite}
\iff
\text{every eigenvalue of }S\text{ is positive}.
$$

**Proof**

Spectral theorem으로

$$
S=Q\Lambda Q^{\mathsf T}
$$

라고 쓰고 $y:=Q^{\mathsf T}x$라고 하자. 그러면

$$
x^{\mathsf T}Sx
=
y^{\mathsf T}\Lambda y
=
\sum_{i=1}^n\lambda_i y_i^2.
$$

모든 $y$에 대해 이 값이 nonnegative인 것과 모든 $\lambda_i\ge0$인 것은 동치다. 또한 $y\ne0$일 때 항상 positive인 것과 모든 $\lambda_i>0$인 것도 동치다. $\qed$

## Why $A^{\mathsf T}A$ Appears in SVD

Arbitrary real matrix $A\in\R^{m\times n}$에 대해

$$
(A^{\mathsf T}A)^{\mathsf T}
=
A^{\mathsf T}A
$$

이므로 $A^{\mathsf T}A$는 symmetric하다. 또한 모든 $x\in\R^n$에 대해

$$
x^{\mathsf T}A^{\mathsf T}Ax
=
\lVert Ax\rVert_2^2
\ge
0
$$

이므로 positive semidefinite다. 따라서 spectral theorem으로 orthonormal eigenvector basis

$$
(v_1,\ldots,v_n)
$$

와 nonnegative eigenvalue $\lambda_i$를 선택할 수 있다.

$$
A^{\mathsf T}Av_i
=
\lambda_i v_i.
$$

$\lambda_i>0$일 때

$$
\sigma_i:=\sqrt{\lambda_i},
\qquad
u_i:=\frac{Av_i}{\sigma_i}
$$

라고 정의하면

$$
\lVert u_i\rVert_2^2
=
\frac{v_i^{\mathsf T}A^{\mathsf T}Av_i}{\sigma_i^2}
=
1.
$$

또한 positive eigenvalue에 대응하는 orthonormal $v_i,v_j$에 대해

$$
u_i^{\mathsf T}u_j
=
\frac{v_i^{\mathsf T}A^{\mathsf T}Av_j}
{\sigma_i\sigma_j}
=
\frac{\lambda_j v_i^{\mathsf T}v_j}
{\sigma_i\sigma_j}
=
\delta_{ij}.
$$

따라서 $u_i$들도 orthonormal하다.

반면 $\lambda_i=0$이면

$$
\lVert Av_i\rVert_2^2
=
v_i^{\mathsf T}A^{\mathsf T}Av_i
=
0
$$

이므로 $Av_i=0$다. 즉 zero eigenvalue에 대응하는 $v_i$는 $A$의 null-space direction이다.

이 구성을 완성해 rectangular matrix를 분해하는 과정은 [Singular Value Decomposition](<41 Singular Value Decomposition.md>)에서 증명한다.

## 관련 문서

- [Eigenvector & Eigenvalue & EigenSpace](<24 Eigenvector & Eigenvalue & EigenSpace.md>)
- [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)
- [Orthogonal Map](<37 Orthogonal Map.md>)
- [Schur's Theorem](<39 Schur's Theorem.md>)
- [Singular Value Decomposition](<41 Singular Value Decomposition.md>)

## References

- Sheldon Axler, *Linear Algebra Done Right*
- Gene H. Golub and Charles F. Van Loan, *Matrix Computations*
