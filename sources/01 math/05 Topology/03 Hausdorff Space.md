# Hausdorff Space
## 정의
Topological Space $X$가 있다고 하자.

다음을 만족하는 $X$를 `Hausdorff space`라고 한다.

$$ \text{disjoint } x,y \in X \implies \exist \text{ disjoint } N_x,N_y $$

### 명제1
다음을 증명하여라.

$$ \text{Every metric spaces are Hausdorff space} $$

**Proof**

Metric space $M$이 있다고 하자.

Metric space의 neighborhood의 성질에 의해 다음이 성립한다.

$$ \text{disjoint } x,y \in M \implies \exist \text{ disjoint } N_x,N_y $$

따라서, $M$은 Hausdorff space이다. $\qed$

### 명제2
다음을 증명하여라.

$$ \text{Every subspace of a Hausdorff space is a Hausdorff space} $$

**Proof**

$X$가 Hausdroff space라고 하자.

$U$가 $X$의 subspace이고 $x,y \in U$라 하면, $X$가 Hausdroff space이기 때문에 다음이 성립한다.

$$ \exist \text{ disjoint } N_x, N_y \text{ on } X $$

$U$가 $X$의 subspace임으로 다음이 성립한다.

$$ N_x \cap U \text{ and } N_y \cap U \text{ are open set and disjoint in } U $$

그럼으로 다음이 성립한다.

$$ \exist \text{ disjoint } \mathcal N^U_x, \mathcal N^U_y \text{ on } U $$

따라서 Hausdorff space의 정의에 의해 $U$는 Hausdorff space이다.$\qed$

