# Closure
## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, collection $C$를 다음과 같이 정의하자.

$$ C := \Set{A \subseteq X | U \subseteq A \land A\text{ is a closed set of } X} $$

$X$에서 $U$의 closure $\bar{U}$는 다음과 같이 정의된다.

$$ \bar{U} := \bigcap_{A \in C}A $$

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄, 다음을 증명하여라.

$$ \bar{U} \text{ is a closed set of } X $$

**Proof**

Closed set의 성질에 의해 다음이 성립한다.

$$ \text{infinite intersection of closed set is closed set} $$

따라서, closed set의 intersection인 closure은 closed set이다. $\qed$

#### 참고
정의에 의해 closure은 $U$를 포함하는 가장 작은 $X$의 closed set이다.

### 명제2
Topological space $X$가 있다고 하자.

$x \in X$와 $X$의 subset을 $U$가 있을 떄, 다음을 증명하여라.

$$ x \in \bar{U} \iff \forall \mathcal{N_x}, \quad \mathcal{N_x} \cap U \neq \empty $$

**Proof**

[$\implies$]  
 다음을 가정하자.

$$ \exist \mathcal{N_x} \quad s.t. \quad \mathcal{N_x} \cap U = \empty $$

위를 만족하는 $\mathcal{N_x}$에 대해 다음이 성립한다.

$$ U \subseteq X-\mathcal{N_x} \land X - \mathcal{N_x} \text{ is closed set of }X $$

전제에 의해 $x \in \bar{U}$임으로, Closure 정의에 의해 다음이 성립한다.

$$ x \in X - \mathcal{N_x} $$

그러나, neighborhood의 정의에 의해 $x \in \mathcal{N_x}$임으로 모순이 발생한다.

따라서, proof by contradiction에 의해 다음이 성립한다.

$$ \forall \mathcal{N_x}, \quad \mathcal{N_x} \cap U \neq \empty \qed $$

[$\impliedby$]  
다음을 가정하자.

$$ x \notin \bar{U} $$

Collection $C$를 다음과 같이 정의하자.

$$ C := \Set{A \subseteq X | U \subseteq A \land A \text{ is a closed set of } X} $$

가정에 의해 다음이 성립한다.

$$ \exist A \in C \quad s.t. \quad x \notin A  $$

위를 만족하는 $A$에 대해 다음이 성립한다.

$$\begin{aligned} &X-A \text{ is an open set of } X \land x \in X-A \\\implies& X-A \in \Set{\mathcal{N_x}} \end{aligned} $$

따라서, 전제에 의해 다음이 성립한다.

$$ (X-A) \cap U \neq \empty $$

이는 $U \subseteq A$라는 사실과 모순됨으로, proof by contradiction에 의해 다음이 성립한다.

$$ x \in \bar{U} \qed $$

> Reference  
> [northeastern.edu](https://web.northeastern.edu/suciu/MATH4565/MATH4565-fa21-handout2.pdf)

#### 따름명제2.1
Topological space $X$가 있다고 하자.

$X$의 subset $A$가 있을 때, 다음을 증명하여라.

$$ X = \bar{A} \iff \text{Every nonempty open subset of } X \text{ contains a point of } A $$

**Proof**

[$\implies$]  
전제에 의해 $\bar{A} = X$임으로, 명제2에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall x \in \bar{A},  \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \cap A \neq \empty \\\implies& \forall x \in X,  \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \cap A \neq \empty  \end{aligned} $$

즉, 다음이 성립한다.

$$ \text{Every nonempty open subset of } X \text{ contains a point of } A \qed $$

[$\impliedby$]  
전제에 의해서 다음이 성립한다.

$$ x \in X \implies \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \cap A \neq \empty $$

따라서, closure의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & x \in \bar{A} \\ \implies& X = \bar{A} \end{aligned}  $$

### 명제3
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

$A$가 $X$의 open set일 떄, 다음을 증명하여라.

$$ A \subseteq X - U \iff A  \subseteq X - \bar{U} $$

**Proof**

[$\implies$]  
다음을 가정하자.

$$ A \nsubseteq X - \bar{U} $$

그러면 다음이 성립한다.

$$ \begin{aligned} &\exist x \in A \quad s.t. \quad x \notin X - \bar{U} \\ \implies& \exist x \in A \quad s.t. \quad x \in \bar{U} \end{aligned} $$

위를 만족하는 $x$에 대해, 명제2에 의해 다음이 성립한다.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \cap U \neq \empty $$

그리고 neighborhood의 정의에 의해 다음이 성립한다.

$$ A \in \Set{\mathcal{N_x}} $$

따라서, 다음이 성립한다.

$$ A \cap U \neq \empty $$

하지만 이는 $A \subseteq X-U$라는 전제에 모순된다.

그럼으로 proof by contradiction에 의해 다음이 성립한다.

$$ A \subseteq X - \bar{U} \qed $$

[$\impliedby$]  
$U \subseteq \bar{U}$임으로, 다음이 성립한다.

$$ X - \bar{U} \subseteq X - U $$

그럼으로, 다음이 성립한다.

$$ A \subseteq X-\bar{U} \implies A \subseteq X-U \qed $$

### 명제4
Topological space $X$가 있다고 하자.

$X$의 closed set $U$가 있을 때, 다음을 증명하여라.

$$ \bar{U} = U $$

**Proof**

Collection $C$를 다음과 같이 정의하자.

$$ C := \Set{A \subseteq X | U \subseteq A \land A \text{ is a closed set of } X} $$

$U$가 $X$의 closed set이기 때문에 다음을 만족한다.

$$ \forall A \in C, \quad U \subseteq A$$

따라서, 다음이 성립한다.

$$ \begin{aligned} \bar{U} &= \bigcap_{A \in C} A \\&= U \qed \end{aligned} $$

### 명제5
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ U \text{ is not a closed set of } X \implies \exist x \in X-U \st x \in \bar{U} $$

**Proof**

다음을 가정하자.

$$ \forall x \in X-U, \quad x \in X - \bar{U} $$

그러면 $U = \bar{U}$여야 함으로 다음이 성립한다.

$$ U \text{ is a closed set of } X $$

이는 전제에 모순됨으로 proof by contradiction에 의해 다음이 성립한다.

$$ \exist x \in X-U \st x \in \bar{U} \qed $$