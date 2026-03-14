# Closed Under Binary Operation
## 정의
집합 $S$와 $S$의 subset $T$가 있다고 하자.

$S$위의 이항연산 $*_S$이 $T$에서 다음 성질을 만족할 경우, $T$은 $*_S$에 대해 `닫혀있다`(`closed` under $*$)고 표현한다.

$$ \forall a,b \in T, \quad  a*b \in T $$

### 명제
집합 $S$와 $S$위의 이항연산 $*$이 있다고 하자.

다음을 증명하여라.

$$ S \text{ is closed under } * $$

**Proof**

이항연산의 정의에 의해 $*$는 다음과 같은 함수이다.

$$ * : S \times S \rightarrow S $$

따라서, 다음이 성립한다.

$$ \forall a,b \in S, \quad  a*b \in S $$

그럼으로 closed의 정의에 의해 다음이 성립한다.

$$ S \text{ is closed under } * \qed $$