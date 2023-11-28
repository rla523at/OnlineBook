# Total Derivative
## Motivation
### Limitation of directional derivative
일변수 함수에서 미분계수가 존재하는 점에서 미분가능하다고 한다.

따라서, 다변수 함수에서는 모든 방향에서 directional derivative가 존재하는 경우 미분가능하다고 정의하는것이 자연스러운 선택일 것이다.

그렇다면 이렇게 정의한 다변수함수의 미분가능성이 기존의 일변수 함수의 미분가능성과 같은 성질을 가지고 있는지 확인해보자.

일변수 함수에서 미분가능성에 대해 다음 두가지 성질이 성립함을 알고 있다.
1. 미분가능하면 연속이다.
2. chain rule이 성립한다. $(f\circ g)' = f'g'$

하지만 현재 정의는 위 성질이 만족되지 않는 반례가 존재한다.

1번의 반례를 보이기 위해 함수 $f(x,y)$를 다음과 같이 정의하자.

$$ f(x,y) = \begin{cases} 0 & y \le 0 \lor x \le \sqrt{y} \\ 1 & \text{else} \end{cases} $$

$f$는 $(0,0)$에서 불연속이지만 임의의 $v$에 대해서 $D_vf(0,0)$은 잘 정의된다.

즉, 위의 정의대로라면 (0,0)에서 미분가능하지만 연속이 아닌 $f$가 존재하게 된다.

따라서, directional derivative의 존재성으로 미분가능성을 정의하기에는 부족한 점이 있다는것을 알 수 있다.

### Revisiting single variable derivative
Directional derivative를 다변수 함수에서 미분계수로 정의하기에는 부족한 점이 있다는것을 보았다.

따라서 일변수 함수의 미분계수의 정의를 활용해서 open set $U \subset \R^n$와 함수 $f : U \rightarrow \R^m$이 있을 때 $a \in U$에서 미분계수를 정의해보자.

#### Altenative form1
다차원 확장을 고려하여 정의를 다음과 같이 변경해보자.

$$ f'(a) := \lim_{h \rightarrow 0} \frac{1}{\norm{h}}(f(a + h) - f(a)) $$

이 경우에는 다차원으로 확장하더라도 vector의 크기로 나누기 때문에 정의되지 않은 연산은 없다. 

하지만 이 경우에는 $h \rightarrow 0$로 가면서 부호 문제가 발생하여 기존의 미분 가능하던 함수들이 미분 불가능하게 된다.

##### 예시
$f(x) = x^2$라 하자.

$f'(a)$를 계산하면 다음과 같다.

$$ f'(a) = \lim_{h \rightarrow 0} \frac{1}{\norm{h}}(2ah + h^2) = \begin{cases} 2a & h>0 \\ -2a & h<0 \end{cases} $$

따라서, 극한값이 존재하지 않아 미분불가능해지는 문제가 발생한다.

#### Alternative form2
기존의 미분 정의로부터 다음 형태를 유도할 수 있다.

