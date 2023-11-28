# Inverse Function
## 정의
집합 $X,Y$와 bijective function $f : X \rightarrow Y$가 있다고 하자.

`역함수(inverse function)` $f^{-1}$은 다음과 같이 정의된 함수다.

$$ f^{-1} := \Set{(y,x) \in Y \times X | y = f(x)}  $$

### 명제1
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있을 때 다음을 증명하여라.

$$ f \text{ is bijective} \iff \exist! f^{-1} $$

**proof**

[$\implies$]  
$f$ 전사 함수이기 때문에 임의의 $y \in Y$에 대해 반드시 $f(x)=y$를 만족하는 $x$가 존재한다.

동시에 단사 함수이기 때문에 이런 관계를 만족하는 $x$는 유일하다.

즉 임의의 $y$에 대해서 $x$가 유일하게 존재함으로 함수의 정의를 만족시키는 $f^{-1}$는 유일하게 존재한다. $\qed$

[$\impliedby$]  
$f^{-1}$는 정의상 $f$가 bijective function일 떄, 정의됨으로 자명하다. $\qed$

### 명제2
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있을 때 다음을 증명하여라.

$$ f \text{ is bijective} \Rightarrow f^{-1} \text{ is bijective}. $$

**proof**

$f^{-1}$의 정의에 의해 $f \circ f^{-1} = id_Y$이다.

$id_Y$는 injective function임으로 injective function의 성질에 의해 다음이 성립한다.

$$ f^{-1} \text{ is a injective function} $$

$f^{-1}$의 정의에 의해 $f^{-1} \circ f = id_X$이다.

$id_X$는 surjective function임으로 surjective function의 성질에 의해 다음이 성립한다. 

$$ f^{-1} \text{ is a surjective function} $$

따라서 $f^{-1}$는 전단사함수이다. $\qed$

### 명제3
집합 $X,Y$와 bijective 함수 $f: X \rightarrow Y$가 있다고 하자.

subset $U \subseteq X$가 있을 때, 다음을 증명하여라.

$$ (f|_{U \times f(U)})^{-1} = f^{-1}|_{f(U) \times U} $$

**Proof**

다음이 성립한다.

$$ \forall y \in f(U), \quad (f|_{U \times f(U)})^{-1}(y) = f^{-1}(y) $$

또한 다음도 성립한다.

$$ \forall y \in f(U), \quad f^{-1}|_{f(U) \times U} (y) = f^{-1}(y) $$

따라서 다음이 성립한다.

$$ (f|_{U \times f(U)})^{-1} = f^{-1}|_{f(U) \times U} \qed $$

> Reference  
> [proofwiki](https://proofwiki.org/wiki/Restriction_of_Inverse_is_Inverse_of_Restriction)