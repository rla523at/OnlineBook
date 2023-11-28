# Preimage

## 정의
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$V \subseteq Y$가 있을 때, $f$에 의한 $V$의 `원상(preimage)` $\preimg(V)$는 다음과 같이 정의된 집합이다.

$$ \preimg(V) := \Set{x \in X | f(x) \in V} $$

### 명제1

집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$X_1 \subseteq X$에 대해 다음을 증명하여라.

$$ X_1 \subseteq \preimg(f(X_1)) $$

**proof**

$X_1$의 임의의 element를 $x$라고 하자.

그러면 함수의 정의에 의해 다음이 성립한다.

$$ f(x) \in f(X_1) $$

이 떄, $\preimg(f(X_1)) := \set{x \in X \mid f(x) \in f(X_1)}$임으로 다음이 성립한다.

$$ x\in \preimg(f(X_1)) \qed  $$

> Reference  
> [kocw 충북대 집합론](http://contents.kocw.or.kr/document/lec/2011/chungbuk/teaching/08_1.pdf)

### 명제2
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$X_1 \subseteq X$에 대해 다음 명제가 `거짓`임을 증명하여라.

$$ \preimg(f(X_1)) \subseteq X_1 $$

**Proof**

$x_1,x_2 \in X$와 $y \in Y$가 있을 떄, 다음을 만족한다고 하자.

$$ f(x_1) = f(x_2) = y $$

$X_1 = \Set{x_1}$이라고 하면, 다음이 성립한다.

$$ \begin{aligned} \preimg(f(X_1)) &= \preimg(y) \\&= \Set{x_1,x_2} \\&\nsubseteq X_1  \end{aligned} $$

따라서, 반례가 존재함으로 주어진 명제는 거짓이다. $\qed$

#### 참고1
$f(x)$의 preimage중에 $x$외에 또 다른 원소가 있을 수 있기 때문에

$f$에 의해 갔다가 preimage에 의해 돌아오는 길에는 커질 수 있다.

#### 참고2
명제2에 의해서 다음 명제가 거짓이 된다.

$$ X_1 = \preimg(f(X_1)) $$

#### 참고3
반례를 보면 알 수 있듯이, $X-X_1$에 존재하는 element와 $X_1$에 존재하는 element가 동일한 $Y$의 element가 될 떄, 위의 명제가 거짓이 된다.

그러나 $X_1$의 element들이 동일한 $Y$의 element가 되더라도 위 명제는 참이다.

### 명제3
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$Y_1 \subseteq Y$에 대해 다음을 증명하여라.

$$ f(\preimg(Y_1)) \subseteq Y_1 $$

**proof**

함수의 정의에 의해 다음이 성립한다.

$$ \forall y \in f(\preimg(Y_1)), \quad \exist x \in \preimg(Y_1) \st y = f(x) $$

위를 만족하는 $x$에 대해서, preimg의 정의에 의해 다음이 성립한다.

$$ \forall x \in \preimg(Y_1), \quad f(x) \in Y_1 $$

따라서, 위의 두 결과를 합치면 다음이 성립한다.

$$ \forall y \in f(\preimg(Y_1)),\quad y \in Y_1 \qed $$

### 명제4
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$Y_1 \subseteq Y$에 대해 다음 명제가 `거짓`임을 증명하여라.

$$ Y_1 \subseteq f(\preimg(Y_1)) $$

**Proof**

$y \in Y$가 다음을 만족한다고 하자.

$$ y \notin f(X) $$

$Y_1 = \Set{y}$이라고 하면, 다음이 성립한다.

$$ \begin{aligned} f(\preimg(Y_1)) &= f(\empty) \\&= \empty \end{aligned} $$

$Y_1 \nsubseteq f(\preimg(Y_1))$임으로 반례가 존재한다.

따라서, 주어진 명제는 거짓이다. $\qed$

#### 참고1
preimage에 의해 $Y$의 원소중 $\empty$로 가는 원소가 있기 때문에

preimage에 의해 가는 길에는 작아 질 수 있다.



#### 참고2
명제4에 의해서 다음 명제가 거짓이 된다.

$$ Y_1 = f(\preimg(Y_1)) $$


### 명제5
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$Y_1, Y_2 \subseteq Y$에 대해 다음을 증명하여라.

$$ \preimg(Y_1 \cup Y_2) = \preimg(Y_1) \cup \preimg(Y_2) $$

**proof**

$$ \begin{aligned} & x \in \preimg(Y_1 \cup Y_2) \\ \iff& f(x) \in Y_1 \cup Y_2 \\ \iff& \exist i \in \Set{1,2}, \quad f(x) \in Y_i \\ \iff& \exist i \in \Set{1,2}, \quad x \in \preimg(Y_i) \\ \iff& x \in \preimg(Y_1) \cup \preimg(Y_2) \qed \end{aligned} $$

> Reference  
> [kocw 충북대 집합론](http://contents.kocw.or.kr/document/lec/2011/chungbuk/teaching/08_1.pdf)

### 명제6
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$Y_1, Y_2 \subseteq Y$에 대해 다음을 증명하여라.

$$ \preimg(Y_1 \cap Y_2) = \preimg(Y_1) \cap \preimg(Y_2). $$

**proof**

$$ \begin{aligned} & x \in \preimg(Y_1 \cap Y_2) \\ \iff& f(x) \in Y_1 \cap Y_2 \\ \iff& \forall i \in \Set{1,2}, \quad f(x) \in Y_i \\ \iff& \forall i \in \Set{1,2}, \quad x \in \preimg(Y_i) \\ \iff& x \in \preimg(Y_1) \cap \preimg(Y_2) \qed \end{aligned} $$

> Reference  
> [kocw 충북대 집합론](http://contents.kocw.or.kr/document/lec/2011/chungbuk/teaching/08_1.pdf)

### 명제7
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$X_1 \subseteq X$와 $Y_1, Y_2 \subseteq Y$에 대해 다음을 증명하여라.

$$ \preimg_{f|_{X_1 \times Y_1}}(Y_2) = X_1 \cap \preimg_f(Y_1) \cap \preimg_f(Y_2) $$

**Proof**

preimage의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \preimg_{f|_{X_1 \times Y_1}}(Y_2) &= \Set{ x \in X_1 | f(x) \in Y_1 \cap Y_2} \\&= X_1 \cap \Set{ x \in X | f(x) \in Y_1 \cap Y_2} \\&= X_1 \cap \Set{ x \in X | f(x) \in Y_1} \cap \Set{ x \in X | f(x) \in Y_2}  \\&= X_1 \cap \preimg_f(Y_1) \cap \preimg_f(Y_2) \qed \end{aligned} $$

### 명제8
집합 $X,Y,Z$와 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있다고 하자.

$Z$의 subset을 $W$라고 할 때, 다음을 증명하여라.

$$ \preimg_{g\circ f}(W) = \preimg_f(\preimg_g(W)) $$

**Proof**

preimage의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \preimg_g(W) &= \Set{y \in Y | g(y) \in W} \\ \preimg_f(\preimg_g(W)) &= \Set{ x \in X | g(f(x)) \in W} \\&= \preimg_{g\circ f}(W) \qed \end{aligned} $$


