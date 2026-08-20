# Dual Space

## 한 줄 요약

Dual space $V^*$는 vector를 scalar로 측정하는 모든 linear functional의 vector space이며, basis가 주어지면 coordinate를 추출하는 dual basis를 갖는다.

## Motivation

Vector $v\in V$ 자체는 direction과 magnitude를 나타내지만, 계산에서는 $v$에서 특정 coordinate나 linear quantity를 읽어 내는 함수도 필요하다. 예를 들어 basis

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

에 대해

$$
v=a_1\beta_1+\cdots+a_n\beta_n
$$

라고 썼을 때 $v\mapsto a_i$는 vector에서 $i$번째 coordinate를 추출한다. 이 함수는 vector addition과 scalar multiplication을 보존한다. 이처럼 vector를 scalar로 보내면서 linear structure를 보존하는 함수를 linear functional이라고 한다. Linear functional들을 다시 addition과 scalar multiplication이 가능한 하나의 vector space로 모은 것이 dual space다.

## Dual Space

### Definition

Vector space $V/\F$에서 scalar-valued function

$$
f:V\rightarrow\F
$$

가 모든 $v,w\in V$와 $a,b\in\F$에 대해

$$
f(av+bw)=af(v)+bf(w)
$$

를 만족하면 $f$를 `linear functional`이라고 한다.

$V$의 모든 linear functional의 집합을 $V$의 `dual space`라고 하며 다음과 같이 표기한다.

$$
V^*
:=
L(V,\F).
$$

Addition과 scalar multiplication은 pointwise하게 정의한다.

$$
(f+g)(v):=f(v)+g(v),
\qquad
(af)(v):=a f(v).
$$

이 연산 아래에서 $V^*$는 $\F$ 위의 vector space가 된다. Zero vector는 모든 $v\in V$를 $0_\F$로 보내는 zero functional이다.

Dual space의 정의에는 inner product가 필요하지 않다. Inner product가 주어졌을 때 vector와 linear functional을 연결하는 추가 구조는 [Riesz Representation Theorem](<43 Riesz Representation Theorem.md>)에서 다룬다.

## Dual Basis

### Motivation

Finite-dimensional vector space에 basis가 정해지면 각 vector는 유일한 coordinate를 갖는다. 각 coordinate를 하나씩 추출하는 linear functional들을 만들면, vector의 coordinate를 dual space의 basis로 표현할 수 있다.

### Definition

$n$-dimensional vector space $V/\F$의 ordered basis를

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

이라고 하자. 각 $i\in\{1,\ldots,n\}$에 대해

$$
\beta^i(\beta_j)=\delta^i_j
$$

를 만족하는 linear functional $\beta^i\in V^*$가 유일하게 존재한다. Ordered subset

$$
\beta^*
:=
(\beta^1,\ldots,\beta^n)
$$

를 $\beta$의 `dual basis`라고 한다.

실제로

$$
v=\sum_{j=1}^n a_j\beta_j
$$

이면 다음과 같이 정의할 수 있다.

$$
\beta^i(v):=a_i.
$$

Basis coordinate의 uniqueness 때문에 이 함수는 well-defined이고, coordinate가 linear combination과 호환되므로 $\beta^i$는 linear functional이다. 또한 basis vector에서의 함수값이 모든 vector에서의 값을 결정하므로 이 조건을 만족하는 functional은 유일하다.

### 정리1 (Dual basis theorem)

$n$-dimensional vector space $V/\F$의 basis가 $\beta$이면 dual basis $\beta^*$는 $V^*$의 basis다. 특히

$$
\dim V^*=\dim V=n.
$$

**Proof**

먼저 scalar $c_1,\ldots,c_n\in\F$에 대해

$$
\sum_{i=1}^n c_i\beta^i=0_{V^*}
$$

라고 하자. 양변을 $\beta_j$에 적용하면

$$
0_\F
=
\sum_{i=1}^n c_i\beta^i(\beta_j)
=
c_j
$$

를 얻는다. 모든 $j$에 대해 $c_j=0_\F$이므로 $\beta^*$는 linearly independent하다.

이제 $f\in V^*$를 잡고

$$
g
:=
\sum_{i=1}^n f(\beta_i)\beta^i
$$

라고 하자. $v=\sum_i a_i\beta_i$에 대해

$$
g(v)
=
\sum_{i=1}^n a_i f(\beta_i)
=
f(v)
$$

이므로 $g=f$다. 따라서 $\beta^*$는 $V^*$를 span한다. $\qed$

Dual basis를 사용하면 모든 $v\in V$와 $f\in V^*$를 다음처럼 복원할 수 있다.

$$
v
=
\sum_{i=1}^n\beta^i(v)\beta_i,
\qquad
f
=
\sum_{i=1}^n f(\beta_i)\beta^i.
$$

## Double Dual

### Motivation

Dual space $V^*$도 vector space이므로 다시 dual을 취해

$$
V^{**}:=(V^*)^*
$$

를 정의할 수 있다. Vector $v\in V$는 모든 functional $f\in V^*$에 대해 scalar $f(v)$를 정한다. 따라서 $v$를 $V^*$ 위의 evaluation functional로 볼 수 있다.

