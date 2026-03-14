# Dense
## 정의
Topological space $X$가 있다고 하자.

$X$의 subset $A$가 다음을 만족할 떄, $A$가 $X$에서 `dense`하다고 하다.

$$ \bar{A} = X $$

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $A$가 있을 때, 다음을 증명하여라.

$$ A \text{ is dense in } X \iff \text{Every nonempty open subset of } X \text{ contains a point of } A $$

**Proof**

Closure의 성질에 의해 다음이 성립한다.

$$ X = \bar{A} \iff \text{Every nonempty open subset of } X \text{ contains a point of } A $$

따라서, dense의 정의에 의해 다음이 성립한다.

$$ A \text{ is dense in } X \iff \text{Every nonempty open subset of } X \text{ contains a point of } A \qed $$
