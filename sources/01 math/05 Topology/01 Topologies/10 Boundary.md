# Boundary

## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, $X$에서 $U$의 `boundary` $\partial U$는 다음과 같이 정의된다.

$$ \partial U := X - (\Int(U) \cup \Ext(U)) $$

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, 다음을 증명하여라.

$$ \partial U = \bar{U} - \Int(U) $$

**Proof**

Boundary의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \partial U &= X - (\Int(U) \cup \Ext(U)) \\&= X \cap (\Int(U)^C \cap \Ext(U)^C) \\&= (X-\Int(U)) \cap (X-\Ext(U)) \end{aligned} $$

Exterior의 성질에 의해 다음이 성립한다.

$$ X - \Ext(U) = \bar{U} $$

따라서 다음이 성립한다.

$$ \begin{aligned} \partial U &= (X-\Int(U)) \cap \bar{U} \\&= X \cap \Int(U)^c \cap \bar{U} \\&= (X \cap \bar{U}) \cap \Int(U)^c \\&= \bar{U} \cap \Int(U)^C \\&= \bar{U} - \Int(U) \qed \end{aligned} $$

> Reference  
> [northeastern.edu](https://web.northeastern.edu/suciu/MATH4565/MATH4565-fa21-handout2.pdf)

#### 따름명제1.1
다음을 증명하여라.

$$ x \in \partial U \iff x \in \bar{U} \land x \notin \Int(U) $$

#### 따름명제1.2
다음을 증명하여라.

$$ x \in \partial U \implies x \in \bar{U} $$

#### 따름명제1.3
다음을 증명하여라.

$$ \partial U \subseteq \bar{U} $$

### 명제2
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, 다음을 증명하여라.

$$ \bar{U} = \Int(U) \cup \partial U $$

**Proof1**

명제 1에 의해 다음이 성립한다.

$$ \begin{aligned} \Int(U) \cup \partial U &= \Int(U) \cup (\bar{U} - \Int(U)) \\&= \Int(U) \cup (\bar{U} \cap \Int(U)^c) \\&= (\Int(U) \cup \bar{U}) \cap (\Int(U) \cup \Int(U)^c) \end{aligned} $$

$\Int(U)\subseteq\bar{U}$임으로 다음이 성립한다.

$$ \bar{U}\cup\Int(U)=\bar{U} $$

그리고 $\bar{U} \subseteq \Int(U) \cup \Int(U)^c$임으로, 다음이 성립한다.

$$ \bar{U} = \Int(U) \cup \partial U \qed $$

**Proof2**

두 집합$A,B$가 있을 떄, 집합 연산의 성질에 의해 다음이 성립한다.

$$ A = (A \cap B) \cup (A - B) $$

따라서, 다음이 성립한다

$$ \bar{U} = (\bar{U} \cap \Int(U)) \cup (\bar{U} - \Int(U)) $$

$\Int(U)\subseteq\bar{U}$임으로 다음이 성립한다.

$$ \bar{U}\cap\Int(U)=\Int(U) $$

따라서, 명제1에 의해 다음이 성립한다.

$$ \bar{U} = \Int(U)\cup\partial U \qed $$

#### 따름명제 2.1
다음을 증명하여라.

$$ x \in \bar{U} \iff x \in \Int(U) \cup \partial U $$

### 명제3
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, 다음을 증명하여라.

$$ \bar{U} = U \cup \partial U $$

**Proof**

명제 1에 의해 다음이 성립한다.

$$ \begin{aligned} U \cup \partial U &= U \cup (\bar{U} - \Int(U)) \\&= U \cup (\bar{U} \cap \Int(U)^c) \\&= (U \cup \bar{U}) \cap (U \cup \Int(U)^c) \end{aligned} $$

$U\subseteq\bar{U}$임으로 다음이 성립한다.

$$ \bar{U}\cup U=\bar{U} $$

그리고 $\Int(U) \subseteq U$ 임으로, 다음이 성립한다.

$$ \Int(U) \cup \Int(U)^c \subseteq U \cup \Int(U)^c $$

그리고 $\bar{U} \subseteq \Int(U) \cup \Int(U)^c$임으로, 다음이 성립한다.

$$ \bar{U} \subseteq U \cup \Int(U)^c $$

따라서, 다음이 성립한다.

$$ \bar{U} = U \cup \partial U \qed $$

#### 따름명제3.1
다음을 증명하여라.

$$ U \cup \partial U = \Int(U) \cup \partial U $$

### 명제4
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 때, 다음을 증명하여라.

$$ x \in \partial U \iff \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist y_1,y_2 \in \mathcal{N_x} \quad s.t. \quad y_1 \in U \land y_2 \in X-U $$

**Proof**

[$\implies$]  
명제1에 의해 다음이 성립한다.

$$ x \in \partial U \iff x \notin \Int(U) \land x \in \bar{U} $$

Interior의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & x \notin \Int(U) \\\implies&\nexists \mathcal{N_x} \quad s.t. \quad \mathcal{N_x} \subseteq U \\\implies& \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist y \in \mathcal{N_x} \quad s.t. \quad y \in X-U \end{aligned} $$

또한, closure의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & x \in \bar{U} \\\implies&\forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \cap U \neq \empty \\\implies& \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad  \exist y \in \mathcal{N_x} \quad s.t \quad y \in U \qed \end{aligned}  $$

[$\impliedby$]  
$x \in X$가 있다고 하자.

전제에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\mathcal{N_x}, \quad \exist y_1 \in \mathcal{N_x} \st  y_1 \in U \\\implies& \forall\mathcal{N_x}, \quad \mathcal{N_x} \cap U \neq \empty \\\implies& x \in \bar{U} \\& \forall\mathcal{N_x}, \quad \exist y_2 \in \mathcal{N_x} \st y_2 \in X-U \\\implies& \nexists\mathcal{N_x} \st \mathcal{N_x} \subseteq U \\\implies& x \notin \Int(U) \end{aligned} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} &x \notin \Int(U) \land x \in \bar{U} \\\implies& x \in \bar{U} - \Int(U) \\\implies& x \in \partial U \qed  \end{aligned}  $$

### 명제5
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄,다음을 증명하여라.

$$ \partial U \text{ is a closed set of } X $$

**Proof**

Boudnary의 정의에 의해 다음이 성립한다.

$$ X - \partial U = \Int(U)\cup\Ext(U) $$

이 떄, $\Int(U), \Ext(U)$ 모두 $X$의 open set이고 open set의 union은 open set임으로 다음이 성립한다.

$$ X - \partial U \text{ is an open set of } X $$

따라서 closed set의 정의에 의해 다음이 성립한다.

$$ \partial U \text{ is a closed set of } X \qed $$

### 명제6
Topological space $X$가 있다고 하자.

$X$의 open set $U$가 있을 떄, 다음을 증명하여라.

$$ U \cap \partial U = \empty $$

**Proof**

Boundary의 정의에 의해 다음이 성립한다.

$$ \partial U \cap \Int(U) = \empty  $$

이 떄, $U$가 open set임으로 다음이 성립한다.

$$ U = \Int(U) $$

따라서 다음이 성립한다.

$$ U \cap \partial U = \empty \qed $$

### 명제7
Topological space $X$가 있다고 하자.

$X$의 closed set $U$가 있을 떄, 다음을 증명하여라.

$$ \partial U \subseteq U $$

**Proof**

$U$가 closed set임으로 다음이 성립한다.

$$ U = \bar{U} $$

따라서, 명제1에의해 다음이 성립한다.

$$ \partial U \subseteq \bar{U} \implies \partial U \subseteq U \qed $$

