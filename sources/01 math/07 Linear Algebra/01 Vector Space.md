# Vector Space

## 한 줄 요약

Vector space는 addition과 scalar multiplication이 일관된 규칙을 만족하는 대상들의 집합이다.

## Motivation

$\F^n$의 column, polynomial과 function은 겉모습이 다르다. 하지만 두 대상을 더하거나 scalar를 곱할 수 있고, 이 연산들이 같은 대수 법칙을 만족한다면 linear combination을 만드는 방법도 같다. Vector space는 이러한 공통 구조만 추려서 다루기 위한 개념이다.

Vector space의 element를 `vector`, vector에 곱하는 field $\F$의 element를 `scalar`라고 한다. Vector는 반드시 화살표나 숫자 column일 필요가 없다. Coordinate column은 basis를 선택한 뒤 vector를 나타내는 표현이며, vector 자체와는 구분해야 한다.

## Definition

Set $V$와 field $\F$가 있다고 하자. Addition과 scalar multiplication이

$$
+ : V\times V\rightarrow V,
\qquad
\cdot : \F\times V\rightarrow V
$$

로 주어졌다고 하자. 모든 $u,v,w\in V$와 $a,b\in\F$에 대해 다음 조건을 만족하면 $V$를 $\F$ 위의 `vector space`라고 한다.

1. Addition은 associative하다.
   $$
   (u+v)+w=u+(v+w)
   $$
2. Addition은 commutative하다.
   $$
   u+v=v+u
   $$
3. Zero vector $0_V\in V$가 존재한다.
   $$
   v+0_V=v
   $$
4. 각 $v\in V$에 additive inverse $-v\in V$가 존재한다.
   $$
   v+(-v)=0_V
   $$
5. Scalar multiplication은 vector addition에 대해 distributive하다.
   $$
   a(u+v)=au+av
   $$
6. Scalar addition은 scalar multiplication에 대해 distributive하다.
   $$
   (a+b)v=av+bv
   $$
7. Scalar multiplication은 field multiplication과 compatible하다.
   $$
   (ab)v=a(bv)
   $$
8. Field의 multiplicative identity가 vector를 바꾸지 않는다.
   $$
   1_\F v=v
   $$

$V$가 $\F$ 위의 vector space라는 사실을 $V/\F$라고 나타내기도 한다.

## 기본 결과

Vector space의 공리에서 모든 $v\in V$와 $a\in\F$에 대해 다음이 성립한다.

$$
0_\F v=0_V,
\qquad
a0_V=0_V,
\qquad
(-1_\F)v=-v.
$$

첫 번째 식을 확인해 보자. Distributive law에 의해

$$
0_\F v
=
(0_\F+0_\F)v
=
0_\F v+0_\F v
$$

이다. 양변에 $0_\F v$의 additive inverse를 더하면 왼쪽은 $0_V$가 되고 오른쪽에는 $0_\F v$만 남으므로 $0_\F v=0_V$를 얻는다. 나머지 식도 같은 공리와 additive inverse를 사용해 얻을 수 있다.

## Examples

### Coordinate space

$\F^n$에서 addition과 scalar multiplication을 componentwise하게 정의하면 vector space가 된다.

$$
(x_1,\ldots,x_n)+(y_1,\ldots,y_n)
:=
(x_1+y_1,\ldots,x_n+y_n),
$$

$$
a(x_1,\ldots,x_n)
:=
(ax_1,\ldots,ax_n).
$$

### Polynomial space

Degree가 $n$ 이하인 polynomial 전체의 집합

$$
\mathcal P_n(\F)
:=
\left\{
a_0+a_1t+\cdots+a_nt^n
\mid
a_i\in\F
\right\}
$$

은 usual polynomial addition과 scalar multiplication에 대해 vector space다.

### Function space

Set $S$에서 $\F$로 가는 모든 function의 집합을

$$
\mathcal F(S,\F)
:=
\{f:S\rightarrow\F\}
$$

라고 하자. Addition과 scalar multiplication을 pointwise하게 정의한다.

$$
(f+g)(s):=f(s)+g(s),
\qquad
(af)(s):=a f(s).
$$

예를 들어 associativity는 모든 $s\in S$에 대해

$$
\begin{aligned}
((f+g)+h)(s)
&=(f(s)+g(s))+h(s)\\
&=f(s)+(g(s)+h(s))\\
&=(f+(g+h))(s)
\end{aligned}
$$

이므로 성립한다. Zero function은 $0_{\mathcal F}(s)=0_\F$이고 additive inverse는 $(-f)(s)=-f(s)$다. Distributive laws도 각 $s$에서 field의 법칙을 적용하면 성립한다. 따라서 $\mathcal F(S,\F)$는 $\F$ 위의 vector space다.

## 관련 문서

- [Subspace](<02 Subspace.md>)
- [Span](<03 Span.md>)
- [Basis](<05 Basis.md>)
- [Coordinate](<06 Coordinate.md>)
