# Affine Transformation

Affine map은 한 기준점의 image와 모든 vector에 공통으로 작용하는 linear map으로 결정된다. Coordinate system을 선택하면 이 구조는 익숙한 $y=Mx+t$ 형태로 표현된다.

이 문서는 [Affine space](<./11 Affine space.md>)에서 정의한 point, vector, subtraction과 [Affine Combination](<./12 Affine Combination.md>)에서 정의한 affine combination을 사용한다.

## Affine Homomorphism

### Motivation

같은 field $\mathbb F$ 위의 affine space $A,B$가 있고, associated vector space를 각각 $V_A,V_B$라고 하자.

Affine space의 구조는 point들의 집합만을 뜻하지 않는다. Associated vector space와 point에 vector를 더하는 다음 연산을 함께 뜻한다.

$$
+:A\times V_A\rightarrow A,
\qquad
(a,v)\mapsto a+v.
$$

이 연산은 두 point를 잇는 vector와 point 사이의 상대적 위치 관계를 정한다. 반면 특별한 origin이나 coordinate, distance, angle은 affine structure에 포함되지 않는다. 합이 $1$인 계수로 만든 affine combination은 이러한 상대적 위치 관계를 표현하는 방법이다.

함수 $F:A\rightarrow B$가 이 affine structure를 보존한다는 것이 무엇인지 생각해 보자.

[Affine Combination](<./12 Affine Combination.md>)에서 살펴본 것처럼, point $p_1,\ldots,p_n$과 합이 $1$인 계수 $c_1,\ldots,c_n$을 정하면 다음 affine combination은 하나의 point $q$와 point 사이의 상대적 위치 관계를 정한다.

$$
q:=\sum_{i=1}^n c_ip_i,
\qquad
\sum_{i=1}^n c_i=1.
$$

이 등식은 $q$가 하나의 point라는 사실뿐만 아니라, $q$의 위치가 $p_1,\ldots,p_n$에 대해 계수 $c_1,\ldots,c_n$으로 정해진다는 상대적 위치 관계를 표현한다. 예를 들어 $2$의 multiplicative inverse가 $\mathbb F$에 존재할 때 다음 식은 point $m$을 정하는 동시에 $m$이 $a,b$의 midpoint라는 관계를 나타낸다.

$$
m=\frac12a+\frac12b.
$$

이러한 affine-combination 관계는 특별한 origin이나 coordinate를 선택하지 않아도 의미가 있다. 따라서 affine structure를 보존하는 함수 $F:A\rightarrow B$라면 변환 전의 관계가 변환 후에도 같은 계수로 성립해야 한다. 즉, $q=\sum_{i=1}^n c_ip_i$이면 다음이 성립해야 한다.

$$
F(q)=\sum_{i=1}^n c_iF(p_i).
$$

이를 하나의 식으로 쓰면 다음과 같다.

$$
F\left(\sum_{i=1}^n c_ip_i\right)
=
\sum_{i=1}^n c_iF(p_i),
\qquad
\sum_{i=1}^n c_i=1.
$$

이 조건을 직접 확인하는 대신, 같은 vector를 서로 다른 point에 적용했을 때 그 vector가 $F$에 의해 어떻게 변하는지 살펴보면 더 간결한 조건을 얻을 수 있다.

#### 서로 다른 시작점에 적용한 같은 vector

같은 vector $v\in V_A$를 서로 다른 시작점 $a,c\in A$에 적용하면 각각 $a+v$, $c+v$를 얻는다. 두 point 쌍이 나타내는 vector는 다음과 같이 같다.

$$
(a+v)-a=v=(c+v)-c.
$$

이 관계를 네 point 사이의 affine relation으로 표현하면 다음과 같다.

$$
c+v=[-a+(a+v)+c]_{\mathrm{aff}}.
$$

우변의 계수 합은 $-1+1+1=1$이다. $F$가 affine combination을 보존한다면 다음이 성립한다.

