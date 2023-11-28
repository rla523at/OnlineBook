# Continuous at a Point
## 정의
Metric space $M_1,M_2$가 있다고 하자.

$X$가 $M_1$의 open set일 때, 함수 $f : X \rightarrow M_2$가 $x \in X$에서 `연속(continuous)`이라는 말은 다음과 동치이다.

$$ \forall \epsilon \in \R^+, \enspace \exist \delta \in \R^+ \st \forall t \in X, \enspace d_1(x,t) < \delta \implies d_2(f(x),f(t)) < \epsilon $$

> Reference  
> {cite}`hubbard` Chapter 1.5

### 참고1
연속을 정의하기 위해서는 $f$가 $x$에서 정의되어 있음으로 함수의 수렴과는 정의상 약간의 차이가 있다.

함수의 수렴의 정의에서 $f$가 $x$에서 정의되지 않아도 동작하도록 하게 만드는 모든 장치들이 함수의 연속의 정의에서는 빠져있는것을 알 수 있다.

* limit point
* $0 < d <\delta$

### 참고2
연속의 정의를 Open ball을 이용해 표현하면 다음과 같다.

$$ \forall \epsilon \in \R^+, \enspace \exist \delta \in \R^+ \st f (B_{M_1}(x,\delta)) \subseteq B_{M_2}(f(x),\epsilon) $$

혹은 다음과 같이 표현할 수 있다.

$$ \forall \epsilon \in \R^+, \enspace \exist \delta \in \R^+ \st B_{M_1}(x,\delta) \subseteq \preimg(B_{M_2}(f(x),\epsilon)) $$


즉, $x \in X$에서 continuous하다는 말은 $f(x)$를 중심으로 하는 open ball $B$마다 $f$에 의해서 $B$에 포함되는 $x$를 중심으로 하는 open ball이 존재한다는 말이다.



### 참고2
연속의 정의는 다음 세가지를 요구한다.
1. $f(x_0)$가 정의되어 있어야 한다.
2. $\lim_{x \rightarrow x_0} f(x)$이 존재해야 한다.
3. $\lim_{x \rightarrow x_0} f(x) = f(x_0)$

> Reference  
> {cite}`stewart` Chapter 1.8   

### 참고3
함수 $f$가 $x_0$에서 연속이라면 $x \rightarrow x_0$일 때, $f(x) \rightarrow f(x_0)$임으로 $x$가 $x_0$ 근처에서 조금 변화면 $f$도 조금 변한다는 의미이다.

> Reference  
> {cite}`stewart` Chapter 1.8   

### 명제1
open subsset $X \subset \R^n$과 함수 $\mathbf f : X \rightarrow \R^m$이 있을 때,

$X$안의 수열 중, $\mathbf x_0$로 수렴하는 수열의 집합을 $S$로 정의하자.

$$ S := \{ s(n) : \N \rightarrow X \enspace | \enspace  \lim_{n \rightarrow \infty} \mathbf s(n) = \mathbf x_0 \}$$

이 때, 다음을 증명하여라.

$$ \lim_{\mathbf x \rightarrow \mathbf x_0} \mathbf f(\mathbf x) = \mathbf f(\mathbf x_0) \enspace \iff \enspace \forall s \in S, \enspace \lim_{n \rightarrow \infty} (\mathbf f \circ s)(n) = \mathbf f(\mathbf x_0) $$

**Proof**

[$\implies$]  
$\mathbf f$가 $\mathbf x_0$에서 연속임으로, 다음이 성립한다.

$$ \forall \epsilon >0, \enspace \exist \delta \st |\mathbf x - \mathbf x_0| < \delta \implies |\mathbf {f(x) - f(x_0)}| < \epsilon$$

이 때, $s \in S$가 $\mathbf x_0$에 수렴하는 수열임으로 다음이 성립한다.

$$ \exist N \st N < n \implies |s(n) - \mathbf x_0| < \delta $$

즉, 이러한 $N$에 대해서는 다음이 성립한다.

$$ N < n \implies |(\mathbf f \circ s)(n)) - \mathbf{f(x_0)}| < \epsilon $$

따라서, $(\mathbf f\circ s)(n)$은 $\mathbf{f(x_0)}$로 수렴하는 수열이다. $\enspace {_\blacksquare}$

[$\impliedby$]  
다음과 같이 가정하자.

$$\forall s \in S, \enspace \lim_{n \rightarrow \infty} (\mathbf f \circ s)(n) = \mathbf f(\mathbf x_0) \implies \lim_{\mathbf x \rightarrow \mathbf x_0} \mathbf f(\mathbf x) \neq \mathbf f(\mathbf x_0)$$

그러면 다음이 성립한다.

