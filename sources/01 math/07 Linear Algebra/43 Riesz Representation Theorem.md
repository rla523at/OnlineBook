# Riesz Representation Theorem

## 한 줄 요약

Finite-dimensional inner product space의 모든 linear functional은 어떤 vector와의 inner product로 유일하게 표현되며, 이 correspondence가 dual map과 adjoint를 연결한다.

## Motivation

[Dual Space](<42 Dual Space.md>)의 linear functional $g\in V^*$는 vector를 scalar로 측정한다. 한편 [Inner Product Space](<30 Inner Product Space.md>)의 inner product에서 두 번째 argument를 $v$로 고정한 함수

$$
B(\mathord{\cdot},v):V\rightarrow\F
$$

도 첫 번째 argument에 대해 linear하므로 linear functional이다.

그렇다면 반대로 모든 linear functional $g$를

$$
g(x)=B(x,v_g)
$$

형태로 표현할 수 있는지 묻게 된다. 가능하다면 abstract function $g$를 하나의 concrete vector $v_g$로 다룰 수 있다. Finite-dimensional inner product space에서는 Riesz representation theorem이 이러한 vector의 existence와 uniqueness를 보장한다.

## Riesz Representation Theorem

### 정리1

$\F\in\{\R,\C\}$인 finite-dimensional inner product space $V/\F$가 있다고 하자. 모든 $g\in V^*$에 대해 다음을 만족하는 unique vector $v_g\in V$가 존재한다.

$$
g(x)
=
B(x,v_g)
\qquad
\text{for every }x\in V.
$$

**Proof**

[Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)에 의해 $V$의 orthonormal basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

를 선택할 수 있다. 다음 vector를 정의하자.

$$
v_g
:=
\sum_{i=1}^n
\overline{g(\beta_i)}\beta_i.
$$

임의의 $x\in V$를

$$
x
=
\sum_{i=1}^n a_i\beta_i
$$

라고 쓰면, 첫 번째 argument의 linearity와 두 번째 argument의 conjugate linearity에 의해

$$
\begin{aligned}
B(x,v_g)
&=
B\left(
\sum_{i=1}^n a_i\beta_i,
\sum_{j=1}^n\overline{g(\beta_j)}\beta_j
\right)\\
&=
\sum_{i=1}^n\sum_{j=1}^n
a_i g(\beta_j)B(\beta_i,\beta_j)\\
&=
\sum_{i=1}^n a_i g(\beta_i)\\
&=
g(x).
\end{aligned}
$$

따라서 existence가 성립한다.

이제 $u,v\in V$가 모든 $x\in V$에 대해

$$
B(x,u)=B(x,v)
$$

를 만족한다고 하자. 그러면 $B(x,u-v)=0_\F$이고, 특히 $x=u-v$로 두면

$$
B(u-v,u-v)=0_\F.
$$

Positive definiteness에 의해 $u-v=0_V$, 즉 $u=v$다. 따라서 representation은 unique하다. $\qed$

Vector $v_g$를 $g$의 `Riesz representation`이라고 한다. 이 vector는 basis를 사용해 구성했지만 uniqueness에 의해 최종 결과는 선택한 orthonormal basis에 의존하지 않는다.

Infinite-dimensional inner product space에서는 같은 결론에 completeness와 functional의 continuity 같은 추가 조건이 필요하다. 이 문서에서는 finite-dimensional case만 다룬다.

## Coordinate Formula

Arbitrary basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

를 사용해 Riesz representation의 coordinate를 계산해 보자. Gram matrix와 coordinate column을

$$
G_{ij}:=B(\beta_i,\beta_j),
\qquad
a:=[v_g]_\beta,
\qquad
f_i:=g(\beta_i)
$$

라고 하자. 각 basis vector에 theorem의 식을 적용하면

$$
f_i
=
g(\beta_i)
=
B(\beta_i,v_g)
=
\sum_{j=1}^n
G_{ij}\overline{a_j}.
$$

따라서 matrix form은

$$
f=G\overline a
$$

이고,

$$
a
=
\overline{G^{-1}f}
$$

를 얻는다. $G$는 positive-definite Gram matrix이므로 invertible하다.

Basis가 orthonormal이면 $G=I$이고 공식이

$$
[v_g]_\beta
=
\overline f
$$

로 단순해진다. 이는 theorem의 proof에서 사용한

$$
v_g
=
\sum_{i=1}^n
\overline{g(\beta_i)}\beta_i
$$

와 같다.

## Riesz Map

Inner product $B$가 정하는 `Riesz map`을 다음과 같이 정의하자.

$$
\mathcal R_B:V\rightarrow V^*,
\qquad
\mathcal R_B(v)
:=
B(\mathord{\cdot},v).
$$

Riesz representation theorem의 existence는 $\mathcal R_B$가 surjective임을, uniqueness는 injective임을 뜻한다. 따라서 $\mathcal R_B$는 bijective다.

다만 scalar field에 따라 linearity가 달라진다.

- $\F=\R$이면
  $$
  \mathcal R_B(av+bw)
  =
  a\mathcal R_B(v)+b\mathcal R_B(w)
  $$
  이므로 real vector space isomorphism이다.
- $\F=\C$이면
  $$
  \mathcal R_B(av+bw)
  =
  \overline a\,\mathcal R_B(v)
  +
  \overline b\,\mathcal R_B(w)
  $$
  이므로 conjugate-linear bijection이다.

