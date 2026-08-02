# Affine Space

Affine space는 특별한 origin을 지정하지 않고 point와 vector 사이의 관계를 다루는 공간이다. Point에 vector를 더할 수 있고 두 point를 빼면 vector를 얻지만, point끼리의 덧셈은 정의하지 않는다.

## Motivation

Vector space $V$에는 zero vector가 있으므로 모든 vector를 $0$에서 시작하는 위치로 해석할 수 있다. 그러나 geometric point를 다룰 때 어느 point를 origin으로 선택할지는 공간 자체가 정해 주지 않는다. Origin을 바꾸면 각 point의 coordinate는 달라지지만 두 point를 잇는 vector는 달라지지 않는다.

따라서 origin을 선택하지 않은 상태에서도 다음 관계는 표현할 수 있어야 한다.

- Point $a$에 vector $v$를 더하여 얻는 point를 $a+v$로 나타낸다.
- Point $a$에서 $b$로 가는 vector를 $b-a$로 나타낸다.

반면 특별한 origin이 없으므로 point의 덧셈 $a+b$나 scalar multiplication $\lambda a$는 그 자체로 정의하지 않는다. Affine space는 point의 절대적인 위치 대신 point와 vector 사이의 관계만 남긴 구조다.

Point에 vector를 더하여 다른 point로 이동한다는 규칙은 vector addition과 자연스럽게 맞아야 한다. 먼저 zero vector $0$은 이동량이 없다는 뜻이므로, 어떤 point $a$에서도 $a+0=a$여야 한다. 또한 $a$에 $v$를 더한 다음 $w$를 더하는 것은 두 vector의 합 $v+w$를 한 번에 더하는 것과 같아야 한다.

$$
(a+v)+w=a+(v+w)
$$

이 두 조건은 vector space $V_A$의 additive group이 point set $A$에 작용한다는 것, 즉 group action으로 표현된다.

여기에 더해, 임의의 두 point $a,b$에 대해 $a+v=b$를 만족하는 vector $v$가 정확히 하나 존재해야 한다. 그래야 $a$에서 $b$로 가는 vector $b-a$를 모호함 없이 정의할 수 있다. 출발점 $a$를 고정하면 각 vector $v$를 도착점 $a+v$에 대응시키는 함수 $\phi_a:V_A\rightarrow A$를 생각할 수 있다. 이 함수가 surjective라는 것은 $a$에서 모든 point에 도달할 수 있다는 뜻이고, injective라는 것은 같은 point로 가는 vector가 둘 이상 없다는 뜻이다. 따라서 $\phi_a$가 bijective라는 조건은 임의의 목적지로 가는 vector가 정확히 하나 존재한다는 요구를 표현한다. Affine space는 이 group action과 bijectivity를 만족하는 공간이다.

## Definition

Field $\mathbb F$ 위의 vector space $V_A$와 nonempty set $A$가 있다고 하자. 다음 map을 생각하자.

$$
+:A\times V_A\rightarrow A,
\qquad
(a,v)\mapsto a+v.
$$

다음 세 조건을 모두 만족할 때 $(A,V_A,+)$를 field $\mathbb F$ 위의 `affine space`라고 한다.

1. 모든 $a\in A$에 대해 다음이 성립한다.

   $$
   a+0_{V_A}=a.
   $$

2. 모든 $a\in A$, $v,w\in V_A$에 대해 다음이 성립한다.

   $$
   (a+v)+w=a+(v+w).
   $$

3. 모든 $a,b\in A$에 대해 다음을 만족하는 $v\in V_A$가 유일하게 존재한다.

   $$
   a+v=b.
   $$

- $A$의 원소를 `point`라고 한다.
- $V_A$를 $A$의 `associated vector space`라고 하고, 그 원소를 `vector`라고 한다.

### 조건의 의미

첫 번째와 두 번째 조건은 $V_A$의 additive group이 $A$에 작용하는 [right group action](<../02 Abstract Algebra/05 Group/51 Group Action.md>) 조건이다.

세 번째 조건은 각 $a\in A$에 대해 다음 함수가 bijective라는 것과 동치다.

$$
\phi_a:V_A\rightarrow A,
\qquad
v\mapsto a+v.
$$

$\phi_a$의 surjectivity는 $a$에서 임의의 point로 이동할 수 있다는 뜻이고, injectivity는 그 이동에 필요한 vector가 유일하다는 뜻이다. 따라서 세 번째 조건은 “두 point를 잇는 vector가 항상 유일하게 존재한다”는 요구를 정확히 표현한다.

