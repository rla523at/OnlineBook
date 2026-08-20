# Kernel

## 한 줄 요약

Kernel은 linear map이 zero vector로 보내는 모든 input의 subspace이며, map이 구분하지 못하는 direction을 나타낸다.

## Motivation

Linear map $T:V\rightarrow W$에서 서로 다른 input $v_1,v_2$가 같은 output을 만들면

$$
T(v_1)=T(v_2)
\iff
T(v_1-v_2)=0_W.
$$

따라서 map이 input을 구분하지 못하는 원인은 zero로 사라지는 difference direction에 있다. 이 direction들을 모은 집합이 kernel이다.

## Definition

Vector spaces $V,W/\F$와 $T\in L(V,W)$가 있다고 하자. $T$의 `kernel`을

$$
\ker T
:=
\{v\in V\mid T(v)=0_W\}
$$

로 정의한다.

## Kernel Is a Subspace

### 정리1

$$
\ker T\le V.
$$

**Proof**

Linear map은 zero vector를 zero vector로 보내므로 $0_V\in\ker T$다. $u,v\in\ker T$와 $a,b\in\F$에 대해

$$
T(au+bv)
=
aT(u)+bT(v)
=
0_W.
$$

따라서 $au+bv\in\ker T$이고 subspace test에 의해 $\ker T\le V$다. $\qed$

이 결과에는 finite-dimensional 가정이 필요하지 않다.

## Kernel and Injectivity

### 정리2

$$
T\text{ is injective}
\iff
\ker T=\{0_V\}.
$$

**Proof**

$T$가 injective이면 $T(v)=0_W=T(0_V)$를 만족하는 $v$는 $0_V$뿐이므로 $\ker T=\{0_V\}$다.

반대로 $\ker T=\{0_V\}$라고 하자. $T(v_1)=T(v_2)$이면

$$
T(v_1-v_2)=0_W,
$$

따라서 $v_1-v_2\in\ker T$다. 전제에 의해 $v_1-v_2=0_V$이므로 $v_1=v_2$다. 따라서 $T$는 injective다. $\qed$

Kernel이 nonzero vector를 포함하면 그 direction으로 input을 바꾸어도 output이 달라지지 않는다. 실제로 $k\in\ker T$이면

$$
T(v+k)=T(v)+T(k)=T(v).
$$

## Nullity

$V$가 finite-dimensional이면 kernel의 dimension을 $T$의 `nullity`라고 한다.

$$
\operatorname{nullity}(T)
:=
\dim\ker T.
$$

Nullity는 $T$가 zero로 보내는 independent direction의 수를 나타낸다. Image의 dimension인 rank와의 관계는 [Image](<12 Image.md>)의 rank-nullity theorem에서 다룬다.

## Example

Projection

$$
T:\R^3\rightarrow\R^2,
\qquad
T(x,y,z):=(x,y)
$$

에 대해

$$
\ker T
=
\{(0,0,z)\mid z\in\R\}
=
\operatorname{span}\{(0,0,1)\}.
$$

$z$-direction의 변화는 output에 나타나지 않으며 $\operatorname{nullity}(T)=1$이다.

## 관련 문서

- [Linear Map](<10 Linear Map.md>)
- [Image](<12 Image.md>)
- [Linear Systems and Row Reduction](<21 Linear Systems and Row Reduction.md>)
