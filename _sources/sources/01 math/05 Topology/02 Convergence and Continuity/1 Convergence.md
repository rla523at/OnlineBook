# Convergence
## 정의
Topological space $X$가 있다고 하자.

$s(n)$이 $X$위의 sequence일 때, $s(n)$이 $x \in X$에 `수렴(convergence)`한다는 말은 다음과 동치이다.

$$\forall N_x \in \mathcal{N_x}, \quad  \exist N \in \N \quad s.t. \quad  N \le n \implies s(n) \in N_x$$

> Referece   
> {cite}`LeeTM` p.26

### 참고1
$s(n)$이 $x$에 수렴한다는 말은, $s(n)$의 `극한(limit)`이 $x$라고도 한다.

### 참고2
$s(n)$이 $x$에 수렴한다는 다음과 같이 표기한다.

$$ s(n) \rightarrow x $$

또는 다음과 같이 표기한다.

$$ \lim_{i\rightarrow\infty} s(i) = x $$

### 명제1
Metric space $M$이 있을 떄, 다음을 증명하여라.

$$\text{metric space convegence definition } \iff \text{topological space convegence definition} $$

> Referece  
> {cite}`LeeTM` exercise 2.12

### 명제2
Topological space $X$와 $a \in X$가 있다고 하자.

$X$위의 constant sequence $s=\{a,a,\cdots\}$가 있을 때, 다음을 증명하여라.

$$ s \text{ may not converge to } a $$

**Proof**

$X$가 다음과 같이 주어졌다고 하자.

$$ X = \{ 1,2,3 \}, \enspace \mathcal{T}_X = \{ \empty, \{ 1 \}, \{ 1,2 \}, \{ 1,2,3 \} \} $$

$X$위의 constant sequence $s = \{ 2,2,\cdots \}$가 있다고 하자.

$\forall\mathcal N_3 \in \Set{\mathcal{N_3}}$에 대해, $2 \in \mathcal{N_3}$임으로 $s(n)$은 $3$에도 수렴한다. $\qed$

### 명제3
Topological space $X$가 있다고 하자.

$X$위의 sequence $s$가 있을 때, 다음을 증명하여라.

$$ s \text{ may not converge to one point } $$

#### 예제3.1

$X$가 다음과 같이 주어졌다고 하자.

$$ X = \{ 1,2,3 \}, \enspace \mathcal{T}_X = \{ \empty, \{ 1 \}, \{ 1,2 \}, \{ 1,2,3 \} \} $$

$X$위의 constant sequence $s = \{ 2,2,\cdots \}$가 있다고 하자.

$2 \in \forall\mathcal N_2$인 동시에 $2 \in \forall\mathcal N_3$임으로 $s(n)$은 $2,3$에 수렴한다. $\qed$

#### 예제3.2
$X$에 Trivial topology가 주어졌다고 하자.

$x \in X$라 하자.

$X$가 trivial topology를 갖음으로 다음이 성립한다.

$$ \mathcal N_x = X $$

$x$의 유일한 neighborhood가 $X$임으로, $s \in \mathcal N_x$이고 다음이 성립한다.

$$ s \text{ is converge to } x $$

임의의 $x \in X$에 대해 위가 성립함으로 다음이 성립한다.

$$ s \text{ is coverge to every points in } X $$

#### 예제3.3
$X$가 $\N$에 cofinite topology가 주어진 topological space라고 하자.

$\N$위의 sequence $a_n = n$이 있고 $\N$의 임의의 element를 $x$라고 하면 다음이 성립한다.

$$ \begin{aligned} &\forall N_x \in \mathcal{N_x}, \quad x \le n \implies a_n \in N_x \\\implies& \forall N_x \in \mathcal{N_x}, \quad \exist N \in \N \st N \le n \implies a_n \in N_x \\\implies& a_n \text{ is converge to } x \end{aligned} $$

임의의 $x\in\N$에 수렴함으로 다음이 성립한다.

$$ a_n \text{ is coverge to every points in } \N $$

#### 참고
명제2와 명제3은 기존의 가지고 있던 convergence의 직관과는 다른 결과들이다.

이는 topology가 가지고 있는 유연한 구조 때문이며, 기존의 convergence의 직관을 유지하기 위해서는 "좋은 topology"의 필요성이 생긴다.

### 명제4
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

$x \in X$가 있을 떄, 다음을 증명하여라.

$$  \exist\text{ sequence } s \text{ on } U \st \text{ converge to } x \implies x \in \bar{U} $$

**Proof**

$s$가 $x$에 converge함으로 다음을 만족한다.

$$ \forall N_x \in \mathcal{N_x}, \quad \exist N \in \N \st N \le n \implies s(n) \in \mathcal{N_x} $$

전제에 의해 $s(n)$은 $U$위의 sequence임으로,  $N \le n$에 대해 다음이 성립한다.

$$ \forall N_x \in \mathcal{N_x}, \quad s(n) \in U \cap N_x $$

따라서 다음이 성립한다.

$$ \forall N_x \in \mathcal{N_x}, \quad U \cap N_x \neq \empty $$


Closure의 성질에 의해 다음이 성립한다.

$$ x \in \bar{U} \qed $$