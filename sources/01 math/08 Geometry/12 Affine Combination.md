# Affine Combination

Affine combination은 affine space의 point들을 origin의 선택과 무관하게 결합하는 방법이다. 계수의 합을 $1$로 제한하면 affine parameter, midpoint, barycenter처럼 point 사이의 위치 관계를 표현할 수 있다.

이 문서는 [Affine space](<./11 Affine space.md>)에서 정의한 point, vector, subtraction을 사용한다.

## Motivation

Affine space $A$의 point에는 덧셈과 scalar multiplication이 정의되어 있지 않으므로 다음 식은 그대로는 의미가 없다.

$$
c_1p_1+\cdots+c_np_n.
$$

하지만 기준 point $o\in A$를 하나 선택하면 각 point $p_i$를 vector $p_i-o\in V_A$로 나타낼 수 있다. Vector는 linear combination할 수 있으므로 다음 point는 정의할 수 있다.

$$
o+\sum_{i=1}^n c_i(p_i-o).
$$

이 식이 affine space 자체의 연산이 되려면 결과가 임의로 선택한 기준 point $o$에 의존하지 않아야 한다. 계수의 합이 $1$이라는 조건이 바로 이 독립성을 보장한다.

## Definition

Field $\mathbb F$ 위의 affine space $A$와 point $p_1,\ldots,p_n\in A$, scalar $c_1,\ldots,c_n\in\mathbb F$가 다음을 만족한다고 하자.

$$
\sum_{i=1}^n c_i=1.
$$

임의의 기준 point $o\in A$를 선택하여 다음 point를 생각하자.

$$
o+\sum_{i=1}^n c_i(p_i-o).
$$

이 point는 $o$의 선택과 무관하며, 이를 $p_1,\ldots,p_n$의 `affine combination`이라고 한다. 따라서 point $p_1,\ldots,p_n$과 계수 $c_1,\ldots,c_n$을 정하면 affine combination의 결과는 $A$에 속하는 하나의 point로 정해진다.

다음 표기는 point의 일반적인 덧셈이나 scalar multiplication이 아니라, affine combination 전체를 나타내는 약속이다.

$$
\sum_{i=1}^n c_ip_i
:=
o+\sum_{i=1}^n c_i(p_i-o).
$$

### 기준 point의 선택과 무관함

다른 기준 point $q\in A$를 선택하자. 각 $i$에 대해 다음이 성립한다.

$$
p_i-q=(p_i-o)+(o-q).
$$

따라서 다음이 성립한다.

$$
\begin{aligned}
q+\sum_{i=1}^n c_i(p_i-q)
&=q+\left(
\sum_{i=1}^n c_i(p_i-o)
+\left(\sum_{i=1}^n c_i\right)(o-q)
\right) \\
&=q+\left(\sum_{i=1}^n c_i(p_i-o)+(o-q)\right) \\
&=(q+(o-q))+\sum_{i=1}^n c_i(p_i-o) \\
&=o+\sum_{i=1}^n c_i(p_i-o).
\end{aligned}
$$

그러므로 어느 기준 point를 사용해도 같은 affine combination을 얻는다.

### 계수 합에 따른 affine expression의 해석

Affine combination의 표기는 point들을 일반적인 대수식처럼 계산하는 것으로 보일 수 있다. 이 표기를 정확히 구분하기 위해 point $p_1,\ldots,p_n\in A$로 이루어진 다음 `formal affine expression`을 생각하자.

$$
E=\sum_{i=1}^n c_ip_i.
$$

여기서 각 $c_ip_i$는 point에 대한 실제 scalar multiplication이 아니라 expression 안에서 point 기호 $p_i$의 coefficient를 기록하는 형식적인 항이다. Formal expression의 덧셈과 scalar multiplication도 point에 적용하는 연산이 아니라 각 point 기호의 coefficient에 적용하는 연산으로 정의한다. 이 연산에 대해 formal expression들의 집합은 vector space가 되지만, 그 원소는 $A$의 point가 아니라 형식적인 기호식이다.

두 formal expression에서 각 point 기호의 coefficient를 모두 모았을 때 같은 coefficient를 얻으면 두 expression은 형식적으로 같다. 따라서 formal expression 단계에서는 coefficient sum과 관계없이 같은 point 기호의 coefficient를 모으거나 소거할 수 있다. 예를 들어 다음은 point를 직접 연산한 결과가 아니라 각 기호의 coefficient를 정리한 형식적 등식이다.

$$
p-q+q-r=p-r.
$$

이 형식적 계산이 실제 affine space의 vector나 point에 대한 등식을 나타내는지는 별도로 확인해야 한다.

#### 기준 point에서의 vector evaluation

