# Matrix representation
## Vector
$n$ 차원 vector space $V / \mathbb F$와 기저 $\beta$가 있다고 하자.

$V$의 임의의 element를 $v$라고 하고, $v$의 coordinate를 $(a_1,\cdots,a_n)\in\F^n$이라고 하자.

basis가 결정되었을 때, $v$의 coordinate는 유일함으로 $v$의 $\beta$에 대한 `행렬표현(matrix representation)` 을 다음과 같이 정의하자.

$$ [v]_\beta := \begin{bmatrix} a^1 \\ \vdots \\ a^n \end{bmatrix} \in M_{n1}(\mathbb F) $$

### 명제1
$n$차원 vector space $V/ \mathbb F$와 기저 $\beta$가 있다고 하자.

이 떄, 함수 $\frak{m}_\beta$를 다음과 같이 정의하자.

$$ \frak{m}_\beta : V \rightarrow M_{n1}(\mathbb F) \quad s.t. \quad v \mapsto [v]_\beta $$  

이 때, 다음을 증명하여라.

$$ \frak m_\beta \text{ is a vector space isomorphism} $$

**Proof**

[$m_\beta \in L(V; \mathbb M_{n1}(\mathbb F))$]  
$v_1 = a_i\beta_i, \enspace v_2 = b_i\beta_i \in V, \enspace c \in \mathbb F$가 있을 때 다음이 성립한다.


$$ \frak m_\beta(cv_1 + v_2) = \begin{bmatrix} ca^1 + b_1 \\ \vdots \\ ca^n+ b_n \end{bmatrix} = c\begin{bmatrix} a^1 \\ \vdots \\ a^n \end{bmatrix} + \begin{bmatrix} b_1 \\ \vdots \\ b_n \end{bmatrix} = c \frak m_\beta(v_1) + \frak m_\beta(v_2) $$

따라서, $m_\beta \in L(V; \mathbb M_{n1}(\mathbb F))$이다.

[$m_\beta$ is bijective]  
$\ker(m_\beta) = \{ 0_V \}$이고 $\dim(V) = \dim(\mathbb M_{n1}(\mathbb F))$임으로 dimension theorem에 의해 $m_\beta$는 bijective이다.$\quad\tiny\blacksquare$

## Linear Map
$n,m$차원 벡터공간 $V,W / \F$와 $T \in L(V;W)$가 있다고 하자.

$V,W$ 각각의 기저를 $\beta, \gamma$라고 할 때, $V$의 임의의 elemnt를 $v$와 $T(v)$를 다음과 같이 표현하자.

$$ v = a_i\beta_i, \enspace T(v) = b_j\gamma_j $$

이 떄, $T$는 linear map임으로 다음이 성립한다.

$$ \begin{aligned} T(v) &= T(a_i\beta_i) \\&= a_iT(\beta_i) \end{aligned} $$

$T(\beta_i) = A_{ji}\gamma_j$라고 하면 다음이 성립한다.

$$ T(v) = a_iA_{ji}\gamma_j = b_j\gamma_j $$

이를 행렬 형태로 나타내면 다음과 같다.

$$ \begin{bmatrix} b_1 \\ \vdots \\ b_m \end{bmatrix} = \begin{bmatrix} A_{11} & \cdots & A_{1n} \\ \vdots & \ddots & \vdots \\ A_{m1} & \cdots & A_{mn} \end{bmatrix} \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} $$

이 떄, $\begin{bmatrix} b_1 \\ \vdots \\ b_m \end{bmatrix}= [T(v)]_\gamma$이고 $\begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} = [v]_\beta$임으로, $T$의 $\beta,\gamma$에 대한 `행렬표현(matrix representation)`을 다음과 같이 정의하자.

$$ [T]^\gamma_\beta := \begin{bmatrix} A_{11} & \cdots & A_{1n} \\ \vdots & \ddots & \vdots \\ A_{m1} & \cdots & A_{mn} \end{bmatrix} $$

그러면 위에 행렬 형태는 다음과 같이 간단하게 표현할 수 있다.

$$ [T(v)]_\gamma = [T]^\gamma_\beta[v]_\beta $$

그리고 $T(\beta_i) = A_{ji}\gamma_j$임으로 $[T]^\gamma_\beta$는 다음과 같이 표현이 가능하다.

$$ [T]^\gamma_\beta = \begin{bmatrix} [T(\beta_1)]_\gamma & \cdots & [T(\beta_n)]_\gamma  \end{bmatrix} $$



### Remark
#### 1 
$L(V;W)$의 $\beta,\gamma$에 대한 basis를 $f^i_j, i=1,\cdots,n, j=1,\cdots,m$라고 하자.

$T \in L(V;W)$가 있을 때, $\frak{m}^\gamma_\beta(T) = A$일 때, 다음이 성립한다.

$$ T = A_{ji}f^i_j $$

이 때, $f^i_j$는 $i$ coordinate를 $j$ coordinate로 보내주는 함수임으로 $[f^i_j]_\beta^\gamma = B$라고 하면 다음이 성립한다.

