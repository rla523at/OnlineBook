# Affine Transformation

Affine map은 한 기준점의 image와 모든 vector에 공통으로 작용하는 linear map으로 결정된다. Coordinate system을 선택하면 affine map은 익숙한 $y=Mx+t$ 형태로 표현된다.

이 문서는 [Affine space](<./11 Affine space.md>)에서 정의한 point, vector, subtraction과 [Affine Combination](<./12 Affine Combination.md>)에서 정의한 affine combination을 사용한다.

## Affine Homomorphism

### Motivation

같은 field $\mathbb F$ 위의 affine space $A,B$가 있고, associated vector space를 각각 $V_A,V_B$라고 하자.

Affine space $A$의 `affine structure`는 point set $A$, associated vector space $V_A$, 그리고 point에 vector를 더하는 다음 action을 함께 뜻한다.

$$
+:A\times V_A\rightarrow A,
\qquad
(a,v)\mapsto a+v.
$$

이 action은 point $a$에서 point $a+v$로 가는 vector가 $v$라는 상대적 위치 관계를 정한다. 

이제 두 affine space 사이에서 affine structure를 보존하는 함수가 어떤 조건을 만족해야 하는지 생각해 보자. Affine structure의 핵심 연산은 point에 vector를 더하는 action이므로, 구조를 보존하려면 이 action이 두 공간 사이에서 호환되어야 한다.

이를 구체화하기 위해 함수 $F:A\rightarrow B$가 point-vector 관계를 어떻게 옮기는지 살펴보자. 

$A$에서 $a\in A$에 $v\in V_A$를 적용하면 다음 point를 얻는다.

$$
a+v\in A.
$$

이 point를 $F$로 변환한 결과는 다음과 같다.

$$
F(a+v)\in B.
$$

이 관계를 $B$에서도 재현하려면 $F(a)$에 어떤 vector를 적용해야 한다. 그러나

$$
F(a)\in B,
\qquad
v\in V_A
$$

이므로 일반적으로 $F(a)+v$는 정의되지 않는다. $B$의 point에 적용할 수 있는 것은 associated vector space $V_B$의 vector이기 때문이다.

$$
+:B\times V_B\rightarrow B.
$$

따라서 $v\in V_A$를 $B$에서 적용할 수 있는 vector로 변환하는 함수가 필요하다. 이를 다음과 같이 나타내자.

$$
T_F:V_A\rightarrow V_B,
\qquad
v\mapsto T_F(v).
$$

이제 $a$와 $v$를 각각 $F(a)$와 $T_F(v)$로 변환한 뒤 $B$의 action을 적용하면 다음 point를 얻는다.

$$
F(a)+T_F(v)\in B.
$$

원래의 point-vector 관계가 변환 후에도 보존되려면, $A$에서 먼저 action을 적용한 뒤 $F$로 변환한 결과와 point와 vector를 각각 변환한 뒤 $B$에서 action을 적용한 결과가 같아야 한다.

$$
F(a+v)=F(a)+T_F(v).
$$

여기서 좌변의 $+$는 $A$의 action이고 우변의 $+$는 $B$의 action이다. 이 식은 point의 변환 $F$와 vector의 변환 $T_F$가 두 affine space의 action과 호환된다는 뜻이다.

Affine structure에서 vector addition은 displacement의 합성을 나타낸다. 즉, $v+w$를 한 번에 적용하는 것과 $v$와 $w$를 차례로 적용하는 것은 같은 point를 만들어야 한다.

$$
a+(v+w)=(a+v)+w.
$$

따라서 변환 후에도 두 방법이 같은 displacement를 나타내려면 다음이 성립해야 한다.

$$
T_F(v+w)=T_F(v)+T_F(w).
$$

이 조건이 없다면 하나의 displacement를 $v+w$로 적용할 때와 $v$, $w$로 나누어 적용할 때 서로 다른 결과를 얻게 되므로, point-vector action이 일관되게 보존되지 않는다. 실제로 이 덧셈 보존은 위의 action 호환성에서 자동으로 따라온다.

