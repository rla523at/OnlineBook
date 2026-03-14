# Monoid
## 정의
Semi-group $M$이 있다고 하자.

$M$이 다음을 만족하는 경우, $M$을 `모노이드(monoid)`라고 한다.

$$ \exist e \in M \st \forall a \in M, \quad a*e = e*a = a $$  

### 참고
이 때, $e$를 `항등원(identity)`이라고 한다. 

즉, monoid란 identity 원소을 갖는 semi-group이다. 

### 예시
$(\N,+)$은 모노이드가 아니다. $(\because 0 \notin \N)$.  

$(\N,\times),(\Z,\times)$는 모노이드다.

### 명제1
다음을 증명하여라.

$$ \text{Identity element in monoid is unique}  $$

**Proof**

Monoid $M$과 $M$의 두 항등원 $e_1, e_2 \in M$가 있다고 하자.

$e_2$가 항등원임으로 다음이 성립한다.

$$ e_1 = e_1 * e_2 = e_2 * e_1 $$

이 때, $e_1$도 항등원임으로 다음이 성립한다.

$$ e_2*e_1 = e_2 $$

따라서, 다음이 성립한다.

$$ e_1 = e_2 $$

두 항등원이 있다고 했을 떄, 두 항등원은 반드시 같을 수 밖에 없음으로 다음이 성립한다.

$$ \text{Identity element in monoid is unique} \qed $$