$$ B_{mn} = \begin{cases} 1 & (m,n) = (j,i) \\ 0 & else \end{cases} $$

따라서, 다음이 성립한다.

$$ [T]^\gamma_\beta = A_{ji}[f^i_j]^\gamma_\beta = A $$

#### 2
행렬 곱 형태를 다음과 같이 바꿔보자.

$$ \begin{bmatrix} b_1 \\ \vdots \\ b_m \end{bmatrix} = \begin{bmatrix} A_{11} \\ \vdots \\ A_{m1} \end{bmatrix} a_1 + \cdots $$

$a_1$ 이라는 coordinate에 $A_{j1}$만큼 scailing 해준 만큼 $\gamma_j$에 해당하는 coordinate에 영향을 준다.

따라서, $A_{ji}$는 $i$ coordinate를 $j$ coordinate로 변환할 때, 얼마나 scailing 해줄 것인지를 결정하는 값이다.

#### 3
함수 $L_A$를 다음과 같이 정의하자.

$$ L_A : M_{n1} \rightarrow M_{m1} \st x \mapsto Ax $$

이 떄, $L_A$의 의미에 대해서 조금더 살펴보자.

임의의 $n,m$차원 vector space를 $V,W$와 각각의 기저를 $\beta,\gamma$라고 하면, $\frak{m}_{\beta,\gamma}$는 vector space isomorphism임으로 다음과 같은 도식을 그릴 수 있다.

$$ \begin{CD} V @>T>> W \\ @V{\frak m_\beta}VV @VV{\frak m_\gamma}V \\ M_{n1} @>>L_A> M_{m1} \end{CD} $$

따라서, $L_A$는 3가지 선형변환의 합성으로 표현할 수 있다.

$$ L_A = \frak{m}_\gamma \circ T \circ \frak{m}_\beta^{-1} $$

$\epsilon^n, \epsilon^m$을 $M_{n1},M_{m1}$의 standarad basis라고 할 때, 명제3에 의해 다음이 성립한다.

$$[L_A]^{\epsilon^m}_{\epsilon^n} = [\frak{m}_\gamma]^{\epsilon^m}_{\gamma} [T]^\gamma_\beta [\frak{m}_\beta^{-1}]^{\beta}_{\epsilon^n}$$

이 때, $[\frak{m}_\gamma]^{\epsilon^m}_{\gamma}$와 $[\frak{m}_\beta^{-1}]^{\beta}_{\epsilon^n}$는 다음과 같다.

$$ [\frak{m}_\gamma]^{\epsilon^m}_{\gamma} = \begin{bmatrix} [\frak{m}_\gamma(\gamma_1)]_{\epsilon^m} & \cdots & [\frak{m}_\gamma(\gamma_m)]_{\epsilon^m} \end{bmatrix}  = I_m \\ [\frak{m}_\beta^{-1}]^{\beta}_{\epsilon^n} = \begin{bmatrix} [\frak{m}_\beta^{-1}(\epsilon^n_1)]_{\beta} & \cdots & [\frak{m}_\beta^{-1}(\epsilon^n_n)]_{\beta} \end{bmatrix} = I_n $$

따라서 다음이 성립한다.

$$ [L_A]^{\epsilon^m}_{\epsilon^n} = [T]^\gamma_\beta $$

즉, $n\times 1$행렬을 $m \times 1$ 행렬로 옮겨주는 $m \times n$행렬이 있다면, 이에 상응하는 vector space간의 linear map이 있다는 얘기이다.

### 명제1
차원이 각 각 $n,m$인 벡터 공간 $V,W/ \mathbb F$와 각각의 기저 $\beta, \gamma$, $T \in L(V;W)$가 있다고 하자.

$\frak m_\beta^\gamma$은 다음과 같이 정의된 함수이다.

$$ \frak m_\beta^\gamma : L(V;W) \rightarrow M_{mn}(\mathbb F) \quad s.t. \quad T \mapsto [T]_\beta^\gamma $$  

이 때, 다음을 증명하여라.

$$ \frak m_\beta^\gamma \text{ is well defined.} $$

**Proof**

$L(V;W)$의 임의의 element를 $T$와 $T_2=T$인 $T_2 \in L(V;W)$가 있다고 하자.

그러면 $T(\beta_i) = A_{ji}\gamma_j$면 $T_2(\beta_i) = T(\beta_i) = A_{ji}\gamma_j$임으로 다음이 성립한다.

$$ [T_2]^\gamma_\beta = \begin{bmatrix} [T_2(\beta_1)]_\gamma & \cdots & [T_2(\beta_n)]_\gamma  \end{bmatrix}  = \begin{bmatrix} [T(\beta_1)]_\gamma & \cdots & [T(\beta_n)]_\gamma \end{bmatrix} = [T]^\gamma_\beta $$

따라서, 다음이 성립한다.

$$T = T_2 \implies \mathfrak{m}^\gamma_\beta(T) = \mathfrak{m}^\gamma_\beta(T_2) \qed$$


