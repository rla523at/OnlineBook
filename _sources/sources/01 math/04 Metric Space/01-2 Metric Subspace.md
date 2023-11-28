# Metric Subspace
## 정의
Metric space $(M,d)$가 있다고 하자.

$S \subseteq M$일 때, $d$의 정의역과 치역이 $S$로 restriction된 함수 $d_s$를 다음과 같이 정의하자.

$$d_S := d |_{S \times S} : S \times S \rightarrow \R \st (s_1,s_2) \mapsto d(s_1,s_2) $$

이 때, $(S,d_s)$는 metric space이며 이를 `metric subspace`라 한다.

> Reference  
> {cite}`apostol` p.61 

### 참고
$S$가 $M$의 metric subspace라는 것을 간단하게 다음과 같이 표기한다.

$$ S \le M $$