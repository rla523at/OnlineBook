# Image

## 한 줄 요약

Image는 linear map이 만들 수 있는 모든 output의 subspace이고, rank-nullity theorem은 가능한 output direction과 사라지는 input direction의 수를 연결한다.

## Motivation

[Kernel](<11 Kernel.md>)에서는 linear map $T:V\rightarrow W$를 고정하고, 서로 다른 input이 같은 output을 만드는 원인을 zero로 사라지는 input direction에서 찾았다. 이제 같은 $T$를 고정한 채 target $w\in W$를 변화시키면서 어떤 target이 실제 output으로 나타나는지 묻자.

Equation

$$
T(v)=w
$$

가 solution을 갖는 target 전체는

$$
\{T(v)\mid v\in V\}
$$

이다. Codomain $W$의 모든 vector가 이 집합에 속할 필요는 없다. 이 reachable output들의 집합을 하나의 대상으로 다루면 solution이 존재하는 target을 membership으로 판별할 수 있다.

## Definition

$T\in L(V,W)$의 `image`를

$$
\operatorname{im}T
:=
T(V)
=
\{T(v)\mid v\in V\}
$$

로 정의한다.

## Image Is a Subspace

### 정리1

$$
\operatorname{im}T\le W.
$$

**Proof**

$T(0_V)=0_W$이므로 $0_W\in\operatorname{im}T$다. $w_1,w_2\in\operatorname{im}T$이면 어떤 $v_1,v_2\in V$에 대해 $w_i=T(v_i)$다. 모든 $a,b\in\F$에 대해

$$
aw_1+bw_2
=
aT(v_1)+bT(v_2)
=
T(av_1+bv_2)
\in\operatorname{im}T.
$$

Subspace test에 의해 $\operatorname{im}T\le W$다. $\qed$

$T$가 surjective인 것은

$$
\operatorname{im}T=W
$$

인 것과 같다.

## Rank

Image의 dimension을 $T$의 `rank`라고 한다.

$$
\operatorname{rank}(T)
:=
\dim\operatorname{im}T.
$$

Finite-dimensional case에서 rank는 $T$가 만들 수 있는 independent output direction의 수다.

## A Basis of the Image

### 정리2

$V$가 finite-dimensional이라고 하자. Kernel의 basis

$$
(\kappa_1,\ldots,\kappa_p)
$$

를 $V$의 basis

$$
(\kappa_1,\ldots,\kappa_p,v_1,\ldots,v_r)
$$

로 확장하면

$$
(T(v_1),\ldots,T(v_r))
$$

는 $\operatorname{im}T$의 basis다.

**Proof**

임의의 $x\in V$는

$$
x
=
\sum_{i=1}^p b_i\kappa_i
+
\sum_{j=1}^r a_jv_j
$$

로 표현된다. $\kappa_i\in\ker T$이므로

$$
T(x)
=
\sum_{j=1}^r a_jT(v_j).
$$

따라서 $T(v_1),\ldots,T(v_r)$는 image를 span한다.

이 vector들의 linear combination이 zero라고 하자.

$$
\sum_{j=1}^r a_jT(v_j)=0_W.
$$

그러면

$$
T\left(\sum_{j=1}^r a_jv_j\right)=0_W
$$

이므로 $\sum_j a_jv_j\in\ker T$다. 따라서 어떤 $b_i\in\F$에 대해

$$
\sum_{j=1}^r a_jv_j
=
\sum_{i=1}^p b_i\kappa_i.
$$

양변을 한쪽으로 옮기면 $V$의 basis에 대한 linear relation이 된다. Basis가 linearly independent이므로 모든 $a_j$와 $b_i$가 zero다. 따라서 $T(v_1),\ldots,T(v_r)$는 linearly independent하고 image의 basis다. $\qed$

## Rank-Nullity Theorem

### 정리3

Finite-dimensional vector space $V$와 linear map $T:V\rightarrow W$에 대해

$$
\boxed{
\dim V
=
\operatorname{nullity}(T)
+
\operatorname{rank}(T)
}
$$

가 성립한다.

**Proof**

정리2의 notation에서

$$
\dim V=p+r,
$$

$$
p=\dim\ker T=\operatorname{nullity}(T),
$$

$$
r=\dim\operatorname{im}T=\operatorname{rank}(T)
$$

이므로 식이 성립한다. $\qed$

이 식은 domain의 independent direction들이 두 종류로 나뉜다는 뜻이다. Kernel basis direction들은 output에서 사라지고, 나머지 basis direction들의 image는 가능한 output의 basis가 된다.

## Consequences

Finite-dimensional vector spaces $V,W$가 같은 dimension을 갖고 $T:V\rightarrow W$가 linear라고 하자. Rank-nullity theorem에 의해 다음 조건은 동치다.

$$
T\text{ is injective}
\iff
T\text{ is surjective}
\iff
T\text{ is bijective}.
$$

Injective이면 nullity가 $0$이므로 rank가 $\dim V=\dim W$다. Image는 $W$와 같은 dimension을 갖는 subspace이므로 $W$ 전체다. 반대로 surjective이면 rank가 $\dim W=\dim V$이므로 nullity가 $0$이고 injective다.

또한 $T$가 injective이고 $\beta$가 $V$의 basis이면 $T(\beta)$는 $\operatorname{im}T$의 basis다. $T$가 bijective이면 $\operatorname{im}T=W$이므로 $T(\beta)$는 $W$의 basis다.

## 관련 문서

- [Kernel](<11 Kernel.md>)
- [Vector Space Isomorphism](<13 Vector Space Isomorphism.md>)
- [Column Space, Row Space, and Rank](<../04 Linear Systems and Rank/22 Column Space, Row Space, and Rank.md>)
