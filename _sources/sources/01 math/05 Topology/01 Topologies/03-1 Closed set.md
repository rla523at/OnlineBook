# Closed set
## 정의
Topological space $X$가 있다고 하자.

다음을 만족하는 $X$의 subset $U$를 $X$의 closed set이라고 한다.

$$ X-U \text{ is open set on } X $$

### 참고(Clopen)
open set이면서 동시에 closed set을 `clopen set`이라고 한다.

### 명제1
Topological space $X$가 있다고 하자.

다음을 증명하여라.

$$ \text{finite union of closed set is closed set} $$

**Proof**

$X$의 모든 closed set의 집합을 $U$라 하자.

$U$의 finite subset을 $U'$이라 할 때, 집합 연산의 성질에 의해 다음이 성립한다.

$$ X - \bigcup U' = \bigcap (X - U') $$

$U'$의 element는 closed set임으로 open set의 finite intersection이고 open set의 성질에 의해 다음이 성립한다.

$$ \bigcap (X - U') \text{ is an open set of } X $$

따라서 closed set의 정의에 의해 다음이 성립한다.

$$ \bigcup U' \text{ is an closed set.} \qed $$

### 명제2
Topological space $X$가 있다고 하자.

다음을 증명하여라.

$$ \text{infinite intersection of closed set is closed set} $$

**Proof**

$X$의 모든 closed set의 집합을 $U$라 하자.

집합 연산의 성질에 의해 다음이 성립한다.

$$ X - \bigcap_{\forall U_i \in U} U_i = \bigcup_{\forall U_i \in U} (X - U_i) $$

이 떄, $U_i$는 closed set임으로 $X - U_i$는 open set이 된다.

open set의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \bigcup_{\forall U_i \in U} (X - U_i) \text{ is an open set.} \\ \Rightarrow\enspace& X - \bigcap_{\forall U_i \in U} U_i \text{ is an open set.} \end{aligned}  $$

따라서 closed set의 정의에 의해 다음이 성립한다.

$$ \bigcap_{\forall U_i \in U} U_i \text{ is an closed set.} \qed $$

### 명제3
Topological space $X$가 있다고 하자.

$a \in X$에 대해 다음을 증명하여라.

$$ \{a\} \text{ may not be a closed set} $$

**Proof**

$X$가 다음과 같이 주어졌다고 하자.

$$ X = \{ 1,2,3 \}, \enspace \mathcal{T}_X = \{ \empty, \{ 1 \}, \{ 1,2 \}, \{ 1,2,3 \} \} $$

$\{1\}$의 경우 $\{2,3\}$이 open set이 아님으로 closed set이 아니다.$\qed$

### 명제4
Topological space $X$가 있다고 하자.

$X$의 open set $U$가 있을 떄, 다음을 증명하여라.

$$ X-U \text{ is an closed set of } X $$

**Proof**

집합 연산의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} X-(X-U) &= X \cap (X \cap U^c)^c \\&= X \cap (X^c \cup U) \\&= (X \cap X^c) \cup (X \cap U) \\&= \empty \cup U \\&= U \end{aligned} $$

이 때, $U$가 open set임으로 closed set의 정의에 의해 다음이 성립한다.

$$ X-U \text{ is an closed set of } X \qed $$

### 명제5
Topological space $X$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ X \text{ has discrete topology } \iff \text{every subset of }X\text{ are clopen} $$

**Proof**

[$\implies$]  
$X$의 임의의 subset을 $U$라고 하자.

그러면 다음이 성립한다.

$$ U^c \text{ is an subset of } X $$

전제에 의해 다음이 성립한다. 

$$ U,U^c \text{ is an open set} $$

$U^c$가 open set임으로 closed set의 정의에 의해 다음이 성립한다.

$$ U \text{ is a closed set} $$

$X$의 임의의 subset이 clopen set임으로 다음이 성립한다.

$$ \text{every subset of }X\text{ are clopen} \qed $$

[$\impliedby$]  
$X$의 모든 subset이 open set임으로 다음이 성립한다.

$$ X \text{ has discrete topology } \qed $$