기준 point $o\in A$를 선택하자. Formal expression $E$에 다음 vector를 대응시키는 `vector evaluation` $L_o$를 정의한다.

$$
L_o(E)
:=
\sum_{i=1}^n c_i(p_i-o)
\in V_A.
$$

오른쪽에서는 point difference $p_i-o$가 모두 $V_A$의 vector이므로 일반적인 vector linear combination을 사용할 수 있다. 이 정의에 의해 $L_o$는 formal expression의 coefficient 계산을 $V_A$의 vector 계산으로 보존한다. 실제로 formal expression $E,F$와 scalar $\lambda\in\mathbb F$에 대해 다음이 성립한다.

$$
L_o(E+F)=L_o(E)+L_o(F),
$$

$$
L_o(\lambda E)=\lambda L_o(E).
$$

즉, $L_o$는 formal expression에 대해 linear map이다. 따라서 coefficient를 모으고 소거하여 formal expression $E$와 $F$가 같아지면 실제 vector evaluation도 같다.

$$
E=F
\quad\Longrightarrow\quad
L_o(E)=L_o(F).
$$

예를 들어 앞의 형식적 등식은 $L_o$에 의해 다음 실제 vector 계산으로 옮겨진다.

$$
\begin{aligned}
L_o(p-q+q-r)
&=(p-o)-(q-o)+(q-o)-(r-o) \\
&=(p-o)-(r-o) \\
&=L_o(p-r).
\end{aligned}
$$

중간의 $(q-o)$와 $-(q-o)$는 실제로 $V_A$의 vector이므로 vector addition의 cancellation을 적용할 수 있다. 이것이 formal expression의 coefficient cancellation이 실제 계산과 일치하는 이유다.

#### Coefficient sum과 기준 point 독립성

Formal expression $E$의 `coefficient sum`을 다음과 같이 정의하자.

$$
w(E):=\sum_{i=1}^n c_i.
$$

다른 기준 point $q\in A$를 선택하면 vector evaluation은 다음과 같이 변한다.

$$
\begin{aligned}
L_q(E)
&=\sum_{i=1}^n c_i\big((p_i-o)+(o-q)\big) \\
&=L_o(E)+w(E)(o-q).
\end{aligned}
$$

이제 coefficient sum에 따라 formal expression의 geometric interpretation이 달라진다.

- $w(E)=0$이면 다음이 성립한다.

  $$
  L_q(E)=L_o(E).
  $$

  따라서 $L_o(E)$는 기준 point의 선택과 무관한 하나의 vector를 나타낸다.

- $w(E)=1$이면 $o+L_o(E)$를 $E$가 나타내는 point로 해석할 수 있다. 실제로 $V_A$의 vector addition이 commutative이므로 다음이 성립한다.

  $$
  \begin{aligned}
  q+L_q(E)
  &=q+\big(L_o(E)+(o-q)\big) \\
  &=(q+(o-q))+L_o(E) \\
  &=o+L_o(E).
  \end{aligned}
  $$

  따라서 이 point도 기준 point의 선택과 무관하며, 이것이 앞에서 정의한 affine combination이다.

- $w(E)$가 $0$도 $1$도 아니면 coefficient를 모으고 소거하는 formal calculation 자체는 가능하지만, 그 결과를 기준 point와 무관한 하나의 vector나 point로 해석할 수 없다.

이 문서에서는 $L_o$의 linearity와 위의 기준 point 독립성을 합쳐 `coefficient sum principle`이라고 하자. 이 원리는 다음 두 단계를 구분한다.

$$
\text{formal coefficient calculation}
\xrightarrow{\ L_o\text{의 linearity}\ }
\text{actual vector calculation in }V_A,
$$

$$
w(E)=0\text{ 또는 }1
\quad\Longrightarrow\quad
\text{기준 point와 무관한 vector 또는 point}.
$$

따라서 coefficient sum이 $0$인 formal equality는 기준 point와 무관한 vector equality로, coefficient sum이 $1$인 formal equality는 기준 point와 무관한 point equality로 해석할 수 있다.

그러나 이 원리는 $-p$나 $p+q$ 같은 부분식 자체에 실제 point 연산의 의미를 부여하지 않는다. 예를 들어 $-a+b+c$에서는 세 항 전체의 coefficient sum이 $1$이므로 전체 expression만 affine combination으로 해석한다. 반면 $\mathbb F=\mathbb R$에서 $a+b$는 formal expression으로 coefficient를 정리할 수는 있지만 coefficient sum이 $2$이므로 기준 point와 무관한 point나 vector를 나타내지 않는다.

#### 연쇄 차이 공식

Point $p_0,\ldots,p_n\in A$에 대해 각 $p_k-p_{k+1}$은 vector이고, 다음 expression의 coefficient sum은 $0$이다. 중간 point들의 coefficient를 모으면 다음을 얻는다.

