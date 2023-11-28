# Gram-Schmidts Process
$n$차원 inner product space $V / \mathbb F$가 있다고 하자.

$S = \{ w_1, \cdots, w_k \} \subset V$가 있을 때, `Gram-Schmidts Process`는 $S$로 부터 orthogonal subset을 $S' = \{ v_1, \cdots, v_k \}$을 찾는 방법이며 다음과 같다.
$$ v_i = w_i - \sum_{j=1}^{i-1} \frac{B(w_i,v_j)}{B(v_j, v_j)} v_j $$

### 참고1
$\frac{B(w_i,v_j)}{B(v_j, v_j)} v_j$은 $w_i$를 $v_j$방향으로 projection 시킨 vector를 의미한다.

### 참고2
Gram-Schmidts Process에 의해 $n$차원 inner product space $V / \mathbb F$가 있을 때, $V$는 orthogonal basis를 갖는다.
