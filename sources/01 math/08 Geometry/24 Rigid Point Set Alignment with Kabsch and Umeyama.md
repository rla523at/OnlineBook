# Rigid Point Set Alignment with Kabsch and Umeyama

## 한 줄 요약

대응하는 두 point set의 rigid alignment는 centroid로 translation을 분리하고,
cross-covariance matrix의 SVD로 하나의 optimal rotation을 구한 뒤 모든 point에 같은
$R,t$를 적용하는 constrained least-squares 문제다.

## 문제 설정

Source point $x_i\in\mathbb R^3$와 대응하는 target point
$y_i\in\mathbb R^3$가 $N$쌍 있다고 하자.

$$
\{(x_i,y_i)\}_{i=1}^{N}
$$

Rigid alignment는 다음 objective를 최소화하는 $R,t$를 찾는다.

$$
(R^\star,t^\star)
=
\underset{R\in SO(3),\ t\in\mathbb R^3}
{\operatorname{argmin}}
\sum_{i=1}^{N}
\lVert y_i-(Rx_i+t)\rVert_2^2
$$

이 문제는 다음을 가정한다.

- $x_i$와 $y_i$의 correspondence가 알려져 있다.
- 두 point set의 length unit과 scale이 같다.
- 모든 source point에 같은 rigid transformation이 작용한다.
- Residual은 squared Euclidean distance로 평가한다.

Correspondence가 잘못됐거나 timestamp가 맞지 않으면 closed-form solution이
존재하더라도 원하는 physical relation을 나타내지 않는다.

## 1. Centroid를 계산한다

두 point set의 centroid를 계산한다.

$$
\mu_x
:=
\frac{1}{N}\sum_{i=1}^{N}x_i,
\qquad
\mu_y
:=
\frac{1}{N}\sum_{i=1}^{N}y_i
$$

Centered point를 다음처럼 정의한다.

$$
\tilde x_i:=x_i-\mu_x,
\qquad
\tilde y_i:=y_i-\mu_y
$$

Centered point의 합은 0이다.

$$
\sum_i\tilde x_i=0,
\qquad
\sum_i\tilde y_i=0
$$

## 2. Translation을 rotation의 함수로 제거한다

Rotation $R$을 고정했을 때 objective를 최소로 만드는 translation은 다음과 같다.

$$
t^\star(R)=\mu_y-R\mu_x
$$

이 값을 적용하면 source centroid가 target centroid에 정확히 놓인다.

$$
R\mu_x+t^\star=\mu_y
$$

원래 objective는 centered point 사이의 rotation fitting 문제로 줄어든다.

$$
R^\star
=
\underset{R\in SO(3)}
{\operatorname{argmin}}
\sum_{i=1}^{N}
\lVert\tilde y_i-R\tilde x_i\rVert_2^2
$$

Centering이 translation을 제거한다는 말은 모든 translation error가 실제로 coordinate
frame 차이라는 뜻이 아니다. 단지 optimization variable $t$를 $R$과 분리해 계산할
수 있다는 뜻이다.

## 3. Cross-covariance matrix를 만든다

이 문서에서는 source를 먼저 쓰는 다음 convention을 사용한다.

$$
H
:=
\frac{1}{N}
\sum_{i=1}^{N}
\tilde x_i\tilde y_i^{\mathsf T}
$$

Centered objective를 전개하면 $R$에 의존하지 않는 term을 제외하고 다음 문제가
남는다.

$$
R^\star
=
\underset{R\in SO(3)}
{\operatorname{argmax}}
\operatorname{tr}(RH)
$$

문헌이나 library가 $H$를
$\sum\tilde y_i\tilde x_i^{\mathsf T}$ 순서로 정의하면 최종 $U,V$ formula도
transpose된다. SVD formula를 외우기 전에 cross-covariance의 source·target 순서를
확인해야 한다.

## 4. SVD로 rotation을 구한다

$H$의 SVD를 계산한다.