$$ \lim_{h \rightarrow 0} \frac{1}{h} \Big( f(a + h) - f(a) - f'(a)h \Big) = 0 $$

따라서, 다음 형태를 만족하는 $T \in L(\R^n,\R^m)$이 있을 떄, $T$를 $f$의 $a$에서 미분계수라고 정의하자.

$$ \lim_{h \rightarrow 0_n} \frac{1}{\norm{h}} \Big( f(a+h)-f(a)-T(h) \Big) = 0 $$

일변수 함수의 경우 $T(h) = f'(a)h$라고 두면 Alternative form1과 다르게 부호 문제가 발생하지 않는다.

##### 예시
$f(x) = x^2$라 하자.

정의에 의해 $f'(a)$는 다음과 같다.

$$ \lim_{h \rightarrow 0} \frac{1}{\norm{h}}(2ah + h^2 - f'(a)h) = 0 $$

$f'(a)$를 계산하면 다음과 같다.

$$ \begin{cases} 2a - f'(a) = 0 & h>0 \\ -2a + f'(a) = 0 & h<0 \end{cases} $$

이전 예시와 다르게 부호 문제 없이 $f'(a) = 2a$임을 알 수 있다.

##### 참고1
$f(a + h) - f(a)$항은 $\Delta f$를 나타내고, $L(a)h$항은 $\Delta f$를 선형근사한 값으로 볼 수 있다.

##### 참고2
위의 식이 성립하기 위해서는 $\Delta f$와 선형근사인 $L(a)h$사이의 차이가 $h \rightarrow 0$일 때, 선형보다 작아야 한다.

###### 예시
linear map $L,D$가 있다고 하자.

$L$은 $\Delta f - L(h) = c_1h, \enspace c_1 \in \R - \{ 0 \}$로 차이가 선형이라고 하면 다음과 같다.

$$ \lim_{h \rightarrow 0} \frac{1}{\norm{h}} \Big( f(a+h)-f(a)-L(h) \Big) = \lim_{h \rightarrow 0}\frac{c_1h}{\norm{h}} = \pm c_1 \neq 0 $$

즉, 차이가 선형인 경우 위의 식을 만족시키지 못한다.

$D$는 $\Delta f - D(h) = c_2h^2, \enspace c_2 \in \R - \{ 0 \}$로 차이가 선형보다 작다고 하면 다음과 같다.

$$ \lim_{h \rightarrow 0} \frac{1}{\norm{h}} \Big( f(a+h)-f(a)-D(h) \Big) = \lim_{h \rightarrow 0}\frac{c_2 h^2}{\norm{h}} = 0 $$

즉, 차이가 선형보다 작은 경우 위의 식을 만족시킨다.

> Reference  
> {cite}`hubbard` Remark 1.7.6.  

##### 참고3
모든 방향에서 directional derivative가 있는것과 새로운 정의에 차이는 directional derivative는 특정 직선을 따라서 $a$에 가까워진다는 한계점이 있지만 새로운 정의는 그러한 한계점 없이 어떤 방식으로라도 $a$에 가까워지기만 하면 된다.

즉, 굳이 일직선을 따라서 예쁘게 가까워질 필요가 없고 온갖 말도 안되는 경로를 따라서라도 가까워지기만 하면 된다. 

## 정의
open subset $U \subset \R^n$과 함수 $f : U \rightarrow \R^m$이 있다고 하자.

$a \in U$와 linear map $T \in L(\R^n,\R^m)$이 있다고 하자.

$T$가 다음을 만족할 경우, $T$를 $a$에서 $f$의  `total derivative`라고 한다.

$$ \lim_{h \rightarrow 0_n} \frac{1}{\norm{h}}(f(a + h) - f(a) - T(h)) = 0_m $$

### 참고1
$a$에서 $f$의 total derivative를 $T =Df(a)$로 표기한다.

### 참고2
$Df(a)$가 존재할 떄 $f$는 $a$에서 `미분가능(differentiable)`하다고 한다.

따라서, 미분가능한 함수 $f$는 각 점마다 total derivative라는 linear map을 갖으며 이 linear map을 derivative라고 정의한다.

### 명제1
$\R^n$의 open subset $U$와 함수 $f : U \rightarrow \R^m$가 있다고 하자.

$a \in U$에서 $f$가 미분가능할 떄, 다음을 증명하여라.

$$ Df(a) \text{ is unique} $$

**Proof**

$T_1,T_2 \in L(\R^n,\R^m)$가 다음을 만족한다고 하자.

$$ \lim_{h \rightarrow 0_n} \frac{1}{\norm{h}}(f(a + h) - f(a) - T_{1,2}(h)) = 0_m $$

$e_i, \enspace i=1,\cdots,n$가 $\R^n$의 standard basis일 떄, $h = te_i, \enspace i=1,\cdots,n$으로 두면 다음이 성립한다.

$$ \lim_{t \rightarrow 0} \frac{1}{t}(f(a + te_i) - f(a))  = T_{1,2}(e_i), \enspace i=1,\cdots,n $$

즉, $i=1,\cdots,n$에 대해서 다음이 성립한다.

$$ T_1(e_i) = T_2(e_i) = \pdiff{f}{x_i}(a) $$

$\Set{e_i}$는 $\R^n$의 basis임으로 다음이 성립한다.

$$ T_1 = T_2 \qed $$

#### 따름명제
$\epsilon_{n,m}$을 $\R^{n,m}$의 standard basis라고 할 때, 다음을 증명하여라.

$$ \frak{m}_{\epsilon_n}^{\epsilon_m}(Df(a)) = \pdiff{f_i}{x_j}(a)  $$

##### 참고1
$Df(a)$의 행렬표현을 $f$의 `Jacobian matrix`라고 부르며 $J_f(a)$로 표기한다.

$$ \begin{aligned} J_f(a) &:= \mathfrak{m}_{\epsilon_n}^{\epsilon_m}(Df(a)) \\&= \begin{bmatrix} \mathfrak{m}_{\epsilon_m}(\pdiff{f}{x_1}(a)) &\cdots& \mathfrak{m}_{\epsilon_m}(\pdiff{f}{x_n}(a)) \end{bmatrix} \\&= \begin{bmatrix} \pdiff{f_1}{x_1}(a) &\cdots& \pdiff{f_1}{x_n}(a) \\ \vdots && \vdots \\ \pdiff{f_m}{x_1}(a) &\cdots& \pdiff{f_m}{x_n}(a) \end{bmatrix} \end{aligned} $$

##### 참고2
모든 방향의 directional derivative가 존재해도 미분 가능하지 않을 수 있음을 알고 있다.

따라서, $J_f(a)$가 있다고 하더라도 $Df(a)$가 존재할 이유가 없다.

그럼으로, 

이 때, $Df(a)$

$\mathfrak{m}_{\epsilon_n}^{\epsilon_m}(Df(a)) =J_f(a)$이려면 $f$가 differtiable해야 한다.

### 명제2
$\R^n$의 open set $U$와 linear map $f : U \rightarrow \R^m$가 있다고 하자.

$\R^n$과 $\R^m$의 basis가 각 각 $\epsilon_n,\epsilon_m$이고 $a \in U$에서 $f$가 미분가능할 때, 다음을 증명하여라.

$$ Df(a) = f $$

**Proof**

명제1에 의해 다음이 성립한다.

$$ \mathfrak{m}_{\epsilon_n}^{\epsilon_m}(Df(a)) = \begin{bmatrix} \mathfrak{m}_{\epsilon_m}(\pdiff{f}{x_1}(a)) &\cdots& \mathfrak{m}_{\epsilon_m}(\pdiff{f}{x_n}(a)) \end{bmatrix} $$

Partial derivative의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} \begin{bmatrix} \mathfrak{m}_{\epsilon_m}(D_1^{f(a)}) &\cdots& \mathfrak{m}_{\epsilon_m}(D_n^{f(a)}) \end{bmatrix} &= \begin{bmatrix} \mathfrak{m}_{\epsilon_m}(f(e_1)) &\cdots& \mathfrak{m}_{\epsilon_m}(f(e_n)) \end{bmatrix} \\&= \mathfrak{m}_{\epsilon_n}^{\epsilon_m}(f) \end{aligned} $$

위의 결과를 종합하면 $\mathfrak{m}_{\epsilon_n}^{\epsilon_m}(Df(a)) = \mathfrak{m}_{\epsilon_n}^{\epsilon_m}(f)$임으로 linear algebra에 의해 다음이 성립한다.

$$ Df(a) = f \qed $$

### 명제3(Chain Rule)
$\R^n$의 open set $U$와 $\R^m$의 open set $V$가 있다고 하자.

$f:U \rightarrow V$과 $g:V \rightarrow\R^p$가 있을 때, $a \in U$에서 $f$가 differentiable하고 $f(a) \in V$에서 $g$가 differentiable하다고 하자.

이 때, 다음을 증명하여라.

$$ \begin{gathered} g \circ f \text{ is diffrentiable at } a \\ D{(g \circ f)(a)} = D{g(f(a))} \circ Df(a) \end{gathered} $$

**Proof**

함수 $r : \R^n \rightarrow \R^m$과 함수 $s : \R^m\rightarrow\R^p$를 다음과 같이 정의하자.

$$ \begin{gathered} r(h) = f(a+h) - f(a) - Df(a)(h) \\ s(t) = g(f(a)+t) - g(f(a)) - Dg(f(a))(t) \end{gathered} $$

$T_1 = Df(a), T_2 = Dg(f(a))$로 두고 $\Delta f(h) = r(h) + T_1(h)$라고 할 떄, $r,s$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} (g\circ f)(a+h) &= g(f(a) + r(h) + T_1(h)) \\&= g(f(a) + \Delta f) \\&= (g\circ f)(a) + T_2(\Delta f) + s(\Delta f)  \end{aligned} $$

