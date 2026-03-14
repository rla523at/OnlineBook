# Product Open Set
## 정의
Product space $\prod_{i=1}^n X_i$가 있다고 하자.

$\prod_{i=1}^n X_i$의 Product topology의 basis를 $\mathcal{B}$라고 할 때, $\mathcal{B}$의 원소를 `product open set`이라고 한다.

### 명제1
Product topology $\prod_{i=1}^n X_i$가 있다고 하자.

함수 $\pi_i$가 canoncial projection이라고 하자.

$$ \pi_i : \prod_{j=1}^n X_j \mapsto X_i $$

임의의 Topological space $Y$가 있을 때, 다음을 증명하여라.

$$ f : Y \rightarrow \prod_{i=1}^n X_i \text{ is continuous} \iff \forall i \in [1,n], \quad \pi_i \circ f \text{ is continuous } $$

**Proof**
$f_i = \pi_i\circ f$ 라고 하자.

[$\implies$]  
$f_i : Y \rightarrow X_i$임으로, 임의의 $i \in [1,n]$에 대해서 다음을 가정하자.

$$ U \text{ is open set of } X_i $$

그러면, product topology의 정의에 의해 다음이 성립한다.

$$ \prod_{\substack{j=1\\j\neq i}}^n X_i \times U \in \mathcal{B} $$

따라서, basis의 정의에 의해 다음이 성립한다.

$$ \prod_{\substack{j=1\\j\neq i}}^n X_i \times U \text{ is an open set of } \prod_{i=1}^n X_i $$

전제에 의해 다음이 성립한다.

$$ \preimg_f\left(\prod_{\substack{j=1\\j\neq i}}^n X_i \times U\right) \text{ is an open set of } Y $$

보조명제1.1에 의해 다음이 성립한다.

$$ \preimg_{f_i}\left(U\right) \text{ is an open set of } Y $$

따라서, continuous function의 정의에 의해 다음이 성립한다.

$$ f_i \text{ is a continuous function } \qed $$

[$\impliedby$]  
$\prod_{i=1}^n X_i$의 basis를 $\mathcal{B}$라고 하자.

임의의 $B \in \mathcal{B}$를 다음과 같이 표기하자.

$$ B = \prod_{i=1}^n U_i^B $$

그러면 다음이 성립한다.

$$ \begin{aligned} y \in \preimg(B) &\iff f(y) \in \prod_{i=1}^n U_i^B \\& \iff \forall i \in [1,n], \quad f_i(y) \in U_i^B \end{aligned}  $$

따라서, 다음이 성립한다.

$$ \preimg(B) = \bigcap_{i=1}^n\preimg_{f_i}(U_i^B) $$

이 때, 전제에 의해서 다음이 성립한다.

$$ \forall i \in [1,n], \quad \preimg_{f_i}(U_i^B) \text{ is open set of } Y $$

따라서, open set의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \bigcap_{i=1}^n\preimg_{f_i}(U_i^B) \text{ is open set of } Y \\ \implies& \preimg(B) \text{ is open set of } Y \end{aligned}  $$

임의의 $B \in \mathcal{B}$의 preimage가 $Y$의 open set임으로, basis의 성질에 의해 다음이 성립한다.

$$ f \text{ is continuous } \qed $$

#### 보조명제1.1
다음을 증명하여라.

$$ \preimg_{\pi_i \circ f}(U) = \preimg_f\left(\prod_{\substack{j=1\\j\neq i}}^n X_i \times U\right) $$

**Proof**

$$ \begin{aligned} x \in \preimg_{\pi_i \circ f}(U) &\iff \pi_i(f(x)) \in U \\&\iff f(x) \in \prod_{\substack{j=1\\j\neq i}}^n X_i \times U \\&\iff x \in \preimg_f\left(\prod_{\substack{j=1\\j\neq i}}^n X_i \times U\right) \qed  \end{aligned} $$

> Reference  
> {cite}`LeeTM`Theorem3.27