$$
H=U\Sigma V^{\mathsf T}
$$

Orthogonal matrix 전체 $O(3)$에서 optimum을 찾으면 $VU^{\mathsf T}$가 후보가 된다.
그러나 determinant가 $-1$이면 reflection이므로 $SO(3)$ constraint를 만족하지 않는다.

다음 correction matrix를 정의한다.

$$
D
:=
\operatorname{diag}
\left(
1,\,
1,\,
\det(VU^{\mathsf T})
\right)
$$

Optimal proper rotation은 다음과 같다.

$$
R^\star=VDU^{\mathsf T}
$$

$$
\det R^\star=1
$$

$\det(VU^{\mathsf T})=-1$이면 $D$의 마지막 element가 $-1$이 된다. Singular value를
큰 순서로 배치했을 때 가장 작은 singular direction을 뒤집어 reflection을 제거하면서
objective 증가를 최소화한다.

## 5. Translation을 복원한다

구한 rotation을 centroid 식에 대입한다.

$$
t^\star=\mu_y-R^\star\mu_x
$$

최종 $SE(3)$ transformation은 다음과 같다.

$$
T_{Y\_X}
=
\begin{bmatrix}
R^\star&t^\star\\
0&1
\end{bmatrix}
$$

모든 source point에 같은 transformation을 적용한다.

$$
x_i^{\mathrm{aligned}}
=
R^\star x_i+t^\star
$$

Point마다 별도의 $R_i,t_i$를 선택하지 않는다.

## Kabsch와 Umeyama의 관계

| 방법 | Rotation | Translation | Scale |
|---|---|---|---|
| Kabsch rigid alignment | SVD와 determinant correction | Centering 후 복원 | $1$로 고정 |
| Umeyama similarity alignment | SVD와 determinant correction | Centering 후 복원 | 선택적으로 추정 |

Umeyama method에서 uniform scale $c$까지 허용하면 objective는 다음과 같다.

$$
\underset{c,R,t}{\operatorname{minimize}}
\sum_i
\lVert y_i-(cRx_i+t)\rVert_2^2
$$

Source variance를 다음처럼 정의하자.

$$
\sigma_x^2
:=
\frac{1}{N}
\sum_i
\lVert\tilde x_i\rVert_2^2
$$

앞의 normalized cross-covariance convention에서는 optimal scale이 다음과 같다.

$$
c^\star
=
\frac{\operatorname{tr}(\Sigma D)}
{\sigma_x^2}
$$

Translation은 scale을 포함해 다시 계산한다.

$$
t^\star
=
\mu_y-c^\star R^\star\mu_x
$$

$c=1$로 고정하면 metric scale을 보존하는 rigid alignment가 된다. Scale을 추정하는
$Sim(3)$ alignment와 scale을 고정한 $SE(3)$ alignment를 결과에서 구분해야 한다.

## 2차원 직관 예시

계산을 눈으로 확인하기 위해 2차원 point를 사용하자. 실제 $SE(3)$ alignment는 같은
절차를 3차원에서 수행한다.

Target point는 다음과 같다.

$$
(0,0),\ (1,0),\ (2,0)
$$

Source point는 같은 직선 motion을 다른 origin과 방향에서 표현한다.

$$
(10,5),\ (10,6),\ (10,7)
$$

Centroid는 다음과 같다.

$$
\mu_x=(10,6),
\qquad
\mu_y=(1,0)
$$

Centered point는 각각 다음과 같다.

$$
(0,-1),\ (0,0),\ (0,1)
$$

$$
(-1,0),\ (0,0),\ (1,0)
$$

SVD solution은 clockwise $90^\circ$ rotation을 준다.

$$
R=
\begin{bmatrix}
0&1\\
-1&0
\end{bmatrix}
$$

Translation은 centroid 식으로 계산한다.

$$
t
=
\mu_y-R\mu_x
=
(-5,10)
$$

이를 모든 source point에 적용하면 target point와 정확히 일치한다.

