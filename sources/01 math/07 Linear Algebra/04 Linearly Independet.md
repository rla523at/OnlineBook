# Linearly Independet
## Finite Set
Vector space $V/\F$와 $V$의 finite subset $S = \Set{v_1,\cdots,v_n}$이 있다고 하자.

$\F$의 elements $a^1,\cdots,a^n$에 대해서 다음을 만족할 때, 각각의 $v_i$들을 `선형 독립(linearly independent)`이라고 하고 $S$를 `선형 독립 집합(linearly independent set)`라고 한다.

$$ \sum_{k=1}^{n}a_k v_k = 0_V \implies \forall a_k=0_\F $$ 


## Infinite Set
Vector space $V/\F$와 $V$의 subset $S$가 있다고 하자.

$S$의 임의의 $n$개의 element $v_1, \cdots, v_n$이 항상 linearly independent일 때, $S$를 linearly independent set이라고 한다.

> Reference  
> {cite}`friedberg` Chapter1.5

### 참고1
$S$가 finite set일 경우에는 $S$의 모든 element가 linearly independent인지 확인하면 된다.

하지만 $S$가 infinite set일 경우에는 모든 element에 대한 linearly independent를 확인할 수 없음으로, 임의의 선택에 대해서 linear independent를 보장하는 방식으로 정의된다.

여기서 임의의 선택이란 $n$개에도 자유도가 있고, 그 선택에도 자유도가 있다는 말을 의미한다.

다시 말해 2개를 뽑던지 100개를 뽑던 지 위에가 만족해야 되고 또한 2개를 어떻게 뽑더라도 100개를 어떻게 뽑더라도 항상 위를 만족해야 한다.

따라서 set에서 선택가능한 모든 vector들의 조합이 linearly independent해야 한다.

### 참고2
linearly independent set이 아닌 집합을 `선형 종속 집합(linear dependent set)`이라고 한다.

### 참고3
선형 종속 집합 $S \subseteq V$가 있다면 다음을 만족한다.

$$\exist a_i \in \F,v_i \in S \quad s.t. \quad \sum_{k=1}^{n}a_k v_k = 0_V \land \exist a_i  \neq 0_\F $$

이 때, 일반성을 잃지 않고 $a_1 \neq 0_\F$이라고 가정하면 다음 관계식이 성립한다.
$$  v_1 = - \frac{1}{a_1}\sum_{k=2}^{n} a_k v_k $$

즉, $v_1$은 $S$에 속하는 다른 원소들의 선형결합으로 표현이 가능하다.

### 참고4
$v \in V- \{0_V\}$에 대해 $\beta=\{v\}$면 $\beta$는 선형 독립 집합이다.

### 명제1
Vector space $V/\F$와 $V$의 linearly independent set $S$가 있다고 하자.

$v \in V - \span(S)$일 때, 다음을 증명하여라.

$$ S \cup \Set{v} \text{ is an linearly independent set} $$

**Proof**

$S$의 임의의 $n$개의 element를 $s_1,\cdots,s_n$이라 하자.

$a_1v + a_2s_1 + \cdots + a_{n+1}s_{n} = 0_V$일 때, 다음을 가정하자.

$$ a_1 \neq 0_\F $$

그러면 다음이 성립한다.

$$ v = -\left(\frac{a_2}{a_1}s_1 + \cdots + \frac{a_{n+1}}{a_{1}}s_n \right) $$

하지만 이는 $v \in V-\span(S)$라는 전제에 모순됨으로, proof by contradiction에 의해 다음이 성립한다

$$ a_1 = 0_\F $$

그러면 $S$가 linearly independent set임으로 다음이 성립한다.

$$ a_2 = \cdots = a_{n+1} = 0_\F $$

따라서, linearly independet set의 정의에 따라 다음이 성립한다.

$$ S \cup \Set{v} \text{ is an linearly independent set} \qed $$

