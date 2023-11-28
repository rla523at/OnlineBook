# Group
## 정의
Monoid $G$가 있다고 하자.

$G$가 다음을 만족할 떄, $G$를 `군(group)`이라고 한다.

$$ \forall x \in G, \quad \exist x^{-1} \in G \quad s.t. \quad x * x^{-1} = x^{-1} * x = e $$ 

### 참고
이 때, $x^{-1}$를 $x$의 `역원(inverse element)`이라고 한다.

즉, group이란 각 원소의 inverse element를 갖는 monoid이다.

### 예시
$(\N,\times)$은 군이 아니다. $\left( \because x\in \N-\{1\}, \quad \frac{1}{x} \notin \N \right)$  

$(\Z,+),(\mathbb{Q},+),(\R,+)$은 군이다.

$(\mathbb{Q}-\{0\},\times),(\R-\{0\}, \times)$은 군이다.

$(\{A \in \R^{n \times n} | \det(A) \neq 0\}, \times)$은 군이다.

$(\{f:A \rightarrow A | f \text{ is bijective} \},\circ)$은 군이다.

### 명제1 
Group $G$가 있을 때, 다음을 증명하여라.

$$ \forall x \in G,\quad !x^{-1} $$

**Proof**  

$x\in G$가 있을 떄, $x$의 inverse element를 $y,z \in G$라 하자.  

Group의 성질에 의해 다음이 성립한다.

$$ \exist e \st y = y*e $$

이 때, inverse element의 정의에 의해 다음이 성립한다.

$$ y * x * y = y * x * z $$

$y$가 inverse element임으로 다음이 성립한다.

$$ \begin{aligned} y * x * y &= y * x * z \\ e*y &= e*z \\ y&=z \end{aligned} $$

임의의 $x \in G$에 두개의 inverse element가 있다면 반드시 같아야 함으로 다음이 성립한다.

$$ \forall x \in G,\quad !x^{-1} \qed $$

### 명제2(Cancellation Law)
Group $G$가 있을 때, 다음을 증명하여라.

$$ \forall x,y,z, \in G, \quad x*z = y*z \implies x=y $$

**Proof**  

$x\in G$가 있다고 하자.

Group의 성질에 의해 다음이 성립한다.

$$ \exist e \st x = x*e $$

이 때, inverse element의 정의에 의해 다음이 성립한다.

$$ x * e = x * z * z^{-1} $$

전제에 의해 다음이 성립한다.

$$ x * z * z^{-1} = y * z * z^{-1} = y $$

따라서, 위를 종합하면 다음이 성립한다.

$$ x = y \qed $$

### 명제3
집합 $S$가 있을 떄, 다음을 증명하여라.

$$ S \text{ is a group} \iff \begin{gathered} \exist * \st S \text{ is closed under }* \\ \forall x \in S, \quad \exist x^{-1} \in S \end{gathered}  $$

**Proof**

[$\implies$]  
$S$는 group이기 때문에, semi-group의 성질 또한 만족한다.

따라서 semi-group의 정의에 의해 다음이 성립한다.

$$ \exist * \st S \text{ is closed under }* $$

또한, group의 정의에 의해 inverse element가 존재해야 함으로 다음이 성립한다.

$$ \forall x \in S, \quad  x^{-1} \in S \qed $$

[$\impliedby$]  
첫번째 전제에 의해 $*$가 $S$위의 binary operation임으로 다음이 성립한다.

$$ S \text{ is a semi group} $$

두번째 전제에 의해 inverse element가 존재함으로, inverse element의 정의에 의해 다음이 성립한다.

$$ \exist e \in S \st e \text{ is an indentity element of } S $$ 

따라서 $H$는 monoid이고, 두번째 전제에 의해 모든 원소에 inverse element가 존재함으로 다음이 성립한다.

$$ S \text{ is a group} \qed $$