### Definition

`Canonical evaluation map`

$$
\iota_V:V\rightarrow V^{**}
$$

를 다음과 같이 정의한다.

$$
\bigl(\iota_V(v)\bigr)(f)
:=
f(v),
\qquad
v\in V,\ f\in V^*.
$$

각 $\iota_V(v)$는 $f$에 대해 linear하고,

$$
\iota_V(av+bw)
=
a\iota_V(v)+b\iota_V(w)
$$

이므로 $\iota_V$는 linear map이다. 이 정의에는 basis 선택이 들어가지 않으므로 canonical하다.

### 정리2 (Finite-dimensional double dual)

Finite-dimensional vector space $V/\F$에서 canonical evaluation map

$$
\iota_V:V\rightarrow V^{**}
$$

는 vector space isomorphism이다.

**Proof**

$\iota_V(v)=0_{V^{**}}$라고 하자. Basis $\beta$와 dual basis $\beta^*$를 선택하면 모든 $i$에 대해

$$
0_\F
=
\bigl(\iota_V(v)\bigr)(\beta^i)
=
\beta^i(v).
$$

따라서 $v$의 모든 coordinate가 $0_\F$이고 $v=0_V$다. 그러므로 $\iota_V$는 injective다.

정리1에 의해

$$
\dim V^{**}
=
\dim V^*
=
\dim V
$$

이므로 같은 finite dimension 사이의 injective linear map $\iota_V$는 surjective다. 따라서 $\iota_V$는 isomorphism이다. $\qed$

Finite-dimensional 가정은 surjectivity에 필요하다. Infinite-dimensional vector space에서도 canonical evaluation map은 정의되지만 일반적으로 $V^{**}$ 전체를 채우지 못하므로 $V\cong V^{**}$라고 결론내릴 수 없다.

## Dual Map

### Motivation

Linear map

$$
T:V\rightarrow W
$$

와 $W$의 linear functional $g:W\rightarrow\F$가 있으면 composition

$$
g\circ T:V\rightarrow\F
$$

도 linear functional이다. 따라서 $T$는 $W^*$의 functional을 $V^*$의 functional로 pull back한다. 원래 map과 반대 방향으로 움직인다는 점이 dual map의 핵심이다.

### Definition

Linear map $T:V\rightarrow W$의 `dual map`을 다음과 같이 정의한다.

$$
T^\vee:W^*\rightarrow V^*,
\qquad
T^\vee(g)
:=
g\circ T.
$$

모든 $a,b\in\F$와 $f,g\in W^*$에 대해

$$
T^\vee(af+bg)
=
aT^\vee(f)+bT^\vee(g)
$$

이므로 $T^\vee$는 linear map이다.

Map의 composition을 취하면 순서가 뒤집힌다. $T:U\rightarrow V$, $S:V\rightarrow W$에 대해

$$
(S\circ T)^\vee
=
T^\vee\circ S^\vee.
$$

### 정리3 (Matrix of the dual map)

Finite-dimensional vector space $V,W$의 basis를 각각 $\beta,\gamma$라고 하고

$$
A:=[T]_\beta^\gamma
$$

라고 하자. Dual basis $\beta^*,\gamma^*$에 대한 dual map의 matrix는

$$
[T^\vee]_{\gamma^*}^{\beta^*}
=
A^{\mathsf T}
$$

다.

**Proof**

$A_{ij}$는 $T(\beta_j)$의 $\gamma_i$ 방향 coordinate이므로

$$
A_{ij}
=
\gamma^i(T\beta_j).
$$

한편 $T^\vee(\gamma^i)$를 $\beta_j$에 적용하면

$$
\bigl(T^\vee(\gamma^i)\bigr)(\beta_j)
=
\gamma^i(T\beta_j)
=
A_{ij}.
$$

따라서 $T^\vee(\gamma^i)$의 $\beta^*$-coordinate column의 $j$번째 entry는 $A_{ij}$다. 즉 dual map matrix의 $(j,i)$ entry가 $A_{ij}$이므로 전체 matrix는 $A^{\mathsf T}$다. $\qed$

### Dual map과 adjoint의 구분

Dual map은 inner product 없이 정의되며

$$
T^\vee:W^*\rightarrow V^*
$$

처럼 dual space 사이에서 원래 map의 방향을 뒤집는다. 반면 inner product가 주어졌을 때 정의하는 adjoint는

$$
T^*:W\rightarrow V
$$

처럼 원래 vector space 사이를 움직인다.

두 map 모두 흔히 $T^*$로 표기되지만 서로 같은 type의 map은 아니다. 이 문서에서는 혼동을 피하기 위해 dual map에는 $T^\vee$, adjoint에는 $T^*$를 사용한다. Riesz representation을 통한 두 map의 관계는 [Riesz Representation Theorem](<43 Riesz Representation Theorem.md>)에서 설명한다.

## 관련 문서

- [Linear Map](<10 Linear Map.md>)
- [Coordinate](<06 Coordinate.md>)
- [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)
- [Riesz Representation Theorem](<43 Riesz Representation Theorem.md>)
