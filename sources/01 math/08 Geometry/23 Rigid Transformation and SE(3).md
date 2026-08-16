# Rigid Transformation and SE(3)

## 한 줄 요약

$SE(3)$는 3차원 rotation과 translation을 하나의 orientation-preserving rigid
transformation으로 묶은 집합이며, composition과 inverse가 가능한 group이다.

## Rotation에 translation을 더해야 하는 이유

$R\in SO(3)$는 vector를 회전하지만 origin은 움직이지 않는다.

$$
R0=0
$$

서로 다른 위치와 방향을 가진 두 coordinate frame 사이에서 point coordinate를
변환하려면 rotation뿐 아니라 origin 사이의 translation도 필요하다.

$$
p_A=R_{A\_B}p_B+t_{A\_B}
$$

여기서:

- $R_{A\_B}$는 frame $B$의 vector coordinate를 frame $A$로 회전한다.
- $t_{A\_B}$는 frame $A$에서 표현한 frame $B$ origin의 위치다.

이 식은 affine transformation이고, linear part가 $SO(3)$에 속하므로 distance와
angle을 보존하는 orientation-preserving rigid transformation이다.

## Homogeneous transformation matrix

Point에 homogeneous coordinate를 사용하면 rotation과 translation을 하나의 matrix
multiplication으로 표현할 수 있다.

$$
\bar p
:=
\begin{bmatrix}
p\\1
\end{bmatrix}
$$

$$
T
:=
\begin{bmatrix}
R&t\\
0&1
\end{bmatrix}
$$

$$
\bar p_A
=
T_{A\_B}\bar p_B
$$

$4\times4$ matrix를 사용한다고 3차원 point가 실제로 4차원 공간으로 이동하는 것은
아니다. 마지막 coordinate는 translation을 matrix multiplication 안에 포함하기
위한 표현 장치다.

## $SE(3)$의 정의

Special Euclidean group $SE(3)$를 다음과 같이 정의한다.

$$
SE(3)
:=
\left\{
\begin{bmatrix}
R&t\\
0&1
\end{bmatrix}
\ \middle|\
R\in SO(3),\ t\in\mathbb R^3
\right\}
$$

Rotation의 3 degree of freedom과 translation의 3 degree of freedom을 합쳐
$SE(3)$ pose는 6 degree of freedom을 가진다. Matrix element 16개가 독립이라는
뜻은 아니다.

## Composition

두 rigid transformation을 순서대로 적용하면:

$$
T_1T_2
=
\begin{bmatrix}
R_1R_2&R_1t_2+t_1\\
0&1
\end{bmatrix}
$$

Translation이 단순히 $t_1+t_2$가 아니라 $R_1t_2+t_1$인 이유는 $t_2$를 먼저
$T_1$의 target coordinate 방향으로 회전해야 하기 때문이다.

Frame notation을 사용하면 중간 frame이 일치하는 순서로 곱한다.

$$
T_{A\_C}
=
T_{A\_B}T_{B\_C}
$$

$$
p_A
=
T_{A\_B}T_{B\_C}p_C
$$

## Inverse

$$
T
=
\begin{bmatrix}
R&t\\
0&1
\end{bmatrix}
$$

의 inverse는 다음과 같다.

$$
T^{-1}
=
\begin{bmatrix}
R^{\mathsf T}&-R^{\mathsf T}t\\
0&1
\end{bmatrix}
$$

단순히 translation의 sign만 바꾸지 않는 이유는 inverse translation을 반대
coordinate frame에서 표현해야 하기 때문이다.

$$
T_{B\_A}=T_{A\_B}^{-1}
$$

## Point, direction과 pose에 작용하는 방법

Point의 homogeneous coordinate는 마지막 element가 1이므로 translation의 영향을
받는다.

$$
\begin{bmatrix}
R&t\\0&1
\end{bmatrix}
\begin{bmatrix}
p\\1
\end{bmatrix}
=
\begin{bmatrix}
Rp+t\\1
\end{bmatrix}
$$