### 명제2(Steinitz exchange lemma)
Vector space $V/\F$와 $V$의 finite linearly independent set $X$ 그리고 $V$의 finite generating set $Y$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \begin{gathered} |X| \le |Y| \\ \exist Y' \subseteq Y \st |Y'| = |Y| - |X| \land \span(X \cup Y') = V \end{gathered} $$

**Proof**

$X = \Set{x_1,\cdots,x_n}, Y = \Set{y_1,\cdots,y_m}$이라고 하자.

이 떄, $X_i,Y_i, \enspace i=0,\cdots,n$를 다음과 같이 정의하자

$$ X_i := \Set{x_1, \cdots, x_i}, \enspace Y_i := \Set{y_{i+1}, \cdots, y_m} $$

보조명제2.1에 의해 다음이 성립한다.

$$  \begin{gathered} n \le m \\ \span(X_n \cup Y_n) = V \end{gathered} $$
 
따라서 $|X| \le |Y|$이고  $Y' = Y_n$이라고 하면 명제를 모두 만족시킴을 알 수 있다. $\qed$

#### 보조명제2.1
다음을 증명하여라.

$$ \begin{gathered} n \le m \\ \span(X_n \cup Y_n) = V \end{gathered} $$

**Proof**

mathematrical induction을 통해 $n \le m$이고 $\span(X_n \cup Y_n) = V$임을 보이자.

[$i=0$]  
$0 \le m$이고  $X_0 = \empty, Y_0 = Y$임으로 $\span(X_0\cup Y_0) = V$이다. $\qed$

[$i=k<n$]  
$k \le m$이고 $\span(X_k\cup Y_k) = V$라고 가정하자.

[$i=k+1$]    
$x_{k+1} \in V$이고 $\span(X_k\cup Y_k) = V$임으로 다음이 성립한다.

$$ x_{k+1} = a_1x_1 + \cdots a_kx_k + a_{k+1}y_{k+1} + \cdots + a_my_m $$

이 때, $X_{k+1}$은 linearly independent set임으로 다음이 성립한다.

$$ \exist j \in [k+1,m] \st a_j \neq 0_\F $$

즉, $[k+1,m]$에 어떤 정수가 존재함으로 다음이 성립한다.

$$ k+1 \le m $$

그리고 $Y$의 element의 순서를 적절하게 바꿔줌으로써 일반성을 잃지 않고 $a_{k+1} \neq 0_\F$라고 할수 있음으로 다음이 성립한다.

$$ \begin{aligned} & y_{k+1} = \frac{1}{a_{k+1}} \left( x_{k+1} - \sum_{i=1}^ka_ix_i - \sum_{i=k+2}^m a_iy_i \right) \\ \implies& y_{k+1} \in \span(X_{k+1}\cup Y_{k+1}) \end{aligned}  $$

또한, $x_1,\cdots,x_k, y_{k+2},\cdots,y_m \in X_{k+1}\cup Y_{k+1}$이고 $X_{k}\cup Y_{k}$은 generating set임으로 다음이 성립한다.

$$ X_{k}\cup Y_{k} \subseteq \span(X_{k+1}\cup Y_{k+1})  \implies \span(X_{k+1}\cup Y_{k+1}) = V \qed $$

[결론]  
따라서 mathematical induction에 의해 $n \le m$이고 $\span(X_{n}\cup Y_{n}) =V$이다. $\qed$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Steinitz_exchange_lemma)

### 명제3
Vector space $V/\F$와 $V$의 subset $S$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ S \text{ is an linearly independent set of } V \implies  0_V \notin S $$

**Proof**

다음을 가정하자.

$$ 0_V \in S $$

그러면 $S$에서 $0_V$를 뽑으면 다음이 성립한다.

$$ \exist a \in \F - \Set{0_\F} \st a0_V = 0_\F $$

이는 $S$가 linearly independent set이라는 전제에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ S \text{ is an linearly independent set of } V \implies  0_V \notin S \qed $$

### 명제4
Vector space $V/\F$와 $V$의 nonempty subset $S$가 있다고 하자.

$S$의 maximal linearly independent set을 $M$이라고 할 때, 다음을 증명하여라.

$$ S \subseteq \span(M) $$

**Proof**

다음을 가정하자.

$$ S \nsubseteq \span(M) $$

그러면 다음이 성립한다.

$$ \begin{aligned} & S - \span(M) \neq \empty \\ \implies& \exist v \in S - \span(M) \\\implies& M \cup \Set{v} \text{ is an linearly independent set} \end{aligned} $$

하지만 이는 $M$이 maximal linearly independent set이라는 사실에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ S \subseteq \span(M) \qed $$
