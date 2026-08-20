# Linear Map

## 한 줄 요약

Linear map은 linear combination을 보존하는 함수이며, basis vector에서의 값만 알면 모든 vector에서의 값이 결정된다.

## Motivation

일반적인 함수는 vector space의 addition과 scalar multiplication을 보존할 필요가 없다. 그러면 input을 linear combination으로 표현해도 각 basis direction의 결과를 더해 output을 계산할 수 없다.

반대로 함수 $T$가

$$
T(av+bw)=aT(v)+bT(w)
$$

를 만족하면 input을 구성한 것과 같은 coefficient로 output을 구성할 수 있다. 따라서 basis vector들의 image만 알면 모든 input의 image를 계산할 수 있다. 이처럼 vector space의 linear structure를 보존하는 함수가 linear map이다.

## Definition

같은 field $\F$ 위의 vector spaces $V,W$와 function

$$
T:V\rightarrow W
$$

가 있다고 하자. 모든 $v,w\in V$와 $a,b\in\F$에 대해

$$
T(av+bw)=aT(v)+bT(w)
$$

를 만족하면 $T$를 `linear map` 또는 `linear transformation`이라고 한다.

$V$에서 $W$로 가는 linear map 전체의 집합을

$$
L(V,W)
$$

라고 쓴다. $T:V\rightarrow V$인 linear map은 `endomorphism`이라고 하며, 그 집합을

$$
\operatorname{End}(V):=L(V,V)
$$

라고 쓴다.

## Basic Consequences

Linear map은 zero vector를 zero vector로 보낸다. 실제로

$$
T(0_V)
=
T(0_\F0_V)
=
0_\F T(0_V)
=
0_W.
$$

또한 finite sum에 대해

$$
T\left(\sum_{i=1}^k a_iv_i\right)
=
\sum_{i=1}^k a_iT(v_i)
$$

가 성립한다. 따라서 linear relation은 map을 적용한 뒤에도 유지된다.

Affine translation

$$
F(v):=T(v)+w_0
$$

은 $w_0\ne0_W$이면 linear map이 아니다. $F(0_V)=w_0\ne0_W$이기 때문이다. Matrix multiplication $x\mapsto Ax$는 linear하지만 $x\mapsto Ax+b$는 $b\ne0$이면 일반적으로 linear하지 않다는 차이가 여기서 나온다.

## A Linear Map Is Determined by a Basis

### 정리1

$V$의 basis가

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

이고 $w_1,\ldots,w_n\in W$가 주어지면

$$
T(\beta_i)=w_i
\qquad
(i=1,\ldots,n)
$$

를 만족하는 unique linear map $T:V\rightarrow W$가 존재한다.

**Proof**

모든 $v\in V$는 unique하게

$$
v=\sum_{i=1}^n a_i\beta_i
$$

로 표현된다. 따라서

$$
T(v)
:=
\sum_{i=1}^n a_iw_i
$$

로 정의하면 basis coordinate의 uniqueness 때문에 well-defined다. Coefficient가 addition과 scalar multiplication에 따라 componentwise하게 변하므로 이 $T$는 linear하고 $T(\beta_i)=w_i$를 만족한다.

다른 linear map $S$도 $S(\beta_i)=w_i$를 만족한다고 하자. Linearity에 의해

$$
S(v)
=
\sum_{i=1}^n a_iS(\beta_i)
=
\sum_{i=1}^n a_iw_i
=
T(v)
$$

이므로 $S=T$다. $\qed$

따라서 두 linear map $S,T\in L(V,W)$가 모든 basis vector에서 같은 값을 가지면 $S=T$다.

## The Vector Space of Linear Maps

$S,T\in L(V,W)$와 $a\in\F$에 대해 pointwise operation을

$$
(S+T)(v):=S(v)+T(v),
\qquad
(aT)(v):=aT(v)
$$

로 정의하면 $L(V,W)$는 $\F$ 위의 vector space가 된다. 예를 들어

$$
\begin{aligned}
(S+T)(av+bw)
&=S(av+bw)+T(av+bw)\\
&=a(S+T)(v)+b(S+T)(w)
\end{aligned}
$$

이므로 addition에 닫혀 있다. Zero vector는 모든 input을 $0_W$로 보내는 zero map이고, 나머지 vector-space laws는 $W$의 laws를 pointwise하게 물려받는다.

$\dim V=n$, $\dim W=m$이고 basis가 각각 $\beta=(\beta_1,\ldots,\beta_n)$, $\gamma=(\gamma_1,\ldots,\gamma_m)$라고 하자. $1\le i\le n$, $1\le j\le m$에 대해

$$
E_{ji}(\beta_k):=\delta_{ik}\gamma_j
$$

로 정한 linear map $E_{ji}$를 생각하자. 정리1에 의해 이 map은 unique하게 존재한다.

임의의 $T\in L(V,W)$에 대해

$$
T(\beta_i)
=
\sum_{j=1}^m A_{ji}\gamma_j
$$

라고 쓰면

$$
T
=
\sum_{i=1}^n\sum_{j=1}^m A_{ji}E_{ji}.
$$

또한 이 표현은 unique하므로 $\{E_{ji}\}$는 $L(V,W)$의 basis다. 따라서

$$
\dim L(V,W)=mn.
$$

Coefficient $A_{ji}$를 matrix에 배열하는 과정은 [Matrix Representation](<20 Matrix Representation.md>)에서 다룬다.

## Composition

Linear maps

$$
T:U\rightarrow V,
\qquad
S:V\rightarrow W
$$

의 composition도 linear하다. 모든 $u_1,u_2\in U$와 $a,b\in\F$에 대해

$$
\begin{aligned}
(S\circ T)(au_1+bu_2)
&=S(aT(u_1)+bT(u_2))\\
&=a(S\circ T)(u_1)+b(S\circ T)(u_2).
\end{aligned}
$$

## Invariant Subspace

$T\in\operatorname{End}(V)$와 subspace $U\le V$에 대해

$$
T(U)\subseteq U
$$

이면 $U$를 $T$-`invariant subspace`라고 한다. 이 경우 domain과 codomain을 $U$로 제한한 map

$$
T|_U:U\rightarrow U
$$

을 정의할 수 있다. Invariant direct-sum decomposition은 적절한 basis에서 $T$의 matrix를 block diagonal form으로 만든다.

## 관련 문서

- [Kernel](<11 Kernel.md>)
- [Image](<12 Image.md>)
- [Vector Space Isomorphism](<13 Vector Space Isomorphism.md>)
- [Matrix Representation](<20 Matrix Representation.md>)
