# Rotation Matrix and SO(3)

## 한 줄 요약

3차원 rotation matrix는 length, angle과 orientation을 보존하는 orthogonal matrix이며,
이 matrix들의 집합은 multiplication에 대해 special orthogonal group $SO(3)$를 이룬다.

## Orthogonal matrix만으로 rotation을 정의할 수 없는 이유

Euclidean space에서 linear map이 length와 angle을 보존하려면 matrix $Q$가 다음을
만족해야 한다.

$$
Q^{\mathsf T}Q=I
$$

이 조건에서 determinant를 계산하면 다음을 얻는다.

$$
\det(Q)^2
=
\det(Q^{\mathsf T}Q)
=1
$$

따라서 orthogonal matrix의 determinant는 $+1$ 또는 $-1$이다.

$$
\det Q\in\{-1,+1\}
$$

두 경우 모두 length와 angle은 보존하지만 geometric 의미가 다르다.

- $\det Q=+1$: orientation을 보존하는 rotation
- $\det Q=-1$: orientation을 뒤집는 reflection 또는 reflection을 포함한 변환

예를 들어 다음 matrix는 $x$ coordinate의 sign을 뒤집는다.

$$
Q=
\begin{bmatrix}
-1&0&0\\
0&1&0\\
0&0&1
\end{bmatrix}
$$

$Q^{\mathsf T}Q=I$이지만 $\det Q=-1$이므로 3차원 rotation matrix는 아니다.

## $O(3)$와 $SO(3)$

3차원 orthogonal matrix 전체의 집합을 orthogonal group $O(3)$라고 한다.

$$
O(3)
:=
\left\{
Q\in\mathbb R^{3\times3}
\mid
Q^{\mathsf T}Q=I
\right\}
$$

그중 determinant가 $+1$인 matrix만 모은 집합이 special orthogonal group
$SO(3)$다.

$$
SO(3)
:=
\left\{
R\in\mathbb R^{3\times3}
\mid
R^{\mathsf T}R=I,\ \det R=1
\right\}
$$

따라서 다음 inclusion이 성립한다.

$$
SO(3)\subset O(3)
$$

문자 special은 determinant가 $+1$인 subgroup을 선택했다는 뜻이다.

## Rotation matrix의 column과 row

$R^{\mathsf T}R=I$이므로 $R$의 column은 orthonormal basis를 이룬다.

$$
R=
\begin{bmatrix}
r_1&r_2&r_3
\end{bmatrix}
$$

$$
r_i^{\mathsf T}r_j=\delta_{ij}
$$

$RR^{\mathsf T}=I$도 성립하므로 row 역시 orthonormal하다. Rotation matrix가
9개의 element를 가지더라도 이 orthonormal constraint 때문에 independent degree of
freedom은 3개다.

## Length와 angle의 보존

$R\in SO(3)$와 vector $x,y\in\mathbb R^3$에 대해 inner product가 보존된다.

$$
(Rx)^{\mathsf T}(Ry)
=
x^{\mathsf T}R^{\mathsf T}Ry
=
x^{\mathsf T}y
$$

따라서 length와 angle도 보존된다.

$$
\lVert Rx\rVert_2=\lVert x\rVert_2
$$

이 성질 때문에 rotation은 scaling과 shear를 포함하지 않는다.

## $SO(3)$가 group인 이유

Matrix multiplication을 operation으로 사용하면 $SO(3)$는 group의 조건을 만족한다.

### Identity

$$
I\in SO(3)
$$

### Closure

$R_1,R_2\in SO(3)$이면:

$$
(R_1R_2)^{\mathsf T}(R_1R_2)
=
R_2^{\mathsf T}R_1^{\mathsf T}R_1R_2
=I
$$

$$
\det(R_1R_2)
=
\det R_1\det R_2
=1
$$

따라서 $R_1R_2\in SO(3)$다.

### Inverse

$$
R^{-1}=R^{\mathsf T}\in SO(3)
$$

### Associativity

Matrix multiplication이 associative이므로 rotation composition도 associative다.

$SO(3)$는 group이지만 vector space는 아니다. 일반적으로 두 rotation matrix의 합이나
rotation matrix의 scalar multiple은 rotation matrix가 아니다.

## Composition 순서

Column vector convention에서 $R_1$을 먼저 적용하고 $R_2$를 나중에 적용하면:

$$
x'
=
R_2(R_1x)
=
(R_2R_1)x
$$

따라서 matrix product는 실제 적용 순서의 반대로 읽힌다. Rotation을 여러 개 합성할
때 symbol의 source와 target frame을 함께 적으면 순서 오류를 줄일 수 있다.

## Active rotation과 passive coordinate change

같은 식 $y=Rx$는 두 문맥에서 나타날 수 있다.

- Active rotation: coordinate system은 그대로 두고 geometric vector를 회전한다.
- Passive coordinate change: geometric vector는 그대로 두고 다른 basis에서 coordinate를 다시 표현한다.

두 해석은 수식만 보고 자동으로 구분되지 않는다. Frame $B$의 coordinate를 frame
$A$의 coordinate로 바꾸는 rotation을 $R_{A\_B}$라고 정하면 다음처럼 방향을
식으로 고정할 수 있다.

$$
p_A=R_{A\_B}p_B
$$

$R_{A\_B}$의 column은 frame $B$의 basis vector를 frame $A$의 coordinate로 표현한
값이다. 반대 방향은 transpose다.

$$
R_{B\_A}
=
R_{A\_B}^{-1}
=
R_{A\_B}^{\mathsf T}
$$

## Rotation만으로 부족한 경우

Linear rotation은 origin을 origin으로 보낸다.

$$
R0=0
$$

따라서 서로 다른 origin을 가진 coordinate frame 사이를 표현하려면 translation이
추가로 필요하다. Rotation과 translation을 함께 묶은 orientation-preserving rigid
transformation이 $SE(3)$의 element다.

## 관련 문서

- [Group](<../02 Abstract Algebra/05 Group/Group.md>)
- [Orthogonal Map](<../07 Linear Algebra/31 Orthogonal Map.md>)
- [Euclidean Space](<./21 Euclidean Space.md>)
- [Affine Transformation](<./13 Affine Transformation.md>)
- [Rigid Transformation and SE(3)](<./23 Rigid Transformation and SE(3).md>)

## References

- John M. Lee, *Introduction to Smooth Manifolds*
- Timothy D. Barfoot, *State Estimation for Robotics*
