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

### Complex number와 2차원 rotation

2차원 vector $p=(x,y)\in\mathbb R^2$를 complex number로 나타내자.

$$
z=x+y\mathbf i,
\qquad
\mathbf i^2=-1.
$$

먼저 2차원 plane의 positive rotation 방향을 정한다. 이 문서에서는 $+x$축에서 $+y$축으로 도는 counterclockwise 방향을 positive로 사용한다. 이처럼 positive 방향이 선택된 plane을 `oriented plane`이라고 한다.

$z$에 $\mathbf i$를 곱해 보자.

$$
\begin{aligned}
\mathbf i z
&=\mathbf i(x+y\mathbf i) \\
&=x\mathbf i+y\mathbf i^2 \\
&=-y+x\mathbf i.
\end{aligned}
$$

Real part와 imaginary part를 coordinate로 다시 읽으면 다음 대응을 얻는다.

$$
(x,y)\longmapsto(-y,x).
$$

예를 들어 $(1,0)$은 $(0,1)$로 이동하므로, $\mathbf i$를 곱하는 것은 이 oriented plane에서 $90^\circ$만큼 positive rotation하는 연산이다.

이제 $90^\circ$뿐 아니라 임의의 angle $\theta$만큼 회전시키는 complex number를 찾아보자.

Complex plane에서 $1=(1,0)$은 positive real axis 방향의 unit vector다. Angle $\theta$의 rotation을 complex multiplication으로 나타내는 값을 $r_\theta$라고 하자. 그러면 $1$을 회전한 결과는 $r_\theta 1$이어야 한다. $1$은 multiplicative identity이므로 다음이 성립한다.

$$
r_\theta 1=r_\theta.
$$

따라서 $r_\theta$ 자체가 unit circle에서 angle이 $\theta$인 점이어야 한다. 이 점의 coordinate는 $(\cos\theta,\sin\theta)$다. 문서 앞에서 2차원 coordinate $(x,y)$를 complex number $x+y\mathbf i$로 나타냈으므로, $r_\theta$를 다음과 같이 둔다.

$$
r_\theta=\cos\theta+\mathbf i\sin\theta.
$$

이제 이 $r_\theta$가 $1$뿐 아니라 임의의 vector $z=x+y\mathbf i$도 실제로 $\theta$만큼 회전시키는지 확인하자. Complex multiplication을 전개하면 다음과 같다.

$$
\begin{aligned}
r_\theta z
&=(\cos\theta+\mathbf i\sin\theta)(x+y\mathbf i) \\
&=(x\cos\theta-y\sin\theta)
+\mathbf i(x\sin\theta+y\cos\theta).
\end{aligned}
$$

Real part와 imaginary part를 다시 2차원 coordinate로 읽으면 다음 rotation과 같다.

$$
\begin{bmatrix}
x'\\y'
\end{bmatrix}
=
\begin{bmatrix}
\cos\theta&-\sin\theta\\
\sin\theta&\cos\theta
\end{bmatrix}
\begin{bmatrix}
x\\y
\end{bmatrix}.
$$

Complex multiplication은 rotation의 composition도 그대로 표현한다. 여기서 `composition`은 rotation을 차례로 적용한다는 뜻이다. 먼저 $z$를 $\theta$만큼 회전하면 다음 값을 얻는다.

$$
z_1=r_\theta z.
$$

이 결과를 다시 $\phi$만큼 회전하면 다음과 같다.

$$
\begin{aligned}
z_2
&=r_\phi z_1 \\
&=r_\phi(r_\theta z) \\
&=(r_\phi r_\theta)z.
\end{aligned}
$$

따라서 두 rotation을 차례로 적용한 결과는 $r_\phi r_\theta$ 하나를 곱한 것과 같다. 이 product를 전개하면 다음과 같다.

$$
\begin{aligned}
r_\phi r_\theta
&=(\cos\phi+\mathbf i\sin\phi)
  (\cos\theta+\mathbf i\sin\theta) \\
&=(\cos\phi\cos\theta-\sin\phi\sin\theta) \\
&\quad
  +\mathbf i(\sin\phi\cos\theta+\cos\phi\sin\theta) \\
&=\cos(\phi+\theta)+\mathbf i\sin(\phi+\theta) \\
&=r_{\phi+\theta}.
\end{aligned}
$$

전개한 real part와 imaginary part를 $\cos(\phi+\theta)$와 $\sin(\phi+\theta)$로 바꾸는 단계에서는 sine과 cosine의 angle addition formula를 사용했다. 즉, 2차원 rotation을 나타내는 complex number를 곱하면 rotation angle은 더해진다. 예를 들어 먼저 $30^\circ$, 그다음 $45^\circ$ 회전하는 것은 한 번에 $75^\circ$ 회전하는 것과 같다.

$$
r_{45^\circ}r_{30^\circ}=r_{75^\circ}.
$$

역회전도 complex number 안에서 구할 수 있다. Complex number의 `conjugate`는 imaginary part의 부호를 바꾸는 연산이다.

$$
(a+\mathbf i b)^*=a-\mathbf i b.
$$

$r_\theta$의 conjugate는 다음과 같다.

$$
\begin{aligned}
r_\theta^*
&=\cos\theta-\mathbf i\sin\theta \\
&=\cos(-\theta)+\mathbf i\sin(-\theta) \\
&=r_{-\theta}.
\end{aligned}
$$

즉, $+\theta$ rotation의 conjugate는 $-\theta$ rotation을 나타낸다. 두 값을 곱하면 다음과 같다.

$$
\begin{aligned}
r_\theta r_\theta^*
&=(\cos\theta+\mathbf i\sin\theta)
  (\cos\theta-\mathbf i\sin\theta) \\
&=\cos^2\theta+\sin^2\theta \\
&=1.
\end{aligned}
$$

$1$은 angle이 $0$인 identity rotation이다. 따라서 $r_\theta^*$는 $r_\theta$의 rotation을 되돌리는 multiplicative inverse다.

$$
\boxed{
r_\theta^{-1}=r_\theta^*=r_{-\theta}
}.
$$

