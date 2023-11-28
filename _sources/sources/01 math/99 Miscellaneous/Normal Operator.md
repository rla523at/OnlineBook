# Normal Operator
$n$차원 inner product vector space $V/\F$와 $T\in\End(V)$가 있다고 하자.

이 때, $T$가 다음 성질을 만족할 경우 `normal operator`라고 한다.
$$ T \circ T^* = T^* \circ T $$

### 명제1
$n$차원 inner product vector space $V/\F$와 normal operator $T\in\End(V)$가 있다고 하자.

$x \in V$에 대해 다음을 증명하여라.
$$ \norm{T(x)} = \norm{T^*(x)} $$

**Proof**

Adjoint operator와 normal operator의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} \norm{T(x)}^2 &= B(T(x), T(X)) \\&= B(x, T^*(T(x))) \\&= B(x, T(T^*(x))) \\&= B(T^*(x), T^*(x)) \\&= \norm{T^*(x)}^2 \end{aligned} $$

### 명제2
$n$차원 inner product vector space $V/\F$와 normal operator $T \in \End(V)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$ T(v) = \lambda v \Rightarrow T^*(v) = \overline \lambda v $$

**Proof**

명제1에 의해 다음이 성립한다.
$$ \begin{aligned} \lVert T^*(v) - \overline\lambda v \rVert &= \lVert (T^* - \overline\lambda id)(v) \rVert \\&= \lVert (T - \lambda id)^*(v) \rVert \\&= \lVert (T - \lambda id)(v) \rVert \\&= 0 \end{aligned} $$

따라서 norm의 성질에 의해 다음이 성립한다.
$$ \lVert T^*(v) - \overline\lambda v \rVert \Rightarrow T^*(v) = \overline\lambda v \quad {_\blacksquare} $$

### 명제3
$n$차원 inner product vector space $V/\F$와 normal operator $T \in \End(V)$가 있다고 하자.

$\lambda_1 \neq \lambda_2$일 때, 다음을 증명하여라.
$$ T(v_1) = \lambda_1 v_1 \enspace \land \enspace T(v_2) = \lambda_2 v_2 \Rightarrow B(v_1,v_2) = 0 $$

**Proof**

명제2에 의해 다음이 성립한다.
$$ \begin{aligned} \lambda_1 B(v_1,v_2) &= B(\lambda_1v_1, v_2) \\&= B(T(v_1),v_2) \\&= B(v_1, T^*(v_2)) \\&= B(v_1, \overline\lambda_2v_2) \\&= \lambda_2 B(v_1,v_2) \end{aligned} $$

따라서, 다음이 성립한다.
$$ (\lambda_1 - \lambda_2)B(v_1,v_2) = 0 \Rightarrow B(v_1,v_2) = 0 \quad {_\blacksquare} $$

#### 참고
$T$가 normal operator이면 eigenvector끼리 수직한다.

### 명제4
$n$차원 inner product vector space $V/\F$와 normal operator $T \in \End(V)$가 있다고 하자.

$c \in \mathbb F$에 대해 다음을 증명하여라.
$$ T-cid \text{ is a normal operator }  $$

**Proof**

Normal operator의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} (T-cid) \circ (T-cid)^* &= T \circ T^* - T^* - T + c^2 id \\&= T^*\circ T - T^* - T + c^2id \\&= (T-cid)^* \circ (T-cid) \end{aligned} $$

### 명제5
$n$차원 inner product vector space $V / \Complex$와 $T \in \End(V)$가 있다고 하자.

다음을 증명하여라.
$$ T \text{ is a normal operator } \Leftrightarrow \exist \text{ an orthonormal basis } \beta \text{ consisting of eigenvectors} $$

**Proof**

[$\Rightarrow$]  
Field가 $\Complex$임으로 fundamental theorem of algebra에 의해 $\varphi(\lambda)$가 split한다.

따라서, Schur's theorem에 의해 다음이 성립한다.
$$ \exist \text{ an orthonormal basis } \beta \quad s.t. \quad \frak m^\beta_\beta(T) \text{ is an upper triangular matrix} $$

[mathmatical induction]  
$\frak m^\beta_\beta(T)$이 upper triangular matrix임으로 $\beta_1$은 eigenvector임을 알 수 있다.

$\beta_1, \cdots, \beta_n$이 eigenvector라고 가정하면 다음이 성립한다.
$$ \frak m_{\beta}^{\beta}(T) = \begin{bmatrix} \begin{array}{c | c} \begin{matrix} \lambda_1 & \\ & \ddots \\ & & \lambda_{n} \end{matrix} & A \\ \hline 0 & B \end{array} \end{bmatrix} $$

Adjoint operator의 성질 의해 다음이 성립한다.
$$ \frak m_{\beta}^{\beta}(T^*) = \begin{bmatrix} \begin{array}{c | c} \begin{matrix} \bar\lambda_1 & \\ & \ddots \\ & & \bar\lambda_{n} \end{matrix} & 0 \\ \hline A^* & B^* \end{array} \end{bmatrix} $$

이 때, $T$가 normal operator임으로 명제2에 의해 다음이 성립한다.
$$ T^*(\beta_i) = \bar\lambda_i\beta_i $$

따라서, 다음이 성립한다.
$$\begin{aligned} & \frak m_{\beta}^{\beta}(T^*) = \begin{bmatrix} \begin{array}{c | c} \begin{matrix} \bar\lambda_1 & \\ & \ddots \\ & & \bar\lambda_{n} \end{matrix} & 0 \\ \hline 0 & B^* \end{array} \end{bmatrix} \\ \Rightarrow\enspace& \frak m_{\beta}^{\beta}(T) = \begin{bmatrix} \begin{array}{c | c} \begin{matrix} \lambda_1 & \\ & \ddots \\ & & \lambda_{n} \end{matrix} & 0 \\ \hline 0 & B \end{array} \end{bmatrix} \end{aligned} $$

