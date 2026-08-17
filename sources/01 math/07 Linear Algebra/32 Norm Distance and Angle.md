# Norm, Distance and Angle

## 한 줄 요약

Inner product에 같은 vector 또는 서로 다른 두 vector를 넣어 norm, distance와 angle을 정의하면 vector space에 Euclidean-type geometry가 생긴다.

## Motivation

[Inner Product Space](<30 Inner Product Space.md>)에서 $B(v,v)$는 vector $v$의 squared length 역할을 한다고 보았다. 그렇다면 length 자체는 왜 $B(v,v)$가 아니라 그 square root여야 하는지부터 생각해 보자. Vector를 scalar $c$배하면

$$
B(cv,cv)
=
c\overline c B(v,v)
=
\lvert c\rvert^2B(v,v)
$$

이므로 squared length는 $\lvert c\rvert^2$배가 된다. 반면 length는 vector를 늘인 비율인 $\lvert c\rvert$배가 되어야 자연스럽다. 따라서 square root를 취한

$$
\lVert v\rVert
:=
\sqrt{B(v,v)}
$$

를 length로 사용하면 $\lVert cv\rVert=\lvert c\rvert\lVert v\rVert$가 되어 scalar multiplication과 기대한 방식으로 맞는다. 이것이 inner product로부터 norm을 정의하는 이유다.

두 point 사이의 distance는 원점을 어디에 잡았는지와 무관해야 한다. $v$와 $w$를 같은 vector $u$만큼 옮겨도 두 point 사이의 차이는

$$
(v+u)-(w+u)=v-w
$$

로 변하지 않는다. 따라서 두 point의 relative displacement $v-w$의 length를 재는

$$
d(v,w)
:=
\lVert v-w\rVert
$$

가 자연스러운 distance가 된다. 이 정의는 두 point를 함께 translation해도 값을 보존하고, $v=w$일 때만 distance가 $0$이 되게 한다.

마지막으로 angle은 vector의 크기가 아니라 두 방향 사이의 관계를 나타내야 하며, familiar Euclidean geometry의 cosine law를 유지해야 한다. Real inner product space에서 위의 norm을 사용해 $\lVert v-w\rVert^2$을 전개하면

$$
\begin{aligned}
\lVert v-w\rVert^2
&=B(v-w,v-w) \\
&=\lVert v\rVert^2+\lVert w\rVert^2-2B(v,w).
\end{aligned}
$$

한편 Euclidean cosine law는 두 nonzero vector 사이의 angle을 $\theta$라고 할 때 같은 양을 다음과 같이 표현한다.

$$
\lVert v-w\rVert^2
=
\lVert v\rVert^2+\lVert w\rVert^2
-2\lVert v\rVert\lVert w\rVert\cos\theta.
$$

두 식이 같은 geometry를 나타내게 하려면 반드시

$$
\cos\theta
=
\frac{B(v,w)}{\lVert v\rVert\lVert w\rVert}
$$

로 두어야 한다. Denominator로 두 vector의 length를 나누었으므로 이 비율은 vector의 크기를 positive scalar배해도 변하지 않고 방향 사이의 관계만 남긴다. 따라서 norm, distance와 angle의 정의는 임의로 선택한 공식이 아니라, inner product가 담고 있는 squared length와 pairwise relation을 기존 Euclidean geometry의 scaling, translation과 cosine law에 맞게 해석한 결과다.

## Norm

Motivation에서 얻은 length 측정법을 다음 함수로 정의하자.

$$
\lVert\cdot\rVert:V\rightarrow\R_{\ge0},
\qquad
\lVert v\rVert:=\sqrt{B(v,v)}.
$$

이 함수 $\lVert\cdot\rVert$를 inner product $B$에서 유도된 `norm`이라고 한다. Vector $v$에 대한 함수값 $\lVert v\rVert$는 $v$의 norm이며, inner product $B$가 정하는 geometry에서 $v$의 length를 나타낸다.

즉, `norm`은 vector의 length를 측정하는 함수의 수학적 이름이고, $\lVert v\rVert$는 그 함수가 $v$에 부여한 length다.

Inner product의 성질에 의해 induced norm은 모든 $v,w\in V$와 $c\in\F$에 대해 다음을 만족한다.

$$
\lVert v\rVert\ge0,
\qquad
\lVert v\rVert=0\iff v=0_V,
\qquad
\lVert cv\rVert=\lvert c\rvert\lVert v\rVert.
$$

또한 Cauchy–Schwarz inequality로부터 triangle inequality가 성립한다.

$$
\lVert v+w\rVert
\le
\lVert v\rVert+\lVert w\rVert.
$$

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