실제로 $z_1=r_\theta z$를 다시 conjugate로 회전시키면 원래 $z$로 돌아간다.

$$
r_\theta^*z_1
=r_\theta^*(r_\theta z)
=(r_\theta^*r_\theta)z
=z.
$$

Conjugate가 그대로 inverse가 되는 것은 $r_\theta$의 magnitude가 $1$이기 때문이다. 일반적인 nonzero complex number $c$에서는 $c^{-1}=c^*/|c|^2$이지만, unit complex number에서는 $|r_\theta|=1$이므로 denominator가 사라진다.

즉, complex number는 2차원 vector의 표현과 rotation의 적용·composition·inverse를 같은 algebra 안에서 다룰 수 있게 한다.

### 3차원에서 필요한 multiplication

같은 생각을 3차원 rotation에 적용하고 싶다고 하자. 2차원에서 원점 중심 rotation은 모두 같은 $xy$ plane 안에서 일어난다. Positive 방향을 한 번 정하면 signed angle $\theta$만으로 rotation이 결정된다. 2차원 plane을 3차원에서 바라보면 모든 rotation이 그 plane에 수직인 같은 axis를 공유하므로 axis를 매번 별도로 지정할 필요가 없다.

반면 3차원에서는 angle이 같아도 rotation axis가 다르면 서로 다른 rotation이다. 예를 들어 $x$축 주위의 $30^\circ$ rotation, $y$축 주위의 $30^\circ$ rotation과 $z$축 주위의 $30^\circ$ rotation은 서로 다른 결과를 만든다. 특히 $x$축 방향 vector는 $x$축 주위로 회전해도 그대로지만, $z$축 주위로 회전하면 $xy$ plane 안에서 방향이 바뀐다.

따라서 3차원 rotation을 지정하려면 angle $\theta$뿐 아니라 unit rotation axis $\mathbf u$도 필요하다.

$$
\mathbf u
=
u_x\mathbf e_x+u_y\mathbf e_y+u_z\mathbf e_z,
\qquad
\lVert\mathbf u\rVert=1.
$$

Complex number의 두 real basis $1,\mathbf i$는 하나의 고정된 2차원 plane 안의 방향을 표현하기에는 충분하지만, 임의의 3차원 axis를 구분할 수는 없다. 3차원 axis를 표현하려면 $x$, $y$, $z$ 방향을 담는 세 component가 필요하다.

또한 complex multiplication은 commutative하지만 3차원 rotation의 composition은 일반적으로 commutative하지 않다. 예를 들어 x축 rotation 뒤에 y축 rotation을 적용한 결과와 그 순서를 바꾼 결과는 다르다. 따라서 3차원 rotation을 multiplication으로 합성하려면 순서를 구분하는 noncommutative product가 필요하다.

이제 complex number의 어떤 부분을 확장해야 하는지부터 생각해 보자. Complex number $a+b\mathbf i$에서 $a$는 scalar part이고, $b\mathbf i$는 하나의 imaginary direction $\mathbf i$를 따라 놓인 부분이다. 2차원에서는 이 imaginary direction 하나로 충분했지만, 3차원 rotation axis를 나타내려면 $x$, $y$, $z$ 방향에 대응하는 세 imaginary direction $\mathbf i$, $\mathbf j$, $\mathbf k$가 필요하다.

Unit rotation axis $\mathbf u=(u_x,u_y,u_z)$의 세 component를 이 imaginary direction들의 coefficient로 사용하여 다음과 같이 나타내자.

$$
(0,\mathbf u)
:=
u_x\mathbf i+u_y\mathbf j+u_z\mathbf k.
$$

$(0,\mathbf u)$의 앞에 있는 $0$은 zero vector라는 뜻이 아니라 scalar part가 $0$이라는 뜻이고, 두 번째 항 $\mathbf u$가 vector part다. 즉, 원래 vector의 세 component를 바꾸는 것이 아니라 새 multiplication을 적용할 수 있는 원소로 옮겨 적은 것이다. 이처럼 scalar part가 $0$인 원소를 `pure quaternion`이라고 한다. 앞의 scalar 자리가 실제로 필요한 이유는 곧 multiplication의 closure를 살펴보면서 드러난다.

Complex number에서 pure imaginary unit $\mathbf i$는 $\mathbf i^2=-1$을 만족한다. 이 핵심 관계를 3차원으로 확장하려면, 각 unit axis $\mathbf u$를 나타내는 pure quaternion도 다음 관계를 만족하도록 multiplication을 정하는 것이 자연스럽다.

$$
(0,\mathbf u)^2=(-1,\mathbf 0).
$$

이 조건을 만족시키면서 서로 다른 두 axis의 순서도 구분할 수 있는 product가 필요하다. 두 vector $\mathbf u,\mathbf v$의 관계에서 scalar를 만드는 자연스러운 연산은 dot product이고, 두 방향의 순서와 orientation을 담는 vector 연산은 cross product다. 이 두 정보를 함께 보존하도록 pure quaternion의 product를 다음과 같이 정하자.

$$
\boxed{
(0,\mathbf u)(0,\mathbf v)
=
(-\mathbf u\cdot\mathbf v,\ \mathbf u\times\mathbf v)
}.
$$

$\mathbf v=\mathbf u$이면 cross product는 $0$이므로 다음을 얻는다.

$$
(0,\mathbf u)^2
=
(-\lVert\mathbf u\rVert^2,\mathbf 0).
$$

따라서 unit vector를 나타내는 pure quaternion은 의도한 대로 square가 $-1$이 된다. 서로 수직인 standard basis $\mathbf e_x,\mathbf e_y$에는 다음 관계가 성립한다.

$$
(0,\mathbf e_x)(0,\mathbf e_y)=(0,\mathbf e_z),
$$

$$
(0,\mathbf e_y)(0,\mathbf e_x)=-(0,\mathbf e_z).
$$

Cross product의 방향 때문에 multiplication 순서도 구분된다.

### Scalar part가 필요한 이유