$T_2$는 linear map임으로 다음이 성립한다.

$$ \begin{aligned} T_2(\Delta f) &= T_2(r(h) + T_1(h)) \\&= (T_2 \circ r)(h) + (T_2 \circ T_1)(h) \end{aligned} $$

따라서, 다음이 성립한다.

$$ (g \circ f)(a+h) - (g\circ f)(a) - (T_2 \circ T_1)(h) = (T_2 \circ r)(h) + s(\Delta f) $$

이 때, 보조명제3.1에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}} \left( (g \circ f)(a+h) - (g\circ f)(a) - (T_2 \circ T_1)(h) \right) = 0_p $$

$T_2$와 $T_1$ 모두 linear map임으로 다음이 성립한다.

$$ T_2 \circ T_1 \text{ is an linear map} $$

따라서, total derivative의 정의에 의해 다음이 성립한다.

$$ \begin{gathered} f \circ g \text{ is diffrentiable at } a \\ D{(g \circ f)(a)} = T_2 \circ T_1 = D{g(f(a))} \circ Df(a) \end{gathered} \qed $$

#### 보조명제3.1
다음을 증명하여라.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}} \left( (T_2 \circ r)(h) + s(\Delta f) \right) = 0_p $$

**Proof**

[첫번째 항]  
다음이 성립한다. 

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}}\norm{(T_2 \circ r)(h)} \le \norm{T_2}\lim_{h\rightarrow 0_n} \frac{1}{\norm{h}}\norm{r(h)} $$

