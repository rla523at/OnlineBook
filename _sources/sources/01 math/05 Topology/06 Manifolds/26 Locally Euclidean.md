# Locally Euclidean
## 정의
Topological space $X$가 있다고 하자.

이 때, $X$가 다음을 만족할 경우, $X$가 `locally Euclidean` of dimension $n$이라고 한다.

$$ \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to an open subset of } \R^n $$

### 명제1
Topological space $X$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ X \text{ is locally Euclidean of dimension } n \iff \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to an open ball of } \R^n $$

**Proof**

[$\implies$]  
전제에 의해 다음이 성립한다.

$$ \forall x \in X, \quad \exist \mathcal{N_x} \in \Set{\mathcal{N_x}}, V \in \mathcal{T}_{\R^n} \st \mathcal{N_x} \text{ and } V \text{ are homeomorphic } $$

$\mathcal{N_x}$와 $V$ 사이의 homeomorphism을 $\phi: \mathcal{N_x} \rightarrow V$라 하자.

이 떄, $V$가 $\R^n$의 open set임으로 다음이 성립한다.

$$ \exist r \st B_{\R^n}(\phi(x),r) \subseteq V $$

위를 만족하는 open ball을 $B$라고 할 때, 보조명제1.1에 의해 다음이 성립한다.

$$ \preimg(B) \in \Set{\mathcal{N_x}} $$

따라서, $\preimg(B)$는 $X$의 open set임으로 homeomorphism의 성질에 의해 다음이 성립한다.

$$ \phi|_{\preimg(B) \times B} \text{ is a homeomorphism} $$

위의 결과를 종합하면 다음과 같다.

$$ \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to an open ball of } \R^n \qed $$

[$\impliedby$]  
open ball은 open set임으로 locally Euclidean의 정의에 의해 다음이 성립한다.

$$ X \text{ is locally Euclidean of dimension } n \qed $$

#### 보조명제1.1
다음을 증명하여라.

$$ \preimg(B) \in \Set{\mathcal{N_x}} $$

**Proof**

$B$는 $\R^n$의 open set임으로 subspace의 정의에 의해 다음이 성립한다.

$$ B \text{ is an open set of } V $$

$\phi$가 continuous 함으로 다음이 성립한다.

$$ \text{Preimage of every open set of } V \text{ is open set of } \mathcal{N_x} $$

따라서, 다음이 성립한다.

$$ \preimg(B) \text{ is an open set of } \mathcal{N_x} $$

$\mathcal{N_x}$는 $X$의 open set임으로 subspace의 성질에 의해 다음이 성립한다.

$$ \preimg(B) \text{ is an open set of } X $$

따라서, Neighborhood의 정의에 의해 다음이 성립한다.

$$ \preimg(B) \in \Set{\mathcal{N_x}} \qed $$

### 명제2
Topological space $X$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ X \text{ is locally Euclidean of dimension } n \iff \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to } \R^n $$

**Proof**

명제1에 의해 다음이 성립한다.

$$ X \text{ is locally Euclidean of dimension } n \iff \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to an open ball of } \R^n $$

이 때의, homeomorphism을 $\varphi_1$이라 하자.

다음으로, homeomorphism의 성질에 의해 다음이 성립한다.

$$ \text{Any open ball in } \R^n \text{ and } \R^n \text{ are homeomorphic} $$

이 때의, homeomorphism을 $\varphi_2$이라 하자.

$\varphi_{1,2}$ 모두 homeomorphism임으로, homeomorphism의 성질에 의해 다음이 성립한다.

$$ \varphi_2 \circ \varphi_1 \text{ is a homeomorphism} $$

따라서, 다음이 성립한다.

$$ X \text{ is locally Euclidean of dimension } n \iff \forall x \in X, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to } \R^n \qed $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1072741/is-an-open-n-ball-homeomorphic-to-mathbbrn)


### 명제3
$n$차원 locally Euclidean space $X$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \text{Every open set of X is an } n \text{ dimensional locally Euclidean space } $$

**Proof**

$U$가 $X$의 open set이라고 하자.

$X$가 locally Euclidean space임으로 다음이 성립한다.

$$ \forall x \in U, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N_x}} \st \text{homeomorphic to an open subset of } \R^n $$

이떄의, homeomorphism을 $\varphi$라고 하자.

각 $x$에서 위를 만족하는 $\mathcal{N_x}$에 대해, subspace의 성질에 의해 다음이 성립한다.

$$ \forall x \in U, \quad U \cap \mathcal{N_x} \text{ is an open set of } U $$

$U_x' = U \cap \mathcal{N_x}$이라 할 때, subspace의 성질에 의해 다음이 성립한다.

$$ U_x' \text{ is an open set of } X$$

$\varphi$가 homeomorphism임으로 다음이 성립한다.

$$ \varphi(U_x') \text{ is an open set of } \R^n $$

또한, homeomorphism의 성질에 의해 다음이 성립한다.

$$ \varphi|_{U_x' \times \varphi(U_x')} \text{ is an homeomorphism} $$

즉, 위의 결과를 종합하면 다음과 같다.

$$ \forall x \in U, \quad \exist\mathcal{N_x} \in \Set{\mathcal{N^U_x}} \st \text{homeomorphic to an open subset of } \R^n $$

따라서, locally Euclidean space의 정의에 의해 다음이 성립한다.

$$ U \text{ is an locally Euclidean space of dimension } n $$

그럼으로, 다음이 성립한다.

$$ \text{Every open set of X is an } n \text{ dimensional locally Euclidean space } \qed $$
