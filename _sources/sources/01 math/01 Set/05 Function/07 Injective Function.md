# Injective Function
## 정의
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$f$가 다음을 만족할 떄, `단사 함수(injective function)` 혹은 `일대일 함수(one-to-one function)`라고 한다.

$$ x_1,x_2 \in X, \quad f(x_1) = f(x_2) \implies x_1 = x_2 $$

### 참고1
대우 명제는 다음과 같다.

$$ x_1,x_2 \in X, \quad x_1 \neq x_2 \implies f(x_1) \neq f(x_2) $$

### 명제1

집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$f$가 injective fnction일 떄, $X_1 \subseteq X$에 대해 다음을 증명하여라.

$$ X_1 = \preimg(f(X_1)) $$

**Proof**

[$X_1 \subseteq \preimg(f(X_1))$]  
Preimage의 성질에 의해 성립한다. $\qed$

[$\preimg(f(X_1)) \subseteq X_1$]   
$\forall x \in \preimg(f(X_1))$에 대해 다음이 성립한다.

$$ f(x) \in f(X_1) $$

그러면, 다음이 성립한다.

$$ \exist y \in X_1 \st f(x) = f(y) $$

이 때, $f$가 injective function임으로, 다음이 성립한다.

$$ x = y $$

따라서, 다음이 성립한다.

$$ x \in X_1 \qed $$

### 명제2

집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있을 때 다음을 증명하여라.

$$ f \text{ is injective} \iff \exist g : Y \rightarrow X, \quad g \circ f = id_X $$

**proof**

[$\implies$]  
임의의 $x_0 \in X$에 대해 함수 $g$를 다음과 같이 정의하자.  

$$ g : Y \rightarrow X \st y \mapsto \begin{cases} x & \text{if} & y \in f(X) \\ x_0 & \text{else}\end{cases} $$

전제에 의해 $f$는 injective function임으로, 함수 $g$는 잘 정의된다(well defined). 

함수 $g$의 정의에 의해 다음이 성립한다. 

$$\forall x \in X, \quad (g \circ f)(x) = x$$

따라서, 다음이 성립한다.

$$ g \circ f = id_X \qed $$

[$\impliedby$]  
$x_1, x_2 \in X$가 다음을 만족한다고 하자.

$$ f(x_1) = f(x_2) $$

이 떄, 전제에 의해 다음이 성립한다.

$$ \begin{aligned} g(f(x_1)) &= g(f(x_2)) \\ x_1 &= x_2 \end{aligned} $$

따라서, injective function의 정의에 의해 다음이 성립한다.

$$ f \text{ is injective} \qed $$

### 명제3
집합 $X,Y,Z$와 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있을 때 다음을 증명하여라.

$$ g \circ f \text{ is injective} \implies f \text{ is injective} $$

**proof**  
$x_1,x_2 \in X$가 다음을 만족한다고 하자

$$ f(x_1) = f(x_2) $$

이 때, 전제에 의해 다음이 성립한다.

$$ \begin{aligned} g(f(x_1)) &= g(f(x_2)) \\ x_1 &= x_2 \end{aligned} $$

따라서, injevtive function의 정의에 의해 다음이 성립한다.

$$ f \text{ is an injective function} \qed $$

### 명제4
집합 $X,Y,Z$와 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있을 때 다음을 증명하여라.

$$ f,g \text{ is injective} \implies g \circ f : X \rightarrow Z \text{ is injective} $$

**proof**  
$x_1,x_2 \in X$가 다음을 만족한다고 하자.

$$ g(f(x_1)) = g(f(x_2)) $$

전제에 의해 $g$가 injective function임으로 다음이 성립한다.

$$ f(x_1) = f(x_2) $$

또, 전제에 의해 $f$가 injective function임으로 다음이 성립한다.

$$ x_1 = x_2 $$

따라서, injevtive function의 정의에 의해 다음이 성립한다.

$$ g \circ f \text{ is an injective function} \qed $$

### 명제5
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

Subset $U \subseteq X$가 있을 때, 다음을 증명하여라.

$$ f \text{ is injective} \implies f|_U \text{ is injective} $$

**Proof**

$x_1,x_2 \in U$라고 하자.

$f$가 injective임으로 다음이 성립한다.

$$ \begin{aligned} & f|_U(x_1) = f|_U(x_2) \\\implies\enspace& f(x_1) = f(x_2) \\\implies\enspace& x_1 = x_2 \qed \end{aligned}   $$