Scalar multiplication은 displacement의 배율을 나타낸다. Point $a+\lambda v$는 $a$에서 $v$ 방향으로 affine parameter $\lambda$만큼 이동한 point이다. 이 parameter를 변환 후에도 그대로 유지하려면 다음이 성립해야 한다.

$$
F(a+\lambda v)=F(a)+\lambda T_F(v).
$$

그런데 action 호환성에 따르면 좌변은 $F(a)+T_F(\lambda v)$이므로 다음 조건이 필요하다.

$$
T_F(\lambda v)=\lambda T_F(v).
$$

이 조건이 없다면 직선 위 point들의 affine parameter와 그에 따른 affine combination이 보존되지 않는다.

따라서 $T_F$는 displacement의 합성과 배율을 모두 보존하는 linear map이어야 한다. 결국 affine structure를 보존한다는 것은 linear map $T_F$가 존재하고, $F$와 $T_F$가 위 식을 통해 두 affine space의 action과 호환된다는 뜻이다. 이를 다음과 같이 정의한다.

### Definition

같은 field $\mathbb F$ 위의 affine space $A,B$와 함수 $F:A\rightarrow B$가 있다고 하자. 다음을 만족하는 linear map $T_F:V_A\rightarrow V_B$가 존재할 때, $F$를 `affine homomorphism` 또는 `affine map`이라고 한다.

$$
\boxed{
F(a+v)=F(a)+T_F(v)
},
\qquad \forall a\in A,\ v\in V_A.
$$

$T_F$를 $F$의 `associated linear map` 또는 `linear part`라고 한다.

Definition의 식에서 $v=b-a$로 두면 다음과 같은 point difference 표현을 얻는다.

$$
\boxed{
F(b)-F(a)=T_F(b-a)
}.
$$

즉, 두 point 사이의 vector $b-a$는 associated linear map에 의해 $T_F(b-a)$로 변환된다.

Associated linear map의 유일성을 보이자. Definition을 만족하는 두 linear map $T_1,T_2:V_A\rightarrow V_B$가 있다고 하자. 임의의 기준점 $a_0\in A$와 vector $v\in V_A$에 대해 다음이 성립한다.

$$
T_1(v)
=F(a_0+v)-F(a_0)
=T_2(v).
$$

이 등식이 모든 $v\in V_A$에 대해 성립하므로 $T_1=T_2$다. 따라서 associated linear map이 존재한다면 유일하다. 즉, 기준점 $a_0$ 하나를 고정하면 모든 함수값이

$$
T_F(v)=F(a_0+v)-F(a_0)
$$

로 완전히 결정된다. 이는 유일성에 관한 설명이다. 우변이 기준점에 따라 달라지면 $T_F$는 존재하지 않으며, 하나의 공통된 map을 구성할 수 있는 조건은 다음 절에서 살펴본다.

