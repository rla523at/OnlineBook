# Subspace
## 정의
Topological space $X$가 있다고 하자.

$U$가 $X$의 subset이고 $\mathcal{T_U}$가 subspace topology일 때, $(U,\mathcal{T_U})$를 $X$의 subspace라고 한다.

### 명제1
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

$X$의 subspace $U$가 있을 떄, 다음을 증명하여라.

$$ \text{open set in }U \text{ may not be open in } X $$

**Proof**

$U$는 subspace $U$의 open set이지만, $X$의 open set은 아닐 수 있다. $\qed$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1138151/show-that-a-set-that-is-open-in-the-subspace-topology-is-open-in-the-full-space)

### 명제2
Topological space $X$와 $X$의 openset $U$가 있다고 하자.

$X$의 subspace $U$가 있을 떄, 다음을 증명하여라.

$$ \text{open set in }U \text{ is an open set in } X $$

**Proof**

$U$의 open set을 $A$라 하자.

subspace의 정의에 의해 다음이 성립한다.

$$ \exist V \in \mathcal T_X \quad s.t. \quad A = U \cap V $$

$U \in \mathcal{T_X}$임으로, topology의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & U \cap V \in \mathcal T_x \\\implies& A \in \mathcal T_x \qed \end{aligned} $$

### 명제3
Topological space $X$와 $X$의 open set $U,V$가 있다고 하자.

$U,V$가 $X$의 subspace일 때, 다음을 증명하여라.

$$ U \cap V \text{ is an open set on } U,V $$

**Proof**

Topology의 정의에 의해 다음이 성립한다.

$$ U \cap V \text{ is an open set on X} $$

$U,V$가 $X$의 subspace임으로, 다음이 성립한다.

$$ \text{Every open set of } X \text{ in } U,V \text{ is an open set on } U,V $$

이 떄, $U \cap V$는 $X$의 open set이면서 동시에 $U,V$에 포함됨으로 다음이 성립한다.

$$ U \cap V \text{ is an open set on } U,V \qed $$