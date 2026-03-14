# First Countable
## 정의
Topological space $X$가 있다고 하자.

$X$가 다음을 만족할 때, $X$를 `first countable` topological space라고 한다.

$$ \forall x \in X, \quad \exist\text{ countable neighborhood basis} $$

### 명제1
다음을 증명하여라.

$$ \text{Every metric space is first countable.} $$

**Proof**

Metric space $M$이 있다고 하자.

$x \in M$이 있을 때, collection $\mathcal{B_x}$를 다음과 같이 정의하자.

$$ \mathcal{B_x} := \{ B_M(x,1/r) \enspace|\enspace r \in \N \} $$

Metric space에서 open ball은 open set임으로 다음이 성립한다.

$$ B \in \mathcal{B_x} \implies B \in \Set{\mathcal{N_x}} $$

Neighborhood의 정의에 의해 다음이 성립한다.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist\epsilon \in \R^+ \quad s.t \quad B_M(x,\epsilon) \subseteq \mathcal{N_x} $$

자연수의 성질에 의해 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist r \quad s.t. \quad B_M(x,1/r) \subseteq B_M(x,\epsilon) $$

따라서, 다음이 성립한다.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist B \in \mathcal{B_x} \quad s.t. \quad B \subseteq \mathcal{N_x} $$

Neighborhood basis의 정의에 의해 $\mathcal{B_x}$는 neighborhood basis이며, 정의에 의해 countable하다.

따라서, $\forall x\in M$마다 countable neighborhood basis $\mathcal{B_x}$가 존재함으로 first countable의 정의에 의해 $M$은 first countable하다. $\quad\tiny\blacksquare$

> Reference  
> [Proofwiki](https://proofwiki.org/wiki/Metric_Space_is_First-Countable)

### 명제2
First countable space $X$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \forall x \in X, \quad \exist\text{ a nested neighborhood basis at } x $$

**Proof**

$x \in X$가 있다고 하자.

$x$에서 $X$의 countable neighborhood basis를 $\mathcal{B_x} = \{ B_n|n\in\N \}$라 하자.

이 떄, sequence $\mathcal{B_x}(n)$을 다음과 같이 정의하자.

$$ \mathcal{B_x}(n) := \bigcap_{i=1}^n B_i $$

$\forall i \in \N, \quad B_i \in \Set{\mathcal{N_x}}$임으로 다음이 성립한다.

$$ \forall n \in \N, \quad \mathcal{B_x(n)} \in \Set{\mathcal{N_x}} $$

$\mathcal{B_x}(n)$의 정의에 의해 다음이 성립한다.

$$ \mathcal{B_x}(n+1) \subseteq \mathcal{B_x}(n) $$

$\mathcal{B_x}$가 neighborhood basis이기 때문에 다음이 성립한다.

$$ \forall \mathcal{N_x}, \quad \exist N \in \N \quad s.t. \quad B_N \subseteq \mathcal{N_x} $$

따라서, $\mathcal{B_x}(n)$의 정의에 의해 다음이 성립한다.

$$ \forall \mathcal{N_x}, \quad \exist N \in \N \quad s.t. \quad N \le n \implies  \mathcal{B_x}(n) \subseteq \mathcal{N_x} $$

Nested neighborhood basis의 정의에 의해 $\mathcal{B_x}(n)$은 nested neighborhood basis이다. $\qed$

### 명제3
First countable space $X$와 $X$의 subset $U$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ x \in \bar{U} \implies \exist\text{ sequence } s \text{ on } U \st \text{ converge to } x $$

**Proof**

$x \in \bar{U}$에서 nested neighborhood basis를 $\mathcal{B_x(n)}$라 하자.

$U$위의 sequence $s(n)$을 다음과 같이 정의하자.

$$ s(n) := \text{one of a point in } \mathcal{B_x}(n) \cap U$$

Closure의 성질에 의해 $\mathcal{B_x(n)} \cap U \neq \empty$임으로 $s(n)$은 정의 할 수 있다.

$s(n)$의 정의에 의해 $s(n) \in \mathcal{B_x}(n)$임으로, nested neigborhood basis의 성질에 의해 다음이 성립한다.

$$ \lim_{n \rightarrow \infty} s(n) = x $$

그럼으로 $x$에 수렴하는 $U$위의 sequence $s(n)$이 존재한다. $\qed$

#### 따름명제3.1
First countable space $X$와 $X$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ x \in \bar{U} \iff \exist\text{ sequence } s \text{ on } U \st \text{ converge to } x $$

**Proof**

[$\implies$]  
명제3에 의해 성립한다.

[$\impliedby$]  
convergence의 성질에 의해 성립한다.

> Reference  
> [proofwiki](https://proofwiki.org/wiki/Sequence_Lemma)

### 명제4
First countable space $X$와 $X$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ U \text{ is closed set of } X \iff \begin{gathered} U\text{ contains every limit of} \\ \text{every convergent sequence of points in } U \end{gathered}  $$

**Proof**

[$\implies$]  
$x \in X$가 있다고 하자.

따름명제3.1에 의해, $x$로 수렴하는 $U$위의 모든 convergent sequence $s(n)$에 대해 다음이 성립한다.

$$ x \in \bar{U} $$

전제에 의해 $\bar{U} = U$임으로 다음이 성립한다.

$$ x \in U \qed $$

[$\impliedby$]  
다음을 가정하자.

$$ U \text{ is not a closed set of } X $$

Closure의 성질에 의해 다음이 성립한다.

$$ \exist x \in X-U \st x \in \bar{U} $$

명제3에 의해 위를 만족하는 $x$로 수렴하는 $U$위의 sequence가 존재함으로, 전제에 의해 다음이 성립한다.

$$ x \in U $$

하지만 이는 $x \in X-U$라는 사실에 모순된다.

따라서, proof by contradiction에 의해 다음이 성립한다.

$$ U \text{ is a closed set of } X \qed $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/3002079/what-is-the-proof-that-first-countable-is-sufficient-to-say-that-sequentially-cl)

#### 참고1
$U$가 다음을 만족할 때, $U$를 sequentially closed set이라고 한다.

$$ U\text{ contains every limit of every convergent sequence of points in } U $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1912653/a-subset-of-a-topological-space-is-closed-iff-it-contains-all-its-limit-points)

#### 참고2
$X$가 first countable space가 아니더라도 closed set은 sequentially closed set이지만 그 역은 성립하지 않는다.

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/2940442/why-closed-implies-sequentially-closed-but-not-the-converse)
