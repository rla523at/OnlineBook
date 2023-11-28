# Product Bases
## 정의
Topological space $X_1, \cdots, X_n$이 있다고 하자.

Catesian product를 이용해 다음과 같은 집합을 정의하자.

$$ \prod_{i=1}^n X_i := X_1 \times\cdots\times X_n $$

다음과 같이 정의된 집합 $\prod_{i=1}^n X_i$의 subset collection $\mathcal{B}$를 `product bases`라고 한다.

$$ \mathcal{B} := \Set{\prod_{i=1}^n U_i | U_i \text{ is an open set of } X_i, i=1,\cdots,n} $$

### 명제1
Topological space $X_1, \cdots, X_n$이 있다고 하자.

이 때, 집합 $\prod_{i=1}^n X_i$의 subset collection $\mathcal{B}$를 다음과 같이 정의하자.

$$ \mathcal{B} := \Set{\prod_{i=1}^n U_i | U_i \text{ is an open set of } X_i, i=1,\cdots,n} $$

이 때, 다음을 증명하여라

$$\mathcal{B} \text{ is a basis for some topology on } \prod_{i=1}^n X_i $$

**Proof**

[$\bigcup\mathcal{B} = \prod_{i=1}^n X_i$]  
$X_i$는 $X_i$의 open set임으로 다음이 성립한다.

$$ X \in \mathcal{B} \implies X \subseteq \bigcup\mathcal{B} $$

$\mathcal{B}$는 $X$의 subset collection이고 subset들의 union은 원래 set의 subset임으로 다음이 성립한다.

$$ \bigcup\mathcal{B} \subseteq X $$

따라서, 다음이 성립한다.

$$ \bigcup\mathcal{B} = X \qed $$

[$B_1, B_2 \in \mathcal{B} \implies \forall x \in B_1 \cap B_2, \quad \exist B \in \mathcal{B} \quad s.t. \quad x \in B \subseteq B_1 \cap B_2 $]  
$\mathcal{B}$의 임의의 두 element $B_1,B_2$를 다음과 같이 표현하자.

$$ B_i = \prod_{j=1}^n U_j^i, i=1,2 $$

그러면 Catesian product의 성질에 의해 다음이 성립한다.

$$ \forall x\in B_1 \cap B_2, \quad x \in \prod_{i=1}^n U^1_i \cap \prod_{i=1}^n U^2_i \iff x \in \prod_{i=1}^n (U^1_i \cap U^2_i) $$

$U^1_i, U^2_i, i=1\cdots,n$이 $X_i$의 open set임으로 open set의 성질에 의해 다음이 성립한다.

$$ U_i^1 \cap U_i^j \text{ is open set of } X_i, i=1,\cdots,n $$

따라서, $\mathcal{B}$의 정의에 의해 다음이 성립한다.

$$ \exist B \in \mathcal{B} \st B = \prod_{i=1}^n (U^1_i \cap U^2_i) $$

위의 결과를 종합하면 다음과 같다.

$$ B_1, B_2 \in \mathcal{B} \implies \forall x \in B_1 \cap B_2, \quad \exist B \in \mathcal{B} \quad s.t. \quad x \in B \subseteq B_1 \cap B_2 \qed $$

[결론]  
$\mathcal{B}$가 위의 두 성질을 만족함으로 basis의 성질에 의해, 다음이 성립한다. 

$$\mathcal{B} \text{ is a basis for some topology on } \prod_{i=1}^n X_i \qed $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1714574/basis-for-a-topology-that-we-will-call-the-product-topology)  
> [wiki-catesian product](https://en.wikipedia.org/wiki/Cartesian_product)

#### 참고
$\mathcal{B}$에 의해 생성된 $\prod_{i=1}^n X_i$의 topology를 `product topology`라고 한다.

### 명제2
Metric space $\R^n$의 Euclidean topology를 $\mathcal{T_E}$ product topology를 $\mathcal{T_P}$라할 때, 다음을 증명하여라.

$$ \mathcal{T_E} = \mathcal{T_P} $$

**Proof**

Basis의 성질에 의해, metric space의 open ball collection은 basis이다.

Euclidean topology와 product topology에 의한 open ball collcection을 각 각 $\mathcal{B_E}, \mathcal{B_P}$라고 하자.

[$\mathcal{T_P} \subseteq \mathcal{T_E}$]  
임의의 $x \in \R^n$과 $x$를 포함하는 임의의 product topology의 open ball $B_p \in \mathcal{B_P}$가 있을 때, 보조명제2.1에 의해 다음이 성립한다.

$$ \exist B_E \in \mathcal{B_E} \st x \in B_E \subseteq B_P $$

그러면, Basis의 성질에 의해 다음이 성립한다.

$$ \mathcal{T_P} \subseteq \mathcal{T_E} \qed $$

[$\mathcal{T_E} \subseteq \mathcal{T_P}$]  
임의의 $x \in \R^n$과 $x$를 포함하는 임의의 open ball $B_E \in \mathcal{B_E}$가 있을 때, 보조명제2.2에 의해 다음이 성립한다.

$$ \exist B_P \in \mathcal{B_P} \st x \in B_P \subseteq B_E $$

그러면, Basis의 성질에 의해 다음이 성립한다.

$$ \mathcal{T_E} \subseteq \mathcal{T_P} \qed $$


> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/2847214/topology-induced-by-eulidean-metric-is-the-same-as-product-topology)  

