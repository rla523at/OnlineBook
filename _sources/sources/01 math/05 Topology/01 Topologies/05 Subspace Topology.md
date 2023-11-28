# Subspace Topology
## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을 떄, $U$위의 `subspace topology` $\mathcal{T_U}$는 다음과 같이 정의된 집합이다.

$$ \mathcal{T_U} := \Set{A \subseteq U | A = U \cap V, \enspace V \in \mathcal{T_X}} $$

> Reference  
> {cite}`LeeTM` p.49

### 참고1
Subspace topology는 `relative topology`로 부르기도 한다.

### 명제1
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

$U$위의 subspace topology를 $\mathcal{T_U}$라 할 떄, 다음을 증명하여라.

$$ \mathcal{T_U} \text{ is a topology on } U $$

**Proof**

[$\empty \in \mathcal{T_U}$]  
$\empty \in \mathcal{T_X}$임으로, $U \cap \empty = \empty \in \mathcal{T_U}$이다.$\qed$

[$U \in \mathcal{T_U}$]  
$X \in \mathcal{T_X}$임으로, $U \cap X = U \in \mathcal{T_U}$이다.$\qed$ 

[finite intersection]  
$V_i \in \mathcal{T_X}, \enspace i = 1, \cdots, n$가 있다고 하자.

$A_i = (U \cap V_i) \in \mathcal{T_U}, \enspace i = 1, \cdots, n$이라 하자.

그러면 다음이 성립한다.

$$ \begin{aligned} \bigcap_{i=1}^n A_i &= \bigcap_{i=1}^n (U \cap V_i) \\&= U \cap (\bigcap_{i=1}^n V_i)  \end{aligned}  $$

$V_i \in \mathcal{T_X}$이고 $\mathcal{T_X}$는 topology임으로 다음이 성립한다.

$$ \bigcap_{i=1}^n V_i \in \mathcal{T_X} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} &U \cap (\bigcap_{i=1}^n V_i) \in \mathcal{T_U} \\\implies& \bigcap_{i=1}^n A_i \in \mathcal{T_U} \qed \end{aligned} $$

[infinite intersection]   
$\mathcal{T_U}$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \bigcup_{U \in \mathcal{T_U}} U &= \bigcup_{V \in \mathcal{T_X}} (U \cap V) \\&=  U \cap \bigcup_{V \in \mathcal{T_X}} V \end{aligned} $$

$\mathcal{T_X}$는 topology임으로 다음이 성립한다.

$$ \bigcup_{{V \in \mathcal{T_X}}} V \in \mathcal{T_X} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & U \cap \bigcup_{{V \in \mathcal{T_X}}} V \in \mathcal{T_U} \\\implies& \bigcup_{{U \in \mathcal{T_U}}} U \in \mathcal{T_U} \qed \end{aligned} $$

[결론]  
$\mathcal{T_U}$가 topology의 조건을 전부 만족함으로, $\mathcal{T_U}$는 $U$의 topology이다. $\qed$