# Norm, Distance and Angle

## 한 줄 요약

Inner product에 같은 vector 또는 서로 다른 두 vector를 넣어 norm, distance와 angle을 정의하면 vector space에 Euclidean-type geometry가 생긴다.

## Norm

### Motivation

[Inner Product Space](<30 Inner Product Space.md>)에서 $B(v,v)$는 vector $v$의 squared length 역할을 한다고 보았다. 그렇다면 length 자체는 왜 $B(v,v)$가 아니라 그 square root여야 하는지부터 생각해 보자. Vector를 scalar $c$배하면

$$
B(cv,cv)
=
c\overline c B(v,v)
=
\lvert c\rvert^2B(v,v)
$$

이므로 squared length는 $\lvert c\rvert^2$배가 된다. 반면 length는 vector를 늘인 비율인 $\lvert c\rvert$배가 되어야 자연스럽다. 실제로 square root를 취하면

$$
\sqrt{B(cv,cv)}
=
\lvert c\rvert\sqrt{B(v,v)}
$$

가 되어 scalar multiplication과 기대한 방식으로 맞는다. 따라서 $B(v,v)$의 square root를 vector의 length로 사용하는 것이 자연스럽다.

### Definition

Inner product space $V/\F$에서 inner product $B$가 유도하는 norm을 다음 함수로 정의한다.

$$
\lVert\cdot\rVert:V\rightarrow\R_{\ge0},
\qquad
\lVert v\rVert:=\sqrt{B(v,v)}.
$$

이 함수 $\lVert\cdot\rVert$를 inner product $B$에서 유도된 `norm`이라고 한다. Vector $v$에 대한 함수값 $\lVert v\rVert$는 $v$의 norm이며, inner product $B$가 정하는 geometry에서 $v$의 length를 나타낸다.

즉, `norm`은 vector의 length를 측정하는 함수의 수학적 이름이고, $\lVert v\rVert$는 그 함수가 $v$에 부여한 length다.

### Basic Properties

Inner product의 성질에 의해 induced norm은 모든 $v,w\in V$와 $c\in\F$에 대해 다음을 만족한다.

$$
\lVert v\rVert\ge0,
\qquad
\lVert v\rVert=0\iff v=0_V,
\qquad
\lVert cv\rVert=\lvert c\rvert\lVert v\rVert.
$$

## Cauchy–Schwarz Inequality

### Motivation

Norm이 vector 하나의 length를 측정하더라도, 두 vector의 inner product가 각 norm에 비해 얼마나 커질 수 있는지는 아직 알 수 없다. 이 관계를 제어해야 norm이 triangle inequality를 만족함을 보일 수 있고, 뒤에서 angle을 정의할 때 사용하는 비율이 허용된 범위 안에 있음을 보장할 수 있다. Cauchy–Schwarz inequality는 inner product의 absolute value를 두 norm의 product로 bound하여 이 두 문제를 해결한다.

### 정리1 (Cauchy–Schwarz inequality)

$\F\in\{\R,\C\}$인 inner product space $V/\F$에서 모든 $v,w\in V$에 대해 다음 부등식이 성립한다.

$$
\lvert B(v,w)\rvert
\le
\lVert v\rVert\lVert w\rVert.
$$

**Proof**

$w=0_V$이면 $B(v,w)=0_\F$이므로 부등식이 성립한다. 이제 $w\ne0_V$라 하자. Positive definiteness에 의해 $B(w,w)=\lVert w\rVert^2>0$이므로 다음 scalar와 vector를 정의할 수 있다.

$$
\lambda
:=
\frac{B(v,w)}{\lVert w\rVert^2},
\qquad
u
:=
v-\lambda w.
$$

이 문서의 convention에서 $B$는 첫 번째 argument에 대해 linear이고 두 번째 argument에 대해 conjugate linear하다. 따라서 conjugate symmetry $B(w,v)=\overline{B(v,w)}$와 $B(w,w)=\lVert w\rVert^2$을 사용하면 다음을 얻는다.

$$
\begin{aligned}
\lVert u\rVert^2
&=B(v-\lambda w,v-\lambda w)\\
&=B(v,v)
-\overline\lambda B(v,w)
-\lambda B(w,v)
+\lvert\lambda\rvert^2B(w,w)\\
&=\lVert v\rVert^2
-\frac{\lvert B(v,w)\rvert^2}{\lVert w\rVert^2}.
\end{aligned}
$$

Positive definiteness에 의해 $\lVert u\rVert^2\ge0$이므로

$$
\lvert B(v,w)\rvert^2
\le
\lVert v\rVert^2\lVert w\rVert^2
$$

이다. 양변이 nonnegative이므로 square root를 취하면 다음을 얻는다.

$$
\lvert B(v,w)\rvert
\le
\lVert v\rVert\lVert w\rVert.
\qed
$$

### 따름정리1 (Triangle inequality)

모든 $v,w\in V$에 대해 다음 부등식이 성립한다.

$$
\lVert v+w\rVert
\le
\lVert v\rVert+\lVert w\rVert.
$$

**Proof**

Conjugate symmetry와 Cauchy–Schwarz inequality를 사용하면 다음을 얻는다.

$$
\begin{aligned}
\lVert v+w\rVert^2
&=\lVert v\rVert^2
  +2\operatorname{Re}B(v,w)
  +\lVert w\rVert^2\\
