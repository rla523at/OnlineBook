# Basis
## 정의
Topological space $X$가 있다고 하자.

$\mathcal{T_X}$의 subset $\mathcal{B}$가 다음을 만족할 때, $\mathcal{B}$를 topology $\mathcal{T_X}$의 basis라고 한다.

$$ \forall S \in \mathcal{T_X}, \quad \exist \mathcal{B_S} \subseteq \mathcal{B} \st S = \bigcup \mathcal{B_S}$$

### 참고
$X$의 open set $\empty$는 $\mathcal{B}$의 empty collection의 union이다.

### 명제1(Basis criterion)
Topological sapce $X$와 $\mathcal{T_X}$의 basis $\mathcal{B}$가 있다고 하자.

$X$의 subset $U$가 있을 떄, 다음을 증명하여라.

$$ U \text{ is an open set of } X \iff \forall x \in U, \quad  \exist B_x \in \mathcal{B} \quad s.t. \quad x \in B \subseteq U $$

 **Proof**

[$\implies$]  
전제에 의해 $U$가 open set임으로 basis의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & \exist \mathcal{B_U} \subseteq \mathcal{B} \st U = \bigcup \mathcal{B_U} \\ \implies& \forall x \in U, \quad \exist B_x \in \mathcal{B_U} \st x \in B_x \subseteq U \end{aligned}  $$

$\mathcal{B_U} \subseteq \mathcal{B}$임으로 다음이 성립한다.

$$ \forall x \in U, \quad  \exist B_x \in \mathcal{B} \quad s.t. \quad x \in B_x \subseteq U \qed $$

[$\impliedby$]  
전제에 의해, 다음이 성립한다.

$$ U = \bigcup_{x \in U}B_x $$

이 떄, $B_x$는 $X$의 open set임으로 open set의 성질에 의해 다음이 성립한다.

$$ U \text{ is an open set of } X \qed $$

#### 참고
$X$는 $X$의 open set임으로 다음이 성립한다.

$$ \forall x \in X, \quad \exist B \in \mathcal{B} \quad s.t. \quad x \in B \subseteq X $$

### 명제2
Metric topology $M$이 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Collection of open ball in } M \text{ is a basis for } \mathcal{T_M} $$

**Proof**

Open ball의 성질에 의해 다음이 성립한다.
1. $$ \text{open ball in } M \text{ is an open set of } M $$
2. $$ \text{Every open set of } M \text{ is an union of collection of some open balls } $$

따라서, basis의 정의를 만족함으로 $M$의 collection of open ball은 $M$의 basis이다. $\qed$

### 명제3
Euclidean topology $\R^n$이 있다고 하자.

Open square $S_{\R^n}(x,l)$를 다음과 같이 정의하자.

$$ S_{\R^n}(x,l) := \{y \in \R^n \enspace | \enspace |x_i - y_i| < l/2\}  $$

$S_{\R^n}(x,l)$의 collection $\mathcal{B}$를 다음과 같이 정의하자.

$$ \mathcal{B} := \{ S_{\R^n}(x,l) \enspace | \enspace x \in \R^n \enspace\land\enspace l \in \R^+ \} $$

이 떄, 다음을 증명하여라.

$$ \mathcal{B} \text{ is basis for } \R^n $$

**Proof**

[$S_{\R^n}(x,l)$ is open set]  
$S_{\R^n}(x,l) \in \mathcal{B}$가 있다고 하자.

$\forall y \in S_{\R^n}(x,l)$에 대해 open ball $B_{\R^n}(y,\epsilon)$을 고려해보자.

$\forall z \in B_{\R^n}(y,\epsilon)$에 대해 다음이 성립한다.

$$ \begin{aligned} |x_i - z_i | &= |x_i - y_i + y_i - z_i| \\&< |x_i - y_i| + |y_i - z_i| \\&< |x_i-y_i| + \epsilon  \end{aligned} $$

이 떄, $\epsilon \in \R^+$을 다음과 같이 정의하자.

$$ \epsilon = \frac{s}{2} - |x_i - y_i| $$

그러면 $\forall z \in B_{\R^n}(y,\epsilon)$에 대해 다음이 성립한다.

$$ |x_i - z_i| < \frac{s}{2} $$

따라서 다음이 성립한다.

$$ B_{\R^n}(y,\epsilon) \subseteq S_{\R^n}(x,l) $$

이를 정리하면 다음과 같다.

$$ \forall y \in S_{\R^n}(x,l), \quad \exist \epsilon \quad s.t. \quad B_{\R^n}(y,\epsilon) \subseteq S_{\R^n}(x,l) $$

따라서, Neighborhood의 성질에 의해 $S_{\R^n}(x,l)$는 $\R^n$의 open set이다.$\qed$

