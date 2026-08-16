# Quaternion and 3D Rotation

Quaternion은 네 개의 실수로 이루어진 대수적 대상이다. 그중 norm이 $1$인 unit quaternion은 3차원 rotation을 표현할 수 있다. Rotation matrix와 unit quaternion은 서로 다른 대상이지만, 조건을 만족하는 경우 같은 3차원 rotation에 대응한다.

이 문서에서는 다음 세 개념을 구분한다.

- `Quaternion`: 덧셈과 곱셈이 정의된 대수적 대상
- `Unit quaternion`: norm이 $1$인 quaternion
- `Rotation quaternion`: unit quaternion을 이용해 표현한 3차원 rotation

구체적인 수식에서는 right-handed coordinate system, column vector, Hamilton product와

$$
\widehat{p'}=q\widehat p q^*
$$

형태의 vector rotation을 사용한다. 다른 convention을 사용하는 시스템에서는 일부 수식의 곱셈 순서나 부호가 달라질 수 있다.

## Motivation

3차원 Euclidean space에서 origin을 고정하고 length와 angle을 보존하는 orientation-preserving linear map을 rotation이라고 한다. Orthonormal basis를 선택하면 rotation은 다음 조건을 만족하는 $3\times3$ matrix $R$로 표현된다.

$$
R^{\mathsf T}R=I,
\qquad
\det R=1.
$$

이러한 matrix의 집합을 `special orthogonal group` $SO(3)$라고 한다.

$$
SO(3)
:=
\left\{
R\in\mathbb R^{3\times3}
\mid
R^{\mathsf T}R=I,\ \det R=1
\right\}.
$$

$SO(3)$의 matrix 성질, column의 geometric 의미와 composition은 [Rotation Matrix and SO(3)](<./22 Rotation Matrix and SO(3).md>)에서 자세히 설명한다.

Rotation matrix는 vector에 바로 곱할 수 있고 geometric 의미가 명확하다. 하지만 아홉 component가 서로 독립적이지 않고 orthogonality와 determinant 조건을 계속 만족해야 한다.

Euler angle은 세 angle만으로 rotation을 나타내지만, axis와 적용 순서를 함께 정해야 한다. 또한 특정 자세에서는 서로 다른 angle 변화가 같은 회전 방향을 나타내는 singularity가 생길 수 있다.

Unit quaternion은 네 component와 하나의 unit norm 조건으로 rotation을 나타낸다. Euler angle의 axis-order singularity 없이 rotation을 합성하고 보간할 수 있으며, 수치 오차가 생겼을 때 normalization으로 unit norm을 복구하기도 쉽다. 대신 $q$와 $-q$가 같은 rotation을 나타내므로 표현이 유일하지 않다.

따라서 quaternion은 rotation matrix를 없애는 개념이 아니라, 같은 3차원 rotation을 계산 목적에 맞게 다르게 표현하는 방법이다.

## Quaternion

### Definition

실수 $w,x,y,z\in\mathbb R$와 세 기호 $\mathbf i,\mathbf j,\mathbf k$를 사용하여 quaternion $q$를 다음과 같이 나타낸다.

$$
q=w+x\mathbf i+y\mathbf j+z\mathbf k.
$$

$w$를 `scalar part`, $(x,y,z)$를 `vector part`라고 한다. 이를 scalar-vector pair로 쓰면 다음과 같다.

$$
q=(w,\mathbf v),
\qquad
\mathbf v=(x,y,z)\in\mathbb R^3.
$$

Quaternion을 네 component의 tuple로 나타내는 것은 편리하지만, 일반적인 $\mathbb R^4$ vector와 달리 아래의 quaternion multiplication이 정의되어 있다는 점이 핵심이다.

### Basis multiplication

Quaternion basis는 다음 관계를 만족한다.

$$
\mathbf i^2=\mathbf j^2=\mathbf k^2=-1,
\qquad
\mathbf i\mathbf j\mathbf k=-1.
$$

서로 다른 basis의 곱은 다음과 같다.

$$
\mathbf i\mathbf j=\mathbf k,
\qquad
\mathbf j\mathbf k=\mathbf i,
\qquad
\mathbf k\mathbf i=\mathbf j.
$$

곱셈 순서를 바꾸면 부호가 바뀐다.

$$
\mathbf j\mathbf i=-\mathbf k,
\qquad
\mathbf k\mathbf j=-\mathbf i,
\qquad
\mathbf i\mathbf k=-\mathbf j.
$$

따라서 quaternion multiplication은 일반적으로 commutative하지 않다.

$$
q_1q_2\ne q_2q_1.
$$

이는 3차원 rotation을 적용하는 순서가 결과에 영향을 준다는 사실과 대응한다.

### Scalar-vector product

$q_1=(w_1,\mathbf v_1)$과 $q_2=(w_2,\mathbf v_2)$의 곱은 dot product와 cross product를 사용하여 다음과 같이 쓸 수 있다.

$$
\boxed{
q_1q_2
=
\left(
w_1w_2-\mathbf v_1\cdot\mathbf v_2,
\quad
w_1\mathbf v_2+w_2\mathbf v_1+\mathbf v_1\times\mathbf v_2
\right)
}.
$$

Cross product의 순서가 바뀌면 부호가 바뀌므로 quaternion multiplication의 비가환성이 이 식에 직접 나타난다.

## Conjugate, Norm and Inverse

### Conjugate

$q=(w,\mathbf v)$의 `conjugate` $q^*$는 vector part의 부호를 바꾼 quaternion이다.

$$
q^*:=(w,-\mathbf v)
=w-x\mathbf i-y\mathbf j-z\mathbf k.
$$

### Norm

Quaternion의 norm은 다음과 같다.

$$
\lVert q\rVert
:=
\sqrt{w^2+x^2+y^2+z^2}.
$$

Quaternion과 conjugate를 곱하면 실수 quaternion을 얻는다.

$$
qq^*=q^*q=\lVert q\rVert^2.
$$

### Inverse

$q\ne0$이면 multiplicative inverse는 다음과 같다.

$$
q^{-1}=\frac{q^*}{\lVert q\rVert^2}.
$$

실제로 다음이 성립한다.

$$
qq^{-1}=q^{-1}q=1.
$$

### Unit quaternion

다음 조건을 만족하는 quaternion을 `unit quaternion`이라고 한다.

$$
\lVert q\rVert=1.
$$

Unit quaternion에서는 inverse와 conjugate가 같다.

$$
\boxed{q^{-1}=q^*}.
$$

Unit quaternion을 곱한 결과도 unit quaternion이다. 따라서 unit quaternion은 quaternion multiplication에 대해 group을 이룬다.

## Axis-angle에서 Unit Quaternion으로

단위 vector $\mathbf u\in\mathbb R^3$를 rotation axis, $\theta$를 right-hand rule을 따르는 rotation angle이라고 하자.

$$
\lVert\mathbf u\rVert=1.
$$

이 axis-angle rotation에 대응하는 unit quaternion을 다음과 같이 정의한다.

$$
\boxed{
q(\mathbf u,\theta)
=
\left(
\cos\frac{\theta}{2},
\mathbf u\sin\frac{\theta}{2}
\right)
}.
$$

Component로 쓰면 다음과 같다.

$$
q
=
\left(
\cos\frac{\theta}{2},
u_x\sin\frac{\theta}{2},
u_y\sin\frac{\theta}{2},
u_z\sin\frac{\theta}{2}
\right).
$$

Angle에 $\theta/2$가 사용되는 이유는 vector를 회전할 때 quaternion을 왼쪽과 오른쪽에서 한 번씩 곱하기 때문이다. 다음 절의 $q\widehat p q^*$를 전개하면 최종 vector에는 $\theta$만큼의 rotation이 적용된다.

Norm을 계산하면 이 quaternion이 unit quaternion임을 확인할 수 있다.

$$
\begin{aligned}
\lVert q\rVert^2
&=
\cos^2\frac{\theta}{2}
+
\lVert\mathbf u\rVert^2\sin^2\frac{\theta}{2} \\
&=
\cos^2\frac{\theta}{2}
+
\sin^2\frac{\theta}{2} \\
&=1.
\end{aligned}
$$

## Quaternion으로 Vector를 회전하기

### Pure quaternion

Vector $\mathbf p=(p_x,p_y,p_z)\in\mathbb R^3$를 scalar part가 $0$인 `pure quaternion`으로 포함시킨다.

$$
\widehat p
:=
(0,\mathbf p)
=p_x\mathbf i+p_y\mathbf j+p_z\mathbf k.
$$

Hat은 여기서 3차원 vector를 pure quaternion으로 바꾸었다는 표시다.

### Rotation action

Unit quaternion $q$가 나타내는 rotation을 $\mathbf p$에 적용한 결과를 다음과 같이 정의한다.

$$
\boxed{
\widehat{p'}=q\widehat p q^*
}.
$$

$q\widehat p q^*$의 scalar part는 $0$이므로 다시 3차원 vector $\mathbf p'$로 읽을 수 있다. 이 연산은 vector의 norm을 보존한다.

$$
\lVert\mathbf p'\rVert=\lVert\mathbf p\rVert.
$$

이 문서의 convention에서는 $q=(\cos(\theta/2),\mathbf u\sin(\theta/2))$가 $\mathbf p$를 axis $\mathbf u$ 주위로 $\theta$만큼 active rotation한다.

### Z축 90도 rotation

Z축 단위 vector와 angle을 다음과 같이 두자.

$$
\mathbf u=(0,0,1),
\qquad
\theta=\frac{\pi}{2}.
$$

이에 대응하는 quaternion은 다음과 같다.

$$
q
=
\left(
\cos\frac{\pi}{4},
0,
0,
\sin\frac{\pi}{4}
\right)
=
\left(
\frac{\sqrt2}{2},
0,
0,
\frac{\sqrt2}{2}
\right).
$$

이 문서의 수학적 tuple 순서는 $(w,x,y,z)$다. 같은 quaternion을 $(x,y,z,w)$ 순서로 serialize하면 다음과 같다.

$$
\left(
0,
0,
\frac{\sqrt2}{2},
\frac{\sqrt2}{2}
\right).
$$

두 줄은 component 배치만 다를 뿐 같은 quaternion을 나타낸다.

$\mathbf p=(1,0,0)$을 pure quaternion $\widehat p=(0,1,0,0)$으로 만들고 $q\widehat p q^*$를 계산하면 다음 결과를 얻는다.

$$
\mathbf p'=(0,1,0).
$$

즉, right-handed coordinate system에서 $+x$ 방향이 $+y$ 방향으로 회전한다.

## Rotation Matrix와의 대응

Unit quaternion을 다음과 같이 두자.

$$
q=(w,x,y,z).
$$

$\widehat{p'}=q\widehat p q^*$를 전개하면 다음 rotation matrix를 얻는다.

$$
\boxed{
R(q)=
\begin{bmatrix}
1-2(y^2+z^2) & 2(xy-wz) & 2(xz+wy) \\
2(xy+wz) & 1-2(x^2+z^2) & 2(yz-wx) \\
2(xz-wy) & 2(yz+wx) & 1-2(x^2+y^2)
\end{bmatrix}
}.
$$

그러면 quaternion rotation과 matrix rotation은 같은 결과를 낸다.

$$
\widehat{p'}=q\widehat p q^*
\quad\Longleftrightarrow\quad
\mathbf p'=R(q)\mathbf p.
$$

$R(q)$는 다음 조건을 만족한다.

$$
R(q)^{\mathsf T}R(q)=I,
\qquad
\det R(q)=1.
$$

따라서 $R(q)\in SO(3)$다.

반대로 모든 $R\in SO(3)$에 대해 그 rotation을 나타내는 unit quaternion이 존재한다. 다만 하나의 rotation matrix에는 정확히 두 unit quaternion $q$와 $-q$가 대응한다.

Rotation matrix에서 quaternion을 수치적으로 계산할 때는 matrix trace만 사용하는 단일 식보다 diagonal component의 크기에 따라 계산식을 선택하는 구현이 안정적이다. 특히 rotation angle이 $\pi$에 가까우면 scalar part $w$가 $0$에 가까워지므로 작은 $w$로 나누는 식은 피해야 한다.

## $q$와 $-q$가 같은 Rotation인 이유

$q$ 대신 $-q$를 vector rotation 식에 대입하면 다음과 같다.

$$
(-q)\widehat p(-q)^*
=
(-q)\widehat p(-q^*)
=
q\widehat p q^*.
$$

따라서 다음이 성립한다.

$$
\boxed{R(q)=R(-q)}.
$$

$q$와 $-q$는 quaternion으로서는 서로 다른 값이지만 rotation으로서는 같은 값을 나타낸다. Unit quaternion의 집합은 4차원 Euclidean space의 unit sphere $S^3$이며, 서로 반대편에 있는 두 점 $q$와 $-q$가 $SO(3)$의 같은 rotation에 대응한다. 이를 unit quaternion이 $SO(3)$를 `double cover`한다고 표현한다.

이 성질 때문에 quaternion component를 직접 빼서 orientation error를 계산하면 안 된다. 같은 rotation을 나타내는 $q$와 $-q$의 component 차이는 클 수 있기 때문이다.

연속된 rotation sample을 저장할 때 인접 quaternion $q_{k-1},q_k$가 다음 조건을 만족하면

$$
q_{k-1}\cdot q_k<0,
$$

$q_k$를 $-q_k$로 바꾸어 같은 rotation의 더 가까운 표현을 선택할 수 있다. 이는 component plot과 interpolation에서 불필요한 sign jump를 줄인다.

## Rotation의 합성과 역

### Composition

먼저 $q_1$의 rotation을 적용하고 그다음 $q_2$의 rotation을 적용한다고 하자.

$$
\widehat{p_1}=q_1\widehat p q_1^*,
$$

$$
\widehat{p_2}=q_2\widehat{p_1}q_2^*.
$$

두 식을 합치면 다음과 같다.

$$
\begin{aligned}
\widehat{p_2}
&=q_2(q_1\widehat p q_1^*)q_2^* \\
&=(q_2q_1)\widehat p(q_2q_1)^*.
\end{aligned}
$$

따라서 합성 rotation의 quaternion은 다음과 같다.

$$
\boxed{q_{\mathrm{composed}}=q_2q_1}.
$$

Rotation matrix에서도 같은 순서 관계가 성립한다.

$$
R(q_2q_1)=R(q_2)R(q_1).
$$

Quaternion multiplication은 commutative하지 않으므로 일반적으로 적용 순서를 바꿀 수 없다.

$$
q_2q_1\ne q_1q_2.
$$

### Inverse rotation

Unit quaternion $q$의 rotation을 되돌리는 quaternion은 $q^{-1}=q^*$다.

$$
q^*(q\widehat p q^*)q=\widehat p.
$$

Rotation matrix에서는 transpose가 inverse rotation에 대응한다.

$$
R(q^*)=R(q)^{-1}=R(q)^{\mathsf T}.
$$

## Rotation과 Coordinate Change

Unit quaternion은 rotation을 표현하지만, quaternion 값만으로는 어떤 geometric 대상에 어떤 의미로 적용하는지 알 수 없다. 같은 수식은 active rotation과 coordinate change에 모두 나타날 수 있으므로 vector와 frame에 label을 붙여야 한다.

### Active rotation

하나의 고정된 coordinate system $A$ 안에서 geometric vector $p$ 자체를 rotation시켜 새로운 vector $p'$를 만든다고 하자. 두 vector의 coordinate는 모두 basis $A$로 표현된다.

$$
\widehat{[p']_A}
=
q\widehat{[p]_A}q^*.
$$

이때 변하는 것은 geometric vector이고 coordinate basis는 변하지 않는다. 이를 active rotation이라고 한다.

### Coordinate change

이번에는 하나의 고정된 geometric vector $p$를 서로 다른 frame $A$와 $B$의 coordinate로 표현한다고 하자.

이 문서에서는 $B$의 orientation을 $A$를 기준으로 나타내는 quaternion을 다음과 같이 표기한다.

$$
q_{A\_B}.
$$

이에 대응하는 rotation matrix $R_{A\_B}$를 다음 식으로 정의한다.

$$
\boxed{
[p]_A=R_{A\_B}[p]_B
}.
$$

Quaternion action으로 쓰면 다음과 같다.

$$
\boxed{
\widehat{[p]_A}
=
q_{A\_B}
\widehat{[p]_B}
(q_{A\_B})^*
}.
$$

이 식에서는 geometric vector $p$는 바뀌지 않고 그 coordinate 표현만 $[p]_B$에서 $[p]_A$로 바뀐다. 이를 passive rotation 또는 coordinate change라고 한다.

반대 방향의 coordinate change는 inverse quaternion을 사용한다.

$$
q_{B\_A}=(q_{A\_B})^{-1}=(q_{A\_B})^*.
$$

$$
[p]_B=R_{B\_A}[p]_A
=R_{A\_B}^{\mathsf T}[p]_A.
$$

### 두 해석을 구분하는 방법

Active rotation과 coordinate change는 같은 숫자의 quaternion과 같은 형태의 곱셈을 사용할 수 있다. 차이는 quaternion 내부가 아니라 식의 대상과 frame label에 있다.

$$
\underbrace{[p']_A=R(q)[p]_A}_{\text{vector가 바뀌고 basis는 같음}}
$$

$$
\underbrace{[p]_A=R_{A\_B}[p]_B}_{\text{vector는 같고 basis가 바뀜}}
$$

따라서 `active` 또는 `passive`라는 단어만으로 convention을 정하는 것보다 input과 output coordinate에 frame을 표시한 식을 제시하는 것이 명확하다.

또한 “$A$를 기준으로 표현한 quaternion”이라는 말만으로는 어떤 frame의 orientation인지 빠질 수 있다. 다음 두 정보를 함께 적어야 한다.

- Reference frame: 어느 frame을 기준으로 하는가?
- Oriented frame: 어느 frame의 orientation을 나타내는가?

$q_{A\_B}$는 이 두 정보를 모두 포함하며 “$A$를 기준으로 나타낸 $B$의 orientation”이라고 읽는다. 다만 subscript의 순서는 분야마다 다를 수 있으므로 최종적으로는 $[p]_A=R_{A\_B}[p]_B$와 같은 식으로 정의해야 한다.

## Pose에서 Quaternion의 역할

Quaternion은 rotation만 나타내며 translation은 포함하지 않는다. Frame $B$의 pose를 frame $A$에 대해 나타내려면 translation $t_{A\_B}$와 rotation $q_{A\_B}$가 모두 필요하다.

$t_{A\_B}$를 $A$ coordinate로 표현한 $B$ origin의 위치라고 하면 point coordinate는 다음과 같이 변환된다.

$$
[p]_A
=
R_{A\_B}[p]_B+t_{A\_B}.
$$

Homogeneous coordinate를 사용하면 다음 rigid transformation matrix로 나타낼 수 있다.

$$
T_{A\_B}
=
\begin{bmatrix}
R_{A\_B} & t_{A\_B} \\
0 & 1
\end{bmatrix}.
$$

$$
\begin{bmatrix}
[p]_A\\1
\end{bmatrix}
=
T_{A\_B}
\begin{bmatrix}
[p]_B\\1
\end{bmatrix}.
$$

따라서 pose data에서 quaternion은 transformation matrix 전체가 아니라 rotation block $R_{A\_B}$에 대응한다. Rotation과 translation을 함께 다루는 group $SE(3)$는 [Rigid Transformation and SE(3)](<./23 Rigid Transformation and SE(3).md>)에서 설명한다.

## Spherical Linear Interpolation

두 unit quaternion $q_0,q_1$ 사이의 rotation을 일정한 angular speed로 보간할 때 `spherical linear interpolation` 또는 `SLERP`를 사용할 수 있다.

먼저 dot product를 계산한다.

$$
d=q_0\cdot q_1.
$$

$d<0$이면 $q_1$을 $-q_1$로 바꾼다. 두 값은 같은 endpoint rotation을 나타내지만, 부호를 바꾸면 $S^3$ 위의 더 짧은 arc를 선택할 수 있다.

$$
q_1\leftarrow -q_1,
\qquad
d\leftarrow-d.
$$

$\Omega=\arccos d$라고 하면 $0\le t\le1$에서 SLERP는 다음과 같다.

$$
\operatorname{slerp}(q_0,q_1;t)
=
\frac{\sin((1-t)\Omega)}{\sin\Omega}q_0
+
\frac{\sin(t\Omega)}{\sin\Omega}q_1.
$$

$\Omega$가 $0$에 가까우면 $\sin\Omega$로 나누는 식이 수치적으로 불안정해진다. 이 경우 linear interpolation 결과를 normalize하는 방법을 사용할 수 있다.

## Convention Checklist

Quaternion을 파일이나 API 사이에서 전달할 때에는 다음 항목을 별도로 확인한다.

1. Mathematical action: $q\widehat p q^*$인가, $q^*\widehat p q$인가?
2. Frame relation: 어느 frame의 orientation을 어느 reference frame에 대해 나타내는가?
3. Coordinate equation: $[p]_A=R[p]_B$인가, 그 역방향인가?
4. Vector layout: column vector에 왼쪽에서 곱하는가, row vector에 오른쪽에서 곱하는가?
5. Coordinate handedness: right-handed인가, left-handed인가?
6. Serialization order: $(w,x,y,z)$인가, $(x,y,z,w)$인가?
7. Normalization: quaternion이 unit norm을 만족하는가?
8. Euler conversion: axis와 intrinsic·extrinsic 적용 순서는 무엇인가?

첫 번째부터 다섯 번째 항목은 rotation의 의미와 연산을 결정한다. 여섯 번째 항목은 같은 quaternion을 memory나 text에 어떤 component 순서로 저장하는지를 결정한다. 두 종류의 convention을 분리해야 한다.

## Summary

- Quaternion은 네 실수와 비가환 multiplication으로 이루어진 대수적 대상이다.
- Unit quaternion은 axis-angle을 $q=(\cos(\theta/2),\mathbf u\sin(\theta/2))$로 표현한다.
- $q\widehat p q^*$와 $R(q)\mathbf p$는 이 문서의 convention에서 같은 3차원 rotation을 나타낸다.
- $q$와 $-q$는 서로 다른 quaternion이지만 같은 rotation에 대응한다.
- Quaternion multiplication은 rotation composition에 대응하며 순서가 중요하다.
- Quaternion 값만으로 active rotation과 coordinate change를 구분할 수 없으므로 vector와 frame label이 필요하다.
- Pose에서 quaternion은 translation을 제외한 rotation part만 나타낸다.
- Component storage order와 rotation의 geometric convention은 서로 다른 문제다.

## Related Documents

- [Orthogonal Map](<../07 Linear Algebra/31 Orthogonal Map.md>)
- [Change of Basis and Coordinate Matrix](<../07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)
- [Affine Transformation](<./13 Affine Transformation.md>)
- [Rotation Matrix and SO(3)](<./22 Rotation Matrix and SO(3).md>)
- [Rigid Transformation and SE(3)](<./23 Rigid Transformation and SE(3).md>)
- [Pose Trajectory Coordinate, Time and Alignment](<../../05 Robotics/03 Evaluation/01 Pose Trajectory Coordinate Time and Alignment.md>)
- [Graphics Quaternion](<../../03 programming/02 Graphics/Quaternion.md>)

## References

- Jack B. Kuipers, *Quaternions and Rotation Sequences*
- Ken Shoemake, “Animating Rotation with Quaternion Curves”
- Timothy D. Barfoot, *State Estimation for Robotics*
