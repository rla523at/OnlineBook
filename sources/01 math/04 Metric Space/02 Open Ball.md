# Open ball
## 정의
Metric space $M$이 있다고 하자. 

$x \in M, \enspace r \in \R^+$이 있을 때, `open ball`은 다음과 같이 정의된 $M$의 metric subspace이다.

$$ B_M(x,r) := \{ y \in M \enspace | \enspace d(x,y) < r \} $$

### 명제1
Metric space $M$있다고 하자.

$x,y \in M$이 있을 때, 다음을 증명하여라.

$$ d(x,y) = 2r \implies B_M(x,r) \text{ and } B_M(y,r) \text{ are disjoint.} $$

**Proof**

$B_M(x,r) \cap B_M(y,r) \neq \empty$라고 가정하자.

$z \in B_M(x,r) \cap B_M(y,r)$에 대해 다음이 성립한다.

$$ \begin{aligned} & d(x,z) < r \enspace\land\enspace d(y,z) < r \\ \implies& d(x,z) + d(y,z) < 2r \\ \implies& d(x,y) < 2r \end{aligned}  $$

이는 $d(x,y)=2r$이라는 조건에 모순이다.

따라서, `귀류법(proof by contradiction)`에 의해 $B_M(x,r) \cap B_M(y,r) = \empty$이고 $B_M(x,r), B_M(y,r)$은 disjoint set이다.

> Reference  
> [Proof Wiki](https://proofwiki.org/wiki/Open_Balls_whose_Distance_between_Centers_is_Twice_Radius_are_Disjoint)

#### 따름명제1.1
Metric space $M$있다고 하자.

$x,y \in M$이 있을 때, 다음을 증명하여라.

$$ \exist r \quad s.t. \quad  B_M(x,r) \text{ and } B_M(y,r) \text{ are disjoint.} $$

**Proof**

$r = d(x,y)/2$이라 두면 명제1에 의해 자명하다. $\quad\tiny\blacksquare$

### 참고1
open이기 위해서 반드시 $d(x,y) \neq r$ 이어야 한다.

### 참고2
open ball은 1차원에서 `개구간(open interval)`과 같다.

> Reference  
> {cite}`hubbard` chap 1.5  

### 참고3
$S \le M$이 있을 때, $B_S(x,r)$은 다음과 같이 정의된다.

$$ B_S(x,r) := \{ y \in S \enspace | \enspace d(x,y) < r \} = B_M(x,r) \cap S $$

#### 예시
Metric space $\R$이 있을 때, $B(0,1) = (-1,1)$이다.

$S=[0,1] \le \R$이 있을 때, $B_S(0,1) = [0,1)$이다.

> Refrence  
> {cite}`apostol` p.61  