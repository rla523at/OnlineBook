# Singular Value Decomposition

## 한 줄 요약

Singular value decomposition(SVD)은 rectangular matrix를 두 orthogonal
coordinate change와 축별 nonnegative scaling으로 분해하며, least squares와 rigid
point-set alignment의 핵심 계산 도구다.

## Eigenvalue decomposition만으로 부족한 이유

Eigenvalue decomposition은 기본적으로 square matrix가 자기 space에 작용할 때
사용한다. 그러나 measurement matrix와 cross-covariance matrix는 rectangular일 수
있고, square matrix라도 orthonormal eigenvector basis를 갖지 않을 수 있다.

SVD는 모든 real $m\times n$ matrix에 존재하며, input space와 output space에서 서로
다른 orthonormal basis를 사용한다.

## Full SVD

### 정리1 (Existence of SVD)

모든 real matrix $A\in\R^{m\times n}$에 대해 orthogonal matrix $U,V$와 rectangular diagonal matrix $\Sigma$가 존재하여 다음이 성립한다.

$$
A=U\Sigma V^{\mathsf T}
$$

$$
U\in\mathbb R^{m\times m},
\qquad
V\in\mathbb R^{n\times n},
\qquad
\Sigma\in\mathbb R^{m\times n}
$$

$$
U^{\mathsf T}U=I,
\qquad
V^{\mathsf T}V=I
$$

$\Sigma$의 diagonal entry는 nonnegative이고 decreasing order로 놓을 수 있다.

$$
\sigma_1\ge\sigma_2\ge\cdots\ge 0
$$

**Proof**

[Symmetric Matrix and Spectral Theorem](<./40 Symmetric Matrix and Spectral Theorem.md>)에 의해 positive semidefinite matrix $A^{\mathsf T}A$에는 orthonormal eigenvector basis

$$
(v_1,\ldots,v_n)
$$

가 존재한다. Eigenvalue를 decreasing order로 놓고 positive eigenvalue의 개수를 $r$이라고 하자.

$$
A^{\mathsf T}Av_i
=
\lambda_i v_i,
\qquad
\lambda_1\ge\cdots\ge\lambda_r>0,
\qquad
\lambda_{r+1}=\cdots=\lambda_n=0.
$$

$1\le i\le r$에 대해

$$
\sigma_i:=\sqrt{\lambda_i},
\qquad
u_i:=\frac{Av_i}{\sigma_i}
$$

라고 정의한다. 39번 문서에서 보인 계산에 의해 $u_1,\ldots,u_r$는 $\R^m$의 orthonormal subset이다. 이를 $\R^m$의 orthonormal basis

$$
(u_1,\ldots,u_m)
$$

로 확장한다.

$V$와 $U$를 각각 $v_i$와 $u_i$를 column으로 갖는 matrix로 두고, $\Sigma\in\R^{m\times n}$의 첫 $r$개 diagonal entry를 $\sigma_1,\ldots,\sigma_r$, 나머지 entry를 $0$으로 둔다. $i\le r$이면 정의에 의해

$$
Av_i=\sigma_i u_i.
$$

$i>r$이면

$$
\lVert Av_i\rVert_2^2
=
v_i^{\mathsf T}A^{\mathsf T}Av_i
=
0
$$

이므로 $Av_i=0$다. 따라서 모든 basis vector에 대한 작용을 column으로 모으면

$$
AV=U\Sigma.
$$

$V$가 orthogonal이므로 오른쪽에 $V^{\mathsf T}$를 곱해

$$
A=U\Sigma V^{\mathsf T}
$$

를 얻는다. $\qed$

$\Sigma$의 diagonal entry $\sigma_i$를 `singular value`, $V$의 column $v_i$를 `right singular vector`, $U$의 column $u_i$를 `left singular vector`라고 한다.

## Geometric meaning

Vector $x$에 $A$를 적용하는 과정을 다음 세 단계로 읽을 수 있다.

$$
Ax=U\Sigma V^{\mathsf T}x
$$

1. $V^{\mathsf T}$가 input을 right singular vector basis의 coordinate로 바꾼다.
2. $\Sigma$가 각 orthogonal direction을 $\sigma_i$만큼 scale한다.
3. $U$가 결과를 left singular vector basis 방향으로 옮긴다.

따라서 unit sphere는 $A$를 거치며 singular vector 방향을 principal axis로 갖는
ellipsoid가 된다. Singular value 0인 direction은 한 점이나 더 낮은 차원으로
collapse된다.

## $A^{\mathsf T}A$와의 관계

SVD 식에서 다음을 계산할 수 있다.

$$
A^{\mathsf T}A
=
V\Sigma^{\mathsf T}\Sigma V^{\mathsf T}
$$

따라서:

- $v_i$는 $A^{\mathsf T}A$의 eigenvector다.
- $\sigma_i^2$는 $A^{\mathsf T}A$의 eigenvalue다.
- $\sigma_i>0$이면 $u_i=Av_i/\sigma_i$다.

[Symmetric Matrix and Spectral Theorem](<./40 Symmetric Matrix and Spectral Theorem.md>)이
SVD를 구성할 수 있는 이유를 제공한다.

## Thin SVD

$A$의 rank가 $r$이면 0이 아닌 singular value에 해당하는 vector만 모아 다음처럼
쓸 수 있다.

$$
A=U_r\Sigma_rV_r^{\mathsf T}
$$

$$
U_r\in\mathbb R^{m\times r},
\qquad
\Sigma_r\in\mathbb R^{r\times r},
\qquad
V_r\in\mathbb R^{n\times r}
$$

이를 thin SVD 또는 compact SVD라고 한다. Full SVD와 같은 linear map을 표현하지만
null-space completion에 필요한 column을 생략한다.