두 point의 차이처럼 방향과 displacement를 나타내는 vector는 마지막 element를
0으로 둘 수 있다.

$$
\begin{bmatrix}
R&t\\0&1
\end{bmatrix}
\begin{bmatrix}
v\\0
\end{bmatrix}
=
\begin{bmatrix}
Rv\\0
\end{bmatrix}
$$

따라서 translation은 point에는 작용하지만 direction vector에는 작용하지 않는다.

Pose도 rigid transformation이므로 좌표계 왼쪽 변환은 matrix composition으로
적용한다. Frame $B$에서 표현된 body pose를 frame $A$로 옮기면:

$$
T_{A\_{\mathrm{body}}}
=
T_{A\_B}T_{B\_{\mathrm{body}}}
$$

## $SE(3)$가 group인 이유

- Identity transformation이 존재한다.
- 두 $SE(3)$ transformation의 product도 $SE(3)$에 속한다.
- 모든 element가 위 식의 inverse를 가진다.
- Matrix multiplication은 associative다.

Rotation과 translation의 결합에서는 rotation이 translation에 작용하므로 단순한
direct product가 아니다. 이를 semidirect product로 다음처럼 표기한다.

$$
SE(3)\cong SO(3)\ltimes\mathbb R^3
$$

이 표기를 이해하지 않아도 composition과 inverse를 계산할 수 있다.

## $SE(3)$에 포함되지 않는 변환

$SE(3)$ transformation은 다음을 포함하지 않는다.

- scale change
- reflection
- shear
- point마다 다른 non-rigid deformation

Uniform scale까지 포함한 transformation group은 $Sim(3)$다. Metric scale을
평가해야 하는 trajectory에 $Sim(3)$ alignment를 적용하면 scale error를 제거할 수
있으므로 $SE(3)$와 구분해야 한다.

## Active transformation과 frame transformation

$T$는 object를 실제로 움직이는 active transformation으로도, 같은 object coordinate를
다른 frame에서 표현하는 passive transformation으로도 해석할 수 있다. Matrix 형태는
같으므로 symbol의 source와 target을 생략하면 방향을 혼동하기 쉽다.

이 문서에서는 다음 계약을 사용한다.

$$
T_{\mathrm{target}\_\mathrm{source}}
:
\text{source coordinate를 target coordinate로 변환}
$$

$$
p_{\mathrm{target}}
=
T_{\mathrm{target}\_\mathrm{source}}
p_{\mathrm{source}}
$$

## Alignment에서의 $SE(3)$

두 point set이나 trajectory를 정렬할 때는 하나의 fitted transformation
$T_{Y\_X}\in SE(3)$를 source 전체에 동일하게 적용한다.

$$
p_i^{\mathrm{aligned}}
=
R p_i^{X}+t
$$

Point마다 서로 다른 $R_i,t_i$를 선택하면 rigid alignment가 아니라 deformation이며
원래 측정 error까지 제거한다.

Fitted $SE(3)$가 존재한다는 사실만으로 source와 target의 차이가 coordinate frame
때문이라고 판별할 수는 없다. 어떤 global rotation·translation을 평가에서 제거할지는
system의 observability와 평가 계약이 결정한다.

## Lie group이라는 성질

$SE(3)$는 group operation뿐 아니라 smooth manifold 구조도 가진 Lie group이다.
Exponential map, logarithm map과 Lie algebra $\mathfrak{se}(3)$는 local update와
optimization에 필요하지만, SVD로 closed-form rigid alignment를 이해하는 데는
필수 선행 개념이 아니다.

## 관련 문서

- [Affine Transformation](<./13 Affine Transformation.md>)
- [Euclidean Space](<./21 Euclidean Space.md>)
- [Rotation Matrix and SO(3)](<./22 Rotation Matrix and SO(3).md>)
- [Rigid Point Set Alignment with Kabsch and Umeyama](<./24 Rigid Point Set Alignment with Kabsch and Umeyama.md>)

## References

- Timothy D. Barfoot, *State Estimation for Robotics*
- Ethan Eade, *Lie Groups for 2D and 3D Transformations*
