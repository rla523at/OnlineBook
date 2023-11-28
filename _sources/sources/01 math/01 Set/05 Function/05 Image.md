# Image
## 정의
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$U \subseteq X$가 있을 때, $f$에 의한 $U$의 `상(image)` $\img(U)$는 다음과 같이 정의된 집합이다.

$$ \img(U) := \Set{f(x) \in Y | x \in U} $$

### 참고
$\img(U)$를 간단하게 $f(U)$로 표기하기도 한다.

### 명제1
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$X_1, X_2 \subseteq X$에 대해 다음을 증명하여라.

$$ X_1 \subseteq X_2 \implies f(X_1) \subseteq f(X_2) $$

**proof**

$y \in f(X_1)$이라 하자.

함수의 정의에 의해 다음이 성립한다.
$$ \exist x \in X_1 \st f(x) = y $$

$X_1 \subseteq X_2$임으로 다음이 성립한다.

$$ \begin{aligned} & x \in X_2 \\\implies& f(x) \in f(X_2) \\\implies& y \in f(X_2) \qed \end{aligned} $$

### 명제2
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$X_1, X_2 \subseteq X$에 대해 다음 명제가 `거짓`임을 증명하여라.

$$ f(X_1) \subseteq f(X_2) \implies X_1 \subseteq X_2 $$

**proof**

$b \in Y$가 있을 떄, $f$가 다음과 같이 정의되어 있다고 하자.

$$ \forall a \in X, \quad  f(a) = b $$

이 떄, $X_1, X_2$가 다음을 만족한다고 하자.

$$ X_1 \cap X_2 = \emptyset $$

그러면 다음이 성립한다.

$$f(X_1) \subseteq f(X_2) \land X_1 \nsubseteq X_2 $$

따라서, 반례가 존재함으로 주어진 명제는 거짓이다. $\qed$

### 명제3
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$X_1, X_2 \subseteq X$에 대해 다음을 증명하여라.

$$ f(X_1 \cup X_2) = f(X_1) \cup f(X_2) $$

**proof**

$\forall y \in f(X_1 \cup X_2)$에 대해 다음이 성립한다.

$$ \begin{aligned} & \exists x \in X_1 \cup X_2 \st y = f(x) \\\iff& \exist i \in {1,2} \st x \in X_i \\\iff& \exist i \in {1,2} \st y \in f(X_i) \\\iff& y \in f(X_1) \cup f(X_2) \qed \end{aligned}  $$

> Reference  
> [블로그](https://mathphysics.tistory.com/711)

### 명제4
집합 $X,Y$와 함수 $f:X \rightarrow Y$가 있다고 하자.

$X_1, X_2 \subseteq X$에 대해 다음을 증명하여라.

$$ f(X_1 \cap X_2) \subseteq f(X_1) \cap f(X_2). $$

**proof**

$\forall y \in f(X_1 \cap X_2)$에 대해 다음이 성립한다.

$$ \exist x \in X_1 \cap X_2 \st y = f(x) $$

그리고 교집합의 성질에 의해 다음이 성립한다.

$$ \forall i \in \Set{1,2}, \quad X_1 \cap X_2 \subseteq X_i $$


따라서, 명제1에 의해서 다음이 성립한다.

$$ \begin{aligned} & \forall i \in \Set{1,2}, \quad f(X_1 \cap X_2) \subseteq f(X_i) \\\implies& \forall i \in \Set{1,2}, \quad y \in f(X_i) \\\iff& y \in f(X_1) \cap f(X_2) \qed \end{aligned}  $$

### 명제5
집합 $X,Y$와 함수 $f:X \rightarrow Y$가 있다고 하자.

$X_1, X_2 \subseteq X$가 있을 떄, 다음 명제가 `거짓`임을 증명하여라.

$$ f(X_1) \cap f(X_2) \subseteq f(X_1 \cap X_2) $$

**proof**

$y \in Y$가 있을 떄, $f$가 다음과 같이 정의되어있다고 하자.

$$ \forall x \in X, \quad  f(x) = y $$

$X_1, X_2$가 다음을 만족한다고 하자.

$$ X_1 \cap X_2 = \emptyset $$

그러면 다음이 성립한다.

$$\begin{aligned} & f(X_1) \cap f(X_2) = \Set{b} \land f(X_1 \cap X_2) = \empty \\\implies& f(X_1) \cap f(X_2) \nsubseteq f(X_1 \cap X_2) \end{aligned} $$

따라서, 반례가 존재함으로 주어진 명제는 거짓이다. $\qed$