정의에는 특별한 origin이 포함되지 않는다. 임의의 point를 기준 point로 선택할 수 있지만, 그 선택은 affine space에 추가한 정보다.

## Translation과 subtraction

### Translation

$v\in V_A$에 대해 다음 함수를 $v$에 의한 `translation`이라고 한다.

$$
\tau_v:A\rightarrow A,
\qquad
a\mapsto a+v.
$$

$-v$에 의한 translation은 $\tau_v$의 inverse다.

$$
\tau_{-v}(\tau_v(a))
=(a+v)+(-v)
=a.
$$

따라서 모든 translation은 bijective다. 두 translation을 연속해서 적용하면 vector addition에 대응한다.

$$
\tau_w\circ\tau_v=\tau_{v+w}.
$$

### 두 point의 subtraction

$a,b\in A$라고 하자. $\phi_a$가 bijective이므로 다음을 만족하는 vector $v\in V_A$가 유일하게 존재한다.

$$
a+v=b.
$$

이 vector를 $b-a$로 표기하고, 두 point의 `subtraction`으로 정의한다. 즉, $b-a$는 $a$에서 $b$로 가는 vector다.

$$
b-a:=v
\quad\Longleftrightarrow\quad
a+v=b.
$$

정의에서 바로 다음 관계를 얻는다.

$$
a+(b-a)=b,
$$

$$
(a+v)-a=v,
$$

$$
a-a=0_{V_A},
$$

$$
a-b=-(b-a).
$$

Point와 point의 subtraction은 vector지만, point와 point의 덧셈은 여전히 정의되지 않는다는 점에 주의해야 한다.

### 두 point를 잇는 vector의 합

$a,b,c\in A$라고 하자. $a$에서 $b$로 이동한 뒤 $b$에서 $c$로 이동하면 $a$에서 $c$에 도달하므로 다음이 성립한다.

$$
(b-a)+(c-b)=c-a.
$$

실제로 다음 두 vector는 모두 $a$를 $c$로 보낸다.

$$
a+\big((b-a)+(c-b)\big)=c,
$$

$$
a+(c-a)=c.
$$

$\phi_a$가 injective이므로 두 vector는 같다.

### Parallelogram relation

$a,b,c,d\in A$에 대해 다음 두 조건은 동치다.

$$
b-a=d-c
\quad\Longleftrightarrow\quad
c-a=d-b.
$$

첫 번째 등식은 두 변 $a\rightarrow b$, $c\rightarrow d$를 나타내는 vector가 같다는 뜻이고, 두 번째 등식은 나머지 두 변 $a\rightarrow c$, $b\rightarrow d$를 나타내는 vector가 같다는 뜻이다. 따라서 이 관계는 네 point가 만드는 parallelogram을 origin이나 coordinate 없이 표현한다.

실제로 두 point를 잇는 vector의 합에 의해 다음이 성립한다.

$$
(b-a)+(d-b)
=d-a
=(c-a)+(d-c).
$$

한쪽 변을 나타내는 vector가 같다고 가정하고 vector addition의 cancellation을 적용하면 다른 쪽 변을 나타내는 vector도 같다는 결론을 얻는다. 두 방향에 같은 논리를 적용하면 위의 동치가 성립한다.

## 기준 point의 선택

기준 point $o\in A$를 선택하면 다음 bijection을 얻는다.

$$
\phi_o:V_A\rightarrow A,
\qquad
v\mapsto o+v,
$$

$$
\phi_o^{-1}:A\rightarrow V_A,
\qquad
p\mapsto p-o.
$$

따라서 기준 point를 하나 선택한 뒤에는 각 point $p$를 vector $p-o$와 대응시킬 수 있다. 하지만 이것은 $A$와 $V_A$의 canonical identification이 아니다. 다른 기준 point를 선택하면 같은 point에 대응하는 vector가 달라진다.

Affine space를 “origin을 잊은 vector space”라고 표현하는 이유가 여기에 있다. Origin을 선택하면 vector space처럼 표현할 수 있지만 어느 point를 origin으로 선택할지는 affine structure 자체에 포함되지 않는다.

## Affine subspace

Linear subspace $W\le V_A$와 point $p\in A$가 있다고 하자. 다음 집합을 생각하자.

$$
B:=p+W
=\{p+w\mid w\in W\}.
$$

$B$는 associated vector space가 $W$인 affine space가 되며, 이를 $A$의 `affine subspace`라고 한다. $W$를 $B$의 `direction`이라고 한다.

기준 point를 $q\in B$로 바꾸어도 같은 affine subspace를 얻는다. 실제로 어떤 $w_0\in W$에 대해 $q=p+w_0$이므로 다음이 성립한다.

