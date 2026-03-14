# Seperable
## 정의
Topological space $X$가 있다고 하자.

$X$가 다음을 만족할 경우, `seperable` space라고 한다.

$$ X \text{ contains a countable dense subset} $$

### 명제1
다음을 증명하여라.

$$ \text{Every second countable space is a seperable space} $$

**Proof**

Second countable space $X$가 있다고 하자.

Second countable space의 정의에 의해 다음이 성립한다.

$$ \exist\text{countable basis }\mathcal{B} \text{ of } X $$

이로부터 $\mathcal{B}$ 원소의 collection $\mathcal{B'}$을 다음과 같이 정의하자

$$ \mathcal{B'} := \Set{ B \in \mathcal{B} | B \neq \emptyset} $$

정의에 의해 $\mathcal{B'}$는 countable임으로 다음과 같이 표현하자. 

$$\mathcal{B'} = \Set{B_N'|N \in \N}$$

이 떄, countable set $S$를 다음과 같이 정의하자.

$$ S = \Set{s_N | \text{one of element in } B'_N, \enspace N=1,\cdots }  $$

$X$의 임의의 nonempty open set을 $U$라고 할 떄, Basis의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \exist\mathcal{B'_1} \subseteq \mathcal{B'} \st U =  \bigcup\mathcal{B'_1} \\\implies& \exist N \in \N \st s_N \in U \\\implies& U \cap S \neq \empty \end{aligned} $$

Dense의 성질에 의해 다음이 성립한다.

$$ S \text{ is dense in } X $$

따라서, 모든 second countable space는 $S$라는 countable dense subset을 갖음으로 seperable의 정의에 의해 다음이 성립한다.

$$ \text{Every second countable space is a seperable space} \qed $$