위 product의 결과에는 scalar $-\mathbf u\cdot\mathbf v$와 vector $\mathbf u\times\mathbf v$가 동시에 나타난다. 따라서 $\mathbb R^3$ vector만으로 이루어진 집합은 이 multiplication에 대해 닫혀 있지 않다. Product의 결과를 다시 같은 종류의 원소로 받으려면 scalar 한 개와 3차원 vector를 함께 가져야 한다.

$$
\mathbb R\oplus\mathbb R^3.
$$

따라서 새로운 원소를 다음 pair로 둔다.

$$
q=(w,\mathbf v),
\qquad
w\in\mathbb R,
\quad
\mathbf v\in\mathbb R^3.
$$

Scalar가 vector와 commute하고 multiplication이 distributive하도록 pure quaternion product를 확장해 보자. 각 quaternion을 scalar-only part와 pure quaternion의 합으로 나누면 다음과 같다.

$$
\begin{aligned}
(w_1,\mathbf v_1)(w_2,\mathbf v_2)
&=
\bigl((w_1,\mathbf0)+(0,\mathbf v_1)\bigr)
\bigl((w_2,\mathbf0)+(0,\mathbf v_2)\bigr) \\
&=(w_1w_2,\mathbf0)
+(0,w_1\mathbf v_2)
+(0,w_2\mathbf v_1)
+(-\mathbf v_1\cdot\mathbf v_2,\mathbf v_1\times\mathbf v_2) \\
&=
\left(
w_1w_2-\mathbf v_1\cdot\mathbf v_2,
\quad
w_1\mathbf v_2+w_2\mathbf v_1+\mathbf v_1\times\mathbf v_2
\right).
\end{aligned}
$$

따라서 일반 quaternion의 product는 다음 식으로 정리된다.

$$
\boxed{
(w_1,\mathbf v_1)(w_2,\mathbf v_2)
=
\left(
w_1w_2-\mathbf v_1\cdot\mathbf v_2,
\quad
w_1\mathbf v_2+w_2\mathbf v_1+\mathbf v_1\times\mathbf v_2
\right)
}.
$$

이 scalar-vector pair와 multiplication이 quaternion algebra다. 즉, 네 component와 비가환 multiplication 규칙은 3차원 rotation을 단순히 짧게 저장하려고 임의로 선택한 형식이 아니다. Complex number가 2차원 rotation을 multiplication으로 다루는 방식을 3차원 방향에 맞게 확장하고, dot product와 cross product를 결합한 product에 closure를 요구하면 자연스럽게 나타나는 구조다.

Quaternion만이 3차원 rotation을 표현하는 유일한 방법이라는 뜻은 아니다. Rotation matrix도 rotation을 완전하게 표현한다. 여기서 quaternion을 도입하는 이유는 3차원 direction, rotation composition과 inverse를 하나의 algebraic multiplication 안에서 다루기 위해서다. 이후 절에서는 이 algebra를 정식으로 정의하고, unit quaternion의 conjugation이 실제로 3차원 rotation을 만드는 과정을 설명한다.

## Quaternion

### Definition

실수 위의 4차원 vector space $\mathbb H$를 다음과 같이 두자.

$$
\mathbb H
:=
\left\{
w+x\mathbf i+y\mathbf j+z\mathbf k
\mid
w,x,y,z\in\mathbb R
\right\}.
$$

$\mathbb H$의 원소를 `quaternion`이라고 한다. Quaternion $q$는 다음과 같이 나타낸다.

$$
q=w+x\mathbf i+y\mathbf j+z\mathbf k.
$$

$w$를 `scalar part`, $(x,y,z)$를 `vector part`라고 한다. 이를 scalar-vector pair로 쓰면 다음과 같다.

$$
q=(w,\mathbf v),
\qquad
\mathbf v=(x,y,z)\in\mathbb R^3.
$$

Addition과 real scalar multiplication은 componentwise하게 정의한다. Quaternion multiplication은 다음 절의 scalar-vector product로 정의한다. 이 product는 bilinear하고 associative하지만 일반적으로 commutative하지 않다. 이 multiplication이 추가된다는 점에서 $\mathbb H$는 일반적인 $\mathbb R^4$ vector space보다 더 많은 algebraic structure를 가진다.

### Scalar-vector product

$q_1=(w_1,\mathbf v_1)$과 $q_2=(w_2,\mathbf v_2)$의 곱을 dot product와 cross product를 사용하여 다음과 같이 정의한다.

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

이 식은 Motivation에서 pure quaternion의 product를 구성한 뒤 scalar part까지 포함하도록 확장한 식과 같다. Cross product의 순서가 바뀌면 부호가 바뀌므로 quaternion multiplication의 비가환성도 이 식에 직접 나타난다.

### Basis multiplication

Basis multiplication 규칙은 위 product와 무관하게 따로 정하는 규칙이 아니다. Quaternion의 vector basis를 right-handed orthonormal standard basis와 다음처럼 대응시키고, scalar-vector product에 직접 대입하여 확인할 수 있다.

$$
\mathbf i=(0,\mathbf e_x),
\qquad
\mathbf j=(0,\mathbf e_y),
\qquad
\mathbf k=(0,\mathbf e_z).
$$

먼저 같은 basis $\mathbf i$끼리 곱해 보자. $\mathbf e_x$는 unit vector이고 자기 자신과의 cross product는 $\mathbf 0$이므로 다음과 같다.

$$
\begin{aligned}
\mathbf i^2
&=(0,\mathbf e_x)(0,\mathbf e_x) \\
&=(-\mathbf e_x\cdot\mathbf e_x,\ \mathbf e_x\times\mathbf e_x) \\
&=(-1,\mathbf 0) \\
&=-1.
\end{aligned}
$$

$\mathbf j$와 $\mathbf k$에도 같은 계산을 적용하면 다음을 얻는다.

$$
\mathbf i^2=\mathbf j^2=\mathbf k^2=-1.
$$

이번에는 서로 다른 basis $\mathbf i$와 $\mathbf j$를 이 순서로 곱해 보자. 두 standard basis는 서로 수직이고 $\mathbf e_x\times\mathbf e_y=\mathbf e_z$이므로 다음과 같다.

