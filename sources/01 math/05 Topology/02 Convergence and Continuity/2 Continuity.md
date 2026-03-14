# Continuity
## 정의
Topological space $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

이 때, 다음을 만족하는 $f$를 `연속(continuous)`이라고 한다.

$$ \text{preimage of every open set of } Y \text{ is open set of } X$$

> Referece  
> {cite}`LeeTM` p.26

### 참고
open set의 image를 open set이라고 정의한다면 일반적으로 가지고 있는 continuous function의 직관에 위배된다.

예를 들어, $y =  x^2$을 생각해보자. 

open set $(-1,1)$의 image는 $[0,1)$로open set이 아니다. 

따라서 위의 정의에 따르면 continuous function이 아니게 된다.

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1074769/image-of-open-set-is-not-open)  

### 명제1
Topological space $X,Y$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Every constant map } f : X \rightarrow Y \text{ is continous} $$

**Proof**

$f(X) = y \in Y$라고 하자.
 
open set $S \subset Y$가 있을 때, 다음이 성립한다.

$$ \preimg(S) = \begin{cases} \empty & y \notin S \\ X & y \in S \end{cases} $$

이 때, $\empty, X$는 모두 $Y$의 open set임으로 continuity의 정의에 의해 $f$는 연속함수이다. $\qed$

### 명제2
Topological space $X$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Identity map } I_X : X \rightarrow X \text{ is continous} $$

**Proof**

Identity map의 정의에 의해 자명하게 open set의 preimage 또한 open set이다.

따라서, $f$는 연속함수이다. $\qed$

### 명제3
Topological space $X,Y$와 연속인 함수 $f: X\rightarrow Y$가 있다고 하자.

$U$가 $X$의 subspace일 때, 다음을 증명하여라.

$$ f|_{U} \text{ is a continuous function} $$

**Proof**

$Y$의 open set을 $V$라 하자.

$f$가 연속임으로 다음이 성립한다.

$$ \preimg_{f|_{U}}(V) \text{ is open set of } X $$

또한, Preimage의 성질에 의해 다음이 성립한다.

$$ \preimg_{f|_{U}}(V) = U \cap \preimg(V) \subseteq U $$

위의 결과를 종합하면 다음과 같다.

$$ \preimg_{f|_{U}}(V) \text{ is open set of } X \text{ in } U $$

따라서, subspace의 성질에 의해 다음이 성립한다.

$$ \preimg_{f|_{U}}(V) \text { is open set of } U $$

따라서, $f|_{U}$는 continuous function이다. $\qed$ 

> Refrernce  
> [math.stackexchange](https://math.stackexchange.com/questions/1826827/topology-show-restriction-of-continuous-function-is-continuous-and-restriction)

### 명제4
Topological space $X,Y$와 연속인 함수 $f: X\rightarrow Y$가 있다고 하자.

$U$가 $X$의 subspace이고 $f(U)$가 $Y$의 subspace일 때, 다음을 증명하여라.

$$ f|_{U \times f(U)} \text{ is a continuous function} $$

**Proof**

$V$가 $f(U)$의 open set이라 하자.

그러면 subspace의 성질에 의해 다음이 성립한다.

$$ \exist\text{open set } W \text{ of } Y \st V = f(U) \cap W $$

따라서, $\preimg$의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} \preimg_{f|_{U \times f(U)}}(V) &= \preimg_{f|_{U \times f(U)}}(f(U))\cap\preimg_{f|_{U \times f(U)}}(W) \\&= U \cap U \cap \preimg_f(f(U)) \cap \preimg_f(W) \\&= U \cap \preimg_f(W)\end{aligned} $$

$f$가 연속임으로 다음이 성립한다.

$$ \preimg_f(W) \text{ is open set of } X $$

이 떄, $U$가 $X$의 subspace임으로 다음이 성립한다.

$$ \begin{aligned}& U \cap \preimg_f(W) \text{ is open set of } U \\\implies& \preimg_{f|_{U \times f(U)}}(V) \text{ is open set of } U \end{aligned} $$

따라서, continuous function의 정의에 의해 다음이 성립한다.

$$ f|_{U \times f(U)} \text{ is a continuous function} \qed $$

