# Open set
## 정의
Metric space $M$과 submetric space $S$가 있다고 하자.

다음을 만족하면 $S$를 $M$에서의 `open set`이라고 한다.

$$ \forall x \in S, \quad \exist r \in \R^+ \quad s.t. \quad B_M(x,r) \subseteq S $$

> Reference  
> {cite}`hubbard` chap 1.5  

### 명제1
Metric space $M$이 있을 때, 다음을 증명하여라.

$$ \text {Any union of open sets on } M \text{ is open set on } M. $$

**Proof**

$M$의 모든 open set의 집합을 $\mathcal{S}$라 하자.

$\mathcal{S}$의 임의의 subset을 $\mathcal{S'}$라 하면, 다음을 만족한다.

$$ \forall x \in \bigcup\mathcal{S'}, \quad \exist S \in \mathcal{S'} \quad s.t. \quad x \in S $$
 
이 때, $S$는 $M$위의 open set임으로, 다음이 성립한다.

$$\exist r \in \R^+ \quad s.t. \quad B_M(x,r) \subseteq S \subseteq \bigcup\mathcal{S'} $$

$X$의 정의에 의해 $S \subseteq X$임으로, 다음이 성립한다.

$$ \forall x \in \bigcup\mathcal{S'}, \quad \exist r \in \R^+ \quad s.t. \quad B_M(x,r) \subseteq \bigcup\mathcal{S'} $$

따라서, open set의 정의에 의해 다음이 성립한다.

$$ \text {Any union of open sets on } M \text{ is open set on } M \qed $$


> Refrence  
> {cite}`apostol` chapter 3.7  
> {cite}`hubbard` prob 1.5.3

### 명제2
Metric space $M$이 있을 때, 다음을 증명하여라.

$$ \text {Any finite intersection of open sets is open.} $$

**Proof**

$M$의 모든 open set의 집합을 $\mathcal{S}$라 하자.

$\mathcal{S}$의 finite subset을 $\mathcal{S'}$이라 할 때, 집합 $X$를 다음과 같이 정의하자.

$$ X := \bigcap_{S \in \mathcal{S'}} S $$

$\forall x \in X$에 대해, 다음이 성립한다.

$$ x \in S_i, \enspace i=1,\cdots,n $$

이 때, $S_i$는 open set임으로 다음이 성립한다.

$$\exist r_i \quad s.t. \quad B_M(x, r_i) \subseteq S_i, \enspace i=1,\cdots,n$$

$\min(\{ r_i \}) = r$이라 하면 다음이 성립한다.

$$ B_M(x,r) \subseteq S_i, \enspace i=1,\cdots,n $$

$X$의 정의에 의해 다음이 성립한다.

$$ B_M(x,r) \subseteq X$$

따라서, open set의 정의에 의해 다음이 성립한다.

$$ \text {Any finite intersection of open sets is open} \qed $$

> Refrence  
> {cite}`apostol` chatper 3.8  
> {cite}`hubbard` prob 1.5.3
 
### 명제3
Metric space $M$이 있을 때, 다음을 증명하여라.

$$ \text {An infinite intersection of open sets is not necessarily open.} $$

**Proof**

Metric space $\R$위의 open set들의 집합을 $S$를 다음과 같이 정의하자.

$$ S := \{ {(-1/n, 1/n)} \enspace | \enspace i=1,2,3, \cdots \} $$

모든 $s \in S$의 intersection은 $\{ 0 \}$이 되고 $\{ 0 \}$은 open set이 아니다. 

즉, infinite intersection을 고려하는 경우 open set이 아닌 경우가 있다. $\qed$

> Refrence  
> {cite}`apostol` chapter 3.8  
> {cite}`hubbard` prob 1.5.3

### 명제4
Metric space $M$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ M \text{ is an open set on } M $$

**Proof**

$x \in M$이 있다고 하자.

임의의 $r \in \R^+$에 대해, $B_M(x,r)$의 정의에 의해 다음이 성립한다.

$$ B_M(x,r) \subseteq M $$

이로 인해 $\forall x \in M$에 대해, 다음이 성립한다.

$$ \exist r \in R^+ \quad s.t. \quad B_M(x,r) \subseteq M $$

따라서 $M$는 $M$에서의 open set이 된다.$\qed$


#### 따름명제4.1
Metric space $M$과 $S \subseteq M$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ S \text{ is an open set on } S $$

**Proof**

$\forall x \in S$에 대해, $B_S(x,r) = B_M(x,r) \cap S$임으로 다음이 성립한다.

$$ B_S(x,r) \subseteq S $$

따라서, $S$는 $S$에서의 open set이 된다.$\qed$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1169561/why-is-a-metric-space-an-open-subset-of-itself)

### 명제5
Metric space $M$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ \empty \text{ is an open set on } M $$

**Proof**

open set의 정의상, $x \in \empty$에 대해 open ball을 고려해야 되는데 공집합에서 원소를 뽑을 수는 없다.

따라서, 가정이 항상 거짓인 `공허하게 참인 명제(vacuously true statement)`가 된다.$\qed$

### 명제6
Metric space $M$이 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{open ball in } M \text{ is an open set of } M$$

**Proof**  
$x \in M, r \in \R^+$이 있다고 하자.

임의의 $y \in B_M(x,r)$에 대해 $B_M(y,\epsilon)$가 있다고 하면 다음이 성립한다.

$$ \forall z \in B_M(y, \epsilon), \quad d(x,z) < d(x,y) + d(y,z) < d(x,y) + \epsilon $$

이 떄, $y\in B_M(x,r)$임으로 $r - d(x,y) \in \R^+$이다.

따라서 $\epsilon = r - d(x,y)$로 두면 다음이 성립한다.

$$ \begin{aligned} & d(x,z) < r \\ \iff& B_M(y,\epsilon) \subseteq B_M(x,r) \end{aligned}  $$

그럼으로, 다음이 성립한다.

$$ \forall y \in B_M(x,r) \quad \exist\epsilon \in \R^+ \quad s.t. \quad B_M(y,r) \subseteq B_M(x,r) $$

따라서 open set의 정의에 의해 다음이 성립한다.

$$ \text{open ball in } M \text{ is an open set of } M \qed $$

#### 참고
임의의 open set은 open ball이 아닐 수 있다.

예를 들어 두개의 disjoint open ball의 union은 open set이지만 open ball은 아니다.


### 명제7
Metric space $M$이 있다고 하자.

$M$의 subset $U$가 있을 때, 다음을 증명하여라.

$$ U \text{ is an open set} \iff U \text{ is an union of some collection of open balls} $$

**Proof**

[$\implies$]  
$U$의 정의에 의해 다음이 성립한다.

$$ \forall x \in U, \quad \exist r_x \in \R^+ \quad s.t. \quad B_M(x,r_x) \subseteq S $$

따라서, 다음이 성립한다.

$$ U = \bigcup_{x \in U} B_M(x,r_x) \qed $$

[$\impliedby$]  
명제6에 의해 open ball은 open set이다.

open set의 성질에 의해 open set의 union은 open set이 된다. $\qed$

### 참고
명제1,2,4,5을 만족하는 set의 collection의 원소를 open set으로 정의할 경우 거리가 정의되어 있지 않은 공간에서도 open set과 closed set을 정의할 수 있다.

즉, 더 일반적인 형태의 open set과 closed set을 정의할 수 있게 된다.

> Reference  
> {cite}`hubbard` p.102 


### 예시1
부분집합 $U \subset \R^2$가 아래 그림과 같이 회색으로 표현된 영역에서 boundary를 뺀 부분이라고 하자.

```{figure} _image/0301.png
```

그러면 그림과 같이, 흰색으로 표현된 $\mathbf x \in U$를 어떻게 잡아도 검은색 실선으로 표현된 $B_r(\mathbf X)$이 존재하게 된다.

즉, open set이 되기 위해서는 기하학적으로 boundary를 하나도 포함하지 않아야 된다는 것을 알 수 있다.

> Reference  
> {cite}`hubbard` chap 1.5  

### 예시2
$a,b,c,d \in \R$라 하자.

* $a < b$라 할 때, $(a,b) := \{ x \in \R \enspace | \enspace a < x < b \}$는 open set이다.
* $a < b, \enspace c < d$라 할때, $(a,b) \times (c,d) := \{ (x,y) \in \R^2 \enspace | \enspace a < x < b, \enspace c < y < d \}$는 open set이다.
* $(a,\infty)$는 open set이다.
* $(-\infty, a)$는 open set이다.

> Reference  
> {cite}`hubbard` chap 1.5  