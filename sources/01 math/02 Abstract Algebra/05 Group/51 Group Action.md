# Group Action
Group $G$와 set $X$가 있다고 하자.

## Left Group Action
$G$의 $X$위의 왼쪽 군 작용(left group action of $G$ on $S$) $\cdot$는 다음 조건들을 만족시키는 함수다.

$$ \cdot : G \times X \rightarrow X \st \begin{array}{ll} \text{compatability} &  \forall g_1,g_2 \in G, \quad \forall x \in X, \quad g_1 \cdot (g_2 \cdot x) = (g_1 g_2) \cdot x \\ \text{identity} & \forall x \in X, \quad e_G \cdot x = x \end{array} $$

### 참고1
$G$의 $X$위의 왼쪽 군 작용이 정의되어 있는 경우 "$G$가 $X$에 왼쪽에서 작용한다"라고 얘기하고 $G \curvearrowright X$로 표기한다.

### 참고2
작용을 받는 set $X$를 $G$-set이라고 한다.

### 참고3
group action이 group operation과 compatible하다고 한다.

### 참고4
group action이 정의되어 있다는 얘기는 $\forall g \in G, \enspace \forall x \in X, \enspace g \circ x \in X$라는 얘기이다.

### 명제1
Group $G$와 $G$-set $X$가 있다고 하자.

$G$의 임의의 element를 $g$라고 할 때, 다음을 증명하여라.

$$ \lambda_g : X \rightarrow X \st x \mapsto g \cdot x \text{ is bijective} $$

**Proof**

[injective]  
$X$의 임의의 element를 $x,y$라고 하면 다음이 성립한다.

$$ \begin{aligned} & \lambda_g(x) = \lambda_g(y) \\\implies& g\cdot x = g \cdot y \\\implies& g^{-1} \cdot g\cdot x = g^{-1} \cdot g \cdot y \\\implies& x = y \qed \end{aligned} $$

[surjective]  
$X$의 임의의 element를 $x$라고 하자.

$g^{-1}\cdot x \in X$이고 $\lambda_g(g^{-1}x) = g \cdot g^{-1}\cdot x = x$ 임으로 다음이 성립한다.

$$ \exist y \in X \st \lambda_g(y) = x \qed $$

#### 참고
임의의 group action은 Group의 element가 고정되어 있을 떄 $X\rightarrow X$인 bijective function 이다.

### Example1
$G = \Set{\F - \Set{0_\F}, \times}, \enspace X = \F^n$라 하고 group action으로 scalar multiplication을 준다고 하자.

module의 scalar multiplication은 group action의 property를 전부 만족함으로 scalar multiplication은 group action으로 볼 수 있고 $X$는 $G$-set이다.

### Example2
$G = \text{any group}, X = G$라고 하고 group action으로 $g \cdot x = gxg^{-1}$을 준다고 하자.

그러면 $e_G\cdot x = x$이고 $g_1\cdot g_2 \cdot x = g_1g_2xg_2^{-1}g_1^{-1} = g_1g_2\cdot x$임으로 $X$는 $G$-set이다. 

### Example3
$G = \text{any group}, X = G$라고 하고 group action으로 $g \cdot x = gx$을 준다고 하자.

그러면 $e_G\cdot x = x$이고 $g_1\cdot g_2 \cdot x = g_1g_2x = g_1g_2\cdot x$임으로 $X$는 $G$-set이다. 

### Example4
$G = \text{any group}, X = G$라고 하고 group action으로 $g \cdot x = x$을 준다고 하자.

그러면 $e_G\cdot x = x$이고 $g_1\cdot g_2 \cdot x = x = g_1g_2\cdot x$임으로 $X$는 $G$-set이다. 

### Example5
$G = \text{any group}, X = \Set{x}$라고 하고 group action으로 $g \cdot x = x$을 준다고 하자.

그러면 $e_G\cdot x = x$이고 $g_1\cdot g_2 \cdot x = x = g_1g_2\cdot x$임으로 $X$는 $G$-set이다. 

### Example6
$G = \text{any group}, H \le G, X = G/H$라고 하고 group action으로 $g \cdot xH = gxH$을 준다고 하자.

그러면 $e_G\cdot xH = xH$이고 $g_1\cdot g_2 \cdot x = g_1g_2xH = g_1g_2\cdot xH$임으로 $X$는 $G$-set이다. 


## Right Group Action
$G$의 $S$위의 오른쪽 군 작용(right group action of $G$ on $S$) $+$는 다음 조건들을 만족시키는 binary operation이다.

$$ + : S \times R \rightarrow S \st \begin{array}{ll} \text{compatability}& \forall g_1,g_2 \in G, \quad \forall s \in S, \quad s + (g_1 + g_2) = (s + g_1) + g_2 \\ \text{identity}& \forall s \in S, \quad s + 0_R = s \end{array} $$

> Referemce  
> [wiki](https://en.wikipedia.org/wiki/Group_action)