$$
\boxed{
(p_0-p_1)+(p_1-p_2)+\cdots+(p_{n-1}-p_n)
=p_0-p_n
}.
$$

특히 다음이 성립한다.

$$
(p-q)+(q-r)=p-r.
$$

이 식에서 일어나는 소거는 point addition이 아니라 이미 vector가 된 point difference들의 덧셈에서 일어난다. 이 공식의 affine space 정의에 의한 직접적인 유도는 [Affine space](<./11 Affine space.md>)의 `두 point를 잇는 vector의 합` 절에서 설명한다.

#### Affine combination의 재기준화

Point $s$가 다음 affine combination으로 주어졌다고 하자.

$$
s:=\sum_{i=1}^n c_ip_i,
\qquad
\sum_{i=1}^n c_i=1.
$$

Affine combination의 정의에서 임의의 point $q\in A$를 기준 point로 선택할 수 있으므로 다음 `re-anchoring formula`가 성립한다.

$$
\boxed{
s=q+\sum_{i=1}^n c_i(p_i-q)
}.
$$

특히 $q=p_k$로 선택하면 $p_k-p_k=0$이므로 다음을 얻는다.

$$
\boxed{
s=p_k+\sum_{i\ne k}c_i(p_i-p_k)
}.
$$

예를 들어 coefficient가 $-1,1,1$인 affine combination을 의미가 드러나도록 다음과 같이 쓰자.

$$
s:=[-a+b+c]_{\mathrm{aff}}.
$$

$b$와 $c$를 각각 기준 point로 선택하고 연쇄 차이 공식을 적용하면 다음이 성립한다.

$$
\begin{aligned}
s
&=b+\big(-(a-b)+(c-b)\big)
=b+(c-a), \\
s
&=c+\big(-(a-c)+(b-c)\big)
=c+(b-a).
\end{aligned}
$$

따라서 다음 두 표현은 같은 point를 나타낸다.

$$
[-a+b+c]_{\mathrm{aff}}
=b+(c-a)
=c+(b-a).
$$

오른쪽의 각 식은 point 하나에 vector 하나를 더하는 실제 affine space 연산이다. 반면 왼쪽의 $-a+b+c$는 항별 point 연산이 아니라 coefficient sum이 $1$인 formal affine expression 전체를 나타낸다.

### Affine hull

Point $p_1,\ldots,p_n$을 고정하고 계수의 합이 $1$이라는 조건 아래에서 가능한 모든 계수를 선택하자. 이렇게 얻은 affine combination들을 모은 집합을 $p_1,\ldots,p_n$의 `affine hull`이라고 한다.

$$
\operatorname{aff}\{p_1,\ldots,p_n\}
:=
\left\{
\sum_{i=1}^n c_ip_i
\;\middle|\;
c_1,\ldots,c_n\in\mathbb F,
\ \sum_{i=1}^n c_i=1
\right\}.
$$

따라서 하나의 affine combination은 하나의 point이고, affine hull은 가능한 모든 affine combination으로 이루어진 set이다. 이 affine hull은 $p_1,\ldots,p_n$을 포함하는 가장 작은 affine subspace다.

## 두 point의 affine combination과 affine parameter

$a,b\in A$와 $\lambda\in\mathbb F$가 있다고 하자. 두 계수 $1-\lambda$, $\lambda$의 합은 $1$이므로 다음 affine combination이 정의된다.

$$
(1-\lambda)a+\lambda b.
$$

기준 point로 $a$를 선택하면 이 식은 다음과 같다.

$$
(1-\lambda)a+\lambda b
=
a+\lambda(b-a).
$$

$\lambda$를 $a$와 $b$에 대한 `affine parameter`라고 하고, 이 parameter에 대응하는 point를 $p_\lambda:=(1-\lambda)a+\lambda b$라고 하자. 그러면 다음 관계가 성립한다.

$$
p_\lambda-a=\lambda(b-a).
$$

즉, $a$에서 $p_\lambda$로 가는 vector는 $a$에서 $b$로 가는 vector의 $\lambda$배다.

$\mathbb F=\mathbb R$인 경우에는 $\lambda$를 다음과 같이 해석할 수 있다.

- $\lambda=0$이면 $p_\lambda=a$다.
- $\lambda=1$이면 $p_\lambda=b$다.
- $\lambda=\frac12$이면 $p_\lambda$는 $a$와 $b$의 midpoint다.
- $0<\lambda<1$이면 $p_\lambda$는 $a$와 $b$ 사이에 있다.
- $\lambda<0$ 또는 $\lambda>1$이면 $p_\lambda$는 두 point가 정하는 line 위에서 segment 밖에 있다.