$$
F(c+v)=[-F(a)+F(a+v)+F(c)]_{\mathrm{aff}}.
$$

[Affine Combination](<./12 Affine Combination.md>)의 `기준 point를 이용한 affine combination의 계산`에 따라 $F(c)$를 기준 point로 선택하면 다음이 성립한다.

$$
\begin{aligned}
F(c+v)
&=F(c)+\Big(
-\big(F(a)-F(c)\big)
+\big(F(a+v)-F(c)\big)
\Big) \\
&=F(c)+\Big(
\big(F(a+v)-F(c)\big)
+\big(F(c)-F(a)\big)
\Big) \\
&=F(c)+\big(F(a+v)-F(a)\big).
\end{aligned}
$$

두 번째 줄에서는 vector의 additive inverse와 commutativity를 사용했고, 마지막 줄에서는 연쇄 차이 공식을 사용했다.

이제 양변의 point에서 $F(c)$를 직접 대수적으로 소거하는 것이 아니라, $F(c)$에서 각 point로 가는 vector를 취한다. Point와 vector의 action 및 subtraction 관계 $(p+w)-p=w$에 의해 다음을 얻는다.

$$
F(c+v)-F(c)=F(a+v)-F(a).
$$

즉, affine combination을 보존하는 $F$는 어떤 시작점에 적용한 vector $v$도 하나의 공통된 vector로 변환해야 한다. 이를 나타내는 함수 $T:V_A\rightarrow V_B$를 찾으면 다음과 같이 쓸 수 있다.

$$
F(a+v)=F(a)+T(v).
$$

#### Associated linear map의 후보

함수 $F:A\rightarrow B$가 주어지면 다음 함수는 항상 정의할 수 있다.

$$
D_F:A\times V_A\rightarrow V_B,
\qquad
D_F(a,v):=F(a+v)-F(a).
$$

여기서 $D_F(a,v)$는 시작점 $a$에 vector $v$를 적용했을 때 변환된 두 point $F(a),F(a+v)$를 잇는 vector다. $D_F$는 시작점 $a$와 vector $v$를 모두 입력으로 받는다.

반면 앞 절에서 찾고자 한 함수 $T_F:V_A\rightarrow V_B$는 vector $v$만을 입력으로 받아야 한다. 따라서 $D_F(a,v)$가 $a$에 의존하지 않아야 한다. 즉, $F$가 다음 조건을 만족해야 한다.

$$
F(a+v)-F(a)
=
F(c+v)-F(c),
\qquad \forall a,c\in A,\ v\in V_A.
$$

이 등식은 임의의 함수 $F$에 대해 자동으로 성립하는 명제가 아니라, $F$가 시작점과 무관한 map $T_F$를 유도하기 위해 만족해야 하는 조건이다. 이 조건이 성립하면 어떤 시작점 $a\in A$를 선택해도 같은 값을 얻으므로 다음과 같이 정의할 수 있다.

$$
T_F(v):=D_F(a,v)=F(a+v)-F(a).
$$

우변이 $a$의 선택과 무관하므로 $T_F$는 `well-defined` 함수다. 즉, 변환된 vector는 시작점이 아니라 원래 vector $v$에만 의존한다.

예를 들어 $\mathbb R$을 affine line으로 보고 $F(x)=x^2$라고 하자. 같은 vector $v=1$을 두 시작점 $a=0$, $c=1$에 각각 적용하면 변환된 vector는 서로 다르다.

$$
F(a+v)-F(a)=F(0+1)-F(0)=1,
$$

$$
F(c+v)-F(c)=F(1+1)-F(1)=3.
$$

따라서 같은 입력 vector $1$에 서로 다른 출력이 대응하므로 $T_F$를 잘 정의할 수 없다.

반면 $F(x)=2x+5$이면 다음이 성립한다.

$$
F(a+v)-F(a)=2v.
$$

