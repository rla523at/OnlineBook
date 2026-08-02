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