&\le\lVert v\rVert^2
  +2\lvert B(v,w)\rvert
  +\lVert w\rVert^2\\
&\le\lVert v\rVert^2
  +2\lVert v\rVert\lVert w\rVert
  +\lVert w\rVert^2\\
&=(\lVert v\rVert+\lVert w\rVert)^2.
\end{aligned}
$$

양변이 nonnegative이므로 square root를 취하면 triangle inequality를 얻는다. $\qed$

## Distance

### Motivation

Norm은 하나의 vector의 크기를 측정한다. 그렇다면 두 vector $v,w$가 서로 얼마나 다른지는 어떻게 측정할 수 있을까? Vector space에서는 subtraction이 정의되어 있으므로 두 vector의 discrepancy를 difference vector $v-w$로 나타낼 수 있다. 또한

$$
v-w=0_V
\iff
v=w
$$

이므로 두 vector가 일치하는지는 difference vector가 zero vector인지로 판별된다. Difference vector는 순서를 바꾸면 sign만 달라지므로

$$
\lVert w-v\rVert
=
\lVert -(v-w)\rVert
=
\lVert v-w\rVert
$$

이고, 같은 vector $u$를 두 vector에 더해도

$$
\lVert(v+u)-(w+u)\rVert
=
\lVert v-w\rVert
$$

이다. 따라서 difference vector의 norm은 두 vector의 공통된 성분이 아니라 서로의 차이만 측정한다.

### Definition

Inner product space $V/\F$에서 induced norm을 사용해 `distance`를 다음 함수로 정의한다.

$$
d:V\times V\rightarrow\R_{\ge0},
\qquad
d(v,w):=\lVert v-w\rVert.
$$

즉, inner product는 먼저 norm을 유도하고 norm은 다시 distance를 유도한다.

## Angle in Real Inner Product Space

### Motivation

Angle은 vector의 크기가 아니라 두 방향 사이의 관계를 나타내야 하며, familiar Euclidean geometry의 cosine law와 호환되어야 한다. Real inner product space에서 앞서 정의한 norm을 사용해 $\lVert v-w\rVert^2$을 전개하면

$$
\begin{aligned}
\lVert v-w\rVert^2
&=B(v-w,v-w)\\
&=\lVert v\rVert^2+\lVert w\rVert^2-2B(v,w).
\end{aligned}
$$

한편 두 nonzero vector 사이의 angle을 $\theta$라고 할 때 Euclidean cosine law는 같은 양을 다음과 같이 표현한다.

$$
\lVert v-w\rVert^2
=
\lVert v\rVert^2+\lVert w\rVert^2
-2\lVert v\rVert\lVert w\rVert\cos\theta.
$$

두 식이 같은 geometry를 나타내려면

$$
\cos\theta
=
\frac{B(v,w)}{\lVert v\rVert\lVert w\rVert}
$$

여야 한다. Denominator로 두 vector의 length를 나누었으므로 이 비율은 vector를 positive scalar배해도 변하지 않고 방향 사이의 관계만 남긴다.

### Definition

Real inner product space $V/\R$에서 두 nonzero vector $v,w\in V$ 사이의 `angle` $\theta\in[0,\pi]$를 다음 관계로 정의한다.

$$
\cos\theta
=
\frac{B(v,w)}{\lVert v\rVert\lVert w\rVert}.
$$

Cauchy–Schwarz inequality를 real case에 적용하면

$$
-\lVert v\rVert\lVert w\rVert
\le
B(v,w)
\le
\lVert v\rVert\lVert w\rVert
$$

이므로 오른쪽 비율이 $[-1,1]$에 있고 $\theta$가 정의된다. Complex inner product space에서는 $B(v,w)$가 complex number일 수 있어 이 식을 그대로 angle 정의로 사용할 수 없으며, 목적에 따라 별도의 convention이 필요하다.

## Orthogonality

### Motivation

Real inner product space에서 두 nonzero vector의 angle이 $\pi/2$이면 $\cos\theta=0$이므로 angle 공식에 의해 $B(v,w)=0$이다. 이 조건은 complex inner product space에서도 그대로 표현할 수 있으며, norm을 전개할 때 서로 다른 두 vector 사이의 cross term을 없앤다. 따라서 inner product가 $0$인 관계를 right angle의 개념을 확장한 것으로 사용하는 것이 자연스럽다.

### Definition

Inner product space $V/\F$에서는 $\F=\R$ 또는 $\C$인지와 관계없이 $B(v,w)=0_\F$이면 $v$와 $w$가 `orthogonal`(직교)하다고 정의하고 다음과 같이 표기한다.

$$
v\perp w.
$$

### 정리2 (Pythagorean theorem)

Inner product space $V/\F$와 $v,w\in V$가 있다고 하자. 두 vector가 orthogonal이면 다음이 성립한다.

$$ v \perp w \implies \norm{v+w}^2 = \norm{v}^2 + \norm{w}^2 $$

**Proof**

$v\perp w$이므로 $B(v,w)=B(w,v)=0_\F$이다.

따라서, 다음이 성립한다.

$$ \begin{aligned}
\norm{v+w}^2 &= B(v+w,v+w) \\
&= B(v,v) + B(v,w)+ B(w,v) + B(w,w) \\
&= B(v,v) + B(w,w) \\
&= \norm{v}^2 + \norm{w}^2 \qed
\end{aligned}$$
