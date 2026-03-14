# Equivalence Relation

## 정의
집합 $A$와 관계 $R \subseteq A \times A$가 있다고 하자.

$R$이 다음을 만족할 떄, `동치 관계(equivalence relation)`라고 한다.

$$ \def\arraystretch{1.5}\begin{array}{lll} \text{반사관계(reflexive relation)} & x \in A \Rightarrow (x,x) \in R & x \sim_R x \\ \text{대칭관계(symmetric relation)} & (x,y) \in R \Rightarrow (y,x) \in R & x \sim_R y \Rightarrow y \sim_R x \\ \text{추이관계(transitive relation)} & (x,y) \in R \land (y,z) \in R \Rightarrow (x,z) \in R & x \sim_R y \land y \sim_R z \Rightarrow x \sim_R z \end{array} $$

### 예시1

집합 $A = \Set{1,2,3,4}$로 동치 관계를 만들어보자.

대칭관계와 추이관계는 $A$의 모든 원소가 만족할 필요가 없다. 하지만 반사관계는 모든 원소에 대해 반드시 만족해야 한다. 따라서 최소한 동치관계에는 다음의 원소들이 포함되어야 하며 동치 관계의 `필요 조건(necessary condition)`이라고 볼 수 있다.

$$ R = \Set{\Set{1,1}, \Set{2,2}, \Set{3,3}, \Set{4,4}} $$

만약 $\Set{1,2}$를 $R$에 포함할 경우 대칭관계에 의해 $\Set{2,1}$도 포함되어야 하며, 둘중 하나가 빠질경우 둘다 빠져야 된다.

### 명제1
집합 $A$와 다음과 같이 정의된 관계 $R$이 있다고 하자.

$$R:= \Set{(X \times Y) \in P(A) \times P(A) | \exist f : P(A) \rightarrow P(A), \quad f \text { is bijective}}$$

이 떄, 다음을 증명하여라.

$$R \text{ is an equivalence relation}$$

**Proof**

[반사관계]  
$id_{P(A)}$가 전단사 함수이기 때문에 $R$은 반사관계를 만족시키는 모든 원소를 포함하고 있다. $\qed$

[대칭관계]  
$(X,Y) \in R$이라고 하자.

$R$의 정의에 의해 다음이 성립한다.

$$ \exist\text{bijective function } f \st Y = f(X) $$

$f$가 bijective function임으로 다음이 성립한다.

$$ \exist\text{bijective function } f^{-1} \st X = f^{-1}(Y) $$

따라서, $f^{-1}$가 bijective function임으로 $R$의 정의에 의해 다음이 성립한다.

$$ (Y,X) \in R \qed $$

[추이관계]  
$(X,Y),(Y,Z) \in R$이라고 하자.

$R$의 정의에 의해 다음이 성립한다.

$$ \exist\text{bijective function } f,g \st Y = f(X), Z = (g \circ f)(X) $$

$f,g$가 bijective function임으로 다음이 성립한다.
$$ g \circ f \text{ is a bijective function}  $$

따라서, $R$의 정의에 의해 다음이 성립한다.
$$ (X,Z) \in R \qed $$