전제에 의해 $f$는 $a$에서 differentiable함으로 다음이 성립한다.

$$ \lim_{h\rightarrow 0_n} \frac{r(h)}{\norm{h}} = 0_n \iff \lim_{h\rightarrow 0_n} \frac{\norm{r(h)}}{\norm{h}} = 0 $$

따라서, 위의 결과를 종합하면 다음이 성립한다.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}}(T_2 \circ r)(h) = 0_p \qed $$

[두번째 항]  
$\Delta f$를 풀어 쓰면 다음과 같다.

$$ s(\Delta f) = s(r(h) + T_1(h))) $$

전제에 의해 $\lim_{h\rightarrow 0_n} \frac{r(h)}{\norm{h}} = 0_m$임으로 보조명제3.1.1에 의해 다음이 성립한다.

$$ \exist \delta_1 \in \R^+ \st \forall h \in U, \enspace 0 < \norm{h} < \delta_1 \implies \norm{r(h)} < \norm{h} $$

이 떄, 전제에 의해 $\lim_{t\rightarrow 0_n} \frac{s(h)}{\norm{h}} = 0_n$임으로 metric space의 limt의 정의에 의해 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \delta \in \R^+ \st \forall h \in U, \enspace 0 < \norm{h} < \delta \implies \norm{s(h)}<\epsilon\norm{h}$$