이 경우에는 시작점과 관계없이 $T_F(v)=2v$로 정의할 수 있다. 모든 point에 공통으로 더해진 translation $5$는 $F(a+v)-F(a)$를 계산할 때 소거된다.

#### Linear map이 필요한 이유

여기까지는 $F$가 각 vector $v$에 시작점과 무관한 변환된 vector를 대응시켜 well-defined map $T_F:V_A\rightarrow V_B$를 유도하기 위한 조건을 확인했다. Affine space의 point에는 덧셈과 scalar multiplication이 정의되어 있지 않으므로 $F$ 자체에 vector space의 linearity 조건을 적용할 수는 없다. 대신 associated vector space $V_A,V_B$의 연산이 어떻게 변하는지를 살펴봐야 한다.

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

다음으로 affine combination의 계수를 보존하려면 $T_F$가 scalar multiplication을 보존해야 한다. $b:=a+v$로 두고, $a,b$의 affine parameter $\lambda\in\mathbb F$에 해당하는 point를 $p_\lambda$라고 하자.

$$
p_\lambda
:=(1-\lambda)a+\lambda b
=a+\lambda(b-a)
=a+\lambda v.
$$

Affine combination의 계수까지 보존한다면 $p_\lambda$의 image는 다음과 같아야 한다.

$$
\begin{aligned}
F(p_\lambda)
&=(1-\lambda)F(a)+\lambda F(b) \\
&=F(a)+\lambda\big(F(b)-F(a)\big) \\
&=F(a)+\lambda T_F(v).
\end{aligned}
$$

한편 $p_\lambda=a+\lambda v$이므로 $T_F$의 정의에 따라 다음도 성립한다.

$$
F(p_\lambda)=F(a)+T_F(\lambda v).
$$

두 식이 같은 point를 나타내므로 다음 조건을 얻는다.

$$
T_F(\lambda v)=\lambda T_F(v).
$$

이 조건은 시작점 독립성만으로는 일반적으로 따라오지 않는다. 예를 들어 $\mathbb C$를 $\mathbb C$ 위의 affine space로 보고 $F(z)=\overline z$라고 하자. 이때

$$
F(a+v)-F(a)=\overline v
$$

이므로 $T_F(v)=\overline v$는 well-defined이고 덧셈을 보존한다. 하지만 다음과 같이 complex scalar multiplication은 보존하지 않는다.

$$
T_F(iv)=\overline{iv}=-i\overline v
\neq
i\overline v=iT_F(v).
$$

따라서 이 함수는 $\mathbb R$ 위에서는 affine map이지만 $\mathbb C$ 위에서는 affine map이 아니다. 같은 field $\mathbb F$의 계수를 사용하는 affine combination을 보존하려면 $T_F$가 scalar multiplication도 보존해야 한다. 이미 얻은 덧셈 보존과 이 조건을 합치면 $T_F$는 $\mathbb F$-linear map이 된다.

### Definition

같은 field $\mathbb F$ 위의 affine space $A,B$와 함수 $F:A\rightarrow B$가 있다고 하자. 다음을 만족하는 linear map $T_F:V_A\rightarrow V_B$가 존재할 때, $F$를 `affine homomorphism` 또는 `affine map`이라고 한다.

$$
F(b)-F(a)=T_F(b-a), \qquad \forall a,b\in A.
$$

$T_F$를 $F$의 `associated linear map` 또는 `linear part`라고 한다.

임의의 $v\in V_A$와 $a\in A$에 대해 $b=a+v$로 둘 수 있으므로 다음이 성립한다.

$$
T_F(v)=F(a+v)-F(a).
$$

따라서 $T_F$가 존재하면 유일하다. Motivation에서 살펴본 well-defined 조건은 위 식의 우변이 $a$의 선택과 무관하게 오직 $v$에 의해서만 결정된다는 뜻이다.

