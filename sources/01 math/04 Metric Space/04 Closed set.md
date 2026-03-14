# Closed set
## 정의
Metric space $M$과 $S \le M$이 있다고 하자.

다음을 만족하는 $S$를 $M$에서의 `closed set`이라고 한다.

$$ M - S \text { is an open set on } M $$

> Reference  
> {cite}`hubbard` 1.5  

### 명제1
Metric space $M$이 있다고 하자.

$x \in M$이라 할 때, 다음을 증명하여라.

$$ \{ x \} \text{ is an closed set on } M $$

**Proof**

[$M - \{x\} = \empty$]  
$\empty$는 open set임으로, $M - \{x\} = \empty$는 $M$에서 open set이고 $\{x\}$는 $M$에서 closed set이다.

[$M - \{x\} \neq \empty$]  
$y \in M - \{x\}$이라 하면 다음이 성립한다.

$$ B_M(y, d(x,y)/2) \le M-\{x\} $$

따라서, $M - \{x\}$는 $M$에서 open set이고 $\{ x\}$는 $M$에서 closed set이다. $\quad\tiny\blacksquare$

### 명제2
Metric space $M$과 $S \le M$이 있다고 하자.

$S$위의 임의의 sequence $s(m)$가 있을 때, 다음을 증명하여라.

$$ S \text { is a closed.} \iff \lim_{m \rightarrow \infty} s(m) \in S $$

**Proof**

[$\implies$]  
$\lim_{m \rightarrow \infty} s(m) = x_0 \in M-S$라고 가정하자.

$U$가 closed set임으로 $M-S$는 open set이고 따라서 다음이 성립한다.

$$ \exist r >0 \quad s.t. \quad B(x_0,r) \subset M-S $$

이 떄, 모든 $m$에 대해서 $s(m) \in S$임으로 다음이 성립한다.

$$ r \le  |s(m) - x_0| $$

하지만 수렴의 정의에 의해 다음도 동시에 성립해야 한다.

$$ \forall \epsilon > 0, \quad \exist N \quad s.t. \quad N < m \implies |s(m) - x_0| < \epsilon $$

이는 모순임으로, 귀류법에 의해 $x_0 \in S$이다. $\qed$

[$\impliedby$]  
$M - S$가 open set이 아니라고 가정하자.

그러면 임의의 $n \in \N$에 대해서 다음을 만족하는 $x \in M - S$이 존재한다.

$$ B(x, 1/n) \cap S \neq \empty  $$

$B(x, 1/n) \cap S$ 위의 sequence를 $s_{1/n}(m)$이라 하면 다음이 성립한다.

$$ \lim_{m \rightarrow \infty}s_{1/n}(m) \in S $$

이 때, $n \rightarrow \infty$으로 가면 $B(x, 1/n) = \{ x \}$가 되고 따라서 다음이 성립한다.

$$ \lim_{m \rightarrow \infty}(\lim_{n \rightarrow \infty}s_{1/n})(m) = x $$

따라서, $x \in S$여야 하는데 이는 모순임으로, 귀류법에 의해 $M - S$는 open set이고 $S$는 closed set이다. $\qed$

> Reference  
> {cite}`hubbard` 1.5  

### 예시1
$U \le \R^2$가 아래 그림과 같이 회색으로 표현된 영역과 굵은 선으로 표시된 boundary를 포함한 부분이라고 하자.

```{figure} _image/0401.png
```

그러면 그림과 같이, 흰색으로 표현된 $\mathbf x \in U$를 boundary에 잡게 되면, open set이 되기 위한 $r$이 존재하지 않음을 알 수 있다. 

반면에 $\R^2 - U$의 경우 $U$가 모든 boundary를 포함하고 있기 때문에 반대로 boundary를 포함하지 않게 된다. 따라서 $\R^2 - U$은 open set이 됨을 알 수 있으며, 이로써 $U$는 closed set이 된다.

즉, closed set이 되기 위해서는 기하학적으로 boundary를 전부 포함해야 된다는 것을 알 수 있다.

> Reference  
> {cite}`hubbard` 1.5  

### 예시2
$a,b \in \R$라 하자.
* $a < b$라 할 때, $[a,b] := \{ x \in \R \enspace | \enspace a \le x \le b \}$는 closed set이다.
* $a < b$라 할 때, $(a,b] := \{ x \in \R \enspace | \enspace a < x \le b \}$는 open set도 closed set도 아니다.
* $[a, \infty]$는 closed set이다.

두 번째, 예시에서 알 수 있듯이, open set이 아니라고 반드시 closed set인것은 아니다. open set도 closed set도 아닌 경우가 있다.

> Reference  
> {cite}`hubbard` 1.5  

### 예시3
$f(x,y) = \sqrt{\frac{x}{y}}$의 natural domain(함수가 well-defined되는 정의역)은 open set도 closed set도 아니다.

> Reference  
> {cite}`hubbard` 1.5  