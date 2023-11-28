# Isolated Point
## 정의
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

$x \in U$가 다음을 만족할 경우, $x$를 $U$의 `isolated point`라고 한다.

$$ \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st U \cap \mathcal{N_x} = \{x\} $$

### 참고
$x \in X$인 limit point와 다르게 isolated point는 $x \in U$이다.

### 명제1
Topological space $X$가 있다고 하자.

$X$의 subset $U$가 있을떄, 다음을 증명하여라.

$$ x \in U \implies x \text{ is an limit point or an isolated point of } U $$

**Proof**

다음을 가정하자.

$$ x \text{ is not an limit point and not an isolated point of } U $$

$x$가 limit point가 아님으로 다음이 성립힌다.

$$ \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \mathcal{N_x} \cap U = \Set{x} $$

$x \in U$임으로, 위를 만족하는 $\mathcal{N_x}$에 대해 다음이 성립한다.

$$ x \text{ is an isolated point} $$

이는 $x$가 isolated point가 아니라는 가정에 모순이 됨으로 proof by contradiction에 의해 다음이 성립한다.

$$ x \text{ is an limit point or an isolated point of } U \qed $$

