# Totally Ordered Set
## 정의
집합 $A$와 $A$의 total order $\le$가 있다고 하자.

이 떄, 집합 $(A, \le)$를 `전순서집합(totally ordered set)`이라고 한다.

### 명제1
다음을 증명하여라.

$$ \text{ Every finite subset of totally ordered set has greatest and least element} $$

**Proof**

Totally ordered set $X$와 $X$의 임의의 finite subset $U$가 있다고 하자.

$U$는 finite subset임으로 $U \neq \empty$이다.

$U$의 임의의 element를 $x_0$라고 했을 때, set $S_1^{+,-}$를 다음과 같이 정의하자.

$$ \begin{gathered} S_{1}^{+} := \Set{x \in U | x_0 \le x} \\ S_{1}^{-}  := \Set{x \in U | x \le x_0} \end{gathered}  $$

$\forall i \in N$에 대해서  set $S_i^{+,-}$의 임의의 원소를 $x_i$라고 할 때, set $S_{i+1}^{+,-}$를 다음과 같이 정의하자.

$$ \begin{gathered} S_{i+1}^{+} := \Set{x \in S_{i}^{+} | x_i \le x} \\ S_{i+1}^{-}  := \Set{x \in S_{i}^{-} | x \le x_i} \end{gathered}  $$

$U$가 finite set이기 때문에 다음이 성립한다.

$$ \exist N \in \N \st S_N^{+,-} \text{ are singleton set} $$

그러면 정의에 의해 $S_N^{+}$의 singletone element는 $U$의 greatest element이며 $S_N^{-}$의 singletone element는 $U$의 least element이다 $\qed$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/3915890/does-a-bounded-well-ordered-set-have-a-maximum)

#### 참고
finite하지 않으면 위의 명제는 성립하지 않는다.

$A := \Set{-1/n | n \in \N} \cup \Set{0}$는 well-order set이다.

$B := A - \Set{0}$는 $A$의 subset이다. 

하지만 $B$는 greatest element를 갖지 않는다.