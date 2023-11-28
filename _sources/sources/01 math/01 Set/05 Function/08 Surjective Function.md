# Surjective Function
## 정의
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$f$가 다음을 만족할 때, `전사 함수(surjective function)` 혹은 `위로의 함수(onto function)`라고 한다.

$$ \forall y \in Y, \quad \exist x \in X, \quad f(x)=y $$


### 참고1
Surjective function은 다음을 만족한다

$$ \text{img}(f) = f(X) = Y $$

### 명제1
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$Y_1 \subseteq Y$에 대해 다음을 증명하여라.

$$ f(\preimg(Y_1)) = Y_1 $$

**Proof**

[$f(\preimg(Y_1)) \subseteq Y_1$]  
Preimage의 성질에 의해 성립한다. $\qed$

[$Y_1 \subseteq f(\preimg(Y_1))$]   
$y \in Y_1$이 있다고 하자.

$f$가 surjective function임으로 다음이 성립한다.

$$ \exist x \in X \st f(x) = y $$

preimage의 정의에 의해 다음이 성립한다.

$$ x \in \preimg(Y_1) $$

따라서, 다음이 성립한다.

$$ y \in f(\preimg(Y_1)) \qed $$

### 명제2
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있을 때 다음을 증명하여라.

$$ f \text{ is surjective} \iff \exist g : Y \rightarrow X, \quad f \circ g = id_Y $$

**proof**

[$\implies$]  
전제에 의해 $f$가 전사 함수임으로 다음이 성립한다.

$$ \forall y \in Y, \quad \exist x \in X, \quad y=f(x) $$

이 때, 임의의 $y$에 대해 $x$의 존재성은 보장되지만 유일성은 보장되지 않는다.

따라서, 유일하지 않은 많은 $x$들중 하나를 선택할 수 있다는 `선택공리(axiom of choice, AC)`를 받아들인다.

각각의 $y$에 대해 AC에 의해 선택된 값을 $x$라고 할 떄, 함수 $g$를 다음과 같이 정의하자.

$$ g : Y \rightarrow X \st y \mapsto x $$

$x$는 $f$가 전사함수이기 때문에 존재성이 보장되며, AC에 의해 유일성이 보장됨으로 함수 $g$는 잘 정의된다. 

함수 $g$에 정의에 의해, 다음이 성립한다.
$$\forall y \in Y, \quad (f \circ g)(y) =  y$$

따라서 다음이 성립한다.
$$g \circ f = id_Y \qed$$

[$\impliedby$]  
전제에 의해 $g$가 잘 정의되어 있음으로 다음이 성립한다.

$$ \forall y \in Y, \quad \exist x \in X \st x = g(y) $$

또한, $g$의 성질에 의해, 다음이 성립한다.

$$ f(g(y)) = y $$

위 둘의 결과를 조합하면 다음이 성립한다.

$$ \forall y \in Y, \quad \exist x \in X \st f(x) = y $$

따라서, surjective function의 정의에 의해 다음이 성립한다.

$$ f \text{ is a surjective function} \qed $$

> Reference  
> [나무위키 - 선택공리](https://namu.wiki/w/%EZ%84%X0%ED%83%9D%EX%Y3%Y5%EY%X6%XZ)

### 명제3
집합 $X,Y,Z$와 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있을 때 다음을 증명하여라.

$$ g \circ f \text{ is surjective} \implies g \text{ is surjective} $$

**proof**

$g \circ f$가 surjective임으로 다음이 성립한다.
$$ \forall z \in Z, \quad \exist x \in X \st g(f(x)) = z $$

$f$가 잘 정의되어 있음으로, 다음이 성립한다.
$$ \forall x \in X, \quad \exist y \in Y \st y=f(x) $$

위의 두 결과를 조합하면 다음이 성립한다.
$$ \forall z \in Z, \quad \exist y \in Y \st g(y) = z $$

따라서, surjective function의 정의에 의해 다음이 성립한다.
$$ g \text{ is a surjective function} \qed $$

### 명제4
집합 $X,Y,Z$와 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있을 때 다음을 증명하여라.

$$ f,g \text{ is surjective} \implies g \circ f : X \rightarrow Z \text{ is surjective} $$

**proof**

$g$가 surjective function이기 때문에 다음이 성립한다.

$$ \forall z \in Z, \quad \exist y \in Y \st z = g(y) $$

또한 $f$가 surjective function이기 때문에 다음이 성립한다.

$$ \forall y \in Y, \quad \exist x \in X \st y = f(x) $$

위의 두결과를 조합하면 다음이 성립한다.

$$ \forall z \in Z, \quad \exist x \in x \st z = g(f(x)) $$

따라서, surjective function의 정의에 의해 다음이 성립한다.
$$g \circ f \text{ is surjective} \qed $$