$$
\begin{aligned}
\mathbf i\mathbf j
&=(0,\mathbf e_x)(0,\mathbf e_y) \\
&=(-\mathbf e_x\cdot\mathbf e_y,\ \mathbf e_x\times\mathbf e_y) \\
&=(0,\mathbf e_z) \\
&=\mathbf k.
\end{aligned}
$$

Right-handed cyclic order에 같은 계산을 적용하면 다음 관계를 얻는다.

$$
\mathbf i\mathbf j=\mathbf k,
\qquad
\mathbf j\mathbf k=\mathbf i,
\qquad
\mathbf k\mathbf i=\mathbf j.
$$

곱의 순서를 바꾼 $\mathbf j\mathbf i$도 직접 계산해 보자. Dot product는 그대로지만 cross product의 방향이 반대가 된다.

$$
\begin{aligned}
\mathbf j\mathbf i
&=(0,\mathbf e_y)(0,\mathbf e_x) \\
&=(-\mathbf e_y\cdot\mathbf e_x,\ \mathbf e_y\times\mathbf e_x) \\
&=(0,-\mathbf e_z) \\
&=-\mathbf k.
\end{aligned}
$$

따라서 나머지 reverse order에서도 부호가 바뀐다.

$$
\mathbf j\mathbf i=-\mathbf k,
\qquad
\mathbf k\mathbf j=-\mathbf i,
\qquad
\mathbf i\mathbf k=-\mathbf j.
$$

마지막으로 $\mathbf i\mathbf j\mathbf k=-1$은 독립적으로 추가해야 하는 규칙이 아니다. 앞에서 계산한 두 관계를 사용하면 바로 확인할 수 있다.

$$
\mathbf i\mathbf j\mathbf k
=(\mathbf i\mathbf j)\mathbf k
=\mathbf k^2
=-1.
$$

따라서 standard basis의 dot product와 right-handed cross product가 basis multiplication의 부호를 결정한다. 특히

$$
\mathbf i\mathbf j=\mathbf k
\ne
-\mathbf k=\mathbf j\mathbf i
$$

이므로 quaternion multiplication은 일반적으로 commutative하지 않다. 이는 3차원 rotation을 적용하는 순서가 결과에 영향을 준다는 사실과 대응한다.

## Conjugate, Norm and Inverse

Quaternion multiplication으로 rotation의 composition을 표현하려면, rotation을 되돌리는 inverse도 같은 multiplication 안에서 구할 수 있어야 한다. Nonzero quaternion $q=(w,\mathbf v)$의 inverse를 찾으려면 먼저 $q$와 곱했을 때 vector part가 사라지고 nonzero scalar만 남는 quaternion을 찾아야 한다.

### Conjugate

$q=(w,\mathbf v)$와 scalar part는 같고 vector part의 부호가 반대인 quaternion을 생각해 보자. 이를 $q$의 `conjugate` $q^*$라고 정의한다.

$$
q^*:=(w,-\mathbf v)
=w-x\mathbf i-y\mathbf j-z\mathbf k.
$$

이는 complex conjugate $a+b\mathbf i\mapsto a-b\mathbf i$에서 imaginary part의 부호를 바꾸는 방식을 quaternion의 3차원 vector part 전체로 확장한 것이다. 이 부호 반전이 필요한 이유는 $q$와 $q^*$를 직접 곱하면 확인할 수 있다.

$$
\begin{aligned}
qq^*
&=(w,\mathbf v)(w,-\mathbf v) \\
&=\left(
w^2-\mathbf v\cdot(-\mathbf v),
\ -w\mathbf v+w\mathbf v+\mathbf v\times(-\mathbf v)
\right) \\
&=\left(
w^2+\lVert\mathbf v\rVert^2,
\mathbf 0
\right).
\end{aligned}
$$

Vector part에서는 $-w\mathbf v+w\mathbf v=\mathbf 0$이고 $\mathbf v\times\mathbf v=\mathbf 0$이므로 모든 항이 상쇄된다. 곱의 순서를 바꾼 $q^*q$에서도 같은 결과를 얻는다. 즉, conjugate는 $q$와 곱했을 때 vector part를 없애고 nonnegative scalar만 남긴다.

### Norm

위 곱에 남은 scalar는 $q=(w,x,y,z)$를 $\mathbb R^4$ vector로 보았을 때 Euclidean length의 제곱과 같다.

$$
w^2+\lVert\mathbf v\rVert^2
=w^2+x^2+y^2+z^2.
$$

따라서 이 값의 square root를 quaternion의 `norm`으로 정의한다.

$$
\lVert q\rVert
:=
\sqrt{w^2+x^2+y^2+z^2}.
$$

앞에서 계산한 결과는 norm을 사용하여 다음처럼 쓸 수 있다.

$$
qq^*=q^*q=\lVert q\rVert^2.
$$

이 norm은 quaternion multiplication과도 잘 맞는다. 먼저 $q_1=(a,\mathbf u)$, $q_2=(b,\mathbf v)$를 scalar-vector product에 대입하면 다음 두 quaternion이 같은 것을 직접 확인할 수 있다.

$$
\begin{aligned}
(q_1q_2)^*
&=
\left(
ab-\mathbf u\cdot\mathbf v,
-a\mathbf v-b\mathbf u-\mathbf u\times\mathbf v
\right), \\
q_2^*q_1^*
&=
\left(
ab-\mathbf u\cdot\mathbf v,
-b\mathbf u-a\mathbf v+\mathbf v\times\mathbf u
\right).
\end{aligned}
$$

$\mathbf v\times\mathbf u=-\mathbf u\times\mathbf v$이므로 다음 관계가 성립한다.

$$
(q_1q_2)^*=q_2^*q_1^*.
$$

이를 사용하면 product의 norm을 다음과 같이 계산할 수 있다.