> Reference  
> {cite}`LeeTM` p.32  
> [Stackexchange - math](https://math.stackexchange.com/questions/3442811/topology-hausdorff-space-and-the-subspace-topology)  

### 명제3
Topological space $X$가 있다고 하자.

$x \in X$에 대해 continuous function $f_x : X \rightarrow \R$이 존재해서 $f(x) = 0$일 때, 다음을 증명하여라.

$$ X \text{ is a Hausdorff space} $$

**Proof**

$x,y \in X$라고 하자.

전제에 의해 $f_x$는 bijective임으로 다음이 성립한다.

$$ f_x(y) = r \neq 0 $$

이 때, $\R$은 Hausdorff space임으로 다음이 성립한다.

$$ \exist \text{ disjoint } \mathcal N_0, \mathcal N_r \text{ on } \R $$

$f_x$가 continuous function임으로 다음이 성립한다.

$$ \preimg(\mathcal N_0) \text{ and } \preimg(\mathcal N_r) \text{ are open set on } X $$

또한 $0 \in \mathcal{N_0}$임으로 $x \in \preimg(\mathcal{N_0})$이고, $r \in \mathcal{N_r}$임으로 $y \in \preimg(\mathcal{N_r})$이다.

이 때, 다음이 성립한다.

$$ \begin{aligned} \preimg(\mathcal N_0) \cap \preimg(\mathcal N_r) &=  \preimg(\mathcal N_0 \cap \mathcal N_r) \\&= \preimg(\empty) \\&= \empty \end{aligned} $$

$N_x = \preimg(\mathcal N_0), N_y = \preimg(\mathcal N_r)$라고 하면 다음이 성립한다.

$$ \exist \text{ disjoint } N_x, N_y \text{ on } X $$

따라서, $X$는 Hausdorff space이다.$\qed$

> Reference  
> [Stackexchnage - Math](https://math.stackexchange.com/questions/678138/let-x-be-a-topological-space-suppose-forall-p-in-x-exists-f-in-cx?rq=1)

### 명제4
$X$가 Hausdorff space라고 하자.

이 떄, 다음을 증명하여라.

$$ \text{one point set on } X \text{ is closed} $$

**Proof**

$x \in X$라 하자.

$X$가 Hausdorff space이기 때문에 $\forall y \in X - \Set{x}$에 대해 다음이 성립한다.

$$ \exist \text{ disjoint } N_x,N_y \text{ on } X $$

$\mathcal{N_{x,y}}$가 disjoint neighborhood임으로, $N_y \subseteq X-\Set{x}$이다.

따라서, open set의 성질에 의해 다음이 성립한다.

$$ \forall y \in X-\Set{x}, \quad \exist N_y \st N_y \subseteq X-\Set{x} \iff X-\Set{x} \text{ is an open set} $$

따라서, closed set의 정의에 의해 $\Set{x}$는 $X$의 closed set이다. $\qed$

#### 따름명제4.1
$X$가 Hausdorff space라고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Every finite subset of } X \text{ is closed} $$

**Proof**

$x \in X$라 하자.

명제4에 의해 $\{x\}$는 closed set이다.

closed set의 성질에 의해 다음이 성립한다.

$$ \text{finite union of closed sets are closed.} $$

이 떄, $X$의 finite subset은 단일 원소 집합의 finite union으로 구성되어 있음으로 closed이다. $\qed$

#### 따름명제4.2
$X$가 finite Hausdorff space라고 하자.

이 떄, 다음을 증명하여라.

$$ X \text{ has an discrete topology} $$

**Proof**

$X$의 임의의 subset을 $S$라고 하자.

$X$가 finite임으로 $S$도 finite subset이고, 따라서 다음과 같이 표현할 수 있다.

$$ S := \Set{x_1,\cdots,x_k}, \enspace X-S := \Set{x_{k+1},\cdots, x_n} $$

이 떄, $S$와 $X-S$는 singletone set의 finite union임으로 closed set의 성질에 의해 다음이 성립한다.

$$ S,X-S \text{ are closed sets} $$

$X-S$가 closed set임으로 다음이 성립한다.

$$ S \text{ is an open set} $$

임의의 subset $S$가 clopen임으로 다음이 성립한다.

$$ \text{Every subset of } X \text{ is clopen} $$

따라서, clopen set의 성질에 의해 다음이 성립한다.

$$ X \text{ has an discrete topology} \qed $$



### 명제5
Hausdorff space $X$가 있다고 하자. 

$X$위의 sequence $s$가 있을 때, 다음을 증명하여라.

$$ s \text{ can converge to at most one point in } X $$

**Proof**

$x,y \in X$가 있고 $\lim_{i\rightarrow\infty}s(i) = x$ 이고 $ \lim_{i\rightarrow\infty}s(i) = y$라고 하자. 

이 때, 다음을 가정하자.

$$ x \neq y $$

그러면 Hausdorff space임으로 다음이 성립한다.

$$ \exist \text{disjoint } N_x,N_y $$

이 때 , 수렴에 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \exist N_1 \st N_1 \le n \implies s(n) \in N_x \\ \exist N_2 \st N_2 \le n \implies s(n) \in N_y \end{aligned} $$

$N = \max(N_1,N_2)$라 하면 $N \le n$이면 다음이 성립한다.

$$ s(n) \in N_x \cap N_y $$

하지만 이는 $N_x,N_y$가 disjoint라는 사실에 모순됨으로 proof by contradiction에 의해 다음이 성립한다.

$$ x=y $$

즉, 다음이 성립한다.

$$ s \text{ can converge to at most one point in } X \qed $$

### 명제6
Finite set $X$가 있을 떄, 다음을 증명하여라.

$$ \text{Hausdorff topology on } X \text{ is a discrete topology} $$

**Proof**

$X$가 Hausdorff space라고 하자.

$X$의 subset $U$에 대해 $|X - U| = N$이고 $x_i \in X - U$라 하자.

$X$가 finite set이기 때문에 다음이 성립한다.

$$ U = X - \bigcup_{i=1}^N \{x_i\} $$

$X$가 Hausdorff space임으로 명제4에 의해 $\{x_i\}$는 $X$의 closed set이고 closed set의 성질에 의해 다음이 성립한다.

$$ \bigcup_{i=1}^N \{x_i\} \text{ is closed set on } X $$

따라서, closed set의 성질에 의해 $U$는 $X$의 open set이다.

임의의 subset $U$가 open set임으로 $X$는 discrete topology를 갖는다. $\qed$



> Reference  
> [Stackexchange math](https://math.stackexchange.com/questions/1567152/a-finite-hausdorff-space-is-discrete)
