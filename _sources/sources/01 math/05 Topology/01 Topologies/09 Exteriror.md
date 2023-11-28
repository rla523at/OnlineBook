# Exteriror

## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, $X$에서 $U$의 `exterior` $\Ext(U)$는 다음과 같이 정의된다.

$$ \Ext(U) := X - \bar{U} $$

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄, 다음을 증명하여라.

$$ \Ext(U) \text{ is an open set of } X $$

**Proof**

closure의 성질에 의해 $\bar{U}$가 $X$의 closed set이다.

따라서, closed set의 정의에 의해 $X-\bar{U}$는 $X$의 open set임으로 다음이 성립한다. 

$$ \Ext(U) \text{ is an open set of } X \qed $$


### 명제2
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄, 다음을 증명하여라.

$$ x \in \Ext(U) \iff \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \mathcal{N_x} \subseteq X -U $$

**Proof**

[$\implies$]  
명제1에 의해 $\Ext(U)$는 open set임으로 다음이 성립한다.

$$ \Int(\Ext(U)) = \Ext(U) $$

따라서, interior의 성질에 의해 다음이 성립한다.

$$ x \in \Ext(U) \implies \exist\mathcal{N_x} \subseteq \Ext(U) $$

$\Ext(U)$의 정의에 의해 다음이 성립한다.

$$ \exist\mathcal{N_x} \subseteq \Ext(U) \iff \exist\mathcal{N_x} \subseteq X - \bar{U} $$

$U \subseteq \bar{U}$임으로 다음이 성립한다.

$$ \exist\mathcal{N_x} \subseteq X - \bar{U} \implies \exist\mathcal{N_x} \subseteq X - U $$

따라서, 다음이 성립한다.

$$ x \in \Ext(U) \implies \exist\mathcal{N_x} \subseteq X -U \qed $$

[$\impliedby$]  
$x \in X$가 있다고 하자.

전제에 의해 다음이 성립한다.

$$ \exist \text{open set } A \quad s.t. \quad x \in A \subseteq X-U $$

이 때, closure의 성질에 의해 다음이 성립한다.

$$ A \subseteq X-U \iff A \subseteq X - \bar{U} $$

Exterior의 정의에 의해 $A \subseteq \Ext(U)$임으로 다음이 성립한다.

$$ x  \in \Ext(U) \qed $$

### 명제3
Topological space $X$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ X - \Ext(U) = \bar{U} $$

**Proof**

Exterior의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} X - \Ext(U) &= X - (X - \bar{U})\\&= X \cap (X \cap \bar{U}^c)^c \\&= X \cap (X^c \cup\bar{U}) \\&= (X \cap X^c) \cup (X \cap \bar{U}) \\&= \bar{U}  \qed \end{aligned} $$