### 명제2
차원이 각 각 $n,m$인 벡터 공간 $V,W/ \mathbb F$와 각각의 기저 $\beta, \gamma$, $T \in L(V;W)$가 있다고 하자.

$\frak m_\beta^\gamma$은 다음과 같이 정의된 함수이다.

$$ \frak m_\beta^\gamma : L(V;W) \rightarrow M_{mn}(\mathbb F) \quad s.t. \quad T \mapsto [T]_\beta^\gamma $$  

이 때, 다음을 증명하여라.

$$ \frak m_\beta^\gamma \text{ is a vectorspace isomorphism} $$

**proof**   

### 명제3
차원이 각 각 $n,m,k$인 벡터공간 $V,W,Z/ \mathbb F$와 각 각의 기저 $\alpha,\beta,\gamma$가 있다고 하자.

$T_1 \in L(V;W), \enspace T_2 \in L(W;Z)$가 있을때, 다음을 증명하여라.

$$ [T_2 \circ T_1]^\gamma_\alpha = [T_2]^\gamma_\beta [T_1]^\beta_\alpha $$


**proof**

$V$의 임의의 element를 $v$라고 할 때, $v,T_1(v), (T_2\circ T_1)(v)$를 다음과 같이 표현하자.

$$ \begin{aligned} v &= a_i\alpha_i \\ T_1(v) &= b_j\beta_j \\ (T_2\circ T_1)(v) &= c_k\gamma_k \end{aligned} $$

$[T_1]_\alpha^\beta = A, [T_2]_\beta^\gamma =B, [T_2 \circ T_1]_\alpha^\gamma = C$라고 하면 다음이 성립한다.

$$ \begin{aligned} T_1(v) &\implies b_j = A_{ji}a_i \\ T_2(T_1(v)) &\implies c_k = B_{kj} b_j \implies c_k = B_{kj}A_{ji} a_i \\ (T_2\circ T_1)(v) &\implies c_k = C_{ki}a_i  \end{aligned} $$

따라서, 다음이 성립한다.

$$ C_{ki} = B_{kj}A_{ji} $$

행렬의 곱의 성질에 의해 다음이 성립한다.

$$ [T_2 \circ T_1]^\gamma_\alpha = [T_2]^\gamma_\beta [T_1]^\beta_\alpha \qed $$


### 명제4
$n$ 차원 vector space $V / \mathbb F$와 $U,W \le V$ 그리고 $T \in \text{End}(V)$가 있다고 하자.

이 때, $V = U \oplus W$이고 $U,W$가 $T$ invariant일 때 $V,U,W$ 각각의 기저 $\alpha, \beta, \gamma$에 대해 다음을 증명하여라.

$$ \frak m_\alpha^\alpha(T) = \begin{bmatrix} \begin{array}{c | c} \frak m_\beta^\beta(T|_U) & 0 \\ \hline  0 & \frak m_\gamma^\gamma(T|_W) \end{array} \end{bmatrix} $$

**Proof**

$\dim(U) =k$라고 하자.

$V = U \oplus W$임으로 다음이 성립한다.

$$ \alpha = \beta \cup \gamma = \{ \beta_1, \cdots, \beta_k, \gamma_1, \cdots, \gamma_{n-k} \} $$ 

이 떄, $U,W$가 $T$ invariant임으로 다음이 성립한다.

$$ \begin{aligned} \frak{m}_\alpha^\alpha(T) &= \begin{bmatrix} \frak{m}_\alpha (T(\beta_1)) & \cdots & \frak{m}_\alpha (T(\beta_k)) & \frak{m}_\alpha (T(\gamma_1)) & \cdots & \frak{m}_\alpha (T(\gamma_{n-k})) \end{bmatrix} \\&= \begin{bmatrix} \frak{m}_\beta (T(\beta_1)) & \cdots & \frak{m}_\beta (T(\beta_k)) & \frak{m}_\beta (T(\gamma_1)) & \cdots & \frak{m}_\beta (T(\gamma_{n-k})) \\ \frak{m}_\gamma (T(\beta_1)) & \cdots & \frak{m}_\gamma (T(\beta_k)) & \frak{m}_\gamma (T(\gamma_1)) & \cdots & \frak{m}_\gamma (T(\gamma_{n-k})) \end{bmatrix} \\&= \begin{bmatrix} \frak{m}_\beta^\beta (T) & \frak{m}_\gamma^\beta (T) \\ \frak{m}_\beta^\gamma (T) & \frak{m}_\gamma^\gamma (T) \end{bmatrix} \\&= \begin{bmatrix} \frak{m}_\beta^\beta (T|_U) & 0 \\ 0 & \frak{m}_\gamma^\gamma (T|_W) \end{bmatrix} \\&= \begin{bmatrix} \begin{array}{c | c} \frak{m}_\beta^\beta(T|_U) & 0 \\ \hline  0 & \frak{m}_\gamma^\gamma(T|_W) \end{array} \end{bmatrix} \quad\tiny\blacksquare \end{aligned} $$