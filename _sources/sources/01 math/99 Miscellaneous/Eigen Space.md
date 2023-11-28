# Eigen Space
vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있고, $\lambda$가 고유값이라고 하자.

이 때, 다음과 같이 정의된 집합을 $T$의 $\lambda$에 따른 `고유공간(eigen space)`이라고 한다.
$$ E_\lambda := \{ v \in V \enspace | \enspace T(v) = \lambda v \} $$

### 참고
$\lambda$를 eigen value로 갖는 eigen vector의 집합을 $\beta_\lambda = \{ \beta_1, \cdots, \beta_k \}$라 하자.

그러면 Eigen space의 정의에 의해 다음이 성립한다.
$$ E_\lambda = \text{span}(\beta_\lambda) $$

### 명제1
vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있고, $\lambda$가 고유값이라고 할 때,  다음을 증명하여라.
$$E_\lambda < V$$

**Proof**

[기본 연산 법칙]  
벡터 공간 $V$의 원소들로 이루어져 있음으로, 기본적인 연산 법칙이 성립한다.

[연산에 닫혀있음]  
$a \in \mathbb F, \enspace v_1,v_2 \in E_\lambda$라 하자. $T(av_1 + v_2) = \lambda(av_1 + v_2)$임으로 $av_1 + v_2 \in E_\lambda$이다. $\quad {_\blacksquare}$

[$+$ 항등원의 존재성]  
$T(0_V) = 0_V$이고 $a \in \mathbb F \Rightarrow a0_V = 0_V$임으로 $0_V \in E_\lambda$이다. $\quad {_\blacksquare}$

[$+$ 역원의 존재성]  
상수곱이 정의되어 있음으로, 환의 명제2에의해 존재한다. $\quad {_\blacksquare}$

### 명제2
vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있고, $\lambda$가 고유값이라고 할 때, 다음을 증명하여라.
$$E_\lambda = \ker(T - \lambda id)$$

**Proof**

$T(v) = \lambda v$ 조건은 $(T - \lambda id)(v) = 0$ 조건과 동치이다. 즉, $E_\lambda$는 선형변환 $(T - \lambda id)$의 null space가 된다. $\quad {_\blacksquare}$

#### 참고
Dimension Theorem에 의해 다음이 성립한다.
$$ \dim(E_\lambda) = \dim(V) - \text{rank}(T-\lambda id) $$

이를 활용하면 각 Eigen space의 dimension을 구할 수 있다.

##### 예시
$L_A : \R^3 \rightarrow \R^3$가 다음과 같이 주어져있다고 하자.
$$ A= \begin{bmatrix} 4 & 0 & 1 \\ 2 & 3 & 2 \\ 1 & 0 & 4 \end{bmatrix} $$

$\varphi_A(t)$는 3,5 두 해를 갖는다. 각각 $T-\lambda id$를 행렬로 표현하면 다음과 같다.
$$ L_A-3id = \begin{bmatrix} 1 & 0 & 1 \\ 2 & 0 & 2 \\ 1 & 0 & 1 \end{bmatrix}, L_A-5id = \begin{bmatrix} -1 & 0 & 1 \\ 2 & -2 & 2 \\ 1 & 0 & -1 \end{bmatrix} $$

독립적인 row를 살펴보면 각각 rank가 $1,2$라는 것을 알 수 있다. 

따라서 $\dim(E_3) = 2, \enspace \dim(E_5) = 1$이다.

### 명제3
Vector space $V/\F$와 $T \in \End(V)$가 있다고 하자.
$$ \text{Eigen space is a } T \text{ invariant} $$

**Proof**

$T$의 eigenvalue를 $\lambda$하고 그에 따른 eigenspace를 $E_\lambda$라 하자.

$E_\lambda$의 정의에 의해 다음이 성립한다.
$$ x \in E_\lambda \implies T(x) = \lambda x $$

이 떄, $T$는 linear map임으로 다음이 성립한다. 
$$ T(\lambda x) = \lambda T(x) = \lambda(\lambda x) \in E_\lambda $$

따라서 다음이 성립한다.
$$ T|_{E_\lambda} = \End(E_\lambda) \qed $$