$$
q+W=p+w_0+W=p+W.
$$

두 affine subspace의 direction이 같으면 두 subspace가 `parallel`하다고 한다.

### Affine line

$v\in V_A$가 nonzero vector이고 $a\in A$라고 하자. 다음 affine subspace를 $a$를 지나고 direction이 $\operatorname{span}\{v\}$인 affine line이라고 한다.

$$
L=a+\operatorname{span}\{v\}
=\{a+\lambda v\mid\lambda\in\mathbb F\}.
$$

서로 다른 두 point $a,b\in A$가 정하는 affine line은 다음과 같다.

$$
L_{a,b}
=\{a+\lambda(b-a)\mid\lambda\in\mathbb F\}.
$$

이 표현은 [Affine Combination](<./12 Affine Combination.md>)에서 두 point의 affine parameter로 이어진다.

## Coordinate system

$V_A$가 $n$-dimensional vector space이고 $\beta$가 $V_A$의 basis라고 하자. 기준 point $o\in A$와 basis $\beta$의 pair $(o,\beta)$를 $A$의 `coordinate system`이라고 한다.

Point $p\in A$의 coordinate는 $o$에서 $p$로 가는 vector의 coordinate로 정의한다.

$$
[p]_{(o,\beta)}
:=
[p-o]_{\beta}
\in\mathbb F^n.
$$

Coordinate map은 다음 두 bijection의 composition이므로 bijective다.

$$
A\rightarrow V_A\rightarrow\mathbb F^n,
\qquad
p\mapsto p-o\mapsto[p-o]_{\beta}.
$$

따라서 coordinate system을 선택하면 각 point를 유일한 coordinate column vector로 표현할 수 있다.

### Change of coordinate system

두 coordinate system $(o_1,\beta)$, $(o_2,\gamma)$가 있다고 하자. Point $p\in A$의 두 coordinate를 다음과 같이 두자.

$$
x:=[p]_{(o_1,\beta)},
\qquad
y:=[p]_{(o_2,\gamma)}.
$$

$C=[\operatorname{id}_{V_A}]_{\beta}^{\gamma}$를 basis $\beta$의 coordinate를 basis $\gamma$의 coordinate로 바꾸는 matrix라고 하자. 두 point를 잇는 vector의 합에 의해 다음이 성립한다.

$$
p-o_2=(p-o_1)+(o_1-o_2).
$$

따라서 coordinate는 다음과 같이 변환된다.

$$
\begin{aligned}
y
&=[p-o_2]_{\gamma} \\
&=[p-o_1]_{\gamma}+[o_1-o_2]_{\gamma} \\
&=Cx+[o_1-o_2]_{\gamma}.
\end{aligned}
$$

첫 번째 항은 basis의 변경을, 두 번째 항은 coordinate origin의 변경을 나타낸다. 따라서 affine coordinate change는 일반적으로 linear term과 translation term을 함께 갖는다. Basis change matrix는 [Change of Basis and Coordinate Matrix](<../07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)에서 설명한다.

## Examples

### Vector space에서 얻는 affine space

Field $\mathbb F$ 위의 vector space $V$에 대해 point set도 $A:=V$로 두고 기존 vector addition을 action으로 사용하면 $(A,V,+)$는 affine space가 된다.

$$
a+v\in A,
\qquad
a,v\in V.
$$

이 예시에서는 $A$와 $V$가 같은 underlying set이지만 역할은 다르다. $a\in A$는 point로, $v\in V$는 vector로 사용한다. Vector space에는 zero vector가 있지만 affine structure만 보면 그 point를 반드시 origin으로 선택해야 할 이유는 없다.

### Linear equation의 solution set

Linear map $L:V\rightarrow W$와 $b\in W$가 있고 equation $L(x)=b$가 하나 이상의 solution을 갖는다고 하자. 하나의 solution $x_0$를 선택하면 전체 solution set은 다음과 같다.

$$
\{x\in V\mid L(x)=b\}
=x_0+\ker L.
$$

따라서 nonempty linear equation의 solution set은 direction이 $\ker L$인 affine subspace다. $b=0$일 때에만 solution set이 origin을 포함하는 linear subspace가 된다.

## 관련 문서

- [Affine Combination](<./12 Affine Combination.md>)
- [Affine Transformation](<./13 Affine Transformation.md>)
- [Change of Basis and Coordinate Matrix](<../07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)

## Reference

- [Wikipedia - Affine space](https://en.wikipedia.org/wiki/Affine_space)
- [Wikipedia - Affine subspace](https://en.wikipedia.org/wiki/Affine_space#Affine_subspaces_and_parallelism)