#### 보조명제2.1
다음을 증명하여라.

$$ \exist B_E \in \mathcal{B_E} \st x \in B_E \subseteq B_P $$

**Proof**

임의의 $x,y \in \R^n$에 대해, $\mathcal{T_P}$의 distance function이 다음과 같이 주어졌다고 하자.

$$ d_P(x,y) = \max_{i}{|x_i-y_i|} $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/755586/product-topology-and-standard-euclidean-topology-over-mathbbrn-are-equival)

$d_P$의 정의에 의해 다음이 성립한다.

$$ \exist m \in [1,n] \st d_P(x,y) = |x_m-y_m| $$

Distance function의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} (d_P(x,y))^2 &= (x_m-y_m)^2 \\&\le \sum_{i=1}^n (x_i-y_i)^2 \\&= (d_E(x,y))^2 \end{aligned} $$

이 때, Distance function의 함수값은 항상 양수임으로 다음이 성립한다.

$$ d_P(x,y) \le d_E(x,y) $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & \forall y \in B_E(x, \epsilon), \quad d_E(x,y) \le \epsilon \\\implies& \forall y \in B_E(x, \epsilon), \quad d_P(x,y) \le \epsilon \\\implies& \forall y \in B_E(x, \epsilon), \quad y \in B_P(x, \epsilon) \\\implies& B_E \subseteq B_P \end{aligned} $$

따라서, 다음이 성립한다.

$$ \exist B_E \in \mathcal{B_E} \st x \in B_E \subseteq B_P \qed $$

#### 보조명제 2.2
다음을 증명하여라.

$$ \exist B_P \in \mathcal{B_P} \st x \in B_P \subseteq B_E $$

**Proof**

임의의 $x,y \in \R^n$에 대해, $\mathcal{T_P}$의 distance function이 다음과 같이 주어졌다고 하자.

$$ d_P(x,y) = \max_{i}{|x_i-y_i|} $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/755586/product-topology-and-standard-euclidean-topology-over-mathbbrn-are-equival)

$d_P$의 정의에 의해 다음이 성립한다.

$$ \exist m \in [1,n] \st d_P(x,y) = |x_m-y_m| $$

Distance function의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} (d_E(x,y))^2 &= \sum_{i=1}^n (x_i-y_i)^2 \\&\le n(x_m-y_m)^2 \\&= n(d_P(x,y))^2 \end{aligned} $$

이 떄, distance function의 함수값은 항상 양수임으로 다음이 성립한다.

$$ d_E(x,y) \le \sqrt{n}d_P(x,y) $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & \forall y \in B_P(x, \frac{\epsilon}{\sqrt{n}}), \quad d_P(x,y) \le \frac{\epsilon}{\sqrt{n}} \\\implies& \forall y \in B_P(x, \frac{\epsilon}{\sqrt{n}}), \quad d_E(x,y) \le \epsilon \\\implies& \forall y \in B_P(x, \frac{\epsilon}{\sqrt{n}}), \quad y \in B_E(x, \epsilon) \\\implies& B_P \subseteq B_E \end{aligned} $$

따라서, 다음이 성립한다.

$$ \exist B_P \in \mathcal{B_P} \st x \in B_P \subseteq B_E \qed $$


#### 예시
$\R$의 open set은 임의의 open interval이 된다.

따라서 $\R^2 = \R \times \R$의 product topology는 각각의 open interval을 두 변으로 하는 open rectangle 형태를 이룬다.

> Reference  
> {cite}`LeeTM`p.60

### 명제3(Characteristic property)
Product topology $\prod_{i=1}^n Y_i$가 있다고 하자.

함수 $\pi_j$가 canoncial projection이라고 하자.

$$ \pi_j : \prod_{i=1}^n Y_i \mapsto Y_j $$

임의의 Topological space $X$가 있을 때, 다음을 증명하여라.

$$ f : X \rightarrow \prod_{i=1}^n X_i \text{ is continuous} \iff \forall j \in [1,n], \quad f_j:= \pi_j \circ f \text{ is continuous } $$

