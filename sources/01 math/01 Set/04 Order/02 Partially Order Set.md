# Partially Ordered Set
## 정의
집합 $A$와 $A$의 partial order $\le$가 있다고 하자.

이 떄, 집합 $(A, \le)$를 `부분순서집합(partially ordered set, poset)`이라고 한다.

### 예시
집합 $A$와 $P(A)$가 있을 때, $(P(A), \subseteq)$는 부분순서집합이다.

### 명제
공집합이 아닌 임의의 집합의 collection $C$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$(C,\subseteq) \text{ is a poset }$$ 

**Proof**

[reflexive relation]  
모든 집합은 자기 자신을 부분집합으로 갖음으로 다음이 성립한다.

$$ \forall A \in C, \quad A \subseteq A \qed $$

[anti-symmetric relation]  
$A_{1,2} \in C$가 다음을 만족한다고 하자.

$$ A_1 \subseteq A_2 \land A_2 \subseteq A_1 $$

그러면 set inclusion의 성질에 의해 다음이 성립한다.

$$ A_1=A_2 \qed $$

[transitive relation]  
$A_{1,2,3} \in C$가 다음을 만족한다고 하자.

$$ A_1 \subseteq A_2 \land A_2 \subseteq A_3 $$

그러면 set inclusion의 성질에 의해 다음이 성립한다.

$$ A_1 \subseteq A_3 \qed $$

[결론]  
임의의 집합에 대해서 $\subseteq$는 partial order임으로, 다음이 성립한다.

$$ (C,\subseteq) \text{ is a poset } \qed $$ 