이 때, $B$는 upper triangular matrix임으로, $T(\beta_{n+1}) = k\beta_{n+1}$꼴이 된다.

따라서, $\beta_{n+1}$도 eigenvector가 된다. 

이로써, mathematical induction에 의해 eigenvector로 이루어진 orthonormal basis $\beta$가 존재한다.$\quad\tiny\blacksquare$

[$\Leftarrow$]  
$\beta$가 eigenvector로 이루어져 있음으로 다음이 성립한다.
$$ \frak m_\beta^\beta(T)\frak m_\beta^\beta(T^*) = \frak m_\beta^\beta(T^*)\frak m_\beta^\beta(T) $$

따라서, $V$의 임의의 basis를 $\gamma$할 떄, 다음이 성립한다.
$$ \begin{aligned} \frak m_\gamma^\gamma(T \circ T^*) &= B^{-1}\frak m_\beta^\beta(T \circ T^*)B \\&= B^{-1}\frak m_\beta^\beta(T^* \circ T)B \\&= \frak m_\gamma^\gamma(T^* \circ T) \end{aligned}  $$

임의의 basis에 대해서 표현식이 같음으로, 다음이 성립한다.
$$ T^* \circ T = T \circ T^* $$

따라서, $T$는 normal operator이다. $\quad\tiny\blacksquare$

#### 따름명제 5.1
$A \in M_{nn}(\mathbb C)$가 있을 때, 다음을 증명하여라.
$$ A \text{ is a normal matrix} \Leftrightarrow \exist \text{ unitary matrix } B \quad s.t. \quad B^*AB \text{ is a diagonal matrix} $$

**Proof**

$A$가 normal matrix이면 다음이 성립한다.
$$ L_A : \Complex^n \rightarrow \Complex^n \text{ is a normal operator.} $$

따라서 다음이 성립한다.
$$ \begin{aligned} &\exist \text{ an orthonormal basis } \beta \text{ consisting of eigenvectors.} \\ \Leftrightarrow\enspace& \frak{m}_\beta^\beta(L_A) = D \\ \Leftrightarrow\enspace& \frak{m}_\beta^\epsilon(id)\frak{m}_\epsilon^\epsilon(L_A)\frak{m}_\epsilon^\beta(id) = D \\ \Leftrightarrow\enspace& B^{-1}AB = D  \end{aligned}  $$

이 때, $\beta$가 orthonormal basis로 이루어져 있음으로, $B$는 unitary matrix이다. 따라서 다음이 성립한다.
$$ \begin{aligned} & B^{-1}AB = D \\ \Leftrightarrow\enspace& B^*AB = D  \end{aligned}  $$

# Normal Matrix
$A \in M_{nn}(\mathbb F)$가 있다고 하자.

이 떄, $A$가 다음 성질을 만족할 경우 `normal matrix`라고 한다.
$$ AA^* = A^*A $$



# Unitary Matrix
$A \in M_{nn}(\mathbb C)$가 있다고 하자.

이 떄, $A$가 다음 성질을 만족할 경우 `unitary matrix`라고 한다.
$$ A^{-1} = A^* $$

### 명제1
$A \in M_{nn}(\mathbb C)$가 있다고 하자.

이 떄, 다음을 증명하여라.
$$ A \text{ is an unitary matrix} \Leftrightarrow \{ A_{*1},\cdots,A_{*n}  \} \text{ is an orthonormal basis} $$

**Proof**

[$\Rightarrow$]  
Unitary matrix 성질에 의해 다음이 성립한다.
$$ \begin{aligned} & (A_{*i})^* A_{*j} = \delta_{ij} \\ \Rightarrow\enspace& B(A_{*i}, A_{*j}) = \delta_{ij} \end{aligned}  $$

따라서, $\{ A_{*1},\cdots,A_{*n} \}$는 orthonormal basis이다. $\quad\tiny\blacksquare$

[$\Leftarrow$]  
Orthonormal basis의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} & B(A_{*i}, A_{*j}) = \delta_{ij} \\ \Rightarrow\enspace& (A_{*i})^* A_{*j} = \delta_{ij} \\ \Rightarrow\enspace& A^*A = I \end{aligned}  $$

따라서, 역행렬의 성질에 의해 $A^{-1} = A^*$임으로 $A$는 unitary matrix이다. $\quad\tiny\blacksquare$

### 명제2
$A \in M_{nn}(\mathbb C)$가 있다고 하자.

이 떄, 다음을 증명하여라.
$$ A \text{ is an unitary matrix} \Leftrightarrow \{ A_{1*},\cdots,A_{n*}  \} \text{ is an orthonormal basis} $$

**Proof**

[$\Rightarrow$]  
Unitary matrix 성질에 의해 다음이 성립한다.
$$ \begin{aligned} & A_{i*} (A_{j*})^* = \delta_{ij} \\ \Rightarrow\enspace& B(A_{i*}, A_{j*}) = \delta_{ij} \end{aligned}  $$

따라서, $\{ A_{1*},\cdots,A_{n*} \}$는 orthonormal basis이다. $\quad\tiny\blacksquare$

[$\Leftarrow$]  
Orthonormal basis의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} & B(A_{i*}, A_{j*}) = \delta_{ij} \\ \Rightarrow\enspace& A_{*i} (A_{*j})^* = \delta_{ij} \\ \Rightarrow\enspace& AA^* = I \end{aligned}  $$

따라서, 역행렬의 성질에 의해 $A^{-1} = A^*$임으로 $A$는 unitary matrix이다. $\quad\tiny\blacksquare$