**Proof**
[$\implies$]  
임의의 $j \in [1,n]$에 대해서, $Y_j$의 임의의 open set을 $V_j$라 하자.

Preimage의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \preimg_{f_j}(V_j) &= \Set{x \in X | f_j(x) \in V_j} \\&= \Set{x \in X | f(x) \in \prod_{\substack{i=1 \\ j\neq i}}^{n} Y_i \times V_j} \\&= \preimg_f \left(\prod_{\substack{i=1 \\ j\neq i}}^{n} Y_i \times V_j\right) \end{aligned} $$

$\prod_{i=1}^n Y_i$의 product bases를 $\mathcal{B}$라 할 때, $\mathcal{B}$의 정의에 의해 다음이 성립한다.

$$ \preimg_f \left(\prod_{\substack{i=1 \\ j\neq i}}^{n} Y_i \times V_j\right) \in \mathcal{B} $$

이 떄, $f$가 continuous function임으로 다음이 성립한다.

$$ \begin{aligned} &\preimg_f \left(\prod_{\substack{i=1 \\ j\neq i}}^{n} Y_i \times V_j\right) \text{ is an open set of } X \\\implies& \preimg_{f_j}(V_j) \text{ is an open set of } X \end{aligned} $$

임의의 open set의 preimage가 open set임으로 $f_j$는 continuous function이다.

임의의 $j$에서 $f_j$가 continuous function임으로 다음이 성립한다.

$$ \forall j \in [1,n], \quad f_j:= \pi_j \circ f \text{ is continuous } \qed $$

[$\impliedby$]  
$\prod_{i=1}^n Y_i$의 product bases를 $\mathcal{B}$라고 하자.

임의의 $B \in \mathcal{B}$를 다음과 같이 표기하자.

$$ B = \prod_{i=1}^n V_i $$

그러면 다음이 성립한다.

$$ \begin{aligned} \preimg_f(B) &= \Set{x \in X| f(x) \in \prod_{i=1}^n V_i} \\&= \bigcap_{i=1}^n\Set{x \in X| f_i(x) \in V_i } \\&= \bigcap_{i=1}^n \preimg_{f_i}(V_i) \end{aligned}  $$

전제에 의해 $f_j$가 continuous function임으로 다음이 성립한다.

$$ \forall i \in [1,n], \quad \preimg_{f_i}(V_i) \text{ is open set of } X $$

따라서, open set의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \bigcap_{i=1}^n\preimg_{f_i}(V_i) \text{ is open set of } X \\ \implies& \preimg(B) \text{ is open set of } X \end{aligned}  $$

임의의 $B \in \mathcal{B}$의 preimage가 $X$의 open set임으로, basis의 성질에 의해 다음이 성립한다.

$$ f \text{ is continuous } \qed $$

> Reference  
> {cite}`LeeTM`Theorem3.27

#### 따름명제3.1
Topological space $X_1,\cdots,X_n$이 있다고 하자.

Canonical projection $\pi_j$를 다음과 같이 정의하자.

$$ \pi_j := \prod_{i=1}^n X_i \rightarrow x_j $$

이 떄, 다음을 증명하여라.

$$ \forall i \in [1,n], \quad \pi_i \text{ is continous } $$

**Proof**

Identity map $I$를 다음과 같이 정의하자

$$ I : \prod_{i=1}^n X_i \rightarrow \prod_{i=1}^n X_i $$

$id$는 continuous map이고, $\pi_i \circ id = \pi_i$임으로 명제3에 의해 다음이 성립한다.

$$ \forall i \in [1,n], \quad \pi_i \text{ is continous } \qed $$

#### 따름명제3.2
Topological space $X$가 있다고 하자.

함수 $f:X \rightarrow \R, \enspace g: X \rightarrow \R - \Set{0}$이 continuous 할 때, 다음을 증명하여라.

$$ f +g, f-g, fg, \frac{f}{g} \text{are conitnuous} $$

**Proof**

함수 $h$를 다음과 같이 정의하자.

$$ h:X \rightarrow \R^2 \st x \mapsto (f(x),g(x))   $$

$f,g$가 continuous function임으로 product space의 성질에 의해 다음이 성립한다.

$$ h \text{ is continuos} $$

이 때, $+,-,\times,\div$는 모두 $\R^2 \rightarrow \R$로 가는 continuous function임으로 continuous function의 성질에 의해 다음이 성립한다.

$$ \begin{gathered} + \circ h \\ - \circ h \\ \times \circ h \\ \div \circ h \end{gathered} \text{ are conitnuous} \qed  $$

Continuity of sum/product using characteristic property of product topology

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/807201/continuity-of-sum-product-using-characteristic-property-of-product-topology)