> Refrernce   
> [math.stackexchange](https://math.stackexchange.com/questions/1826827/topology-show-restriction-of-continuous-function-is-continuous-and-restriction)

### 명제5
Topological space $X,Y,Z$가 있다고 하자.

연속 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있을 떄, 다음을 증명하여라.

$$ g \circ f \text{ is a continuous function} $$

**Proof**

$Z$의 임의의 open set을 $W$라고 하자.

preimage의 성질에 의해 다음이 성립한다.

$$ \preimg_{g\circ f}(W) = \preimg_f(\preimg_g(W)) $$

$g$가 연속임으로 다음이 성립한다.

$$ \preimg_g(W) \text{ is a open set on Y} $$

또한 $f$가 연속임으로 다음이 성립한다.

$$ \preimg_f(\preimg_g(W)) \text{ is a open set on X} $$

임의의 open set의 preimage가 open set임으로 continuos function의 성질에 의해 다음이 성립한다.

$$ g \circ f \text{ is a continuous function} \qed $$

### 명제6(Local Criterion for Continuity)
Topological space $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ f \text{ is continous} \iff \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st f|_{\mathcal{N_x} \times f(\mathcal{N_x})} \text{ is continous} $$

**Proof**

[$\implies$]  
$x \in X$에 대해서, $\mathcal{N_x} = X$로 두면 자명하다. $\qed$

[$\impliedby$]  
open set $V \subset Y$라 하자. 

전제에 의해 다음이 성립한다.

$$ \forall x \in \preimg_f(V), \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st f|_{\mathcal{N_x} \times f(\mathcal{N_x})} \text{ is continous} $$

위를 만족하는 $\mathcal{N_x}$에 대해서 $\mathcal{N_x}$를 $X$의 subspace, $f(\mathcal{N_x})$를 $Y$의 subspace라고 할 떄, 다음이 성립한다.

$$ f(\mathcal{N_x}) \cap V \text{ is an open set of } f(\mathcal{N_x}) $$

$f|_{\mathcal{N_x} \times f(\mathcal{N_x})}$가 continous함으로, 다음이 성립한다.

$$ \preimg_{f|_{\mathcal{N_x} \times f(\mathcal{N_x})}}(f(\mathcal{N_x}) \cap V) \text{ is an open set of } \mathcal{N_x}  $$

Preimage의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} \preimg_{f|_{\mathcal{N_x} \times f(\mathcal{N_x})}}(f(\mathcal{N_x}) \cap V) &= \preimg_{f|_{\mathcal{N_x} \times f(\mathcal{N_x})}}(f(\mathcal{N_x})) \cap \preimg_{f|_{\mathcal{N_x} \times f(\mathcal{N_x})}}(V) \\&= \mathcal{N_x} \cap \preimg_f(V) \end{aligned} $$

따라서, 다음이 성립한다.

$$ \mathcal{N_x} \cap \preimg_f(V) \text{ is an open set of } \mathcal{N_x} $$

$\mathcal{N_x}$는 $X$의 open set임으로, subspace의 성질에 의해 다음이 성립한다.

$$ \mathcal{N_x} \cap \preimg_f(V) \text{ is an open set of } X $$

이 떄, $\mathcal{N_x} \cap \preimg_f(V) \subseteq \preimg_f(V)$임으로 다음이 성립한다.

$$ \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \mathcal{N_x} \subseteq \preimg_f(V) $$

위의 결과를 정리하면 다음과 같다.

$$ \forall x \in \preimg_f(V), \quad \exist\mathcal{N_x} \st \mathcal{N_x} \subseteq \preimg_f(V) $$

따라서, neighborhood의 성질에 의해 다음이 성립한다.

$$ \preimg_f(V) \text{ is an open set of } X $$

그럼으로, continuous function의 정의에 의해 다음이 성립한다.

$$ f \text{ is continuous} \qed $$

> Referece  
> {cite}`LeeTM` 2.19

### 명제7
다음을 증명하여라.

$$\text{metric space continuity definition } \iff \text{topological space continuity definition} $$

**Proof**

metric space에서 다음이 성립한다.

$$ f \text{ is a continuous function } \iff \text{preimage of every open subset is open} $$

따라서 topological space의 definition과 동치이다. $\qed$

#### 참고
명제7에 의해 metric space에서 $\epsilon-\delta$ 논법에 의해 continuous function인 함수들은 topological space에서 continous function이다.