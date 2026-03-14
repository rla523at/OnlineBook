# Induction
## 정의
$\N$의 subset $A$가 있다고 하자.

이 때, 다음이 성립한다.

$$ \begin{gathered} 1 \in A \\ \forall n \in \N, \quad  n \in A \implies n+1 \in A \end{gathered} \iff A = \N $$ 

### 명제
다음을 증명하여라.

$$ \N \text{ is well-ordered set} \implies \N \text{ satisfy induction} $$

**Proof**

$\N$의 subset $A$가 다음을 만족한다고 하자.

$$ \begin{gathered} 1 \in A \\ \forall n \in \N, \quad  n \in A \implies n+1 \in A \end{gathered} $$

이 떄, 다음을 가정하자.

$$ A \neq \N $$

집합 $S$를 다음과 같이 정의하자.

$$ S:= \N - A $$

$A \neq \N$임으로 다음이 성립한다.

$$ S \text{ is an nonempty subset of } \N  $$

전제에 의해 well-ordered set임으로 다음이 성립한다.

$$ \exist s_m \in S \st  \text{least element of } S $$

$S$의 least element를 $s_m$이라고 하면 다음이 성립한다.

$$ s_m \in S, \enspace s_m \in \N $$

따라서 $s_m$은 유한한 값을 갖는 $\N$의 원소임으로 $A$의 성질에 의해 다음이 성립한다.

$$ 1 \in A \implies 2 \in A \implies \cdots \implies s_m \in A $$

그럼으로 다음이 성립한다.

$$ \exist s_m \in S \st s_m \in \N \cap A $$

이는 $(\N - A) \cap A = \empty$라는 사실에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ A = \N $$