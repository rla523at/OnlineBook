# Convergence of Function
## 정의
Metric space $M_1,M_2$가 있다고 하자.

$X$가 $M_1$의 open set이고 $x$가 $X$의 limit point일 때, $f:X\rightarrow M_2$가 $x$에서 $y\in M_2$에 수렴한다는 말은 다음과 동치이다.
 
$$ \forall \epsilon \in \R^+, \enspace \exist \delta \in \R^+ \st \forall t \in X, \enspace 0 < d_1(x,t) < \delta \implies d_2(y,f(t)) < \epsilon $$

> Reference  
> {cite}`hubbard` p.92

### 참고1
현재 수렴의 정의는 $f$가 $x$에서 정의되어 있을 필요가 없게끔 정의되어 있다.

$x$가 $X$의 limit point이면서 $0 < d_1(x,t) < \delta$로 둠으로써 $t=x$가 되는 상황을 배제한다.

### 참고2
$t \rightarrow x$일 때, $f(t)$가 $y$로 수렴한다는 말은 다음과 같이 표현하기도 한다.

$$ \lim_{t \rightarrow x}f(t) = y $$

이 떄, $y$를 $t \rightarrow x$일 때 $f(t)$의 극한값이라고 한다.

#### 예시
$\R^n$의 openset $D$와 함수 $f:D\rightarrow \R^m$이 있다고 하자.

$a \in D$에서 $f$가 $b\in \R^m$에 수렴한다는 말은 다음과 같다.

$$ \begin{gathered} \lim_{x \rightarrow a} f(x) = b \\  \forall\epsilon \in \R^+, \enspace \exist\delta \in \R^+ \st \forall x \in D, \enspace 0 < \norm{a-x} < \delta \implies \norm{b - f(x)} < \epsilon \end{gathered} $$

### 참고3
open ball을 이용해서 표현하면 다음과 같다.

$$ \forall \epsilon \in \R^+, \quad \exist \delta \in \R \st t \in B_{M_1}(x,\delta) - \Set{x} \implies f(t) \in B_{M_2}(y,\epsilon) $$

### 참고4
$t$는 $x$와 항상 $\delta$만큼 떨어져 있기 때문에 극한을 정의하는데 있어 함수 $f(x)$는 $t = x$에서 반드시 정의되어 있을 필요는 없다.

> Reference  
> {cite}`stewart` 1.7   

### 참고5
함수의 극한값이 $L$이기 위해서는 $0 < d(x,t) < \delta$을 만족하는 모든 $t$에 대해서 $d(y,f(t)) < \epsilon$을 만족해야 된다.

$0 < d(x,t) < \delta$을 만족하는 특정 $x$에 대해 $d(y, f(t)) < \epsilon$을 만족하는 $L'$은 극한값이 될 수 없다.

#### 예시
$f(x) = \sin \frac{1}{x}$이 있다고 하자.

$x_n = \frac{1}{(2n + 0.5)\pi}, \enspace x_m = \frac{1}{(2m - 0.5)\pi}$로 두면, $x_n,x_m$ 모두 0에 얼마든지 가깝게 갈 수 있으나 $f(x_n) = 0.5, \enspace f(x_m) = -0.5$이다. 

따라서 $f(x)$의 경우에는 극한값이 존재하지 않게 된다.

> Reference  
> {cite}`hubbard` Chapter 1.5

### 참고6
극한의 정의가 갖는 의미를 이해하기 위해 구체적인 예시를 살펴보자. 

$\R$위에서 함수 $f(x)$가 다음과 같이 정의되어 있다고 하자.

$$ f(x) = \begin{cases} 2x-1 & \text{if} \quad x \neq 3 \\ 6 & \text{if} \quad x = 3 \end{cases} $$

함수의 정의로부터 $x \neq 3$일 때, $x$가 $3$으로 다가갈수록 $f(x)$가 5에 가까워진다는 것을 직관적으로 알 수 있다. 하지만 "다가간다"는 표현과 "가까워진다"는 표현이 명확하지 않아 "다가간다는것은 무엇인가?", "$x$가 $3$에 얼마나 다가가야 하는가?", "가까워진다는것은 무엇인가?", "$f(x)$가 $5$에 얼마나 가까워지는가?"에 대한 질문에 대답을 하기가 어렵다. 따라서 모호한 표현을 명확하게 하기 위해 다음과 같은 질문을 해보자.

"$x$와 3의 거리가 얼마 이하여야 $f(x)$와 5의 거리가 $\epsilon$이하가 되는가?"

여기서는 "다가간다", "가까워진다"라는 표현대신 거리라는 표현을 사용하였고 이를 통해 위의 질문을 수학적으로 표현할 수 있으며 위의 질문은 다음과 같은 수학 문제가 된다.


$$ \text{find } \delta \enspace \text{satisfying} \quad 0 < |x-3| < \delta \implies |f(x) - 5| < \epsilon $$

위 문제의 해가 존재한다면 $x$가 $(3-\delta, 3+\delta)$구간에 들어갈정도로 3에 충분히 가까이 가면 $f(x)$와 5의 오차가 $\epsilon$보다 작아진다는 의미를 갖는다. 즉, "$x$가 $3$에 얼마나 다가가야 하는가?"에 대한 대답이 $\delta$가 되고 "$f(x)$가 $5$에 얼마나 가까워지는가?"에 대한 대답이 $\epsilon$이 된다.  이를 그림으로 표현하면 다음과 같다.

```{figure} _image/1201.png
```

만약 임의의 양수 $\epsilon$에 대해서 위 문제에 해가 존재한다면, $x$가 3에 충분히 가까이만 간다면 $f(x)$는 얼마든지 5에 가까워 질 수 있다는 의미를 갖게되고 이러한 논의를 통해 $x$가 $3$으로 갈 때 $f(x)$의 극한값이 $5$라는 말은 다음과 같이 표현할 수 있다.

