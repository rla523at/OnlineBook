# Neighborhood
## 정의
Metric space $M$이 있다고 하자.

$x \in M$의 `neighborhood` $N_x$는 $x$를 포함하는 $M$의 open set이다.

### 참고1
정의에서 알 수 있듯이, neighborhood는 $x$를 중심으로 하는 어떤 open ball을 포함하는 집합일 뿐이다. 

즉, neighborhood 자체가 반드시 open set일 필요는 없다. 

> Reference  
> {cite}`hubbard` chap 1.5 

### 참고2
$x$의 모든 neighborhood의 collection을 $\mathcal{N_x}$라고 표기한다.

### 참고3
Neighborhood의 더 일반적인 정의는  $x$를 중점으로 하는 $M$의 어떤 open ball $B_M(x,\epsilon)$을 포함하는 $M$의 sub set이다. 

$$ B(x,\epsilon) \subseteq N_x $$

> Reference  
> {cite}`hubbard` chap 1.5 


### 명제1
Metric space $M$이 있다고 하자.

$x \in M$이 있을 떄, 다음을 증명하여라.

$$ \forall \epsilon \in \R^+, \quad \exist N_x \in \mathcal{N_x} \st N_x \subseteq B_M(x,\epsilon) $$

**Proof**

유리수의 조밀성에 의해 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist\epsilon' \in \R^+ \st 0 < \epsilon' < \epsilon $$

따라서, 다음이 성립한다.

$$ B_M(x,\epsilon') \subset B_M(x, \epsilon) $$

이 때, $B_M(x,\epsilon') \in \mathcal{N_x}$임으로 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N_x \in \mathcal{N_x} \st N_x \subseteq B_M(x,\epsilon) \qed $$

### 명제2
Metric space $M$과 $S \subseteq M$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ S \text{ is an open set. } \iff \forall x \in S, \quad \exist N_x \in \mathcal{N_x} \st N_x \subseteq S $$

**Proof**

[$\implies$]  

Open set의 정의에 의해 다음이 성립한다.

$$ \forall x \in S, \quad \exist r \in \R^+ \st B(x,r) \subseteq S $$

명제1에 의해 다음이 성립한다.

$$ \forall x \in S, \quad \exist N_x \in \mathcal{N_x} \st N_x \subseteq S \qed $$

[$\impliedby$]  

Neighborhood의 정의에 의해 다음이 성립한다.

$$ \forall x \in S, \quad \exist r \in \R^+ \st B(x,r) \subseteq S $$

따라서, open set의 정의에 의해 다음이 성립한다.

$$ U \text{ is an open set} \qed $$

> Reference  
> {cite}`hubbard` chap 1.5 
 
### 명제3
Metric space $M$있다고 하자.

$x,y \in M$이 있을 때, 다음을 증명하여라.

$$ \text{disjoint }x,y \implies \exist \text{ disjoint } N_x, N_y $$

**Proof**

$x \neq y$임으로, open ball의 성질에 의해 다음이 성립한다.

$$ \exist r \st B_M(x,r) \text{ and } B_M(y,r) \text{ are disjoint.} $$

따라서, $N_x = B_M(x,r), N_y = B_M(y,r)$라고 둘 수 있음으로, $N_x, N_y$는 disjoint이다. 

따라서, $x,y$는 항상 disjoint neighborhood를 갖는다. $\quad\tiny\blacksquare$

> Reference  
> [Proof Wiki](https://proofwiki.org/wiki/Distinct_Points_in_Metric_Space_have_Disjoint_Neighborhoods)