> Reference: [Wikipedia - Affine space: Affine maps](https://en.wikipedia.org/wiki/Affine_space#Affine_maps)

### Associated linear map의 구성

Definition을 만족하는 associated linear map $T_F$가 이미 존재한다면 모든 $a\in A$와 $v\in V_A$에 대해 다음 등식이 성립한다.

$$
T_F(v)=F(a+v)-F(a).
$$

여기서 $=$는 이미 존재하는 $T_F$가 만족하는 등식을 나타내며 새로운 함수를 정의하는 기호가 아니다. 반면 임의의 함수 $F:A\rightarrow B$에서는 아직 associated linear map의 존재를 알 수 없으므로 우변으로 $T_F$를 바로 정의할 수 없다. 먼저 각 시작점에 대한 후보를 정의해야 한다.

#### 시작점별 vector map

각 $a\in A$를 고정하면 다음 함수는 항상 정의할 수 있다.

$$
T_{F,a}:V_A\rightarrow V_B,
\qquad
T_{F,a}(v):=F(a+v)-F(a).
$$

$T_{F,a}(v)$는 $a$에서 $a+v$로 가는 vector가 $F$에 의해 변환된 결과다. 이 단계에서 $T_{F,a}$는 시작점 $a$에 따라 달라질 수 있으며 linear map이라는 보장도 없다.

#### 시작점 독립성

같은 vector $v$를 서로 다른 시작점 $a,c\in A$에 적용하면 다음이 성립한다.

$$
(a+v)-a=v=(c+v)-c.
$$

이 두 vector가 변환 후에도 하나의 공통된 vector에 대응하려면 시작점별 후보가 같아야 한다.

$$
T_{F,a}(v)=T_{F,c}(v),
\qquad \forall a,c\in A,\ v\in V_A.
$$

정의를 대입하면 이는 다음 조건과 같다.

$$
F(a+v)-F(a)
=
F(c+v)-F(c),
\qquad \forall a,c\in A,\ v\in V_A.
$$

이 조건이 성립하면 $T_{F,a}(v)$의 값이 $a$의 선택과 무관하므로 공통된 vector map을 다음과 같이 정의할 수 있다.

$$
\boxed{
T_F(v):=T_{F,a}(v)=F(a+v)-F(a)
}.
$$

여기서 $:=$는 공통된 값을 이용해 $T_F$를 새롭게 정의한다. 시작점 독립성을 먼저 확인했으므로 이 정의는 `well-defined`다. 또한 다음 식이 성립하므로 point의 변환 $F$와 vector의 변환 $T_F$가 affine space의 action과 호환된다.

$$
F(a+v)=F(a)+T_F(v).
$$

예를 들어 $\mathbb R$을 affine line으로 보고 $F(x)=x^2$라고 하자. 같은 vector $v=1$에 대해 시작점별 후보는 다음과 같이 서로 다르다.

$$
T_{F,0}(1)=F(0+1)-F(0)=1,
$$

$$
T_{F,1}(1)=F(1+1)-F(1)=3.
$$

따라서 하나의 공통된 $T_F$를 정의할 수 없다.

반면 $F(x)=2x+5$이면 모든 시작점 $a$에 대해 다음이 성립한다.

$$
T_{F,a}(v)=F(a+v)-F(a)=2v.
$$

이 경우에는 시작점과 관계없이 $T_F(v):=2v$로 정의할 수 있다. 모든 point에 공통으로 더해진 translation $5$는 point difference를 계산할 때 소거된다.

#### Linearity 조건

시작점 독립성은 $T_F:V_A\rightarrow V_B$를 well-defined 함수로 만든다. 그러나 Definition의 associated linear map을 얻으려면 $T_F$가 vector space의 덧셈과 scalar multiplication도 보존해야 한다. Affine space의 point에는 이러한 연산이 정의되어 있지 않으므로 linearity는 $F$가 아니라 point 사이의 vector를 변환하는 $T_F$에 요구한다.

먼저 $T_F$의 덧셈 보존은 별도로 요구하지 않아도 시작점 독립성에서 따라온다. $a\in A$, $v,w\in V_A$에 대해 다음이 성립한다.

$$
\begin{aligned}
T_F(v+w)
&=F(a+v+w)-F(a) \\
&=\big(F((a+v)+w)-F(a+v)\big)
  +\big(F(a+v)-F(a)\big) \\
&=T_F(w)+T_F(v) \\
&=T_F(v)+T_F(w).
\end{aligned}
$$

첫 번째 항은 시작점 $a+v$에 vector $w$를 적용했을 때 얻는 변환된 vector다. 시작점 독립성에 의해 이 값은 $T_F(w)$다. 따라서 point에 vector를 연속해서 더하는 연산은 자동으로 보존된다.

다음으로 $V_A,V_B$의 scalar multiplication을 보존하려면 모든 $\lambda\in\mathbb F$와 $v\in V_A$에 대해 다음 조건이 필요하다.

$$
T_F(\lambda v)=\lambda T_F(v).
$$

이 조건이 성립하면 vector $v$를 $\lambda$배 한 vector를 적용한 point도 변환 후에 같은 $\lambda$배 관계를 유지한다.

$$
F(a+\lambda v)
=F(a)+T_F(\lambda v)
=F(a)+\lambda T_F(v).
$$

Scalar multiplication의 보존은 시작점 독립성만으로는 일반적으로 따라오지 않는다. 예를 들어 $\mathbb C$를 $\mathbb C$ 위의 affine space로 보고 $F(z)=\overline z$라고 하자. 이때

$$
F(a+v)-F(a)=\overline v
$$

이므로 $T_F(v):=\overline v$로 정의하면 well-defined이고 덧셈을 보존한다. 하지만 다음과 같이 complex scalar multiplication은 보존하지 않는다.

$$
T_F(iv)=\overline{iv}=-i\overline v
\neq
i\overline v=iT_F(v).
$$

따라서 이 함수는 $\mathbb R$ 위에서는 affine map이지만 $\mathbb C$ 위에서는 affine map이 아니다.

한마디로 정리하면, 주어진 함수 $F:A\rightarrow B$가 affine map이기 위한 필요충분조건은 모든 시작점별 후보 $T_{F,a}$가 하나의 공통된 map $T_F$로 일치하고, 이 $T_F$가 scalar multiplication을 보존하는 것이다. 이를 식으로 쓰면 다음 두 조건이다.

$$
F(a+v)-F(a)=F(c+v)-F(c),
\qquad \forall a,c\in A,\ v\in V_A,
$$

$$
F(a+\lambda v)-F(a)
=
\lambda\big(F(a+v)-F(a)\big),
\qquad \forall a\in A,\ \lambda\in\mathbb F,\ v\in V_A.
$$

첫 번째 조건으로 공통된 map $T_F(v):=F(a+v)-F(a)$가 well-defined이고, 그 덧셈 보존은 자동으로 따라온다. 두 번째 조건은 이 공통된 map의 scalar multiplication 보존을 $F$만으로 표현한 것이다. 따라서 두 조건을 함께 만족하면 $T_F$는 $\mathbb F$-linear map이 되고 $F$는 Definition의 조건을 만족한다.

### 기준점으로 affine map을 표현하고 구성하기

#### 핵심 명제

Source의 기준점 $p_0\in A$를 하나 고정하자. 이 기준점 아래에서 affine map $F:A\rightarrow B$는 다음 두 데이터에 의해 유일하게 결정된다.

$$
q_0\in B,
\qquad
L:V_A\rightarrow V_B\text{ linear}.
$$

구체적으로 $F$는 다음 식으로 주어진다.

$$
\boxed{
F(p)=q_0+L(p-p_0)
}.
$$

여기서 $q_0$는 기준점 $p_0$의 image이고 $L$은 $F$의 associated linear map이다. 반대로 $q_0$와 $L$을 임의로 선택하면 이 식으로 새로운 affine map을 정의할 수 있다. 다음 두 방향을 각각 확인하자.

#### 주어진 affine map에서 데이터 추출하기

먼저 affine map $F:A\rightarrow B$가 이미 주어졌다고 하자. 기준점 $p_0\in A$를 고정하고 다음과 같이 두자.

$$
q_0:=F(p_0),
\qquad
L:=T_F.
$$

임의의 $p\in A$는 기준점과 그 기준점으로부터의 변위를 사용하여 다음과 같이 표현된다.

$$
p=p_0+(p-p_0).
$$

Affine map의 정의를 적용하면 다음을 얻는다.

$$
\begin{aligned}
F(p)
&=F\big(p_0+(p-p_0)\big) \\
&=F(p_0)+T_F(p-p_0) \\
&=q_0+L(p-p_0).
\end{aligned}
$$

따라서 주어진 affine map에서 기준점의 image $q_0$와 associated linear map $L$을 추출할 수 있다.

#### 데이터로 affine map 구성하기

이번에는 affine map $F$가 미리 주어지지 않았다고 하자. 고정된 $p_0\in A$에 대해 다음 데이터를 임의로 선택한다.

$$
q_0\in B,
\qquad
L:V_A\rightarrow V_B\text{ linear}.
$$

이 데이터로 함수 $F_{q_0,L}:A\rightarrow B$를 다음과 같이 정의한다.

$$
\boxed{
F_{q_0,L}(p):=q_0+L(p-p_0)
}.
$$

각 $p\in A$에 대해 $p-p_0\in V_A$가 유일하므로 이 함수는 well-defined다. 먼저 기준점에는 다음 값이 대응한다.

$$
F_{q_0,L}(p_0)
=q_0+L(0)
=q_0.
$$

또한 $a\in A$, $v\in V_A$에 대해 다음이 성립한다.

$$
\begin{aligned}
F_{q_0,L}(a+v)
&=q_0+L\big((a+v)-p_0\big) \\
&=q_0+L(a-p_0)+L(v) \\
&=F_{q_0,L}(a)+L(v).
\end{aligned}
$$

따라서 $F_{q_0,L}$은 associated linear map이 $L$인 affine map이다. 즉, affine map을 미리 알지 못하더라도 $p_0$, $q_0$, $L$을 선택하면 하나의 affine map을 구성할 수 있다.

#### 고정된 기준점에서의 유일성

기준점 $p_0$가 고정되어 있으면 affine map $F$를 나타내는 $q_0$와 $L$은 유일하다. 실제로 반드시 다음과 같아야 한다.

$$
q_0=F(p_0),
\qquad
L=T_F.
$$

따라서 고정된 $p_0$에 대해 affine map과 데이터 쌍 사이에 다음 대응이 성립한다.

$$
\boxed{
F\longleftrightarrow \big(F(p_0),T_F\big)
}.
$$

#### 기준점 변경

기준점 자체는 유일하지 않다. 새로운 기준점 $p_1\in A$를 선택하면 그 image는 다음과 같다.

$$
q_1:=F(p_1)
=q_0+L(p_1-p_0).
$$

같은 affine map을 새로운 기준점으로 표현하면 다음과 같다.

$$
F(p)=q_1+L(p-p_1).
$$

즉, 기준점을 바꾸면 기준점의 image도 함께 바뀌지만 associated linear map $L=T_F$는 바뀌지 않는다.

#### 함수 합성으로 쓴 기준점 표현

[Affine space](<./11 Affine space.md>)에서 정의한 bijection을 source와 target에 대해 각각 다음과 같이 표기하자.

$$
\phi_{p_0}^A:V_A\rightarrow A,
\qquad
v\mapsto p_0+v,
$$

$$
\phi_{q_0}^B:V_B\rightarrow B,
\qquad
w\mapsto q_0+w.
$$

이때 $(\phi_{p_0}^A)^{-1}(p)=p-p_0$이므로 기준점 표현은 다음 함수 합성으로 쓸 수 있다.

$$
F
=
\phi_{q_0}^B
\circ T_F
\circ (\phi_{p_0}^A)^{-1}.
$$

Source의 기준점 $p_0$와 target에서 그 image인 $q_0=F(p_0)$를 구분해야 한다. 특히 $A=B$인 경우에도 일반적인 affine map에서는 $q_0=p_0$일 필요가 없다. $q_0=p_0$를 요구하면 $p_0$를 고정하는 affine map만 얻는다.

### Affine combination의 보존

Definition에서 point 사이의 vector를 linear하게 변환하도록 정의한 affine map이 affine combination도 같은 계수로 보존하는지 확인하자.

$p_1,\ldots,p_n\in A$와 $c_1,\ldots,c_n\in\mathbb F$가 다음을 만족한다고 하자.

$$
\sum_{i=1}^n c_i=1.
$$

이때 affine combination은 임의의 기준 point $p_1$을 사용하여 다음과 같이 표현할 수 있다.

$$
\sum_{i=1}^n c_ip_i
=
p_1+\sum_{i=1}^n c_i(p_i-p_1).
$$

Affine map $F$는 다음과 같이 affine combination을 보존한다.

$$
F\left(\sum_{i=1}^n c_ip_i\right)
=
\sum_{i=1}^n c_iF(p_i).
$$

실제로 다음이 성립한다.

$$
\begin{aligned}
F\left(p_1+\sum_{i=1}^n c_i(p_i-p_1)\right)
&=F(p_1)+T_F\left(\sum_{i=1}^n c_i(p_i-p_1)\right) \\
&=F(p_1)+\sum_{i=1}^n c_i\big(F(p_i)-F(p_1)\big) \\
&=\sum_{i=1}^n c_iF(p_i).
\end{aligned}
$$

따라서 affine map은 midpoint, line 위의 affine parameter, barycenter처럼 계수의 합이 $1$인 관계를 보존한다. Affine combination의 정의와 기준 point의 선택에 무관한 이유는 [Affine Combination](<./12 Affine Combination.md>)에서 설명한다.

### Composition

Affine map $F:A\rightarrow B$와 $G:B\rightarrow C$가 있으면 $G\circ F:A\rightarrow C$도 affine map이며 associated linear map은 다음과 같다.

$$
T_{G\circ F}=T_G\circ T_F.
$$

실제로 $a,b\in A$에 대해 다음이 성립한다.

$$
\begin{aligned}
G(F(b))-G(F(a))
&=T_G(F(b)-F(a)) \\
&=T_G(T_F(b-a)).
\end{aligned}
$$

이처럼 affine map은 affine structure를 보존하며 합성에 대해서도 닫혀 있다.

## Coordinate Representation

### $y=Mx+t$의 도출

$A$의 coordinate system을 $(a_0,\beta)$, $B$의 coordinate system을 $(b_0,\gamma)$라고 하자. $p\in A$와 $F(p)\in B$의 coordinate column vector를 각각 다음과 같이 두자.

$$
x:=[p]_{(a_0,\beta)}=[p-a_0]_{\beta},
$$

$$
y:=[F(p)]_{(b_0,\gamma)}=[F(p)-b_0]_{\gamma}.
$$

Associated linear map의 matrix representation과 translation coordinate를 다음과 같이 정의하자.

$$
M:=[T_F]_{\beta}^{\gamma},
\qquad
t:=[F(a_0)-b_0]_{\gamma}.
$$

그러면 다음이 성립한다.

$$
\begin{aligned}
y
&=[F(p)-b_0]_{\gamma} \\
&=[F(p)-F(a_0)]_{\gamma}+[F(a_0)-b_0]_{\gamma} \\
&=[T_F(p-a_0)]_{\gamma}+t \\
&=M[p-a_0]_{\beta}+t \\
&=Mx+t.
\end{aligned}
$$

따라서 $y=Mx+t$는 affine map의 정의가 아니라, affine map에 source와 target의 coordinate system을 선택하여 얻은 matrix representation이다.

- $M$은 vector에 작용하는 $T_F$의 matrix representation이다.
- $t$는 source coordinate origin $a_0$의 image $F(a_0)$를 target coordinate system에서 표현한 coordinate다.

$M$의 값은 $V_A,V_B$에서 선택한 basis $\beta,\gamma$에 의존한다. Basis에 따른 linear map의 matrix representation은 [Change of Basis and Coordinate Matrix](<../07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)에서 설명한다.

두 coordinate $x_1,x_2$에 대해 translation term은 차이를 계산할 때 소거된다.

$$
(Mx_2+t)-(Mx_1+t)=M(x_2-x_1).
$$

이는 $F(b)-F(a)=T_F(b-a)$를 coordinate로 표현한 식이다.

### Homogeneous coordinate

$x\in\mathbb F^n$, $y\in\mathbb F^m$에 homogeneous coordinate를 사용하면 affine 식 $y=Mx+t$를 하나의 matrix multiplication으로 표현할 수 있다.

$$
\bar{x}:=
\begin{bmatrix}
x\\1
\end{bmatrix},
\qquad
\bar{y}:=
\begin{bmatrix}
y\\1
\end{bmatrix}.
$$

$$
\bar{y}
=
\begin{bmatrix}
M&t\\
0&1
\end{bmatrix}
\bar{x}.
$$

추가한 마지막 coordinate 덕분에 translation을 포함한 affine map을 더 높은 차원의 linear map으로 계산할 수 있다.

## Affine Transformation

Affine space $A$에서 자기 자신으로 가는 affine map $F:A\rightarrow A$를 `affine endomorphism`이라고 한다. 이 문서에서는 affine endomorphism을 넓은 의미의 `affine transformation`이라고 부른다.

문헌에 따라 `affine transformation`을 bijective affine map에만 사용하는 경우도 있다. Bijective affine endomorphism은 `affine automorphism`이라고 하며, 다음 조건은 동치다.

1. $F$는 affine automorphism이다.
2. Associated linear map $T_F:V_A\rightarrow V_A$는 invertible이다.
3. 하나의 coordinate system에서 얻은 linear part $M$은 invertible matrix다.

$p_0\in A$, $q_0=F(p_0)$라고 하면 inverse는 다음과 같이 표현된다.

$$
F^{-1}(q)
=
p_0+T_F^{-1}(q-q_0).
$$

Coordinate representation $y=Mx+t$에서는 다음과 같다.

$$
x=M^{-1}(y-t).
$$

## Examples

### Translation

$t\in V_A$에 대해 다음과 같이 정의한 함수는 translation이다.

$$
F_t:A\rightarrow A, \qquad p\mapsto p+t.
$$

두 point를 잇는 vector는 변하지 않는다.

$$
F_t(b)-F_t(a)=(b+t)-(a+t)=b-a.
$$

따라서 translation의 associated linear map은 identity map이다.

$$
T_{F_t}=\operatorname{id}_{V_A}.
$$

### Rotation about a point

Euclidean affine space $A$에서 rotation linear map $R:V_A\rightarrow V_A$와 rotation center $c\in A$가 있다고 하자. 다음 함수는 $c$를 중심으로 하는 rotation이다.

$$
F(p)=c+R(p-c).
$$

이 함수의 associated linear map은 $R$이다.

$$
F(b)-F(a)=R(b-a).
$$

$\mathbb R^n$ coordinate에서 $c$를 coordinate column vector로도 표기하면 다음과 같다.

$$
F(x)=R(x-c)+c=Rx+(I-R)c.
$$

따라서 origin이 아닌 point를 중심으로 회전하면 coordinate 식에는 translation term $(I-R)c$가 나타난다.

### Scaling과 shear

$M$이 scaling 또는 shear matrix이면 $F(x)=Mx+t$도 affine transformation이다. 하지만 일반적으로 length와 angle을 보존하지 않으므로 rigid transformation은 아니다.

## Rigid Transformation과의 관계

Euclidean affine space에서 associated linear map이 orthogonal map인 affine transformation은 distance와 angle을 보존하는 `rigid transformation`이다. Coordinate representation은 다음과 같다.

$$
y=Rx+t,
\qquad
R^{\mathsf T}R=I.
$$

$\det R=1$이면 $R$은 rotation이고, $y=Rx+t$는 rotation과 translation으로 이루어진 orientation-preserving rigid transformation이다. 반면 일반적인 affine transformation의 $M$에는 scaling, shear, reflection 등이 포함될 수 있다.

Rigid transformation에서도 두 coordinate의 차를 나타내는 vector에는 rotation만 작용한다.

$$
(Rx_2+t)-(Rx_1+t)=R(x_2-x_1).
$$

따라서 rigid transformation의 associated linear map은 $R$이고, $t$는 선택한 coordinate origin 사이의 위치 관계를 나타낸다.

같은 식 $y=Rx+t$는 문맥에 따라 서로 다른 의미를 가질 수 있다.

- Active transformation에서는 point를 실제로 다른 point로 옮긴다.
- Passive coordinate transformation에서는 같은 geometric point를 다른 coordinate system의 coordinate로 다시 표현한다.

두 해석의 수학적 형태는 같지만 geometric 의미는 다르다.

Determinant가 $+1$인 orthogonal linear part와 translation을 결합한 3차원 rigid
transformation의 group 구조는
[Rigid Transformation and SE(3)](<./23 Rigid Transformation and SE(3).md>)에서
설명한다.

## 관련 문서

- [Affine space](<./11 Affine space.md>)
- [Affine Combination](<./12 Affine Combination.md>)
- [Change of Basis and Coordinate Matrix](<../07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)
- [Rotation Matrix and SO(3)](<./22 Rotation Matrix and SO(3).md>)
- [Rigid Transformation and SE(3)](<./23 Rigid Transformation and SE(3).md>)

## Reference

- [Wikipedia - Affine space: Affine maps](https://en.wikipedia.org/wiki/Affine_space#Affine_maps)
- [Wikipedia - Affine transformation](https://en.wikipedia.org/wiki/Affine_transformation)
