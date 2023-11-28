# Interior
## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, collection $C$를 다음과 같이 정의하자.

$$ C:= \Set{A \subseteq X | A \subseteq U \land A \text{ is open set of } X} $$

$X$에서 $U$의 `interior` $\Int(U)$는 다음과 같이 정의된다.

$$ \Int(U) := \bigcup_{A \in C}A $$

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄, 다음을 증명하여라.

$$ \Int(U) \text{ is open set of } X $$

**Proof**

open set의 성질에 의해 다음이 성립한다.

$$ \text{infinite union of open set is open set} $$

따라서, open set의 union인 iㅜterior은 open set이다. $\qed$

#### 참고
정의에 의해 iterior은 $U$에 포함된 가장 큰 $X$의 open set이다.

### 명제2
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄, 다음을 증명하여라.

$$ x \in \Int(U) \iff \exist\mathcal{N_x} \st \mathcal{N_x} \subseteq U $$

**Proof**

Collection $C$를 다음과 같이 정의하자.

$$ C:= \Set{A \subseteq X | A \subseteq U \land A \text{ is open set of } X} $$


[$\implies$]  
명제1에 의해 $\Int(U)$는 open set임으로, neighborhood의 성질에 의해 다음이 성립한다.

$$ x \in \Int(U) \implies \exist\mathcal{N_x} \st \mathcal{N_x} \subseteq U \qed $$

[$\impliedby$]  
전제에 의해 다음이 성립한다.

$$ \exist A \in C \st x \in A $$

$\Int(U)$의 정의에 의해 다음이 성립한다.

$$ x \in A \implies x \in \Int(U) \qed $$

#### 참고
대우명제는 다음과 같다.

$$ x \notin \Int(U) \iff \nexists\mathcal{N_x} \st \mathcal{N_x} \subseteq U $$

이는 다음과 동치이다.

$$ x \notin \Int(U) \iff \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist y \in \mathcal{N_x} \st y \in  X-U $$

또한 다음과도 동치이다.

$$ x \notin \Int(U) \iff \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x}-U \neq \empty $$


### 명제3
Topological space $X$가 있다고 하자.

$X$의 open set $U$가 있을 때, 다음을 증명하여라.

$$ U = \Int(U) $$

**Proof**

Collection $C$를 다음과 같이 정의하자.

$$ C := \Set{A \subseteq X | A \subseteq U \land A \text{ is open set of } X} $$

$U$가 $X$의 open set이기 때문에 다음을 만족한다.

$$ U \in C$$

따라서, 다음이 성립한다.

$$ \begin{aligned} \Int(U) &= \bigcup_{A \in C} A \\&= U \qed \end{aligned} $$

### 명제4
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, 다음을 증명하여라.

$$ x \in X-U \implies x \notin \Int(U) $$

**Proof**

Interior의 정의에 의해, 다음이 성립한다.

$$ \Int(U) \subseteq U $$

따라서, 다음이 성립한다.

$$ \begin{aligned} x &\in X-U  \\& \in X-\Int(U) \\\implies x & \notin \Int(U) \qed \end{aligned} $$
