# Norm, Distance and Angle

## 한 줄 요약

Inner product에 같은 vector 또는 서로 다른 두 vector를 넣어 norm, distance와 angle을 정의하면 vector space에 Euclidean-type geometry가 생긴다.

## Motivation

[Inner Product Space](<30 Inner Product Space.md>)에서 정의한 inner product는 vector의 squared length와 두 vector 사이의 관계를 하나의 함수로 표현한다. 이 정보에서 length를 나타내는 norm, 두 point 사이의 distance, 두 방향 사이의 angle과 Pythagorean theorem이 어떻게 나오는지 차례로 살펴본다.

## Norm

Inner product에 같은 vector를 두 번 넣은 $B(v,v)$는 nonnegative real number이며 $v=0_V$일 때만 $0$이다. 따라서 그 square root를 vector의 length로 사용할 수 있다. Inner product로부터 유도되는 `norm`을 다음과 같이 정의한다.

$$
\lVert\cdot\rVert:V\rightarrow\R_{\ge0},
\qquad
\lVert v\rVert:=\sqrt{B(v,v)}.
$$

Norm은 vector 하나의 size를 측정하며 $\lVert v\rVert=0$과 $v=0_V$가 동치가 되도록 한다.

## Distance

두 vector 사이의 displacement는 $v-w$이므로 그 norm을 사용해 `distance`를 정의한다.

$$
d:V\times V\rightarrow\R_{\ge0},
\qquad
d(v,w):=\lVert v-w\rVert.
$$

즉, inner product는 먼저 norm을 유도하고 norm은 다시 distance를 유도한다.

## Angle

이 절에서는 real inner product space $V/\R$를 생각하자. 두 nonzero vector $v,w\in V$ 사이의 `angle` $\theta\in[0,\pi]$를 다음 관계로 정의한다.

$$
\cos\theta
=
\frac{B(v,w)}{\lVert v\rVert\lVert w\rVert}.
$$

Cauchy–Schwarz inequality

$$
\lvert B(v,w)\rvert
\le
\lVert v\rVert\lVert w\rVert
$$

에 의해 오른쪽 비율이 $[-1,1]$에 있으므로 $\theta$가 정의된다. Complex inner product space에서는 $B(v,w)$가 complex number일 수 있어 이 식을 그대로 angle 정의로 사용할 수 없으며, 목적에 따라 별도의 convention이 필요하다.

한편 $\F=\R$ 또는 $\C$인지와 관계없이 $B(v,w)=0_\F$이면 $v$와 $w$가 `orthogonal`(직교)하다고 정의하고 다음과 같이 표기한다.

$$
v\perp w.
$$

### 정리1 (Pythagorean theorem)

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