$$ \forall \delta, \enspace \exist \epsilon \st |\mathbf x - \mathbf x_0| < \delta \text{ but } \epsilon \le |\mathbf {f(x) - f(x_0)}|   $$

이 때, $s \in S$가 $\mathbf x_0$에 수렴하는 수열임으로 다음이 성립한다.

$$ \exist N \st N < n \implies |s(n) - \mathbf x_0| < \delta $$

즉, 이러한 $N$에 대해서는 다음이 성립한다.

$$ N < n \implies \epsilon \le |(\mathbf f \circ s)(n)) - \mathbf{f(x_0)}|  $$

이는, $(\mathbf f\circ s)(n)$은 $\mathbf{f(x_0)}$로 수렴하는 수열이라는 가정에 모순됨으로 귀류법에의해 다음 명제는 참이다.

$$\forall s \in S, \enspace \lim_{n \rightarrow \infty} (\mathbf f \circ s)(n) = \mathbf f(\mathbf x_0) \implies \lim_{\mathbf x \rightarrow \mathbf x_0} \mathbf f(\mathbf x) = \mathbf f(\mathbf x_0) \enspace {_\blacksquare}$$

> Reference  
> {cite}`hubbard` Chapter 1.5

### 명제2
$f$와 $g$가 각각 $a$와 $b$에서 연속이라고 있다고 하자.

$$\lim_{x \rightarrow a} f(x) = f(a), \enspace \lim_{x \rightarrow b} g(x) = a$$

이 때, 다음을 증명하여라.

$$ \lim_{x \rightarrow b} (f \circ g)(x) = (f \circ g)(b)$$

**Proof**

함수의 극한의 명제에 의해 다음이 성립한다.

$$ \lim_{x \rightarrow b} (f \circ g)(x) = f(g(b)) = (f \circ g)(b)  \enspace {_\blacksquare}$$

> Reference  
> {cite}`stewart` Chapter 1.8   

#### 따름명제2.1
다음을 증명하여라.

$$ \lim_{x \rightarrow b} f(g(x)) = f(\lim_{x \rightarrow b} g(x))$$

> Reference  
> {cite}`stewart` Chapter 1.8   

### 명제3
Metric space $M_1,M_2$가 있다고 하자.

$X$가 $M_1$의 open set이고 함수 $f:X\rightarrow M_2$가 있을 떄, $x\in X$에 대해서 다음을 증명하여라.

$$ f \text{ is continuous at } x \iff \begin{gathered} \text{Let } a_n \text{ be an any sequence on } X \text{ converge to } x \\ \lim_{n\rightarrow \infty} f(a_n) = f(x) \end{gathered} $$

**Proof**

[$\implies$]  
전제에 의해 다음이 성립한다.

$$ \forall\epsilon\in\R^+, \enspace \exist\delta\in\R^+ \st \forall t \in X, \enspace d_1(x,t) < \delta \implies d_2(f(x),f(t))<\epsilon $$

이 때, $x$로 수렴하는 $X$위의 임의의 sequence를 $a_n$이라 하면 다음이 성립한다.

$$ \forall\delta\in\R^+, \enspace \exist N \in \N \st N \le n \implies d_1(x,a_n) < \delta $$

$a_n \in X$임으로 다음이 성립한다.

$$ \begin{aligned} & \forall \epsilon\in\R^+, \enspace \exist N \in N \st N \le n \implies d_2(f(x),f(a_n)) < \epsilon \\ \implies&  \lim_{n\rightarrow \infty} f(a_n) = f(x) \qed \end{aligned}  $$

[$\impliedby$]  
다음을 가정하자.

$$ \begin{aligned} & f \text{ is not continuous at } x \\\implies& \exist\epsilon\in\R^+ \st \forall\delta\in\R^+, \enspace \exist t \in X \st d_1(x,t)<\delta \land \epsilon \le d_2(f(x),f(t)) \\\implies& \exist\epsilon\in\R^+ \st \forall n \in\N, \enspace \exist t\in X \st d_1(x,t)<\frac{1}{n} \land \epsilon\le d_2(f(x),f(t)) \end{aligned}  $$

$n \in \N$마다 위를 만족하는 $t$를 모은 sequence를 $t_n$이라 하자.

그러면, $t_n$은 $x$로 converge하는 $X$위의 sequence임으로 전제에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{n\rightarrow\infty}f(t_n) = f(x) \\ \iff& \forall\epsilon\in\R^+,\enspace \exist N\in\N \st N\le n \implies d_2(f(x),f(t_n)) < \epsilon \end{aligned}  $$

이는 가정에 모순임으로, proof by contradiction에 의해 다음이 성립한다.

$$ f \text{ is continuous at } x \qed $$