이 차이는 이 문서가 inner product의 첫 번째 argument를 linear, 두 번째 argument를 conjugate linear로 정했기 때문에 생긴다. 따라서 complex case에서 $\mathcal R_B$를 complex-linear isomorphism이라고 부르면 안 된다.

### 따름정리1 (Dual norm)

Linear functional $g\in V^*$의 dual norm을

$$
\lVert g\rVert_*
:=
\sup_{x\ne0_V}
\frac{\lvert g(x)\rvert}{\lVert x\rVert}
$$

로 정의하면 다음이 성립한다.

$$
\lVert g\rVert_*
=
\lVert v_g\rVert.
$$

**Proof**

Cauchy–Schwarz inequality에 의해

$$
\lvert g(x)\rvert
=
\lvert B(x,v_g)\rvert
\le
\lVert x\rVert\lVert v_g\rVert
$$

이므로 $\lVert g\rVert_*\le\lVert v_g\rVert$다. $v_g\ne0_V$이면 $x=v_g/\lVert v_g\rVert$를 대입했을 때 equality가 성립한다. $v_g=0_V$이면 $g=0_{V^*}$이므로 양변이 모두 $0$이다. $\qed$

## Adjoint

### Motivation

Finite-dimensional inner product space $V,W$ 사이의 linear map

$$
T:V\rightarrow W
$$

와 $y\in W$를 고정하면

$$
x\longmapsto B_W(Tx,y)
$$

는 $V$ 위의 linear functional이다. Riesz representation theorem에 의해 이 functional을 $V$의 unique vector로 표현할 수 있다. 이 vector를 $T^*y$라고 두면 $T$의 작용을 inner product의 다른 argument로 옮길 수 있다.

### Definition

$T$의 `adjoint`는 다음 관계를 만족하는 unique linear map이다.

$$
T^*:W\rightarrow V,
\qquad
B_W(Tx,y)
=
B_V(x,T^*y)
$$

for every $x\in V$, $y\in W$.

각 $y$에 대한 existence와 uniqueness는 Riesz representation theorem에서 바로 따라온다. 또한 $a,b\in\F$, $y,z\in W$에 대해

$$
\begin{aligned}
B_V(x,T^*(ay+bz))
&=
B_W(Tx,ay+bz)\\
&=
\overline a B_W(Tx,y)
+
\overline b B_W(Tx,z)\\
&=
B_V(x,aT^*y+bT^*z)
\end{aligned}
$$

이고 representation이 unique하므로

$$
T^*(ay+bz)
=
aT^*y+bT^*z.
$$

따라서 $T^*$는 linear map이다.

### Matrix representation

$V,W$의 orthonormal basis를 각각 $\beta,\gamma$라고 하고

$$
A:=[T]_\beta^\gamma
$$

라고 하자. Adjoint의 matrix는 conjugate transpose다.

$$
[T^*]_\gamma^\beta
=
A^{\mathsf *}
:=
\overline A^{\mathsf T}.
$$

**Proof**

$C:=[T^*]_\gamma^\beta$라고 하자. $x\in V$, $y\in W$의 coordinate column을 각각 $a,b$라고 하면 orthonormal basis에서 inner product는 다음과 같이 계산된다.

$$
\begin{aligned}
B_W(Tx,y)
&=
(Aa)^{\mathsf T}\overline b\\
&=
a^{\mathsf T}A^{\mathsf T}\overline b,
\end{aligned}
$$

$$
\begin{aligned}
B_V(x,T^*y)
&=
a^{\mathsf T}\overline{Cb}\\
&=
a^{\mathsf T}\overline C\,\overline b.
\end{aligned}
$$

Adjoint의 정의에 의해 두 식이 모든 $a,b$에 대해 같으므로

$$
A^{\mathsf T}
=
\overline C.
$$

따라서

$$
C
=
\overline A^{\mathsf T}
=
A^{\mathsf *}.
\qed
$$

Real case에서는 complex conjugation이 사라져

$$
[T^*]_\gamma^\beta
=
A^{\mathsf T}
$$

가 된다.

### Dual map과의 관계

[Dual Space](<42 Dual Space.md>)에서 정의한 dual map

$$
T^\vee:W^*\rightarrow V^*
$$

은 inner product 없이 functional을 precomposition으로 pull back한다. Riesz map을 사용하면 adjoint와 dual map의 관계를 다음 식으로 나타낼 수 있다.

$$
\boxed{
\mathcal R_{B_V}\circ T^*
=
T^\vee\circ\mathcal R_{B_W}
}
$$

실제로 $y\in W$에 대해 양쪽은 모두 $V$ 위의 functional

$$
x\longmapsto B_W(Tx,y)
$$

를 나타낸다. 즉 adjoint는 dual map을 Riesz representation을 통해 원래 vector space 사이의 map으로 옮긴 것이다.

Adjoint는 [Schur's Theorem](<39 Schur's Theorem.md>)에서 orthogonal complement가 invariant한 subspace를 만드는 데 사용된다.

## 관련 문서

- [Inner Product Space](<30 Inner Product Space.md>)
- [Gram Matrix](<31 Gram Matrix.md>)
- [Norm, Distance and Angle](<32 Norm Distance and Angle.md>)
- [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)
- [Dual Space](<42 Dual Space.md>)
