# Boundary
## 정의
Metric space $M$과 $S \le M$이 있다고 하자.

`Boundary`는 다음과 같이 정의된 집합 $\partial S$이다.

$$ \partial S := \{ x \in M \enspace | \enspace \forall \mathcal N_x, \quad \mathcal N_x \cap S \neq \empty \enspace \land \enspace \mathcal N_x \cap S^c \neq \empty \} $$

### 참고1
$x$의 모든 neighborhood가 $S$와 $S^c$ 모두에게 교집합이 존재하는 경우를 boundary로 보겠다는 의미이다.

### 참고2
$\partial U$에 대해서 다음과 같은 관계식이 성립한다.

$$ \begin{aligned} \bar U &= U \cup \partial U \\ \mathring U &= U - \partial U \\ \partial U &= \bar U - \mathring U \end{aligned}  $$

> Reference  
> {cite}`hubbard` chap 1.5  