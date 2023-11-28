# Open Set
## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $S$가 다음을 만족할 때, $S$를 $X$의 `open subset` 또는 `open set`이라고 한다.

$$ S \in \mathcal{T_X} $$

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $S$가 있을 때, 다음을 증명하여라.

$$ U \text { is an open set of } X \iff \forall x \in U, \enspace \exist S_x \in \mathcal{T_X} \quad s.t. \quad S_x \subseteq U $$

**Proof**

[$\implies$]  
$U$가 open set임으로 모든 $x\in U$에서 $S_x = U$로 두면 자명하게 성립한다. $\qed$

[$\impliedby$]  
$\forall x \in X$에 대해, $x \in U$임으로 다음이 성립한다.

$$ U = \bigcup_{\forall x \in U} S_x $$

이 때, 임의의 $x \in U$에 대해서 $S_x \in \mathcal{T_X}$이고, Topology는 union에 닫혀 있음으로 다음이 성립한다.

$$ \bigcup_{\forall x \in U} S_x \in \mathcal{T_X} \implies U \in \mathcal{T_X} $$

따라서, $U$는 $X$의 open set이다. $\qed$

#### 참고
대우명제는 다음과 같다.

$$ \exist x \in S \st \forall\mathcal{N_x}, \quad \mathcal{N_x} \nsubseteq S \iff S \text{ is not an open set of } X $$

이 떄, 좌측 명제는 다음 명제와 동치이다.

$$ \begin{aligned} & \exist x \in S \st \forall\mathcal{N_x}, \quad \mathcal{N_x} \nsubseteq S \\\iff& \exist x \in S \st \forall\mathcal{N_x}, \quad \mathcal{N_x} \cap S \neq \empty \end{aligned} $$


### 명제2
Topological space $X$가 있다고 하자.

$Y$가 $X$의 open set일 떄, $\mathcal{T_Y}$를 다음과 같이 정의하자.

$$ \mathcal{T_Y} := \{ S \subseteq Y \enspace | \enspace S \subseteq Y \enspace\land\enspace S \in \mathcal{T_X} \} $$

이 떄, 다음을 증명하여라.

$$ \mathcal{T_Y} \text{ is a topology on } Y $$

**Proof**

[$\empty \in \mathcal{T_Y}$]  
$\empty$는 모든 집합의 부분집합임으로 자명하다.

[$Y \in \mathcal{T_Y}$]  
모든 집합은 자기 자신을 부분집합으로 갖음으로 자명하다. 

[finite intersection]  
$s_i \in \mathcal{T_Y}, \enspace i = 1, \cdots, n$이라 하자.

이 떄, 집합 $S$를 다음과 같이 정의하자.

$$ S = \bigcap_{i=1}^n s_i $$

$s_i \in \mathcal{T_X}, \enspace i = 1, \cdots, n$이고 $\mathcal{T_X}$는 topology임으로 다음이 성립한다.

$$ S \in \mathcal{T_X} $$

그리고 $\mathcal{T_Y}$의 정의에 의해 $s_i \subset Y, \enspace i = 1, \cdots, n$임으로 다음이 성립한다.

$$ S \subset Y $$

따라서 $\mathcal{T_Y}$의 정의에 의해 다음이 성립한다.

$$ S \in \mathcal{T_Y}$$

[infinite intersection]  
집합 $S$를 다음과 같이 정의하자.

$$ S = \bigcup_{s \in \mathcal{T_Y}} s $$

$s \in \mathcal{T_Y} \Rightarrow s \in T_X$이고 $\mathcal{T_X}$는 topology임으로 다음이 성립한다.

$$ S \in \mathcal{T_X} $$

그리고 $s \in \mathcal{T_Y} \Rightarrow s \subset Y$임으로 다음이 성립한다.

$$ S \subset Y $$

따라서 $\mathcal{T_Y}$의 정의에 의해 다음이 성립한다.

$$ S \in \mathcal{T_Y} $$

$\mathcal{T_Y}$가 topology의 조건을 전부 만족함으로, $\mathcal{T_Y}$는 topology이다. $\qed$