그럼으로 다음도 성립한다.

$$ \forall \epsilon \in \R^+, \quad \delta_\epsilon \in (0, \delta_1) \st \norm{h} < \delta \implies \norm{s(h)}<\epsilon\norm{h}$$

따라서, $\norm{h}<\delta_\epsilon$일 때, $\delta_\epsilon$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} s(r(h) + Df(a)(h)) &\le \epsilon\norm{Df(a)+r(h)} \\&< \epsilon\left(\norm{Df(a)}+1\right)\norm{h} \\ \frac{s(r(h) + Df(a)(h))}{\norm{h}} &< \epsilon\left(\norm{Df(a)}+1\right) \end{aligned} $$

따라서, $\norm{h}<\delta_1$일 떄, 다음이 성립한다.

$$ \begin{aligned} \norm{r(h) + T_1(h)} &\le \norm{r(h)} + \norm{T_1(h)} \\&\le \norm{r(h)} + \norm{T_1}\norm{h} \\&< \left(\norm{T_1}+1\right)\norm{h} \end{aligned} $$


$\epsilon'$을 다음과 같이 정의하자.

$$ \epsilon' := \epsilon\left(\norm{Df(a)}+1\right) $$

그러면 $\epsilon'$의 정의에 의해 다음이 성립한다.

$$ \forall \epsilon' \in \R^+, \quad \delta_\epsilon \st \norm{h} < \delta_\epsilon \implies \frac{s(r(h) + Df(a)(h))}{\norm{h}}<\epsilon' $$

따라서, Metric space에서 limt의 정의에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0_n} \frac{s(r(h) + Df(a)(h))}{\norm{h}} =0_n \qed $$


##### 보조명제3.1.1
$\lim_{h\rightarrow0_n} \frac{f(h)}{\norm{(h)}} = 0$일 때, 다음을 증명하여라.

$$ \forall\epsilon \in \R^+, \quad \exist \delta \st \norm{h} < \delta \implies \norm{f(h)} < \epsilon\norm{h} $$

**Proof**

Metric space에서 limt의 정의에 의해 $\lim_{h\rightarrow0_n} \frac{f(h)}{\norm{(h)}} = 0$면 다음이 성립한다.

$$ \forall\epsilon \in \R^+, \quad \exist \delta \st \norm{h} < \delta \implies \frac{\norm{f(h)}}{\norm{h}} < \epsilon $$

따라서, 다음이 성립한다.

$$ \forall\epsilon \in \R^+, \quad \exist \delta \st \norm{h} < \delta \implies \norm{f(h)} < \epsilon\norm{h} \qed $$

