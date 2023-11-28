# Adherent Point
## 정의
Metric space $M$과 $M$의 subset $U$가 있다고 하자.

$x\in M$이 다음을 만족할 때, $x$를 $U$의 `adherent point`라고 한다.

$$ \forall\epsilon\in\R^+, \quad B_M(x,\epsilon) \cap U \neq \empty $$

### 명제1
Metric space $M$과 $M$의 subset $U$가 있다고 하자.

$x \in M$에 대해, 다음을 증명하여라.

$$ x \text{ is an adherent point } \iff \forall N_x \in \mathcal{N_x}, \quad N_x \cap U \neq \empty $$

**Proof**

[$\implies$]  
$\mathcal{N_x}$의 임의의 element를 $N_x$라고 하자.

$N_x$는 $x$를 포함하는 open set임으로 다음이 성립한다.

$$ \exist\epsilon\in\R^+ \st B_M(x,\epsilon) \subseteq N_x $$

위를 만족하는 $\epsilon$에 대해 전제에 의해 다음이 성립한다.

$$ B_M(x,\epsilon) \cap U \neq \empty \implies N_x \cap U \neq \empty $$

임의의 $N_x$에 대해서 위가 성립함으로 다음이 성립한다.

$$\forall N_x \in \mathcal{N_x}, \quad N_x \cap U \neq \empty \qed$$

[$\impliedby$]  
임의의 open ball은 $\mathcal{N_x}$의 element임으로 adherent point의 정의에 의해 다음이 성립한다.

$$ x \text{ is an adherent point } \qed $$

### 명제2
$\R$의 bounded above subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \sup(U) \text{ is an adherent point of } U $$

**Proof**

supremum의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \quad \exist u \in U \st \sup(U)-\epsilon < u \le \sup(U) \\\implies& \forall\epsilon\in\R^+, \quad B_\R(\sup(U),\epsilon) \cap U \neq \empty \\\implies& \sup(U) \text{ is an adherent point of } U \qed \end{aligned}  $$

### 명제3
Metric space $M$이 있다고 하자.

$M$의 subset을 $U$라고 할 때, 다음을 증명하여라.

$$ U \text{ is closed } \iff U \text{ contains its all adherent points} $$

**Proof**

[$\implies$]  
다음을 가정하자.

$$ U \text{ doesn't contain its all adherent points} $$

이 때, $x$가 $U$에 포함되어 있지 않은 adherent point라고 하자.

그러면 $X-U$가 open set임으로 다음이 성립한다.

$$ \exist\epsilon\in\R^+ \st B_M(x,\epsilon) \subseteq X-U $$

이는 $x$가 adherent point라는 사실에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ U \text{ contains its all adherent points} \qed $$

[$\impliedby$]  
다음을 가정하자.

$$ U \text{ is not closed} $$

그러면 다음이 성립한다.

$$ \begin{aligned} &X-U \text{ is not open} \\\implies& \exist x \in X-U \st \forall \epsilon\in\R^+,\quad B_M(x,\epsilon) \cap U \neq \empty \\\implies& x \text{ is an adherent point of } U \end{aligned} $$

이는 전제에 모순됨으로 proof by contradiction에 의해 다음이 성립한다.

$$ U \text{ is closed} $$


