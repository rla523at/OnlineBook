# Vector Space Isomorphism

## 한 줄 요약

Vector space isomorphism은 linear structure를 보존하는 bijection이며, finite-dimensional vector spaces는 field가 같을 때 dimension만으로 isomorphism 여부가 결정된다.

## Motivation

두 vector spaces의 element 모양이 달라도 addition과 scalar multiplication의 구조가 같을 수 있다. 예를 들어 degree가 $n-1$ 이하인 polynomial과 $\F^n$의 column은 서로 다른 대상이지만, polynomial coefficient를 column으로 보내면 linear combination의 규칙이 그대로 보존된다. 이처럼 element를 빠짐없이 일대일로 대응시키면서 linear structure를 보존하는 map이 있으면 두 vector space를 같은 linear-algebraic 구조로 다룰 수 있다.

## Definition

Vector spaces $V,W/\F$ 사이의 bijective linear map

$$
\Phi:V\rightarrow W
$$

를 `vector space isomorphism`이라고 한다. 이러한 map이 존재하면 $V$와 $W$가 `isomorphic`하다고 하고

$$
V\cong W
$$

라고 쓴다.

## The Inverse Is Linear

### 정리1

Bijective linear map $\Phi:V\rightarrow W$의 inverse

$$
\Phi^{-1}:W\rightarrow V
$$

도 linear하다.

**Proof**

$w_1,w_2\in W$와 $a,b\in\F$를 잡고 $v_i:=\Phi^{-1}(w_i)$라고 하자. Linearity에 의해

$$
\Phi(av_1+bv_2)
=
a\Phi(v_1)+b\Phi(v_2)
=
aw_1+bw_2.
$$

$\Phi$가 bijective이므로 $aw_1+bw_2$의 unique preimage는 $av_1+bv_2$다. 따라서

$$
\Phi^{-1}(aw_1+bw_2)
=
a\Phi^{-1}(w_1)+b\Phi^{-1}(w_2).
$$

즉 $\Phi^{-1}$도 linear하다. $\qed$

따라서 isomorphism은 양방향에서 linear structure를 보존한다.

## Isomorphisms Send Bases to Bases

### 정리2

$\Phi:V\rightarrow W$가 isomorphism이고 $\beta$가 $V$의 basis이면

$$
\Phi(\beta)
:=
\{\Phi(v)\mid v\in\beta\}
$$

는 $W$의 basis다.

**Proof**

$\Phi$가 injective이므로 $\ker\Phi=\{0_V\}$이고, basis vector들의 image는 linearly independent하다. 또한 임의의 $w\in W$에 대해 surjectivity로 $w=\Phi(v)$인 $v\in V$가 존재한다. $v$를 $\beta$의 linear combination으로 쓰고 $\Phi$를 적용하면 $w$는 $\Phi(\beta)$의 linear combination이다. 따라서 $\Phi(\beta)$는 $W$를 span한다. $\qed$

## Classification by Dimension

### 정리3

같은 field $\F$ 위의 finite-dimensional vector spaces $V,W$에 대해

$$
V\cong W
\iff
\dim V=\dim W.
$$

**Proof**

$V\cong W$이면 정리2에 의해 $V$의 basis가 $W$의 basis로 bijectively 대응하므로 두 dimension이 같다.

반대로 $\dim V=\dim W=n$이라고 하자. Ordered bases

$$
\beta=(\beta_1,\ldots,\beta_n),
\qquad
\gamma=(\gamma_1,\ldots,\gamma_n)
$$

를 선택하고

$$
\Phi\left(\sum_{i=1}^n a_i\beta_i\right)
:=
\sum_{i=1}^n a_i\gamma_i
$$

로 정의하자. Basis representation이 unique하므로 $\Phi$는 well-defined이고 linear하다. 임의의 $\gamma$-coordinate를 같은 $\beta$-coordinate에서 얻을 수 있으므로 surjective이고, $\Phi(v)=0_W$이면 모든 coordinate가 zero이므로 injective다. 따라서 $\Phi$는 isomorphism이다. $\qed$

특히 모든 $n$-dimensional vector space는

$$
V\cong\F^n
$$

이다. Basis $\beta$가 정하는 coordinate map $v\mapsto[v]_\beta$가 구체적인 isomorphism을 준다.

## Same Structure Does Not Mean Equal Objects

$V\cong W$는 $V=W$라는 뜻이 아니다. Isomorphism은 두 공간의 element를 대응시켜 addition과 scalar multiplication에 관한 모든 관계를 보존하지만, element의 원래 의미나 집합 자체를 같게 만들지는 않는다.

또한 $V\cong\F^n$인 구체적인 correspondence는 일반적으로 basis 선택에 의존한다. 서로 다른 basis는 서로 다른 coordinate isomorphism을 만든다. Basis가 바뀔 때 coordinate가 어떻게 변하는지는 [Change of Basis and Coordinate Matrix](<23 Change of Basis and Coordinate Matrix.md>)에서 다룬다.

## 관련 문서

- [Basis](<05 Basis.md>)
- [Coordinate](<06 Coordinate.md>)
- [Linear Map](<10 Linear Map.md>)
- [Matrix Representation](<20 Matrix Representation.md>)