$$
\begin{aligned}
\lVert q_1q_2\rVert^2
&=(q_1q_2)(q_1q_2)^* \\
&=q_1q_2q_2^*q_1^* \\
&=q_1\lVert q_2\rVert^2q_1^* \\
&=\lVert q_1\rVert^2\lVert q_2\rVert^2.
\end{aligned}
$$

Norm은 nonnegative이므로 square root를 취하면 다음을 얻는다.

$$
\boxed{
\lVert q_1q_2\rVert
=
\lVert q_1\rVert\lVert q_2\rVert
}.
$$

### Inverse

$q\ne0$이면 $\lVert q\rVert^2>0$이다. 따라서 conjugate $q^*$를 이 scalar로 나누어 $q$에 곱하면 다음을 얻는다.

$$
q\left(\frac{q^*}{\lVert q\rVert^2}\right)
=
\frac{qq^*}{\lVert q\rVert^2}
=1.
$$

곱의 순서를 바꾸어도 $q^*q=\lVert q\rVert^2$이므로 같은 결과를 얻는다. 따라서 $q$의 multiplicative inverse는 별도로 선택하는 공식이 아니라 conjugate와 norm으로부터 다음과 같이 결정된다.

$$
q^{-1}=\frac{q^*}{\lVert q\rVert^2}.
$$

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

두 unit quaternion $q_1,q_2$의 product에는 norm의 multiplicative property를 적용할 수 있다.

$$
\lVert q_1q_2\rVert
=
\lVert q_1\rVert\lVert q_2\rVert
=1.
$$

따라서 product도 unit quaternion이다. Identity $1=(1,\mathbf0)$도 unit quaternion이고, $q^*$는 $q$와 같은 norm을 가지므로 inverse $q^{-1}=q^*$도 unit quaternion이다. Quaternion multiplication의 associativity까지 함께 사용하면 unit quaternion이 multiplication에 대해 group을 이룬다는 것을 확인할 수 있다.

## Quaternion으로 Vector를 회전하기

### Pure quaternion

Quaternion multiplication을 3차원 vector에 적용하려면 먼저 vector를 quaternion으로 나타내야 한다. Vector $\mathbf p=(p_x,p_y,p_z)\in\mathbb R^3$의 세 component를 그대로 유지하고 scalar part만 $0$으로 둔 `pure quaternion`을 사용한다.

$$
\widehat p
:=
(0,\mathbf p)
=p_x\mathbf i+p_y\mathbf j+p_z\mathbf k.
$$

Hat은 여기서 3차원 vector를 pure quaternion으로 옮겨 적었다는 표시다.

### Conjugation을 사용하는 이유

2차원 complex rotation에서는 unit complex number를 vector에 한쪽에서 곱하는 것만으로 충분했다. Quaternion에서도 먼저 unit quaternion $q=(w,\mathbf v)$를 $\widehat p=(0,\mathbf p)$에 한쪽에서만 곱해 보자.

$$
q\widehat p
=
\left(
-\mathbf v\cdot\mathbf p,
\ w\mathbf p+\mathbf v\times\mathbf p
\right).
$$

Scalar part $-\mathbf v\cdot\mathbf p$는 일반적으로 $0$이 아니다. 따라서 $q\widehat p$는 pure quaternion이 아니며, 그 결과를 그대로 3차원 vector로 읽을 수 없다.

이 scalar part를 다시 없애기 위해 $q$의 inverse를 오른쪽에 곱하는 two-sided product를 생각하자. Unit quaternion에서는 $q^{-1}=q^*$이므로 후보는 $q\widehat p q^*$다. 이 product의 scalar part가 실제로 사라지는지 직접 확인해 보자. 먼저

$$
q\widehat p
=
(a,\mathbf b),
\qquad
a=-\mathbf v\cdot\mathbf p,
\quad
\mathbf b=w\mathbf p+\mathbf v\times\mathbf p
$$

라고 두면, $(a,\mathbf b)q^*=(a,\mathbf b)(w,-\mathbf v)$의 scalar part는 다음과 같다.

$$
\begin{aligned}
aw+\mathbf b\cdot\mathbf v
&=-w(\mathbf v\cdot\mathbf p)
+(w\mathbf p+\mathbf v\times\mathbf p)\cdot\mathbf v \\
&=0.
\end{aligned}
$$

마지막 등식에서는 $\mathbf p\cdot\mathbf v=\mathbf v\cdot\mathbf p$와 $(\mathbf v\times\mathbf p)\cdot\mathbf v=0$을 사용했다. 같은 product의 vector part는 vector triple product identity를 사용하여 다음처럼 정리된다.

$$
\begin{aligned}
-a\mathbf v+w\mathbf b-\mathbf b\times\mathbf v
&=(\mathbf v\cdot\mathbf p)\mathbf v
+w(w\mathbf p+\mathbf v\times\mathbf p)
-(w\mathbf p+\mathbf v\times\mathbf p)\times\mathbf v \\
&=(w^2-\lVert\mathbf v\rVert^2)\mathbf p
+2(\mathbf v\cdot\mathbf p)\mathbf v
+2w(\mathbf v\times\mathbf p).
\end{aligned}
$$

두 번째 등식에서는 $\mathbf p\times\mathbf v=-\mathbf v\times\mathbf p$와 다음 vector triple product identity를 사용했다.

$$
(\mathbf v\times\mathbf p)\times\mathbf v
=
\lVert\mathbf v\rVert^2\mathbf p
-(\mathbf v\cdot\mathbf p)\mathbf v.
$$

따라서 two-sided product의 전체 결과는 다음과 같다.

$$
\boxed{
q\widehat p q^*
=
\left(
0,
(w^2-\lVert\mathbf v\rVert^2)\mathbf p
+2(\mathbf v\cdot\mathbf p)\mathbf v
+2w(\mathbf v\times\mathbf p)
\right)
}.
$$

Scalar part가 $0$이므로 결과를 다시 3차원 vector로 읽을 수 있다. 또한 quaternion norm의 multiplicative property와 $\lVert q\rVert=\lVert q^*\rVert=1$을 사용하면 다음을 얻는다.