$$ \forall \epsilon \in \R^+ , \quad \exist \delta \enspace \text{satisfying} \quad 0 < |x-3| < \delta \implies |f(x) - 5| < \epsilon $$

다음은 실제로 $\delta$가 존재하는지를 확인해보자. $|f(x) - 5| = 2|x -3|$임으로, $|x-3| < 0.5\epsilon$면 $|f(x) - 5| < \epsilon$임을 알 수 있다. 따라서 $\delta$는 $0.5 \epsilon$보다 작은 임의의 양수면 되고 이로써 $\delta$는 존재함을 알 수 있다. $\delta$의 존재성을 보였음으로 $x$가 $3$으로 갈 때 $f(x)$의 극한값은 $5$다.

> Reference  
> {cite}`stewart` 1.7   

### 명제1
$U \subset \R^n, \enspace V \subset \R^m$와 함수 $\mathbf f : U \rightarrow V, \enspace \mathbf g : V \rightarrow \R^k$가 있다고 하자.

$\mathbf x \in U$에 대해서 다음이 성립한다.

$$ \lim_{\mathbf x \rightarrow \mathbf x} \mathbf f(\mathbf x) = \mathbf y_0, \enspace \lim_{\mathbf y \rightarrow \mathbf y_0} \mathbf g(\mathbf y) = \mathbf z_0$$

이 때, 다음을 증명하여라.

$$ \lim_{\mathbf x \rightarrow \mathbf x} (\mathbf g \circ \mathbf f)(\mathbf x) = \mathbf z_0 $$

**Proof**

$\lim_{\mathbf y \rightarrow \mathbf y_0} \mathbf g(\mathbf y) = \mathbf z_0$임으로 다음이 성립한다.

$$ \forall \epsilon >0, \quad \exist \delta_1 \st |\mathbf y - \mathbf y_0| < \delta_1 \implies |\mathbf g(\mathbf y) - \mathbf z_0| < \epsilon $$

또한, $\lim_{\mathbf x \rightarrow \mathbf x} \mathbf f(\mathbf x) = \mathbf y_0$임으로 다음이 성립한다.

$$ \exist \delta \st |\mathbf x - \mathbf x| < \delta \implies |\mathbf f(\mathbf x) - \mathbf y_0| < \delta_1 $$

두 결과를 통해 다음이 성립함을 알 수 있다.

$$ \forall \epsilon >0, \quad \exist \delta \st|\mathbf x - \mathbf x| < \delta \implies |(\mathbf g \circ \mathbf f)(\mathbf x) - \mathbf z_0| < \epsilon $$

따라서, 함수의 극한의 정의에 의해 다음이 성립한다.

$$ \lim_{\mathbf x \rightarrow \mathbf x} (\mathbf g \circ \mathbf f)(\mathbf x) = \mathbf z_0 \quad {_\blacksquare} $$

### 명제2
Metric space $M_1,M_2$가 있다고 하자.

$X$가 $M_1$의 open set이고 $x$가 $X$의 limit point이며 함수 $f:X\rightarrow M_2$가 있을 떄, $y \in M_2$에 대해 다음을 증명하여라.

$$ \lim_{t\rightarrow x} f(t) = y \iff \begin{gathered} \text{Let } a_n \text{ be an any sequence on } X-\Set{x} \text{ converge to } x \\ \lim_{n\rightarrow \infty} f(a_n) = y \end{gathered} $$

**Proof**

[$\implies$]  
전제에 의해 다음이 성립한다.

$$ \forall\epsilon\in\R^+, \enspace \exist\delta\in\R^+ \st \forall t \in X, \enspace 0 < d_1(x,t) < \delta \implies d_2(y,f(t))<\epsilon $$

이 때, $x$로 수렴하는 $X- \Set{x}$위의 임의의 sequence를 $a_n$이라 하면 다음이 성립한다.

$$ \forall\delta\in\R^+, \enspace \exist N \in \N \st N \le n \implies 0 < d_1(x,a_n) < \delta $$

$a_n \in X$임으로 다음이 성립한다.

$$ \begin{aligned} & \forall \epsilon\in\R^+, \enspace \exist N \in N \st N \le n \implies d_2(y,f(a_n)) < \epsilon \\ \implies&  \lim_{n\rightarrow \infty} f(a_n) = y \qed \end{aligned}  $$

[$\impliedby$]  
다음을 가정하자.

$$ \begin{aligned} & \lim_{t\rightarrow x} f(t) \neq y \\\implies& \exist\epsilon\in\R^+ \st \forall\delta\in\R^+, \enspace \exist t \in X \st 0 < d_1(x,t)<\delta \land \epsilon \le d_2(y,f(t)) \\\implies& \exist\epsilon\in\R^+ \st \forall n \in\N, \enspace \exist t\in X \st 0 <d_1(x,t)<\frac{1}{n} \land \epsilon\le d_2(y,f(t)) \end{aligned}  $$

$n \in \N$마다 위를 만족하는 $t$를 모은 sequence를 $t_n$이라 하자.

그러면, $t_n$은 $x$로 converge하는 $X-\Set{x}$위의 sequence임으로 전제에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{n\rightarrow\infty}f(t_n) = y \\ \iff& \forall\epsilon\in\R^+,\enspace \exist N\in\N \st N\le n \implies d_2(y,f(t_n)) < \epsilon \end{aligned}  $$

이는 가정에 모순임으로, proof by contradiction에 의해 다음이 성립한다.

$$ \lim_{t\rightarrow x} f(t) = y \qed $$