# Lindelof Space
## 정의
Topological space $X$가 있다고 하자.

$X$가 다음을 만족할 때, $X$를 `Lindelof space`라고 한다.

$$ \text{Every open cover of } X \text{ has a countable subcover} $$

### 명제1
다음을 증명하여라.

$$ \text{Every second countable space is a Lindelof space} $$

**Proof**

Second countable space $X$가 있다고 하자.

$\mathcal{U}$가 $X$의 임의의 open cover라고 하자.

$X$의 countable basis를 $\mathcal{B}$라 할  때, $\mathcal{B}$의 subset $\mathcal{B'}$를 다음과 같이 정의하자.

$$ \mathcal{B'} := \Set{B \in \mathcal{B} | \exist U \in \mathcal{U} \st B \subseteq U} $$

$\mathcal{B'}$는 countable set의 subset임으로 countable set이다.

$\forall B \in \mathcal{B'}$에 대해, $\mathcal{U}$의 subset $\mathcal{U_B'}$를 다음과 같이 정의하자.

$$ \mathcal{U}_B' := \Set{U \in \mathcal{U} | B \subseteq U} $$

다음으로, $\mathcal{U}$의 subset $\mathcal{U'}$를 다음과 같이 정의하자.

$$ \mathcal{U'} := \Set{U \in \mathcal{U} | \forall B \in \mathcal{B'}, U \text{ is a one of element in } \mathcal{U_B'}} $$

$\mathcal{B'}$는 countable set임으로, 정의에 의해 $\mathcal{U'}$는 countable set이다.

또한, 보조명제1.1에 의해 $\mathcal{U'}$은 subcover임으로 다음이 성립한다.

$$ \text{Every open cover of } X \text{ has a countable subcover}  $$

따라서, Lindelof space의 정의에 의해 다음이 성립한다.

$$ X \text{ is a Lindelof space} $$

그럼으로, 다음이 성립한다.

$$ \text{Every second countable space is a Lindelof space} \qed $$


#### 보조명제1.1
다음을 증명하여라.

$$ \mathcal{U'} \text{ is cover of } X $$

**Proof**

$\mathcal{U}$가 open cover임으로 다음이 성립한다.

$$ \forall x \in X, \quad \exist U \in \mathcal{U} \st x \in U $$

위를 만족하는 $U$가 open set임으로, basis criterion에 의해 다음이 성립한다.

$$ \exist B \in \mathcal B \st x \in B \subseteq U $$

위를 만족하는 $B$에 대해 다음이 성립한다.

$$ B \in \mathcal{B'} $$

따라서, $\mathcal{U}'$의 정의에 의해 다음이 성립한다.

$$ \exist U' \in \mathcal{U}' \st B \subseteq U' $$

위의 결과를 종합하면 다음과 같다.

$$ \forall x \in X, \quad \exist U' \in \mathcal{U}' \st x \in U' $$

그럼으로, cover의 정의에 의해 다음이 성립한다.

$$ \mathcal{U'} \text{ is cover of } X \qed $$