일반적인 field에는 순서나 distance가 없을 수 있으므로 `사이`나 `길이의 비율`이라는 해석은 사용할 수 없다. 이 경우에도 $p_\lambda-a=\lambda(b-a)$라는 algebraic relation은 그대로 의미를 갖는다.

## Examples

### Midpoint

$2$의 multiplicative inverse가 $\mathbb F$에 존재하면 $a,b\in A$의 midpoint는 다음 affine combination이다.

$$
m=\frac12a+\frac12b
=a+\frac12(b-a).
$$

### Barycenter

$n$의 multiplicative inverse가 $\mathbb F$에 존재하면 $p_1,\ldots,p_n\in A$에 같은 가중치를 준 barycenter는 다음과 같다.

$$
g=\sum_{i=1}^n \frac1n p_i.
$$

임의의 기준 point $o\in A$를 사용하면 다음과 같이 계산할 수 있다.

$$
g=o+\frac1n\sum_{i=1}^n(p_i-o).
$$

## Coordinate representation

Affine space $A$에 coordinate system $(o,\beta)$를 선택하자. Point $p_i\in A$의 coordinate는 $o$에서 $p_i$로 가는 vector의 coordinate다.

$$
[p_i]_{(o,\beta)}
=
[p_i-o]_\beta.
$$

Point $p_1,\ldots,p_n\in A$와 계수 $c_1,\ldots,c_n\in\mathbb F$가 다음을 만족한다고 하자.

$$
\sum_{i=1}^n c_i=1.
$$

$p$를 $p_1,\ldots,p_n$의 affine combination이라고 하자.

$$
p:=\sum_{i=1}^n c_ip_i.
$$

그러면 $p$의 coordinate는 각 $p_i$의 coordinate를 동일한 계수 $c_i$로 linear combination하여 구할 수 있다.

$$
[p]_{(o,\beta)}
=
\sum_{i=1}^n c_i[p_i]_{(o,\beta)}.
$$

여기서 $p:=\sum c_ip_i$는 point들의 affine combination을 나타내는 표기다. 반면 우변의 $\sum c_i[p_i]_{(o,\beta)}$는 $\mathbb F^n$의 coordinate vector들에 대한 일반적인 linear combination이다.

실제로 affine combination의 정의에 따라 다음이 성립한다.

$$
p
=
o+\sum_{i=1}^n c_i(p_i-o).
$$

따라서 $o$에서 $p$로 가는 vector는 다음과 같다.

$$
p-o
=
\sum_{i=1}^n c_i(p_i-o).
$$

Vector의 coordinate map $v\mapsto[v]_\beta$의 linearity를 적용하면 다음을 얻는다.

$$
\begin{aligned}
[p]_{(o,\beta)}
&=[p-o]_\beta \\
&=\left[\sum_{i=1}^n c_i(p_i-o)\right]_\beta \\
&=\sum_{i=1}^n c_i[p_i-o]_\beta \\
&=\sum_{i=1}^n c_i[p_i]_{(o,\beta)}.
\end{aligned}
$$

예를 들어 두 point $a,b\in A$의 coordinate가 다음과 같다고 하자.

$$
[a]_{(o,\beta)}
=
\begin{bmatrix}
1\\
2
\end{bmatrix},
\qquad
[b]_{(o,\beta)}
=
\begin{bmatrix}
5\\
4
\end{bmatrix}.
$$

Midpoint $m=\frac12a+\frac12b$의 coordinate는 같은 계수 $\frac12,\frac12$를 사용하여 계산할 수 있다.

$$
\begin{aligned}
[m]_{(o,\beta)}
&=\frac12[a]_{(o,\beta)}+\frac12[b]_{(o,\beta)} \\
&=\frac12
\begin{bmatrix}
1\\
2
\end{bmatrix}
+\frac12
\begin{bmatrix}
5\\
4
\end{bmatrix} \\
&=
\begin{bmatrix}
3\\
3
\end{bmatrix}.
\end{aligned}
$$

즉, affine space의 point 자체에는 일반적인 덧셈과 scalar multiplication이 없지만, coordinate system을 선택하면 affine combination을 coordinate vector의 linear combination으로 계산할 수 있다. 계수의 합이 $1$이라는 조건은 위의 linearity 계산 자체가 아니라, $p$가 기준 point의 선택과 무관한 affine combination으로 정의되는 것을 보장한다.

## 관련 문서

- [Affine space](<./11 Affine space.md>)
- [Affine Transformation](<./13 Affine Transformation.md>)

## Reference

- [Wikipedia - Affine combination](https://en.wikipedia.org/wiki/Affine_combination)
- [Wikipedia - Affine space: Affine combinations and barycenter](https://en.wikipedia.org/wiki/Affine_space#Affine_combinations_and_barycenter)