> Reference  
> [Wikipedia - Affine space: Affine maps](https://en.wikipedia.org/wiki/Affine_space#Affine_maps)

### Point와 vector에 대한 동치 표현

Affine space $A,B$와 함수 $F:A\rightarrow B$가 있다고 하자. 그러면 다음 두 조건은 동치다.

1. $F$는 associated linear map $T_F$를 갖는 affine map이다.
2. Linear map $T_F:V_A\rightarrow V_B$가 존재하여 모든 $a\in A$, $v\in V_A$에 대해 다음을 만족한다.

$$
F(a+v)=F(a)+T_F(v).
$$

**Proof**

$F$가 affine map이면 $(a+v)-a=v$이므로 다음이 성립한다.

$$
F(a+v)-F(a)=T_F((a+v)-a)=T_F(v).
$$

반대로 위 식을 만족하는 linear map $T_F$가 있다고 하자. $b=a+(b-a)$이므로 다음이 성립한다.

$$
F(b)-F(a)=T_F(b-a).
$$

따라서 $F$는 affine map이다. $\qed$

이 식은 affine map의 역할을 두 부분으로 나누어 보여준다.

1. 기준 point $a$를 $F(a)$로 보낸다.
2. $a$에 더하는 vector $v$를 $T_F(v)$로 변환한다.

한 point의 image와 associated linear map을 알면 다른 모든 point의 image가 결정된다.

### Affine combination의 보존

Definition에서 vector의 변환을 통해 정의한 affine map이 Motivation에서 목표로 삼은 affine combination을 실제로 보존하는지 확인하자.

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

## 기준점에 의한 표현

Affine map은 source의 기준점 하나, 그 기준점의 image, 그리고 associated linear map으로 완전히 표현할 수 있다.

$p_0\in A$를 선택하고 $q_0:=F(p_0)\in B$라고 하자. 임의의 $p\in A$에 대해 $p=p_0+(p-p_0)$이므로 다음이 성립한다.

$$
F(p)=q_0+T_F(p-p_0).
$$

반대로 $p_0\in A$, $q_0\in B$와 linear map $L:V_A\rightarrow V_B$를 임의로 선택하고 다음과 같이 $F$를 정의하자.

$$
F(p):=q_0+L(p-p_0).
$$

그러면 다음이 성립한다.

$$
F(b)-F(a)
=L(b-p_0)-L(a-p_0)
=L(b-a).
$$

따라서 $F$는 $T_F=L$인 affine map이다. 즉, 모든 affine map은 하나의 기준점 표현을 가지며, 위와 같이 만든 모든 함수는 affine map이다.

[Affine space](<./11 Affine space.md>)에서 정의한 bijection을 구분하기 위해 다음과 같이 표기하자.

$$
\phi_{p_0}^A:V_A\rightarrow A, \qquad v\mapsto p_0+v,
$$

$$
\phi_{q_0}^B:V_B\rightarrow B, \qquad w\mapsto q_0+w.
$$

그러면 기준점 표현은 다음 함수 합성으로 쓸 수 있다.

$$
F
=
\phi_{q_0}^B
\circ T_F
\circ (\phi_{p_0}^A)^{-1}.
$$

여기서 source의 기준점 $p_0$와 target의 기준점 $q_0=F(p_0)$를 구분해야 한다. $q_0=p_0$로 제한하면 $p_0$를 고정하는 affine map만 얻으므로 일반적인 translation을 표현할 수 없다.

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

## 관련 문서

- [Affine space](<./11 Affine space.md>)
- [Affine Combination](<./12 Affine Combination.md>)
- [Change of Basis and Coordinate Matrix](<../07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)

## Reference

- [Wikipedia - Affine space: Affine maps](https://en.wikipedia.org/wiki/Affine_space#Affine_maps)
- [Wikipedia - Affine transformation](https://en.wikipedia.org/wiki/Affine_transformation)
