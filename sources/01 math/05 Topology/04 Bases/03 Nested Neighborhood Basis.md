# Nested Neighborhood Basis
## 정의
Topological space $X$가 있다고 하자.

$x \in X$가 있을 때, 다음을 만족하는 sequence $\mathcal{B}(n)$을 $x$에서 `nested neighborhood basis`라고 한다.

$$ \forall n \in \N, \quad \mathcal{B_x(n)} \in \Set{\mathcal{N_x}} \enspace\land\enspace \mathcal{B_x}(n+1) \subseteq \mathcal{B_x}(n) \enspace\land\enspace \forall \mathcal{N_x}\in \Set{\mathcal{N_x}}, \quad \exist N \in \N \quad s.t. \quad \mathcal{B_x}(N)\subseteq \mathcal{N_x}$$


### 명제1
Topological space $X$가 있다고 하자.

$x \in X$의 nested neighborhood basis $\mathcal{B_x(n)}$가 있을 때, 다음을 증명하여라.

$$ \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist N \in \N \st N \le n \implies  \mathcal{B_x}(n) \subseteq \mathcal{N_x} $$

**Proof**

Nested neighborhood basis의 정의에 의해 다음이 성립한다.

$$ \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist N \in \N \st \mathcal{B_x}(N) \subseteq \mathcal{N_x} $$

또한, Nested neighborhood basis의 정의에 의해 $N \le n$에 대해 다음이 성립한다.

$$ \mathcal{B_x}(n) \subseteq \mathcal{B_x}(N) $$

따라서, 다음이 성립한다.

$$ \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist N \in \N \st N \le n \implies  \mathcal{B_x}(n) \subseteq \mathcal{N_x} \qed $$

### 명제2
Topological space $X$와 $X$위의 sequence $s(n)$이 있다고 하자.

$x \in X$의 nested neighborhood basis $\mathcal{B_x(n)}$가 있을 때, 다음을 증명하여라.

$$ s(n) \in B_x(n) \implies \lim_{n\rightarrow\infty}s(n) =x $$

**Proof**

명제1에 의해 다음이 성립한다.

$$ \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist N \in \N \st N \le n \implies  \mathcal{B_x}(n) \subseteq \mathcal{N_x} $$

전제에 의해 다음이 성립한다.

$$ \forall \mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \exist N \in \N \st N \le n \implies s(n) \in \mathcal{N_x} $$

Convergence의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty}s(n) = x \qed $$