> Reference  
> {cite}`hubbard` Appendix4  
> [illinois note](https://faculty.math.illinois.edu/~carty/ChainRuleNotesSlides.pdf)

### 명제4
$\R$의 open set $U$와 $f : U \rightarrow R^m$이 있다고 하자.

$a \in U$가 있을 때, 다음을 증명하여라.

$$ (Df(a))(1) = f'(a) $$

**Proof**

Total derivative의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{h\rightarrow 0}\frac{f(a+h) - f(a) - (Df(a))(h)}{|h|} = 0 \\\implies& \lim_{h\rightarrow 0}\frac{f(a+h) - f(a)}{|h|} - \frac{h}{|h|}(Df(a))(1) = 0 \\ \implies& \begin{gathered} \lim_{h\rightarrow 0^+}\frac{f(a+h) - f(a)}{h} = (Df(a))(1) \\ \lim_{h\rightarrow 0^-}\frac{f(a+h) - f(a)}{h} = (Df(a))(1) \end{gathered} \\\implies& \lim_{h\rightarrow 0}\frac{f(a+h) - f(a)}{h} = (Df(a))(1) \\\implies& f'(a) = (Df(a))(1) \qed \end{aligned} $$

#### 참고1
$Df(a) \in L(\R,\R)$인 linear map이고 $1, f'(a) \in \R$인 vector이다.

#### 참고2
$J_f = \frak{m}_\epsilon^\epsilon(Df(a)) = \begin{bmatrix} f'(a) \end{bmatrix}$이다.


### 명제2
$\R^n$의 open set $U$와 $f: U \rightarrow \R$이 있다고 하자.

$f$의 partial derivatives가 존재할 때, Euclidean graident $\nabla_Ef$를 다음과 같이 정의하자.

$$ \nabla_Ef = \left( \pdiff{f}{x_1}, \cdots, \pdiff{f}{x_n} \right) $$

$a \in U$와 $v\in \R^n \st \norm{v} = 1$가 있을 때, 다음을 증명하여라.

$$ D_vf(a) = \nabla_E f \cdot v $$

**Proof**

Directional derivative의 정의에 의해 다음이 성립한다.

$$ D_vf(a) = \lim_{h\rightarrow 0} \frac{f(a+hv) - f(a)}{h} $$

이 때, $g(h) = f(x(h)), \enspace x(h) = a+hv$로 두면 다음이 성립한다.

$$ D_vf(a) = \lim_{h\rightarrow 0} \frac{g(h) - g(0)}{h} = g'(0) $$

$x(h)$는 differentiable함으로 chain rule에 의해 다음이 성립한다.

$$ \begin{aligned} g'(0) &= \pdiff{f}{x_1}\diff{x_1}{h} + \cdots + \pdiff{f}{x_n}\diff{x_n}{h} \\&=  \nabla_Ef \cdot v \qed \end{aligned} $$

### 참고2
Total derivative의 의미는 $a\in\R^n$에서 함수 $f: \R^n \rightarrow \R^m$에 대한 best linear approximation이다.

따라서 위 정의는 $f$의 domain과 codomain이 Euclidean vector space일때 만 성립하는 것이 아니라, 임의의 vector space에 대해서도 성립한다.

$f$의 total erivative를 계산하는데 있어서, 정의를 사용하는것보다 $J_f$를 사용하는 것이 더 간단한 경우가 많다. 

하지만 domain과 codomain이 Euclidean vector space가 아닌 vector space인 경우에는 vector space isomorphism을 이용하여 Euclidean vector space에서 Jacobian matrix를 구하는 형태로 계산을 해야 되는데, 이 방법이 최선의 선택이 아닌 경우도 있다.

#### 예시
함수 $f$가 다음과 같이 정의되었다고 하자.

$$f : M_{nn} \rightarrow M_{nn} \st A \mapsto A^2 $$

##### $J_f$를 사용하는 경우
$M_{nn}$과 $\R^{nn}$은 다음 관계에 의해 vector space isomorphic이다.

$$ \begin{CD} M_{nn} @>f>> M_{nn} \\ @V{\varphi}VV @VV{\varphi}V \\ \mathbb \R^{nn} @>>g> \mathbb \R^{nn}  \end{CD} $$

$f,g$의 total derivative를 각 각 $L_f,L_g$라하고 $M_{nn}$과 $\R^{nn}$의 basis를 각 각 $\epsilon_n, \epsilon_m$라 할 때, $\varphi$의 성질에 의해 다음이 성립한다.

$$ \mathfrak{m_\epsilon_n^\epsilon_n}(L_f) = \mathfrak{m_\epsilon_m^\epsilon_m}(L_g) $$

$f$의 정의에 의해 $a = a^{ij}\gamma_{ij} \in \R^{nn}$가 있을 떄, $g(v)$는 다음과 같다.

$$ g(v) = a^{ik}a^{kj}\gamma_{ij} $$

따라서, 다음이 성립한다.

$$ \mathfrak{m_\epsilon_m^\epsilon_m}(L_g) = J_g(a) = \begin{bmatrix} D_{11}g^{11}( a) & \cdots & D_{nn}g^{11}(a) \\ \vdots & & \vdots \\ D_{11}g^{nn}( a) & \cdots & D_{nn}g^{nn}( a) \end{bmatrix} $$


$$ \text{Where, }D_{ij}g^{kl}(a) = \frac{\partial a^{kr}a^{rl}}{\partial a^{ij}} $$

그럼으로 $\mathfrak{m_\epsilon_n^\epsilon_n}(L_f)$를 알고 있기 때문에 $L_f$에 대해 알 수 있다.

예를 들어 $m \in M_{nn}$에 대해서, $L_f(m)$은 다음과 같다.

$$ \begin{aligned} L_f(m) &= \varphi^{-1}(\mathfrak{m_\epsilon_n}(L_f(m))) \\&= \varphi^{-1}(\mathfrak{m_\epsilon_n^\epsilon_n}(L_f)\mathfrak{m_\epsilon_n}(m)) \end{aligned} $$

##### 정의를 사용하는 경우
Total derivative의 정의를 통해 $L_f$을 구해보자.

$$ \begin{aligned} & \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}(f(A + H) - f(A) - \mathit L_f(H)) = 0_{M_{nn}} \\ \implies & \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}(AH + HA + H^2 - \mathit L_f(H)) = 0_{M_{nn}} \end{aligned} $$