$$
R(10,5)^{\mathsf T}+t=(0,0)^{\mathsf T}
$$

$$
R(10,6)^{\mathsf T}+t=(1,0)^{\mathsf T}
$$

$$
R(10,7)^{\mathsf T}+t=(2,0)^{\mathsf T}
$$

이 예시에서는 두 point set이 하나의 rigid transformation으로 정확히 연결되므로
alignment 후 residual이 0이다.

## Alignment 후 residual

각 correspondence의 residual은 다음과 같다.

$$
e_i
:=
y_i-(R^\star x_i+t^\star)
$$

Position RMSE는 다음과 같다.

$$
\operatorname{RMSE}
=
\sqrt{
\frac{1}{N}
\sum_i\lVert e_i\rVert_2^2
}
$$

실제 trajectory에 drift, scale error 또는 non-rigid distortion이 있으면 하나의
$R,t$로 모든 point를 동시에 겹칠 수 없으므로 residual이 남는다.

## Pose trajectory에 적용할 때

Trajectory alignment에서는 timestamp association으로 만든 position pair를 point
correspondence로 사용한다. Position으로 $R,t$를 fit한 뒤 estimated pose 전체에
왼쪽에서 같은 transformation을 곱한다.

$$
T_{\mathrm{target}\_\mathrm{body},i}^{\mathrm{aligned}}
=
T_{\mathrm{target}\_\mathrm{source}}
T_{\mathrm{source}\_\mathrm{body},i}
$$

이때 orientation도 같은 global rotation만큼 함께 바뀐다. Position으로 fit한
objective와 orientation error metric은 별개의 선택이므로 evaluator 설정에 명시해야
한다.

## Degeneracy와 failure mode

Closed-form formula가 있다고 항상 reliable한 alignment가 되는 것은 아니다.

- 모든 point가 같으면 rotation을 결정할 수 없다.
- 3차원 point가 한 직선에만 놓이면 그 직선 주위 rotation이 모호하다.
- Motion excitation이 작거나 singular value가 매우 작으면 noise에 민감하다.
- 잘못된 correspondence와 timestamp association은 잘못된 transform을 만든다.
- Squared residual은 outlier에 민감하다.
- Symmetric point configuration에서는 여러 rotation이 비슷한 objective를 가질 수 있다.

Singular value와 rank를 검사하고, pair 수·coverage·residual distribution을 함께
보고해야 한다. Outlier가 있는 registration에는 robust loss나 RANSAC 같은 별도
방법이 필요하지만, 그것들은 기본 Kabsch·Umeyama solution의 일부가 아니다.

## Alignment가 판별하지 못하는 것

Optimization은 가장 잘 맞는 $R,t$를 찾지만 그 차이의 원인을 판별하지 않는다.
동일한 fitted transform은 다음 두 상황을 모두 설명할 수 있다.

- Estimator가 arbitrary local frame을 사용한 경우
- Estimator가 common global frame에서 constant pose error를 낸 경우

어느 자유도를 제거할지는 data를 본 뒤 정하는 것이 아니라 estimator의 observability와
평가 protocol에서 먼저 정해야 한다.

## 관련 문서

- [Least Squares Problem](<../07 Linear Algebra/37 Least Squares Problem.md>)
- [Singular Value Decomposition](<../07 Linear Algebra/40 Singular Value Decomposition.md>)
- [Rotation Matrix and SO(3)](<./22 Rotation Matrix and SO(3).md>)
- [Rigid Transformation and SE(3)](<./23 Rigid Transformation and SE(3).md>)
- [Gauge Freedom and Trajectory Alignment Policy](<../../05 Robotics/03 Evaluation/02 Gauge Freedom and Trajectory Alignment Policy.md>)

## References

- Wolfgang Kabsch, “A Solution for the Best Rotation to Relate Two Sets of Vectors,” 1976
- Shinji Umeyama, “Least-Squares Estimation of Transformation Parameters Between Two Point Patterns,” 1991
