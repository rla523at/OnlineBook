# Total Order
## 정의
Poset $A$가 있다고 하자.

$A$위의 partial order $\le$가 다음을 만족할 때, $\le$를 `total order`라고 한다.

$$ \forall x,y \in A, \quad  x \le y \lor y \le x \text{ (strongly connected, formerly called total)} $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Total_order)

### 참고
$\forall x,y,z \in A$에 대해서  total order가 만족해야 하는 성질은 다음과 같다.

$$ \begin{array}{cl} x \le x &\text{ (reflexive)} \\ x \le y \land y \le z \implies x \le z &\text{ (transitive)} \\  x\le y \land y \le x \implies x = y &\text{ (antisymmetric)} \\ x \le y \lor y \le x &\text{ (strongly connected, formerly called total)} \end{array} $$