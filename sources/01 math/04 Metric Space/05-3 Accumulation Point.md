# Accumulation Point
## 정의
Metric space $M$과 $M$의 subset $U$가 있다고 하자.

$x \in M$이 다음을 만족할 때, $x$를 $U$의 `accumulation point`라고 한다.

$$ \forall\epsilon\in\R^+, \quad B_M(x,\epsilon) \cap (U - \Set{x}) \neq \empty $$

### 참고
accumulation point를 `limit point` 또는 `cluster point`라고 한다.

### 명제1
$\R$의 subset $U$가 다음과 같이 정의되어 있다고 하자.

$$ U := \Set{\frac{1}{n}| n \in \N} $$

이 떄, 다음을 증명하여라.

$$ 0 \text{ is an accumulation point of } U $$

**Proof**

Archimedean property에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \quad \exist n \in \N \st \frac{1}{n} < \epsilon \\\implies& \forall\epsilon\in\R^+, \quad B_\R(0,\epsilon)\cap (U -\Set{0}) \neq \empty \\\implies& 0 \text{ is an accumulation point of } U \qed \end{aligned} $$