$$
\lVert q\widehat p q^*\rVert
=
\lVert q\rVert\lVert\widehat p\rVert\lVert q^*\rVert
=
\lVert\widehat p\rVert.
$$

즉, $q\widehat p q^*$는 pure quaternion의 집합을 그 안으로 보내면서 vector의 length도 보존한다. 다음 절에서 이 변환이 어떤 axis와 angle의 rotation인지 계산한다. 이 문서에서는 이 two-sided conjugation을 unit quaternion의 vector action으로 사용한다.

$$
\boxed{
\widehat{p'}=q\widehat p q^*
}.
$$

## Axis-angle에서 Unit Quaternion으로

단위 vector $\mathbf u\in\mathbb R^3$를 원하는 rotation axis라고 하자.

$$
\lVert\mathbf u\rVert=1.
$$

Quaternion의 vector part가 이 axis를 나타내려면 $\mathbf u$와 평행해야 한다. 따라서 먼저 다음 형태의 unit quaternion을 후보로 둔다.

$$
q=(a,b\mathbf u),
\qquad
a,b\in\mathbb R.
$$

Unit 조건을 적용하면

$$
1=\lVert q\rVert^2=a^2+b^2\lVert\mathbf u\rVert^2=a^2+b^2
$$

이므로 $(a,b)$는 unit circle 위의 점이다. 따라서 어떤 angle $\alpha$를 사용하여 다음과 같이 나타낼 수 있다.

$$
q_\alpha
=
(\cos\alpha,\mathbf u\sin\alpha).
$$

반대로 vector part가 nonzero인 모든 unit quaternion $q=(w,\mathbf v)$는 $\mathbf u=\mathbf v/\lVert\mathbf v\rVert$로 두어 이 형태로 쓸 수 있다. Vector part가 $0$인 두 unit quaternion $q=1,-1$도 각각 $\alpha=0,\pi$에 해당한다.

이는 $U=(0,\mathbf u)$가 $U^2=-1$을 만족하므로, unit complex number $\cos\alpha+\mathbf i\sin\alpha$에서 $\mathbf i$를 $U$로 바꾼 형태이기도 하다. 아직 $\alpha$가 실제 3차원 rotation angle과 같다고 정한 것은 아니다. 실제 angle은 conjugation action을 계산하여 확인해야 한다.

$c=\cos\alpha$, $s=\sin\alpha$라고 두고, 앞 절의 vector part 공식에 $w=c$, $\mathbf v=s\mathbf u$를 대입하면 다음을 얻는다.

$$
\begin{aligned}
\mathbf p'
&=(c^2-s^2)\mathbf p
+2s^2(\mathbf u\cdot\mathbf p)\mathbf u
+2cs(\mathbf u\times\mathbf p) \\
&=\mathbf p\cos(2\alpha)
+(\mathbf u\times\mathbf p)\sin(2\alpha)
+\mathbf u(\mathbf u\cdot\mathbf p)(1-\cos(2\alpha)).
\end{aligned}
$$

두 번째 줄에서는 다음 double-angle identity를 사용했다.

$$
c^2-s^2=\cos(2\alpha),
\qquad
2cs=\sin(2\alpha),
\qquad
2s^2=1-\cos(2\alpha).
$$

이 식이 실제로 angle $2\alpha$의 rotation인지 vector를 axis에 parallel한 부분과 perpendicular한 부분으로 나누어 확인해 보자.

$$
\mathbf p_{\parallel}
:=
(\mathbf u\cdot\mathbf p)\mathbf u,
\qquad
\mathbf p_{\perp}
:=
\mathbf p-\mathbf p_{\parallel}.
$$

$\mathbf u\times\mathbf p_{\parallel}=\mathbf0$이므로 위 식은 다음처럼 다시 쓸 수 있다.

$$
\mathbf p'
=
\mathbf p_{\parallel}
+\mathbf p_{\perp}\cos(2\alpha)
+(\mathbf u\times\mathbf p_{\perp})\sin(2\alpha).
$$

Parallel component $\mathbf p_{\parallel}$은 그대로 유지된다. Perpendicular plane에서는 $\mathbf p_{\perp}$와 $\mathbf u\times\mathbf p_{\perp}$가 서로 수직이고 magnitude가 같으며, right-hand rule에서 positive 방향을 이룬다. 따라서 뒤의 두 항은 2차원 rotation 공식과 같고 $\mathbf p_{\perp}$를 angle $2\alpha$만큼 회전시킨다. 즉, 전체 식은 axis $\mathbf u$ 주위의 angle $2\alpha$ rotation이며, 이를 Rodrigues formula라고 한다.

따라서 원하는 rotation angle이 $\theta$라면 $2\alpha=\theta$, 즉 $\alpha=\theta/2$로 두어야 한다. 이로부터 axis-angle rotation에 대응하는 unit quaternion이 결정된다.

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

이 quaternion의 norm은 unit 조건을 그대로 만족한다.

$$
\lVert q\rVert^2
=
\cos^2\frac{\theta}{2}
+\lVert\mathbf u\rVert^2\sin^2\frac{\theta}{2}
=1.
$$

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

$\mathbf p=(1,0,0)$을 pure quaternion $\widehat p=\mathbf i$로 나타내자. $c=s=\sqrt2/2$라고 쓰면 $q=c+\mathbf k s$, $q^*=c-\mathbf k s$이므로 conjugation을 직접 계산할 수 있다.

$$
\begin{aligned}
q\mathbf i q^*
&=(c+\mathbf k s)\mathbf i(c-\mathbf k s) \\
&=(c^2-s^2)\mathbf i+2cs\mathbf j \\
&=\mathbf j.
\end{aligned}
$$

따라서 pure quaternion $\mathbf j$를 다시 3차원 coordinate로 읽으면 다음 결과를 얻는다.

$$
\mathbf p'=(0,1,0).
$$

즉, right-handed coordinate system에서 $+x$ 방향이 $+y$ 방향으로 회전한다.

## Rotation Matrix와의 대응

Unit quaternion과 vector를 다음과 같이 두자.

$$
q=(w,\mathbf v)=(w,x,y,z),
\qquad
\mathbf p=(p_x,p_y,p_z).
$$

앞에서 conjugation의 vector part를 다음과 같이 계산했다.

$$
\mathbf p'
=
(w^2-\lVert\mathbf v\rVert^2)\mathbf p
+2(\mathbf v\cdot\mathbf p)\mathbf v
+2w(\mathbf v\times\mathbf p).
$$

이 식에서 $\mathbf p'$의 $x$ component를 직접 모아 보자. $\mathbf v\cdot\mathbf p=x p_x+y p_y+z p_z$이고, $(\mathbf v\times\mathbf p)_x=yp_z-zp_y$이므로 다음을 얻는다.

$$
\begin{aligned}
p_x'
&=(w^2-x^2-y^2-z^2)p_x
+2x(xp_x+yp_y+zp_z)
+2w(yp_z-zp_y) \\
&=(w^2+x^2-y^2-z^2)p_x
+2(xy-wz)p_y
+2(xz+wy)p_z.
\end{aligned}
$$

Unit condition $w^2+x^2+y^2+z^2=1$을 사용하면 첫 coefficient는 다음처럼 바뀐다.

$$
w^2+x^2-y^2-z^2
=
1-2(y^2+z^2).
$$

$y$, $z$ component에도 같은 계산을 적용하고 각 coefficient를 row로 모으면 다음 rotation matrix를 얻는다.

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

따라서 이 matrix는 별도로 선택한 공식이 아니라 conjugation의 vector part를 coordinate별로 정리한 것이다. Quaternion rotation과 matrix rotation은 같은 결과를 낸다.

$$
\widehat{p'}=q\widehat p q^*
\quad\Longleftrightarrow\quad
\mathbf p'=R(q)\mathbf p.
$$

앞에서 unit quaternion의 conjugation이 모든 vector의 norm을 보존한다는 것을 확인했다. 따라서 모든 $\mathbf p$에 대해

$$
\lVert R(q)\mathbf p\rVert^2
=
\mathbf p^{\mathsf T}R(q)^{\mathsf T}R(q)\mathbf p
=
\mathbf p^{\mathsf T}\mathbf p
$$

이고, 이로부터 다음 orthogonality condition을 얻는다.

$$
R(q)^{\mathsf T}R(q)=I.
$$

Orthogonal matrix의 determinant는 $\pm1$이다. Identity quaternion $q=(1,\mathbf0)$에서는 $R(q)=I$이므로 determinant가 $1$이다. 앞에서 본 $q=(\cos\alpha,\mathbf u\sin\alpha)$에 대해

$$
q(t)
=
(\cos(t\alpha),\mathbf u\sin(t\alpha)),
\qquad
0\le t\le1
$$

로 두면 identity $q(0)=1$에서 $q(1)=q$까지 unit quaternion 안에서 연속적으로 이동하는 path를 얻는다. 따라서 $R(q(t))$와 그 determinant도 연속적으로 변한다. Determinant는 $1$과 $-1$ 사이를 연속적으로 이동할 수 없으므로 모든 unit quaternion에서 다음이 성립한다.

$$
\det R(q)=1.
$$

따라서 $R(q)\in SO(3)$다.

반대로 모든 $R\in SO(3)$의 rotation은 axis-angle로 나타낼 수 있으므로, 앞에서 유도한 $q(\mathbf u,\theta)$를 사용하면 그 rotation을 나타내는 unit quaternion을 얻는다. 다만 하나의 rotation matrix에는 정확히 두 unit quaternion $q$와 $-q$가 대응한다.

Rotation matrix에서 quaternion을 수치적으로 계산할 때는 matrix trace만 사용하는 단일 식보다 diagonal component의 크기에 따라 계산식을 선택하는 구현이 안정적이다. 특히 rotation angle이 $\pi$에 가까우면 scalar part $w$가 $0$에 가까워지므로 작은 $w$로 나누는 식은 피해야 한다.

## Rotation 표현으로 Unit Quaternion을 사용하는 이유

Rotation matrix는 vector에 바로 곱할 수 있고 geometric 의미가 명확하다. 하지만 아홉 component가 서로 독립적이지 않으며 다음 orthogonality와 determinant 조건을 계속 만족해야 한다.

$$
R^{\mathsf T}R=I,
\qquad
\det R=1.
$$

이 조건을 만족하는 rotation matrix의 집합 $SO(3)$은 [Rotation Matrix and SO(3)](<./22 Rotation Matrix and SO(3).md>)에서 자세히 설명한다.

Euler angle은 세 angle만으로 rotation을 나타내지만 axis와 적용 순서를 함께 정해야 한다. 또한 특정 자세에서는 서로 다른 angle 변화가 같은 instantaneous rotation 방향을 나타내는 coordinate singularity가 생긴다.

Unit quaternion은 네 component와 하나의 unit norm 조건으로 rotation을 나타낸다. Quaternion multiplication으로 rotation을 합성하고 conjugate로 inverse를 구할 수 있으며, 수치 오차로 norm이 달라졌을 때 normalization으로 unit norm을 복구할 수 있다. SLERP를 사용하면 두 orientation 사이도 일정한 angular speed로 보간할 수 있다.

대신 unit quaternion 표현은 유일하지 않다. $q$와 $-q$가 같은 rotation을 나타내므로 component 비교와 interpolation에서는 두 표현의 sign 동등성을 고려해야 한다.

따라서 quaternion은 rotation matrix를 대체해야 하는 유일한 표현이 아니다. Rotation matrix와 unit quaternion은 같은 rotation을 서로 다른 계산 구조로 나타내며, 필요한 연산과 data format에 따라 적합한 표현을 선택한다.

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

두 부호 외에 같은 rotation을 나타내는 다른 unit quaternion이 있는지도 확인해 보자. Unit quaternion $q,r$이 모든 vector에 같은 rotation을 만든다고 가정하면

$$
q\widehat p q^*
=
r\widehat p r^*
$$

이다. 양쪽에 $r^*$와 $r$을 곱하고 $s:=r^*q$라고 두면 다음을 얻는다.

$$
s\widehat p s^*=\widehat p.
$$

$s$도 unit quaternion이므로 오른쪽에 $s$를 곱하면 $s\widehat p=\widehat p s$다. $s=(a,\mathbf v)$로 두고 scalar-vector product를 사용하면 두 곱의 차이는 다음과 같다.

$$
s(0,\mathbf p)-(0,\mathbf p)s
=
(0,2\mathbf v\times\mathbf p).
$$

이 값이 모든 $\mathbf p$에서 $0$이 되려면 $\mathbf v=\mathbf0$이어야 한다. 또한 $s$가 unit quaternion이므로 $a=\pm1$이고, 따라서 $s=\pm1$이다. 즉, $r^*q=\pm1$에서 $q=\pm r$을 얻는다.

따라서 $q$와 $-q$는 quaternion으로서는 서로 다른 값이지만 rotation으로서는 같은 값을 나타내며, 같은 rotation을 나타내는 unit quaternion은 정확히 이 두 개다. Unit quaternion의 집합은 4차원 Euclidean space의 unit sphere $S^3$이며, 서로 반대편에 있는 두 점 $q$와 $-q$가 $SO(3)$의 같은 rotation에 대응한다. 이를 unit quaternion이 $SO(3)$를 `double cover`한다고 표현한다.

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

$t_{A\_B}$를 $A$ coordinate로 표현한 $B$ origin의 위치라고 하자. $[p]_B$는 $B$ origin에서 point $p$까지의 displacement이므로, 먼저 $R_{A\_B}[p]_B$로 이 displacement를 $A$ coordinate로 회전시킨다. 여기에 $A$에서 본 $B$ origin의 위치 $t_{A\_B}$를 더하면 point의 $A$ coordinate를 얻는다.

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

두 unit quaternion $q_0,q_1$은 $\mathbb R^4$의 unit sphere $S^3$ 위의 점이다. 두 rotation 사이를 일정한 angular speed로 보간하려면 $S^3$ 위에서 두 점을 잇는 shorter great-circle arc를 일정한 속도로 이동하면 된다. 이 경로에서 `spherical linear interpolation` 또는 `SLERP` 공식이 나온다.

먼저 두 quaternion의 4차원 dot product를 계산한다.

$$
d=q_0\cdot q_1.
$$

$d<0$이면 $q_1$을 $-q_1$로 바꾼다. 두 값은 같은 endpoint rotation을 나타내지만, $q_0$과 더 가까운 부호를 선택해야 $S^3$ 위의 shorter arc를 따라갈 수 있다.

$$
q_1\leftarrow -q_1,
\qquad
d\leftarrow-d.
$$

이제 $d\ge0$이고 두 quaternion이 서로 다른 일반적인 경우를 생각하자. 두 unit quaternion 사이의 $S^3$ angle을

$$
\Omega:=\arccos d
$$

라고 두면 $q_0\cdot q_1=\cos\Omega$다. $q_1$에서 $q_0$ 방향 component를 제거하고 normalize한 unit quaternion을 다음과 같이 둔다.

$$
n
:=
\frac{q_1-q_0\cos\Omega}{\sin\Omega}.
$$

$n$이 $q_0$와 수직인지 직접 계산하면 다음과 같다.

$$
q_0\cdot n
=
\frac{q_0\cdot q_1-\cos\Omega}{\sin\Omega}
=0.
$$

또한 numerator의 norm은 다음과 같다.

$$
\begin{aligned}
\lVert q_1-q_0\cos\Omega\rVert^2
&=1-2\cos\Omega(q_0\cdot q_1)+\cos^2\Omega \\
&=1-\cos^2\Omega \\
&=\sin^2\Omega.
\end{aligned}
$$

따라서 $\lVert n\rVert=1$이고, $q_0,n$은 두 quaternion이 놓인 2차원 plane의 orthonormal basis다. 이 plane의 unit circle을 따라 $q_0$에서 출발하는 경로를 다음과 같이 쓸 수 있다.

$$
q(t)
=
q_0\cos(t\Omega)+n\sin(t\Omega),
\qquad
0\le t\le1.
$$

$q_0$와 $n$이 orthonormal이므로 다음과 같이 unit norm과 두 endpoint를 직접 확인할 수 있다.

$$
\begin{aligned}
\lVert q(t)\rVert^2
&=\cos^2(t\Omega)+\sin^2(t\Omega)=1, \\
q(0)&=q_0, \\
q(1)&=q_0\cos\Omega+n\sin\Omega=q_1.
\end{aligned}
$$

경로를 미분하면

$$
q'(t)
=
\Omega\left(
-q_0\sin(t\Omega)+n\cos(t\Omega)
\right)
$$

이고, orthonormality에서 $\lVert q'(t)\rVert=\Omega$를 얻는다. 따라서 $S^3$ 위의 speed는 $t$와 무관하게 일정하다. 실제 3차원 rotation angle은 quaternion angle의 두 배이므로 실제 rotation의 angular speed도 일정하다.

$n$의 식을 $q(t)$에 대입하고

$$
\sin((1-t)\Omega)
=
\sin\Omega\cos(t\Omega)
-
\cos\Omega\sin(t\Omega)
$$

를 사용하면 $0\le t\le1$에서 다음 SLERP 공식을 얻는다.

$$
\operatorname{slerp}(q_0,q_1;t)
=
\frac{\sin((1-t)\Omega)}{\sin\Omega}q_0
+
\frac{\sin(t\Omega)}{\sin\Omega}q_1.
$$

$\Omega$가 $0$에 가까우면 두 quaternion이 거의 같아서 $\sin\Omega$도 매우 작다. 이 경우 위 수식은 같은 endpoint를 향하지만 작은 값으로 나누기 때문에 수치적으로 불안정해진다. 따라서 linear interpolation 결과를 normalize하는 `normalized linear interpolation`을 대신 사용할 수 있다.

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

- [Orthogonal Map](<../07 Linear Algebra/36 Orthogonal Map.md>)
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
