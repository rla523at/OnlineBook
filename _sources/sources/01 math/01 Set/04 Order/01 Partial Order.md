# Partial Order
## 정의
집합 $X$가 있다고 하자.

관계 $R\in X \times X(\sim_R \equiv \le)$이 다음을 만족할 때, $\le$를 $X$의 `부분순서(partial order)`라고 한다.

$$ \begin{array}{cl} \forall x \in X, \quad x \le x &\text{ (reflexive)} \\ \forall x,y,z \in X, \quad x \le y \land y \le z \implies x \le z &\text{ (transitive)} \\ \forall x,y \in X, \quad  x\le y \land y \le x \implies x = y &\text{ (antisymmetric)} \end{array} $$

### 참고1
reflexive 성질은 $X$의 모든 원소에 대해서 만족해야 한다.

하지만 transitive와 antisymmetric 성질은 비교 가능한 원소들간에만 만족하면 된다.

### 참고2
관계가 대칭적이란 말은 $a \sim_R b$이면 $b \sim_R a$이란 얘기이다.

우리가 주고 싶은 관계는 순서를 주고 싶은것이기 때문에 대칭적이면 "$a$가 $b$보다 작으면 $b$도 $a$보다 작다"가 된다.

$a,b$가 같다면 위의 말은 순서가 갖는 의미에 부합하지만 $a,b$가 다르다면 순서가 갖는 의미에 모순된다.

따라서, `반대칭(antisymmetric)` 조건을 부여하여 대칭관계를 갖는 $a,b$가 다를 수 없게 만든다.

### 참고3
antisymmetric 조건의 대우 명제는 $a\neq b$이면 $a \not\sim_R b \lor b \not\sim_R a$이다.

즉, 서로 다르다면 관계가 둘다 있을 수는 없다는 의미이다.