$f$가 $A$에서 미분가능하기 위해서는 위의 식이 만족되어야 함으로 $\norm{H}$로 나눠주면 $0_{M_{nn}}$이 되지 않는 선형 항들을 제거하기 위해 linear map $L_f$를 다음과 같이 정의하자.

$$ L_f : M_{nn} \rightarrow M_{nn} \st H \mapsto AH + HA $$

위와 같이 정의한 $L_f$에 의해 다음이 만족된다.

$$ \begin{aligned} \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}(AH + HA + H^2 - \mathit L(H)) &= \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}H^2 \\&\le \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}\norm{H}\norm{H} \\ &= 0_{M_{nn}} \end{aligned} $$

따라서, $f$의 total derivative는 $L_f(A)$이다.

##### 결론
두 경우를 비교해보면 알 수 있듯이, 정의를 사용하는 경우가 훨씬 간단할수도 있다.

> Reference  
> {cite}`hubbard` p.132
>
> ### 명제
open set $U \subset \R^n$과 함수 $f : U \rightarrow \R^m$이 있다고 하자.

$\R^m$의 basis를 $\epsilon_n$하고 $\forall a \in U$에서 $f$가 differentiable일 때, 다음을 증명하여라.

$$ Df\text{ is well-defined} \enspace\land\enspace \mathfrak{m_\epsilon_n}(D_vf(a)) = \mathfrak{m_\epsilon_n}(J_f(a)v) $$

**Proof**

$f$가 differentiable 함으로 다음이 성립한다.

$$ \begin{aligned} & \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a) - L_a(hv)}{\norm{hv}} = 0 \\\implies& \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a) - hL_a(v)}{h\norm{v}} = 0 \\\implies& \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a) - hL_a(v)}{h} = 0 \\\implies& \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a)}{h} = L_a(v) \end{aligned} $$

이 떄, $f$가 $a$에서 differentiable 함으로, 선형변환 $L_a$은 잘 정의된다. 

따라서, 극한값이 $L_a(v)$로 존재함으로, 함수 $Df$는 잘 정의된다.

또한, $D_vf(a) = L_a(v)$이고 Jacobian matrix의 정의에 따라 $L_a(v) = J_f(a)v$

> Reference  
> {cite}`hubbard` Proposition 1.7.14.