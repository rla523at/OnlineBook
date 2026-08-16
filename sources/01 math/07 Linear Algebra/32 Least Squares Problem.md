# Least Squares Problem

## 한 줄 요약

최소제곱 문제(least squares problem)는 모든 식을 정확히 만족하는 해가 없거나
측정값에 오차가 있을 때, residual의 제곱합이 가장 작은 해를 선택하는 문제다.

## 정확한 해 대신 가장 가까운 해가 필요한 이유

Linear system

$$
Ax=b,
\qquad
A\in\mathbb R^{m\times n},
\quad
x\in\mathbb R^n,
\quad
b\in\mathbb R^m
$$

를 생각하자. $b$가 $A$의 column space에 속하면 $Ax=b$를 정확히 만족하는 $x$가
존재한다. 그러나 측정값으로 만든 $b$에는 noise가 있고, equation 수가 unknown 수보다
많은 overdetermined system에서는 모든 식을 동시에 만족하는 해가 없을 수 있다.

예를 들어 같은 rigid body의 point를 두 coordinate system에서 측정하면 대응하는
모든 point가 하나의 rigid transformation으로 정확히 겹쳐야 한다. 실제 data에서는
sensor noise, timestamp 차이와 estimator error 때문에 하나의 transformation이 모든
point pair를 정확히 일치시키지 못한다. 이때 어느 transformation이 전체 data를 가장
잘 설명하는지 정하는 기준이 필요하다.

## Residual과 objective

Candidate $x$가 equation을 만족하지 못하고 남긴 차이를 residual이라고 한다.

$$
r(x):=Ax-b
$$

Least-squares solution $x^\star$는 residual의 Euclidean norm 제곱을 최소화한다.

$$
x^\star
:=
\underset{x\in\mathbb R^n}{\operatorname{argmin}}
\lVert Ax-b\rVert_2^2
$$

$$
\lVert Ax-b\rVert_2^2
=
\sum_{i=1}^{m}(a_i^{\mathsf T}x-b_i)^2
$$

여기서 $\operatorname{argmin}$은 objective value 자체가 아니라 objective를 가장 작게
만드는 argument를 뜻한다. min과 구분해야 한다.

Residual을 제곱하면 양수와 음수 residual이 서로 상쇄되지 않고, 큰 residual에 더 큰
penalty가 주어진다. 다만 제곱합을 사용한다는 사실만으로 measurement noise가 반드시
Gaussian이라고 단정할 수는 없다. Gaussian noise model은 least squares에 별도의
통계적 해석을 부여하는 추가 가정이다.

## Column space로의 orthogonal projection

$Ax$가 만들 수 있는 모든 vector의 집합은 $A$의 column space다.

$$
\mathcal C(A)
:=
\{Ax\mid x\in\mathbb R^n\}
$$

따라서 least squares는 $b$와 가장 가까운 $\mathcal C(A)$의 point $Ax^\star$를 찾는
문제다. 가장 가까운 point에서는 residual이 column space에 orthogonal하다.

$$
A^{\mathsf T}(Ax^\star-b)=0
$$

이를 정리하면 normal equation을 얻는다.

$$
A^{\mathsf T}A x^\star=A^{\mathsf T}b
$$

$A$의 column이 linearly independent이면 $A^{\mathsf T}A$가 invertible이므로 해가
유일하다.

$$
x^\star
=
(A^{\mathsf T}A)^{-1}A^{\mathsf T}b
$$

이 식은 해의 구조를 설명하지만, numerical implementation에서 inverse matrix를
직접 계산하라는 뜻은 아니다. $A^{\mathsf T}A$를 만들면 condition number가
악화될 수 있으므로 실제 계산에서는 QR decomposition이나
[Singular Value Decomposition](<./34 Singular Value Decomposition.md>)을 사용한다.

## 해가 유일하지 않은 경우

$A$의 column이 linearly dependent이면 서로 다른 $x$가 같은 $Ax$를 만들 수 있다.
이 경우 closest point $Ax^\star$는 유일해도 이를 만드는 parameter $x^\star$는
여러 개일 수 있다.

Singular value decomposition으로 정의하는 Moore-Penrose pseudoinverse
$A^+$를 사용하면 그중 norm이 가장 작은 solution을 선택할 수 있다.

$$
x^\star=A^+b
$$

Pseudoinverse는 없는 정보를 복원하지 않는다. Singular value가 0인 방향은 data가
parameter를 결정하지 못하는 방향이며, 최소 norm 조건은 여러 가능한 해 중 하나를
선택하는 규칙이다.

## Matrix residual과 Frobenius norm

여러 vector residual을 matrix column으로 모으면 Frobenius norm으로 같은 objective를
표현할 수 있다. $M=[m_1,\ldots,m_N]$에 대해 Frobenius norm을 다음처럼 정의한다.

$$
\lVert M\rVert_F^2
:=
\sum_{i,j}M_{ij}^2
=
\sum_{k=1}^{N}\lVert m_k\rVert_2^2
$$

대응하는 point set

$$
X=[x_1,\ldots,x_N],
\qquad
Y=[y_1,\ldots,y_N]
$$

을 rotation $R$과 translation $t$로 맞추는 objective는 다음 두 식으로 똑같이 쓸
수 있다.

$$
\sum_{i=1}^{N}
\lVert y_i-(Rx_i+t)\rVert_2^2
$$

$$
\left\lVert
Y-\left(RX+t\mathbf 1^{\mathsf T}\right)
\right\rVert_F^2
$$

## Rigid alignment는 constrained least squares다

일반 least squares에서는 parameter가 Euclidean space 전체를 움직일 수 있다.
Rigid alignment의 rotation은 다음 constraint를 만족해야 한다.

$$
R^{\mathsf T}R=I,
\qquad
\det R=1
$$

따라서 $R$의 각 element를 독립적인 unknown으로 놓고 normal equation만 풀면
rotation이 아닌 scaling, shear 또는 reflection이 나올 수 있다. Rigid point-set
alignment는 centering으로 translation을 분리하고, SVD로 rotation constraint를
만족하는 optimum을 구한다.

## Best fit과 원인 판별은 다르다

Least squares는 주어진 model 안에서 residual이 가장 작은 parameter를 찾는다.
Residual이 생긴 원인이 coordinate frame 차이인지, sensor noise인지, estimator
error인지 판별하지는 않는다.

두 trajectory 사이의 fitted rigid transformation도 마찬가지다. SE(3) alignment가
작은 ATE를 만들었다는 사실만으로 원래 차이가 coordinate frame 때문이었다고 결론
내릴 수 없다. 제거해도 되는 자유도는 estimator의 입력·출력 계약과 평가 목적에서
먼저 정해야 한다.

## 관련 문서

- [Inner Product Space](<./30 Inner Product Space.md>)
- [Symmetric Matrix and Spectral Theorem](<./33 Symmetric Matrix and Spectral Theorem.md>)
- [Singular Value Decomposition](<./34 Singular Value Decomposition.md>)
- [Rigid Point Set Alignment with Kabsch and Umeyama](<../08 Geometry/24 Rigid Point Set Alignment with Kabsch and Umeyama.md>)

## References

- Gene H. Golub and Charles F. Van Loan, *Matrix Computations*
- Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*
