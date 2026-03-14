# Second Countable
## 정의
Topological space $X$가 있다고 하자.

$X$가 다음을 만족할 때, $X$를 `second countable` space라고 한다.

$$ \mathcal{T_X} \text{ has countable basis} $$

### 명제1
다음을 증명하여라.

$$ \text{Every Euclidean space is a second countable space} $$

**Proof**

Euclidean space $\R^n$이 있다고 하자.

$\R^n$의 open ball의 collection $\mathcal{B}$를 다음과 같이 정의하자.

$$ \mathcal{B} := \Set{ B_{\R^n}(x,r)| r \text{ is rationale and } x \text{ has rationale coordinate}} $$

보조명제1.1에 의해 $\R^n$의 open ball은 $\mathcal{B}$의 원소들의 union으로 표현되고, Open set은 open ball의 union으로 표현됨으로 다음이 성립한다.

$$ \text{every open set in } \R^n \text{ is union of some elements in } \mathcal{B} $$

$\mathcal{B}$는 정의에 의해 countable임으로, $\mathcal{B}$는 $\R^n$의 countable basis이다.$\qed$

#### 보조명제1.1
다음을 증명하여라.

$$ \text{every open ball in } \R^n \text{ is union of some elements in } \mathcal{B} $$

**Proof**

임의의 open ball $B \in B_{\R^n}(x,r)$가 있다고 하자.

$\forall y \in B$에서 집합 $P_y$를 다음과 같이 정의하자.

$$ P_y := \Set{ p \in \R^n | p \text{ has rationale coordinate} \land \norm{p-x} + \norm{p-y} < r } $$

그러면 $p_y \in P_y$에 대해, 다음이 성립한다.

$$ \norm{y-p_y} < r-\norm{x-p_y} $$

또한, rationale number는 dense in real임으로 다음이 성립한다.

$$ \exist l \in \Q \st \norm{y-p_y} < l < r-\norm{x-p_y} $$

위를 만족하는 $l$을 $l_y$라 하면, $\forall z \in B_{\R^n}(p_y,l_y) \in \mathcal{B}$에 대해 다음이 성립한다.

$$ \begin{aligned} \norm{x-z} &= \norm{x-p_y+p_y-z} \\&< \norm{x-p_y} + \norm{p_y - z} \\&< \norm{x-p_y} + l  \end{aligned} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & y \in B_{\R^n}(p_y,l_y) \land B_{\R^n}(p_y,l_y) \subseteq B \\\implies& B = \bigcup_{y \in B} B_{\R^n}(p_y,l_y) \end{aligned}  $$

$B_{\R^n}(p_y,l_y) \in \mathcal{B}$임으로, 다음이 성립한다.

$$ \text{every open ball in } \R^n \text{ is union of some elements in } \mathcal{B} \qed $$

### 명제2
다음을 증명하여라.

$$ \text{Every second countable space is a first countable space} $$

**Proof**

Second countable topological space $X$가 있다고 하자.

Second countable space의 정의에 의해 다음이 성립한다.

$$ \mathcal{T_X} \text{ has countable basis } \mathcal{B} $$

$\forall x \in X$마다 $\mathcal{B}$의 sub collection $\mathcal{B_x}$를 다음과 같이 정의하자.

$$ \mathcal{B_x} := \Set{ B \in \mathcal{B} | x \in B } $$

그러면 Neighborhood basis의 성질에 의해서 다음이 성립한다.

$$ \forall x \in X, \quad \mathcal{B_x} \text{ is a neighborhood basis of } X \text{ at } x $$

이 때, $\mathcal{B}$가 countable임으로 다음이 성립한다.

$$ \mathcal{B_x} \text{ is a countable set} $$

따라서, 위 결과를 종합하면 다음이 성립한다.

$$ \forall x \in X, \quad \exist\text{countable neighborhood basis} $$

그럼으로, first countable space의 정의에 의해 다음이 성립한다.

$$ X \text{ is first countable space } \qed $$

### 명제3
다음을 증명하여라.

$$ \text{Any open set of a second countable space is second countable} $$

**Proof**

Second countable space $X$가 있다고 하자.

Second countable space의 정의에 의해 다음이 성립한다.

$$ \mathcal{T_X} \text{ has countable basis } \mathcal{B} $$

$U$가 $X$의 open set이라고 할 때, $\mathcal{B_U}$를 다음과 같이 정의하자.

$$ \mathcal{B_U} := \Set{ B \in \mathcal{B} | B \subseteq U } $$

Basis의 성질에 의해 다음이 성립한다.

$$ \mathcal{B_U} \text{ is a basis of } \mathcal{T_U} $$

또한 countable set의 subset은 countable임으로 다음이 성립한다.

$$ \mathcal{B_U} \text{ is countable }$$

따라서, $\mathcal{T_U}$가 countable basis를 갖음으로 second countable의 정의에 의해 다음이 성립한다.

$$ U \text{ is second countable } \qed $$