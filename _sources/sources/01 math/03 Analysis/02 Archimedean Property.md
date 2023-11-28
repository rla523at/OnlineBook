# Archimedean Property
## 정의
Ordered field $\F$가 다음을 만족할 때, `archimedean property`를 갖는다고 한다.

$$ \forall x \in \F, \quad \exist n \in \N \st x < n $$

### 명제1
Archimedean property를 만족하는 Ordered field $\F$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \forall x \in \F^+ \implies \exist n \in \N \st \frac{1}{n} < x  $$

**Proof**

$\F^+$의 임의의 element를 $x$라고 하자.

$1/x \in \F$임으로 Archimedean property에 의해 다음이 성립한다.

$$ \begin{aligned} & \exist n \in \N \st \frac{1}{x} < n \\\implies& \exist n \in \N \st \frac{1}{n} < x \qed \end{aligned} $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/61630/archimedean-property-of-the-rational-numbers)