[Every open set is union of some collection of $S_{\R^n}(x,l)$]  
보조명제3.1에 의해 open ball은 open square collection의 union이다.

그리고 metric space에서 모든 open set은 어떤 open ball collection의 union이다.

따라서, Euclidean space에서 모든 open set은 어떤 open square collection의 union이다. $\qed$

#### 보조명제3.1
다음을 증명하여라.

$$ \text{Open ball is union of some collection of open square}  $$

**Proof**

$x \in \R^n, \enspace r \in \R^+$이 있을 떄, $B_{\R^n}(x,r)$이 있다고 하자.

$\forall y \in B_{\R^n}(x,r)$에 $S_{\R^n}(y,l)$를 고려해보자.

$\forall z \in S_{\R^n}(y,l)$에 대해 다음이 성립한다.

$$ \begin{aligned} |x - z| &= |x - y + y - z| \\&< |x - y| + |y - z| \\&< |x - y| + \frac{\sqrt{2}}{2}l \end{aligned} $$

이 떄, $l = \sqrt{2}(r - |x - y|)$로 두면 다음이 성립한다.

$$ |x - z| < r $$

$\forall z \in S_{\R^n}(y,l)$에 대해 $|x - z| < r$임으로 다음이 성립한다.

$$ S_{\R^n}(y,l) \le B_{\R^n}(x,r) $$

$\forall y \in B_{\R^n}(x,r)$에서 $S_{\R^n}(y,l_y) \le B_{\R^n}(x,r)$를 만족하는 $l_y$이 존재함으로 다음이 성립한다.

$$ B_{\R^n}(x,r) = \bigcup_{{y \in B_{\R^n}(x,r)}}  S_{\R^n}(y,l_y) \qed $$

### 명제4
Topological space $X,Y$와 함수 $f:X \rightarrow Y$가 있다고 하자.

$\mathcal{T_Y}$의 basis를 $\mathcal{B}$라 할 때, 다음을 증명하여라.

$$ f \text{ is continuous} \iff \forall B \in \mathcal{B}, \quad \preimg(B) \text{ is open set of } X $$

**Proof**

[$\implies$]  
Basis의 정의에 의해 다음이 성립한다.

$$ \forall B \in \mathcal{B}, \quad B \text{ is an open set of } Y $$

Conitnuous function의 정의에 의해 다음이 성립한다.

$$ \forall B \in \mathcal{B}, \quad \preimg(B) \text{ is an open set of } X \qed $$

[$\impliedby$]  
$V$가 $Y$의 open set이라고 하자.

Basis의 정의에 의해 다음이 성립한다.

$$ V = \bigcup_{i=1}^k B_i $$

preimage의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} \preimg(V) &= \preimg(\bigcup_{i=1}^k B_i) \\&= \bigcup_{i=1}^k\preimg(B_i) \end{aligned}  $$

이 때, 전제에 의해 $\preimg(B_i)$는 $X$의 open set임으로 open set의 성질에 의해 다음이 성립한다.

$$ \preimg(V) \text{ is an open set of } X$$

그럼으로, continuous function의 정의에 의해 다음이 성립한다.

$$ f \text{ is an continuous function.} $$


### 명제5(Topology from a basis)
Set $X$와 $X$의 subset collection $\mathcal{B}$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \begin{array}{} & \mathcal{B} \text{ is a basis for some topology on } X & \iff & \begin{aligned} 1.& \bigcup \mathcal{B} = X \\ 2. & B_1, B_2 \in \mathcal{B}, \enspace \forall x \in B_1 \cap B_2, \quad \exist B \in \mathcal{B} \quad s.t. \quad x \in B \subseteq B_1 \cap B_2 \end{aligned} \end{array} $$

**Proof**

[$\implies$]  
-[1]  
$X$는 $X$의 가장 큰 open set이기 때문에 basis의 정의에 의해 자명하게 성립한다. $\qed$

$$ \bigcup_{B \in \mathcal{B}} B = X \qed $$

-[2]  
$B_{1,2} \in \mathcal{B}$가 있다고 하자.

$B_{1,2}$는 $X$의 open set임으로 다음이 성립한다.

$$ B_1 \cap B_2 \text{ is an open set of } X $$

따라서 명제1에 의해 다음이 성립한다.

$$ \forall x \in B_1 \cap B_2, \quad \exist B \in \mathcal{B} \quad s.t. \quad x \in B \subseteq B_1 \cap B_2 \qed $$


[$\impliedby$]  
$\mathcal{T_X}$를 다음과 같이 정의하자.

$$ \mathcal{T_X} := \Set{ \bigcup \mathcal{B'} | \mathcal{B'} \subseteq \mathcal{B}} $$

보조명제5.1에 의해 $\mathcal{T_X}$는 $X$의 topology이다.

