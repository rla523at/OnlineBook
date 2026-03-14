# Ordered Field
## 정의
Field $\F$가 있다고 하자.

$\F$위의 total order $\le$가 다음을 만족할 때, $\F$를 `ordered filed`라고 한다.

$$ \begin{gathered} \forall x,y,z \in \F, \quad y \le z \implies x+y \le x+z \\ \forall x,y \in \F, \quad 0_\F \le x \land 0_\F \le y \implies 0_\F \le xy \end{gathered} $$

> Reference  
> {cite}`abbott` p.246

### 참고
$\F^+$는 다음과 같이 정의된 집합이다.

$$ \F^+ := \Set{ x \in \F | 0_\F < x} $$


### 명제1
Ordered field $\F$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \begin{gathered} x,y \in \F \\ 0_\F < x,y \\ x \le y \end{gathered} \implies \frac{1}{y} \le \frac{1}{x} $$

**Proof**

$$ \begin{aligned} x &\le y \\ 1_\F &\le \frac{1}{x} \cdot y \\ \frac{1}{y} &\le \frac{1}{x} \qed \end{aligned} $$