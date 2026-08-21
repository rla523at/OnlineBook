# Directional Derivative

## Motivation

$\R^n$의 open set을 $U$라고 하고 다변수 함수 $f : U \rightarrow \R^m$이 있다고 하자.

일변수 함수의 derivative는 scalar parameter의 변화에 따른 함수값의 변화율을 측정한다. 다변수 함수에서 $a\in U$를 지나는 특정 direction $v\in\R^n$을 따라 같은 질문을 하려면, unit vector $v$를 고정하고 다음 일변수 함수를 생각할 수 있다.

$$
g_v(t):=f(a+tv)
$$

$U$가 open이므로 충분히 작은 $\lvert t\rvert$에 대해 $a+tv\in U$다. 따라서 $g_v$의 $t=0$에서 derivative를 계산하면

$$
g_v'(0)
=
\lim_{t\rightarrow 0}
\frac{f(a+tv)-f(a)}{t}
$$

를 얻는다. 이 값은 $a$에서 $v$가 정하는 직선을 따라 움직일 때 $f$가 변하는 비율을 나타낸다.

## 정의
$\R^n$의 open subset $U$와 함수 $f:U \rightarrow \R^m$이 있다고 하자.

$a \in U$에서 $v\in \R^n \st \norm{v}=1$ 방향으로의 $f$의 `방향 미분계수(directional derivative)`는 다음과 같다.

$$ D_vf(a) = \lim_{t \rightarrow 0}\frac{ f(a + t v) - f (a)}{t} $$

### 참고1
$D_vf(a)$는 $\R^m$의 element, 즉 vector이다.

$$ D_vf(a) = \begin{bmatrix} D_vf_1(a) \\ \vdots \\ D_vf_m(a) \end{bmatrix} $$


### 참고2
$U$ 의 부분집합 $V$ 를 $V := \Set{ x \in U | \exist D_vf(x)}$ 라고 정의하면 $V$ 위에서의 $v$ 방향으로의 `방향 도함수(directional derivative)` $D_vf$ 는 다음과 같이 정의된다.

$$ D_vf : V \rightarrow \R^m \st x \mapsto D_vf(x) $$

즉, $D_vf$ 는 $\R^n$ 의 특정 원소들을 $\R^m$으로 보내주는 함수이다.

### 참고3(Partial Derivative)
$\R^n$의 standard basis인 $e_i$방향의 directional derivative를 다음과 같이 표현한다.

$$ D_{e_i}f(a) = \lim_{h \rightarrow 0} \frac{1}{h}(f(a^1, \cdots, a^i + h, \cdots, a^n) - f(a^1, \cdots, a^n)) = \pdiff{f}{x_i}(a) $$

이를 `편미분(partial derivative)`라고 한다. 

편미분은 다변수 함수를 일변수 함수처럼 보고 미분하는 방식이다. 다시 말해, 나머지 변수는 전부 상수로 간주하고 한 변수에 대해서 미분을 구하는 방식이다.

당연하게, $\pdiff{f}{x_i}(a) \in \R^m$도 vector이다.

$$ \pdiff{f}{x_i}(a) = \begin{bmatrix} \pdiff{f_1}{x_i}(a) \\ \vdots \\ \pdiff{f_m}{x_i}(a) \end{bmatrix} $$


### 명제1
$\R^n$의 open set $U$와 linear map $f: U \rightarrow \R^m$이 있다고 하자.

$a \in U$에서 $f$의 partial derivatives가 존재할 때, 다음을 증명하여라.

$$ \pdiff{f}{x_i}(a) = f(e_i) $$

**Proof**

Partial derivatve의 정의에 의해 다음이 성립한다.

$$ \pdiff{f}{x_i}(a) = \lim_{h \rightarrow 0}\frac{f(a + h e_i) -f (a)}{h} $$

$f$가 linear map임으로 다음이 성립한다.

$$ \frac{f(a + h e_i) -f (a)}{h} = \frac{f(a) + hf( e_i) -f (a)}{h} =  f(e_i) $$

따라서, 위의 결과를 종합하면 다음이 성립한다.

$$ \pdiff{f}{x_i}(a) = f(e_i) \qed $$