이 떄, $\mathcal{T_X}$의 정의에 의해 $\mathcal{B}$의 원소는 $\mathcal{T_X}$의 원소이며, $\mathcal{T_X}$의 원소는 $\mathcal{B}$의 어떤 원소들의 union이다.

따라서, basis의 정의에 의해 $\mathcal{B}$는 $\mathcal{T_X}$의 basis이다. $\qed$

#### 보조명제5.1
다음을 증명하여라.

$$ \mathcal{T_X} \text{ is topology of } X  $$

**Proof**

[$\empty \in \mathcal{T_X}$]  
$\empty \subseteq \mathcal{B}$임으로, $\mathcal{T_X}$의 정의에 의해 다음이 성립한다.

$$ \empty \in \mathcal{T_X} \qed $$

[$X \in \mathcal{T_X}$]    
$\mathcal{B}\subseteq\mathcal{B}$임으로 전제1에 의해 다음이 성립한다.

$$ X = \bigcup \mathcal{B} $$

따라서, $\mathcal{T_X}$의 정의에 의해 다음이 성립한다.

$$ X \in \mathcal{T_X} \qed $$

[finite intersection]    
$\mathcal{T_X}$의 finite subset을 $\mathcal{T_X'}$이라고 하자.

$$ \mathcal{T_X'} := \Set{S_i \in \mathcal{T_X} | i=1,\cdots,n} $$

$\mathcal{T_X'}$의 임의의 element를 $S$라 하면 $\mathcal{T_X'}$의 정의에 의해 다음이 성립한다.

$$ \forall x \in S, \quad \exist B_S \in \mathcal{B} \st x \in B_S \subseteq S $$

그럼으로 $\bigcap\mathcal{T_X'}$의 임의의 element를 $x$라고 하면 다음이 성립한다.

$$ \begin{aligned} & x \in S_i, \quad i=1,\cdots,n \\\implies& \exist B_i \in \mathcal{B} \st x \in B_i \subseteq S_i, \quad i=1,\cdots,n \\\implies& x \in \bigcap_{i=1}^n B_i \subseteq \bigcap\mathcal{T_X'} \end{aligned} $$

따라서, 2번전제에 의해 다음이 성립한다.

$$ \exist B_x \in \mathcal{B} \quad s.t. \quad x \in B_x \subseteq \bigcap_{i=1}^n B_i \subseteq \bigcap\mathcal{T_X'} $$

$B_x$의 collection을 $\mathcal{B_x}$라 하면 다음이 성립한다.

$$ \begin{gathered} \mathcal{B_x} \subseteq \mathcal{B} \\ \bigcap\mathcal{T_X'} = \bigcup \mathcal{B_x} \end{gathered} $$

따라서, $\mathcal{T_X}$의 정의에 의해 다음이 성립한다.

$$ \bigcap\mathcal{T_X'} \in \mathcal{T_X} \qed $$

-[union]  
$\mathcal{T_X}$의 subset을 $\mathcal{T_X'}$이라고 하자.

$\mathcal{T_X'}$의 정의에 의해 다음이 성립한다.

$$ \forall S \in \mathcal{T_X'}, \quad S \in \mathcal{T_X} $$

따라서 $\mathcal{T_X'}$의 임의의 element를 $S$라고 하면 다음이 성립한다.

$$ \exist \mathcal{B_S} \subseteq \mathcal{B} \st S = \bigcup \mathcal{B_S} $$

위를 만족하는 모든 $\mathcal{B_S}$의 collection을 $\mathcal{BS}$라고 하면, subset들의 union은 subset임으로 다음이 성립한다.

$$ \exist \mathcal{B'} \subseteq \mathcal{B} \st \mathcal{B'} = \bigcup\mathcal{BS} $$

따라서, $\mathcal{T_X}$의 정의에 의해 다음이 성립한다.

$$ \bigcup\mathcal{B'} = \bigcup\mathcal{T_X'} \in \mathcal{T_X} \qed $$

> Reference  
> {cite}`LeeTM` p.35

#### 따름명제5.2
$\R$의 subset collection $\mathcal{B}$를 다음과 같이 정의하자.

$$ \mathcal{B} := \Set{[a,b) | a,b \in \R \land a \le b} $$

이 떄, 다음을 증명하여라.

$$ \mathcal{B} \text{ is an basis of some topology on } \R $$

**Proof**

$\R$의 Archimedean property에 의해 다음이 성립한다.

$$ \forall x \in \R, \quad \exist n \in \N \st -n < x < n $$

실수의 조밀성에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall x \in \R, \quad \exist a,b \in \R \st -a \le x < b \\ \implies& \forall x \in \R , \quad \exist B_x \in \mathcal{B} \st x \in B_x \\\implies& \R = \bigcup \mathcal{B} \end{aligned}  $$

그리고 $[a_1,b_1),[a_2,b_2) \in \mathcal{B}$가 있을 때, $a_2 \le b_1$이라고 하면 다음이 성립한다.

$$ \forall x \in [a_1,b_1) \cap [a_2,b_2), \quad x \in [a_2,b_1) \in \mathcal{B} $$

따라서, 명제5에 의해 다음이 성립한다.

$$ \mathcal{B} \text{ is an basis of some topology on } \R \qed $$

#### 참고
$$ X = \bigcup \mathcal{B} \iff \forall x \in X, \quad \exist B \in \mathcal{B} \st x \in B $$

### 명제6
Topological space $X$와 $\mathcal{T_X}$의 basis $\mathcal{B}$가 있다고 하자.

$U$가 $X$의 open set일 때, $\mathcal{B_U}$를 다음과 같이 정의하자.

$$ \mathcal{B_U} := \Set{B \in \mathcal{B} | B \subseteq U} $$

$U$를 $X$의 subspace라 할 때, 다음을 증명하여라.

$$ \mathcal{B_U} \text{ is basis of } \mathcal{T_U} $$

**Proof**

$U$의 임의의 open set을 $U'$이라하자.

subspace의 성질에 의해 다음이 성립한다.

$$ U' \text{ is an open set of } X $$

따라서, basis의 성질에 의해 다음이 성립한다.

$$ \exist \mathcal{B'} \subseteq \mathcal{B} \st U' = \bigcup \mathcal{B'} $$

이 때, $\mathcal{B_U}$의 subset $\mathcal{B_{U'}}$을 다음과 같이 정의하자.

$$ \mathcal{B_{U'}} := \Set{B \in \mathcal{B} | B \subseteq U'} $$

$\mathcal{B_{U'}}$의 정의상 다음이 성립한다.

$$ \begin{aligned} & \mathcal{B'} \subseteq \mathcal{B_{U'}} \\\implies& \mathcal{B'} \subseteq \mathcal{B_U}  \end{aligned} $$

따라서, 위의 결과를 정리하면 다음과 같다.

$$ \forall U' \in \mathcal{T_U}, \quad \exist\mathcal{B'} \subseteq \mathcal{B_{U}} \st U' = \bigcup\mathcal{B'}  $$

그럼으로, basis의 정의에 의해 다음이 성립한다.

$$ \mathcal{B_U} \text{ is basis of } \mathcal{T_U} \qed $$

### 명제7
집합 $X$와 $X$의 topology $\mathcal{T_1},\mathcal{T_2}$가 있다고 하자.

$\mathcal{T_i}$의 basis를 $\mathcal{B_i}$라 할 때, 다음을 증명하여라.

$$ \mathcal{T_1} \subseteq \mathcal{T_2} \iff \text{for each } x \in X \text{ and each } x \in B_1 \in \mathcal{B_1}, \quad \exist B_2 \in \mathcal{B_2} \st x \in B_2 \subseteq B_1 $$

**Proof**

[$\implies$]  
$x_0 \in X$이고 $x_0 \in B_1 \in \mathcal{B_1}$이라 하자.

Basis의 정의에 의해 $\mathcal{B_1} \subseteq \mathcal{T_1}$이고, 전제에 의해 $\mathcal{T_1} \subseteq \mathcal{T_2}$임으로 다음이 성립한다.

$$ \mathcal{B_1} \subseteq \mathcal{T_2} $$

따라서 다음이 성립한다.

$$ B_1 \in \mathcal{T_2} $$

그럼으로, basis criterion에 의해 다음이 성립한다.

$$ \forall x \in B_1, \quad \exist B_2 \in \mathcal{B_2} \quad s.t. \quad x \in B_2 \subseteq B_1 $$

$x_0 \in B_1$임으로 다음이 성립한다.

$$ \exist B_2 \in \mathcal{B_2} \quad s.t. \quad x_0 \in B_2 \subseteq B_1 \qed $$

[$\impliedby$]
$U \in \mathcal{T_1}$이라 하자.

Basis criterion에 의해 다음이 성립한다.

$$ \forall x \in U, \quad \exist B_1 \in \mathcal{B_1} \st  x \in B_1 \subseteq U $$

따라서, 전제에 의해 다음이 성립한다.

$$ \forall x \in U, \quad \exist B_2 \in \mathcal{B_2} \quad s.t. \quad x \in B_2 \subseteq B_1 $$

위의 두 결과를 종합하면 다음이 성립한다.

$$ \forall x \in U, \quad \exist B_2 \in \mathcal{B_2} \quad s.t. \quad x \in B_2 \subseteq U $$

그럼으로, Basis criterion에 의해서 다음이 성립한다.

$$ U \in \mathcal{T_2} \qed $$

> Reference  
> {cite}`munkres`Lemma 13.3