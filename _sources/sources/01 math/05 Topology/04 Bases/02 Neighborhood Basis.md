# Neighborhood Basis
## 정의
Topological space $X$가 있다고 하자.

$x \in X$가 있을 때, collection $\mathcal{B_x}$를 다음과 같이 정의하자.

$$ \mathcal{B_x} := \Set{ B \in \Set{\mathcal{N_x}}} $$

이 떄, $\mathcal{B_x}$가 다음을 만족할 경우, $\mathcal{B_x}$를 $x$에서 $X$의 `neighborhood basis`라고 한다.

$$ \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist B\in\mathcal{B_x} \quad s.t. \quad B \subseteq \mathcal{N_x} $$

### 참고
Neighborhood basis를 `local basis`라고도 부른다.

> Reference    
> [Proofwiki](https://proofwiki.org/wiki/Definition:Local_Basis)

### 명제1
Topological space $X$가 있다고 하자.

$\mathcal{B}$가 $\mathcal{T_X}$의 basis일 때, $\forall x \in X$마다 $\mathcal{B}$의 sub collection $\mathcal{B_x}$를 다음과 같이 정의하자.

$$ \mathcal{B_x} := \Set{ B \in \mathcal{B} | x \in B } $$

이 때, 다음을 증명하여라.

$$ \forall x \in X, \quad \mathcal{B_x} \text{ is a neighborhood basis of } X \text{ at } x $$

**Proof**

basis의 정의에 의해 $\mathcal{B}$의 원소는 모두 open set임으로 다음이 성립한다.

$$ \forall B \in \mathcal{B_x}, \quad B \in \Set{\mathcal{N_x}} $$

또한, 보조명제1.1에 의해 다음이 성립한다.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad  \exist B \in \mathcal{B_x} \st B \subseteq \mathcal{N_x} $$

그럼으로 neighborhood basis의 정의에 의해 $\mathcal{B_x}$는 neighbor hood basis이다. $\qed$

> Refernece  
> [math.stackexchange](https://math.stackexchange.com/questions/2771776/topological-basis-vs-local-basis)

#### 보조명제 1.1
다음을 증명하여라.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad  \exist B \in \mathcal{B_x} \st B \subseteq \mathcal{N_x} $$


**Proof**

basis의 정의에 의해 다음이 성립한다.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist \mathcal{B'}\subseteq\mathcal{B} \st \mathcal{N_x} = \bigcup B' $$

이 떄, $\forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad x \in \mathcal{N_x} $임으로 다음이 성립한다.

$$ \exist B \in \mathcal{B'} \st B \in \mathcal{B_x} $$

따라서, 위의 두 결과를 종합하면 다음과 같다.

$$ \forall\mathcal{N_x}, \quad  \exist B \in \mathcal{B_x} \st B \subseteq \mathcal{N_x} $$
