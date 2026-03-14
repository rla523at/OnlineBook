# G-Orbit
## Definition
Group $G$와 $G$-set $X$가 있다고 하자.

$X$의 임의의 element를 $x$라고 할 떄, 다음과 같이 정의된 집합 $G\cdot x$를 $x$의 `G-orbit`이라고 한다.

$$ G\cdot x := \Set{g\cdot x | g \in G} $$

## Isotropy Subgroup
Group $G$와 $G$-set $X$가 있다고 하자.

$X$의 임의의 element를 $x$라고 할 떄, 다음과 같이 정의된 집합 $G_x$를 $x$의 `isotropy subgroup`이라고 한다.

$$ G_x := \Set{g \in G | g\cdot x = x} $$

### 명제1
Group $G$와 $G$-set $X$가 있다고 하자.

$X$의 임의의 element를 $x$라고 할 떄, 다음을 증명하여라.

$$ G_x \le G $$

**Proof**

[closed]  
$G_x$의 임의의 element를 $g_1,g_2$라고 하면 다음이 성립한다.

$$ g_1g_2 \cdot x = g_1 \cdot g_2 \cdot x = g_1 \cdot x = x \implies g_1g_2 \in G_x \qed$$

[inverse]  
$G_x$의 임의의 element를 $g$라고 하면 다음이 성립한다.

$$ x = g^{-1}g \cdot x = g^{-1} \cdot g \cdot x = g^{-1} \cdot x \implies g^{-1} \in G_x \qed$$

### 명제2
Group $G$와 $G$-set $X$가 있다고 하자.

$X$의 임의의 element를 $x$라고 할 떄, 다음을 증명하여라.

$$ \varphi_x : G/G_x \rightarrow G\cdot x \st gG_x \mapsto g\cdot x \text{ is bijective} $$