## Rank, norm과 conditioning

0보다 큰 singular value의 개수는 matrix rank다.

$$
\operatorname{rank}(A)
=
\#\{i\mid \sigma_i>0\}
$$

Spectral norm과 Frobenius norm도 singular value로 표현할 수 있다.

$$
\lVert A\rVert_2=\sigma_1
$$

$$
\lVert A\rVert_F^2=\sum_i\sigma_i^2
$$

실제로 $x=Vy$라고 두면 orthogonal matrix가 norm을 보존하므로

$$
\lVert Ax\rVert_2^2
=
\lVert\Sigma y\rVert_2^2
=
\sum_i\sigma_i^2y_i^2.
$$

$\lVert x\rVert_2=\lVert y\rVert_2=1$인 vector 중 이 값의 maximum은 $y$가 첫 번째 right singular direction일 때의 $\sigma_1^2$이므로 $\lVert A\rVert_2=\sigma_1$이다. Frobenius norm 식은

$$
\lVert A\rVert_F^2
=
\operatorname{tr}(A^{\mathsf T}A)
=
\operatorname{tr}(V\Sigma^{\mathsf T}\Sigma V^{\mathsf T})
=
\sum_i\sigma_i^2
$$

에서 따라온다.

Full column-rank matrix에서 가장 큰 singular value와 가장 작은 singular value의
비는 2-norm condition number다.

$$
\kappa_2(A)=\frac{\sigma_{\max}}{\sigma_{\min}}
$$

$\sigma_{\min}$이 0에 가까우면 작은 input perturbation이나 measurement noise가
solution을 크게 바꿀 수 있다.

## Pseudoinverse와 least squares

0이 아닌 singular value의 reciprocal을 취해 다음 matrix를 정의한다.

$$
\Sigma^+_{ii}
=
\begin{cases}
1/\sigma_i,&\sigma_i>0,\\
0,&\sigma_i=0.
\end{cases}
$$

Moore-Penrose pseudoinverse는 다음과 같다.

$$
A^+=V\Sigma^+U^{\mathsf T}
$$

### 정리2 (Minimum-norm least-squares solution)

모든 $b\in\R^m$에 대해

$$
x^\star:=A^+b
$$

는 $\lVert Ax-b\rVert_2$를 minimize하는 solution 중 Euclidean norm이 가장 작은 unique solution이다.

**Proof**

$$
c:=U^{\mathsf T}b,
\qquad
y:=V^{\mathsf T}x
$$

라고 두자. Orthogonal matrix는 norm을 보존하므로

$$
\lVert Ax-b\rVert_2
=
\lVert\Sigma y-c\rVert_2.
$$

$r=\operatorname{rank}(A)$라고 하면 objective의 square는

$$
\sum_{i=1}^r(\sigma_i y_i-c_i)^2
+
\sum_{i=r+1}^m c_i^2
$$

이다. 따라서 residual을 minimize하려면

$$
y_i=\frac{c_i}{\sigma_i}
\qquad
(1\le i\le r)
$$

여야 한다. $i>r$인 coordinate $y_i$는 residual에 영향을 주지 않으므로 least-squares solution 사이에서 자유롭게 선택할 수 있다. 그중 $\lVert x\rVert_2=\lVert y\rVert_2$가 가장 작으려면 모든 free coordinate를 $0$으로 두어야 한다. 이는

$$
y=\Sigma^+c
$$

와 같으므로

$$
x
=
Vy
=
V\Sigma^+U^{\mathsf T}b
=
A^+b.
$$

Free coordinate를 모두 $0$으로 두는 선택은 유일하므로 minimum-norm solution도 unique하다. $\qed$

매우 작은 singular value를 어디까지 $0$으로 취급할지는 numerical tolerance와 data scale에 의존한다.

## SVD 결과가 곧 rotation은 아니다

$U$와 $V$가 orthogonal matrix라고 해서 각각 반드시 rotation인 것은 아니다.

$$
\det U,\det V\in\{-1,+1\}
$$

따라서 point-set alignment에서 단순히 $VU^{\mathsf T}$를 사용하면 determinant가
$-1$인 reflection이 나올 수 있다. Kabsch algorithm은 diagonal correction matrix를
삽입해 결과가 $SO(3)$에 속하도록 강제한다.

$$
D
=
\operatorname{diag}
\left(1,1,\det(VU^{\mathsf T})\right)
$$

$$
R=VDU^{\mathsf T}
$$

이 correction은 SVD 자체의 일부가 아니라, alignment solution을 orientation-preserving
rotation으로 제한하기 위한 단계다.

## Non-uniqueness

Singular value가 반복되면 해당 singular subspace 안의 basis는 하나로 정해지지
않는다. Singular value가 0인 null-space basis도 여러 방식으로 선택할 수 있다.
따라서 서로 다른 SVD implementation이 다른 $U,V$를 반환해도 product
$U\Sigma V^{\mathsf T}$와 의미 있는 subspace는 같을 수 있다.

Rigid alignment에서 cross-covariance가 rank deficient하면 이 non-uniqueness가
rotation ambiguity나 numerical sensitivity로 나타날 수 있다.

## 관련 문서

- [Four Fundamental Subspaces](<./36 Four Fundamental Subspaces.md>)
- [Least Squares Problem](<./38 Least Squares Problem.md>)
- [Symmetric Matrix and Spectral Theorem](<./40 Symmetric Matrix and Spectral Theorem.md>)
- [Orthogonal Map](<./37 Orthogonal Map.md>)
- [Rigid Point Set Alignment with Kabsch and Umeyama](<../08 Geometry/24 Rigid Point Set Alignment with Kabsch and Umeyama.md>)

## References

- Gene H. Golub and Charles F. Van Loan, *Matrix Computations*
- Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*
