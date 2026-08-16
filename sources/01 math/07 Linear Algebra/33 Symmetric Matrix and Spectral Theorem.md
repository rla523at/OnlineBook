# Symmetric Matrix and Spectral Theorem

## 한 줄 요약

Real symmetric matrix는 orthonormal eigenvector basis로 diagonalize할 수 있으며,
이 성질이 $A^{\mathsf T}A$에서 singular value와 singular vector를 구성할 수 있게
한다.

## SVD 앞에서 symmetric matrix를 보는 이유

일반 square matrix는 real eigenvalue를 갖지 않을 수 있고, eigenvector가 space의
basis를 이루지 못할 수도 있다. 반면 arbitrary real matrix $A$로 만든
$A^{\mathsf T}A$는 항상 symmetric이고 positive semidefinite다.

$$
(A^{\mathsf T}A)^{\mathsf T}
=
A^{\mathsf T}A
$$

이 특별한 구조 덕분에 $A^{\mathsf T}A$는 orthonormal eigenvector basis를 가지며,
그 eigenvalue의 square root가 $A$의 singular value가 된다.

## Symmetric matrix

$S\in\mathbb R^{n\times n}$가 다음을 만족하면 symmetric matrix라고 한다.

$$
S^{\mathsf T}=S
$$

Real symmetric matrix의 eigenvalue는 real number다. 또한 서로 다른 eigenvalue에
속하는 eigenvector는 서로 orthogonal하다.

반복 eigenvalue가 있는 경우 그 eigenspace 안의 eigenvector가 자동으로 하나로
정해지는 것은 아니다. 그러나 Gram-Schmidt process를 이용해 각 eigenspace에서
orthonormal basis를 선택할 수 있다.

## Spectral theorem

Real symmetric matrix $S$에 대해 orthogonal matrix $Q$와 real diagonal matrix
$\Lambda$가 존재하여 다음이 성립한다.

$$
S=Q\Lambda Q^{\mathsf T}
$$

$$
Q^{\mathsf T}Q=QQ^{\mathsf T}=I
$$

$Q$의 column은 $S$의 orthonormal eigenvector이고, $\Lambda$의 diagonal element는
대응하는 eigenvalue다.

$$
Q=
\begin{bmatrix}
q_1&\cdots&q_n
\end{bmatrix},
\qquad
\Lambda=
\operatorname{diag}(\lambda_1,\ldots,\lambda_n)
$$

$$
Sq_i=\lambda_iq_i
$$

Spectral theorem은 symmetric matrix가 적절한 orthonormal coordinate system에서
각 coordinate를 eigenvalue만큼 독립적으로 scale하는 map이라는 뜻이다.

## Positive semidefinite matrix

Symmetric matrix $S$가 모든 $x\in\mathbb R^n$에 대해 다음을 만족하면
positive semidefinite(PSD)라고 한다.

$$
x^{\mathsf T}Sx\ge 0
$$

모든 eigenvalue가 strictly positive일 필요는 없으므로 positive definite와
구분해야 한다. PSD matrix의 eigenvalue는 모두 0 이상이다.

$A\in\mathbb R^{m\times n}$에 대해 $A^{\mathsf T}A$는 PSD다.

$$
x^{\mathsf T}A^{\mathsf T}Ax
=
(Ax)^{\mathsf T}(Ax)
=
\lVert Ax\rVert_2^2
\ge 0
$$

따라서 spectral theorem을 적용하면 다음과 같이 쓸 수 있다.

$$
A^{\mathsf T}A
=
V\Lambda V^{\mathsf T},
\qquad
\lambda_i\ge 0
$$

## Eigenvalue에서 singular value로

$A^{\mathsf T}A$의 eigenpair를 다음처럼 두자.

$$
A^{\mathsf T}A v_i=\lambda_i v_i
$$

$\lambda_i>0$일 때 다음 값을 정의한다.

$$
\sigma_i:=\sqrt{\lambda_i},
\qquad
u_i:=\frac{Av_i}{\sigma_i}
$$

그러면 $u_i$의 norm은 1이다.

$$
\lVert u_i\rVert_2^2
=
\frac{v_i^{\mathsf T}A^{\mathsf T}Av_i}{\sigma_i^2}
=
\frac{\lambda_i}{\lambda_i}
=1
$$

서로 다른 $v_i$에서 만든 $u_i$들도 orthogonal하게 선택할 수 있다. 이렇게 얻은
$v_i$, $\sigma_i$, $u_i$가 SVD의 right singular vector, singular value와 left
singular vector다.

## Zero eigenvalue가 뜻하는 것

$$
A^{\mathsf T}A v=0
$$

이면 다음이 성립한다.

$$
\lVert Av\rVert_2^2
=
v^{\mathsf T}A^{\mathsf T}Av
=0
$$

따라서 $Av=0$이고 $v$는 $A$의 null space 방향이다. 이 방향의 singular value는
0이며, input을 그 방향으로 바꿔도 output에서 구분할 수 없다는 뜻이다.

Point-set alignment에서도 centered point가 충분한 방향으로 퍼져 있지 않으면
cross-covariance matrix의 rank가 낮아지고 일부 rotation을 안정적으로 결정하지
못할 수 있다.

## 관련 문서

- [Eigenvector & Eigenvalue](<./22 Eigenvector & Eigenvalue & EigenSpace.md>)
- [Inner Product Space](<./30 Inner Product Space.md>)
- [Orthogonal Map](<./31 Orthogonal Map.md>)
- [Singular Value Decomposition](<./34 Singular Value Decomposition.md>)

## References

- Sheldon Axler, *Linear Algebra Done Right*
- Gene H. Golub and Charles F. Van Loan, *Matrix Computations*
