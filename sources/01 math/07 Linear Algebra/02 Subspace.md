# Subspace

## 한 줄 요약

Subspace는 원래 vector space의 addition과 scalar multiplication을 그대로 사용해도 다시 vector space가 되는 부분집합이다.

## Motivation

Vector space $V/\F$의 subset이라고 해서 자동으로 vector space가 되는 것은 아니다. 두 vector를 더하거나 scalar를 곱했을 때 subset 밖으로 나가면 그 안에서 linear combination을 계속 만들 수 없기 때문이다. 따라서 subset이 zero vector와 모든 linear combination을 포함하는지 확인해야 한다.

## Definition

Vector space $V/\F$와 subset $U\subseteq V$가 있다고 하자. $V$의 addition과 scalar multiplication을 $U$에 그대로 제한했을 때 $U$가 vector space가 되면 $U$를 $V$의 `subspace`라고 하고

$$
U\le V
$$

라고 표기한다.

## Subspace Test

### 정리1

Subset $U\subseteq V$에 대해 다음 두 조건은 동치다.

1. $U$는 $V$의 subspace다.
2. $U\ne\emptyset$이고 모든 $u,v\in U$, $a,b\in\F$에 대해
   $$
   au+bv\in U
   $$
   다.

**Proof**

$U$가 subspace이면 scalar multiplication과 addition에 닫혀 있으므로 $au,bv\in U$이고 $au+bv\in U$다. 또한 vector space는 zero vector를 포함하므로 $U\ne\emptyset$이다.

반대로 두 번째 조건을 가정하자. $U\ne\emptyset$이므로 $x\in U$를 하나 선택할 수 있다. $a=b=0_\F$를 대입하면

$$
0_\F x+0_\F x=0_V\in U
$$

이고, $u\in U$에 대해 $a=-1_\F$, $v=0_V$, $b=0_\F$를 대입하면 $-u\in U$다. 또한 $a=b=1_\F$이면 $u+v\in U$, $b=0_\F$이면 $au\in U$다. Associativity, commutativity와 distributive laws는 $V$에서 성립하는 식을 그대로 물려받는다. 따라서 $U$는 $V$의 subspace다. $\qed$

이 정리는 zero vector, addition closure와 scalar-multiplication closure를 하나의 linear-combination 조건으로 확인하게 해 준다.

## Examples

### Homogeneous equation의 solution set

$$
q:=
\begin{bmatrix}
1\\
2\\
-1
\end{bmatrix},
\qquad
U
:=
\{x\in\R^3\mid q^{\mathsf T}x=0\}
$$

라고 하자. $u,v\in U$와 $a,b\in\R$에 대해

$$
q^{\mathsf T}(au+bv)
=
a\,q^{\mathsf T}u+b\,q^{\mathsf T}v
=
0
$$

이므로 $au+bv\in U$다. 또한 $0_{\R^3}\in U$이므로 $U\le\R^3$다.

### Origin을 지나지 않는 affine set

$c\ne0$일 때

$$
A
:=
\{x\in\R^n\mid q^{\mathsf T}x=c\}
$$

는 $0_{\R^n}$을 포함하지 않으므로 subspace가 아니다. Linear equation처럼 보이더라도 right-hand side가 nonzero이면 subspace 조건을 잃을 수 있다.

## Intersection and Sum

Subspaces $U,W\le V$의 intersection $U\cap W$는 subspace다. 실제로 $x,y\in U\cap W$이면 모든 $a,b\in\F$에 대해 $ax+by$가 $U$와 $W$ 모두에 속한다.

두 subspace의 `sum`을

$$
U+W
:=
\{u+w\mid u\in U,\ w\in W\}
$$

로 정의한다. $u_1+w_1,u_2+w_2\in U+W$와 $a,b\in\F$에 대해

$$
a(u_1+w_1)+b(u_2+w_2)
=
(au_1+bu_2)+(aw_1+bw_2)
\in U+W
$$

이므로 $U+W$도 subspace다.

## Direct Sum

$V=U+W$이고 모든 $v\in V$가

$$
v=u+w,
\qquad
u\in U,\ w\in W
$$

로 unique하게 표현되면 $V$가 $U$와 $W$의 `direct sum`이라고 하고

$$
V=U\oplus W
$$

라고 쓴다.

두 subspace에 대해서는 다음 조건이 동치다.

$$
V=U\oplus W
\iff
V=U+W
\text{ and }
U\cap W=\{0_V\}.
$$

실제로 $v=u_1+w_1=u_2+w_2$이면

$$
u_1-u_2=w_2-w_1\in U\cap W.
$$

Intersection이 $\{0_V\}$이면 양변이 $0_V$이므로 $u_1=u_2$, $w_1=w_2$다. 반대로 nonzero $x\in U\cap W$가 있다면 $0_V=0_V+0_V=x+(-x)$라는 서로 다른 두 표현이 생겨 uniqueness에 모순이다.

여러 subspace의 direct sum

$$
V=W_1\oplus\cdots\oplus W_k
$$

도 모든 $v\in V$가 $v=w_1+\cdots+w_k$, $w_i\in W_i$로 unique하게 표현된다는 뜻이다. Subspace가 셋 이상일 때 pairwise intersection이 $\{0_V\}$라는 조건만으로는 uniqueness가 보장되지 않으므로, unique decomposition을 기준으로 정의해야 한다.

## 관련 문서

- [Span](<03 Span.md>)
- [Linearly Independent](<04 Linearly Independet.md>)
- [Basis](<05 Basis.md>)
- [Kernel](<11 Kernel.md>)
