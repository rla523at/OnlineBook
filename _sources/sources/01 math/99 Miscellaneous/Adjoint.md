# Adjoint Operator
$n$차원 inner product space $V/\F$와 $T\in\End(V)$가 있다고 하자.

$x,y \in V$에 대해서 다음을 만족하는 $T^*\in\End(V)$를 $T$의 `adjoint operator`라 한다.
$$B(T(x),y) = B(x, T^*(y))$$

### 명제1
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$ \exist! T^* $$

**Proof**

[Existence]  
$\forall v \in V$마다 $v$에 의존하는 $g \in V^*$를 다음과 같이 정의하자.
$$ g : V \rightarrow \F \quad s.t. \quad x \mapsto B(T(x),v) $$

그리고 $g$의 Riesz representation을 $v_g$라 하자.

$\forall v \in V$마다 $g$가 유일하게 결정되고, Riesz representation theorem에 의해 $g$마다 $v_g$가 유일하게 결정된다.

그럼으로 $\forall v \in V$마다 $v_g$는 유일하게 결정된다.

따라서, 함수 $T^*$를 다음과 같이 정의하자.
$$ T^* : V \rightarrow V  \quad s.t. \quad v \mapsto v_g $$

보조명제1.1에 의해서 $B(T(\cdot), v) = B(\cdot, T^*(v))$이 성립한다.
보조명제1.2에 의해서 $T^* \in \End(V)$이다.

따라서, adjoint operator의 정의에 의해 다음이 성립한다.
$$ T^* \text{ is an adjoint operator} $$

그럼으로, adjoint operator가 적어도 하나 이상 존재한다. $\qed$

[Uniquness]  
$T \in \End(V)$의 adjoint operator가 $T^*_1, T^*_2$라 하자.

$\forall x,y \in V$에 대해 다음이 성립한다.
$$ \begin{aligned} & B(T(x),y) = B(x, T^*_1(y)) = B(x, T^*_2(y)) \\ \Rightarrow & \enspace B(x, T^*_1(y) - T^*_2(y)) = 0_\F \\ \Rightarrow & T^*_1(y) - T^*_2(y) = 0_V \end{aligned} $$

즉, 모든 $y \in V$에 대해서 $T^*_1(y) = T^*_2(y)$임으로, $T^*_1 = T^*_2$이고 adjoint operator는 unique하다. $\qed$

#### 보조명제1.1
다음을 증명하여라.
$$ B(T(\cdot), v) = B(\cdot, T^*(v)) $$

**Proof**

$\forall v \in V$에 대해, $T^*$의 정의에 의해 다음이 성립한다.
$$ \begin{aligned} & g(\cdot) = B(\cdot, v_g) \\\implies& B(T(\cdot), v) = B(\cdot, v_g) \\\implies& B(T(\cdot), v) = B(\cdot, T^*(v)) \qed \end{aligned} $$

#### 보조명제1.2
다음을 증명하여라.
$$ T^* \in \End(V) $$

**Proof**

$x,y_1,y_2 \in V, \enspace c \in \F$가 있을 때, 보조명제1.1에 의해 다음이 성립한다.
$$ \begin{aligned} B(x, T^*(y_1 + cy_2)) &= B(T(x), y_1 + cy_2) \\&= B(T(x), y_1) + \bar c B(T(x),y_2) \\&= B(x, T^*(y_1)) + \bar c B(x, T^*(y_2)) \\&= B(x, T^*(y_1)) + B(x, cT^*(y_2)) \\&= B(x, T^*(y_1) +cT^*(y_2)) \end{aligned} $$

따라서, $T^*(y_1 + cy_2) = T^*(y_1) + cT^*(y_2)$임으로 다음이 성립한다.
$$ T^* \in \End(V) \qed $$

### 명제2
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

$\beta$를 $V$의 orthonormal basis라 할 때, 다음을 증명하여라.
$$ \frak m_\beta^\beta(T^*) = \frak m_\beta^\beta(T)^* $$

**Proof**

$\beta$가 orthonormal basis임으로 다음이 성립한다.
$$ \begin{aligned} T(\beta_j) &= B(T(\beta_j),\beta_i)\beta_i, \\ T^*(\beta_j) &= B(T^*(\beta_j),\beta_i)\beta_i \\&= \overline{B(\beta_i, T^*(\beta_j))}\beta_i \\&= \overline{B(T(\beta_i), \beta_j)}\beta_i \end{aligned}  $$

따라서, 다음이 성립한다.
$$ \begin{aligned} \frak m_\beta^\beta(T^*) &= \begin{bmatrix} \frak m_\beta(T^*(\beta_1)) & \cdots & \frak m_\beta(T^*(\beta_n)) \end{bmatrix} \\ &= \begin{bmatrix} \overline{B(T(\beta_1), \beta_1)} & \cdots &\overline{B(T(\beta_1), \beta_n)} \\ \vdots & & \vdots \\ \overline{B(T(\beta_n), \beta_1)} & \cdots &\overline{B(T(\beta_n), \beta_n)} \end{bmatrix} \\ &= \begin{bmatrix} B(T(\beta_1), \beta_1) & \cdots &B(T(\beta_n), \beta_1) \\ \vdots & & \vdots \\ B(T(\beta_1), \beta_n) & \cdots &B(T(\beta_n), \beta_n) \end{bmatrix}^* \\ &= \frak m_\beta^\beta(T)^* \end{aligned}  $$

#### 따름명제
$A \in M_{nn}(\F)$가 있을 때, 다음을 증명하여라.
$$ (L_A)^* = L_{A^*} $$

#### 참고
matrix representation이 adjoint matrix 형태로 나타나기 때문에 adjoint operator라고 표현한다.

### 명제3
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

$x,y \in V$일 때, 다음을 증명하여라.
$$B(T^*(x),y) = B(x, T(y))$$

**Proof**

$$\begin{aligned} B(T^*(x),y) &= \overline{B(y, T^*(x))} \\ &= \overline{B(T(y), x)} \\&= B(x, T(y)) \qed \end{aligned} $$

### 명제4
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$(T_1 \circ T_2)^* = T^*_2 \circ T^*_1$$

**Proof**

$\forall x,y \in V$에 대해 다음이 성립한다. 
$$ \begin{aligned} B(x, (T_1\circ T_2)^*(y)) &= B((T_1\circ T_2)(x), y) \\&= B(T_2(x), T_1^*(y)) \\&= B(x, T_2^*(T_1^*(y))) \\&= B(x, (T_2^* \circ T_1^*)(y)) \end{aligned} $$

따라서 $(T_1 \circ T_2)^* = T^*_2 \circ T^*_1$이다. $\qed$

### 명제5
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$(T^*)^* = T$$

**Proof**

$\forall x,y \in V$에 대해 다음이 성립한다. 
$$ B(x, (T^*)^*(y)) = B(T^*(x), y) = B(x, T(y)) $$

따라서 $(T^*)^* = T$이다. $\qed$

### 명제6
$n$차원 inner product space $V/\F$가 있다고 하자.

이 때, 다음을 증명하여라.
$$(id_V)^* = id_V$$

**Proof**

$\forall x,y \in V$에 대해 다음이 성립한다. 
$$ B(x, (id_V)^*(y)) = B(id_V(x), y) = B(x, id_V(y)) $$

따라서 $(id_V)^* = id_V$이다. $\qed$

### 명제7
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$ \lambda \text{ is an eigenvalue of } T \Rightarrow \overline\lambda \text{ is an eigenvalue of } T^* $$

**Proof**

$\beta$가 $V$의 orthonormal baiss라고 하자.

$\lambda$가 $T$의 eigenvalue 임으로 다음이 성립한다.
$$ \begin{aligned} & \det(T - \lambda id) = 0 \\\implies& \det(\frak m_\beta^\beta(T - \lambda id)) = 0 \\\implies& \det(\frak m_\beta^\beta(T) - \lambda I) = 0 \\\implies& \det(\frak m_\beta^\beta(T)^* - \overline\lambda I) = 0 \\\implies& \det(\frak m_\beta^\beta(T^*) - \overline\lambda I) = 0 \\\implies& \det(\frak m_\beta^\beta(T^*- \overline\lambda id)) = 0 \\\implies& \det(T^*- \overline\lambda id) = 0 \qed \end{aligned} $$

### 명제8
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$ v \text{ is an eigenvector of } T \Rightarrow v \text{ may not an eigenvector of } T^* $$

**Proof**

$A \in M_{22}(\R)$를 다음과 같이 정의하자.
$$ A := \begin{bmatrix} 1 & 1 \\ 0 & 1  \end{bmatrix} $$

eigen vector를 계산해보면 $v = \begin{bmatrix} 0 \\ 1 \end{bmatrix}$임을 알 수 있다.

$L_A$의 adjoint operator를 $L_{A^*}$라 하면 다음이 성립한다.
$$ A^* := \begin{bmatrix} 1 & 0 \\ 1 & 1 \end{bmatrix} $$

eigen vector를 계산해보면 $w = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$임을 알 수 있다.

따라서, $L_A$의 eigen vector $v$는 $L_{A^*}$의 eigen vector가 아니다.

### 연습문제
2차원 inner product space $\mathbb P^1(\R)$가 있다고 하자.

이 때, inner product $B$는 다음과 같이 정의된다.
$$ B : \mathbb P^1(\R) \times \mathbb P^1(\R) \rightarrow \R \quad s.t. \quad (f,g) \mapsto \int_{-1}^1 fg \thinspace dx  $$

다음과 같이 정의된 $T \in \End(\mathbb P^1(\R))$이 있다고 하자.
$$ T: \mathbb P^1(\R) \rightarrow \mathbb P^1(\R) \quad s.t. \quad f \mapsto 3f + f' $$

이 떄, $T^*(4-2x)$를 구하여라.

**Solution1**

$T^*(4-2x) \in \mathbb P^1(\R)$임으로 다음과 같이 가정하자.
$$ T^*(4-2x) = ax + b $$

이 때, adjoint operator의 성질을 이용해 다음 방정식들을 유도할 수 있다.
$$ \begin{aligned} B(1, T^*(4-2x)) &= B(T(1), 4-2x) \\ \Rightarrow \quad\quad B(1, ax+b) &= B(3, 4-2x) \\ B(x, T^*(4-2x)) &= B(T(x), 4-2x) \\ \Rightarrow \quad\quad B(x, ax+b) &= B(3x + 1, 4-2x)  \end{aligned} $$

**Solution2**

$T^*(4-2x) \in \mathbb P^1(\R)$임으로 다음과 같이 가정하자.
$$ T^*(4-2x) = ax + b $$

Gram-Schmidts를 통해 $\mathbb P^1(\R)$의 orthonormal basis $\beta$를 계산하면 다음과 같다.
$$ \beta $$

$\beta$가 orthonormal basis임으로 다음이 성립한다.
$$ \begin{aligned} T^*(4-2x) &= B(T^*(4-2x), \beta_i)\beta_i \\&= B(4-2x, T(\beta_i))\beta_i  \end{aligned} $$

# Adjoint Matrix
$A \in M_{mn}(\F)$가 있다고 하자. $A$의 `adjoint` $A^*$는 다음과 같이 정의된 행렬이다.
$$ A^* \in M_{nm}(\F) \quad s.t. \quad A^*_{ij} = \overline{A_{ji}} $$

### 참고
$\F = \R$이면, adjoint는 단순히 transpose가 되며, $A = A^*$인 경우를  $A$가 symmetric하다고 한다.

만약, $\F = \Complex$이면, $A = A^*$인 경우 $A$를 Hermitian이라고 한다.

# Self-adjoint Operator
Inner product space $V/\mathbb{F}$가 있다고 하자.

다음을 만족하는 $T \in \End(V)$를 self-adjoint operator라고 한다.
$$ T = T^* $$

### 명제1
Inner product space $V/\mathbb{F}$와 self-adjoint operator $T \in \End(V)$가 있다고 하자.

이 떄, 다음을 증명하여라.
$$ \text{Every eigenvalue of } T \text{ is real.} $$

**Proof**

$T(v) = \lambda v$라고 하자.

$T$가 self-adjoint operator이기 때문에, $T$는 normal operator임으로 다음이 성립한다.
$$ T^*(v) = \bar\lambda v $$

이 떄, $T$가 self-adjoint operator임으로 다음이 성립한다.
$$ \begin{aligned} & T(v) = T^*(v) \\ \Rightarrow\enspace& \lambda v = \bar\lambda v \\ \Rightarrow\enspace& (\lambda - \bar\lambda) v =  0_V \\ \Rightarrow\enspace& \lambda - \bar\lambda =  0 \\ \Rightarrow\enspace& \lambda \in \R \quad\tiny\blacksquare \end{aligned} $$

### 명제2
Inner product space $V/\mathbb{R}$와 self-adjoint operator $T \in \End(V)$가 있다고 하자.

이 떄, 다음을 증명하여라.
$$ \varphi_T \text{ is splits.} $$

**Proof**

$\beta$를 $V$의 orthonormal basis라고 하자.

$\frak{m}_\beta^\beta(T) = A \in M_{nn}(\R)$라 할 떄, $L_{\hat{A}} \in \End(\Complex^n)$을 다음과 같이 정의하자.
$$ L_{\hat{A}} : \Complex^n \rightarrow \Complex^n \quad s.t. \quad x \mapsto Ax  $$

$L_{\hat{A}}$는 complex위에서 정의되어 다음이 성립한다.
$$ \varphi_{L_{\hat{A}}}(\lambda) \text{ is splits} $$

이 떄, 보조명제2.1에 의해서 $L_{\hat{A}}$는 self-adjoint operator임으로 명제1에 의해 다음이 성립한다.
$$ \varphi_{L_{\hat{A}}}(\lambda) = \prod_{i=1}^n (\lambda - \lambda_i) $$

$$ \text{Where, } \lambda_i \in \R $$

$\varphi_{L_{\hat{A}}}(\lambda) = \varphi_{L_{A}}(\lambda)$이고 $\varphi_{L_{\hat{A}}}(\lambda)$가 전부 실수 근으로 split 되었음으로 $\varphi_{L_{A}}(\lambda)$가 split 된다.

Q. $L_A$와 $T$의 characteristic polynomial...

#### 보조명제2.1
Inner product space $V/\mathbb{R}$와 self-adjoint operator $T \in \End(V)$가 있다고 하자.

$\beta$를 $V$의 orthonormal basis라고 하자.

$\frak{m}_\beta^\beta(T) = A \in M_{nn}(\R)$라 할 떄, $L_{\hat{A}} \in \End(\Complex^n)$을 다음과 같이 정의하자.
$$ L_{\hat{A}} : \Complex^n \rightarrow \Complex^n \quad s.t. \quad x \mapsto Ax  $$

이 떄, 다음을 증명하여라.
$$ L_{\hat{A}} \text{ is a self-adjoint operator} $$

**Proof**

$\epsilon$을 $\Complex^n$의 standarad basis라고 하면 다음이 성립한다.
$$ \begin{aligned} \frak{m}^\epsilon_\epsilon(L_{\hat{A}}) &= A \\ \frak{m}^\epsilon_\epsilon(L^*_{\hat{A}}) &= \frak{m}^\epsilon_\epsilon(L_{\hat{A}})^* \\&= A^* \end{aligned}  $$

이 떄, $T$가 self-adjoint operator임으로 다음이 성립한다.
$$ \begin{aligned} & T = T^* \\ \Rightarrow\enspace& \frak{m}_\beta^\beta(T) = \frak{m}_\beta^\beta(T^*) \\ \Rightarrow\enspace& \frak{m}_\beta^\beta(T) = \frak{m}_\beta^\beta(T)^* \\ \Rightarrow\enspace& A = A^*  \end{aligned} $$

따라서, $L_{\hat{A}}$는 self-adjoint operator이다. $\quad\tiny\blacksquare$

### 명제3
$n$차원 Inner product space $V/\mathbb{R}$와 self-adjoint operator $T \in \End(V)$가 있다고 하자.

이 떄, 다음을 증명하여라.
$$ T \text{ is a self-adjoint operator } \Leftrightarrow \exist \text{ an orthonormal basis } \beta \text{ consisting of eigenvectors} $$

**Proof**

[$\Rightarrow$]  
$T$가 self-adjoint matrix라고 하면 다음이 성립한다.
$$ \varphi_T(\lambda) \text{ is split} $$

Schur's theorem에 의해 다음이 성립한다.
$$ \exist\text{ orthonormal basis } \quad s.t. \quad \frak{m}_\beta^\beta(T) \text{ is an upper triangular matrix} $$

$T$가 self-adjoint operator임으로 다음이 성립한다.
$$ \begin{aligned} & T = T^* \\ \Rightarrow\enspace& \frak{m}_\beta^\beta(T) = \frak{m}_\beta^\beta(T^*) \\ \Rightarrow\enspace& \frak{m}_\beta^\beta(T) = \frak{m}_\beta^\beta(T)^* \\ \Rightarrow\enspace& \frak{m}_\beta^\beta(T) \text{ is a diagonal matrix } \\ \Rightarrow\enspace& \exist \text{ an orthonormal basis } \beta \text{ consisting of eigenvectors} \quad\tiny\blacksquare  \end{aligned} $$

[$\Leftarrow$]  
$\beta$가 orthonormal basis임으로 다음이 성립한다.
$$ \frak{m}_\beta^\beta(T^*) = \frak{m}_\beta^\beta(T)^* $$

Eigenvalue를 $\lambda_i$라 할 때, $\lambda_i \in \R$임으로 다음이 성립한다.
$$ \frak{m}_\beta^\beta(T)^* = \frak{m}_\beta^\beta(T) $$

따라서 다음이 성립한다.
$$ \begin{aligned} &\frak{m}_\beta^\beta(T) = \frak{m}_\beta^\beta(T^*) \\ \Rightarrow\enspace& T = T^*  \end{aligned} $$


#### 따름명제3.1
$A \in M_{nn}(\R)$가 있을 때, 다음을 증명하여라.
$$ A \text{ is a symmetric matrix} \Leftrightarrow \exist \text{ orthogonal matrix } B \quad s.t. \quad B^TAB \text{ is a diagonal matrix} $$


# Self-adjoint Matrix
$A \in M_{nn}(\F)$가 있다고 하자.

다음을 만족하는 $A$를 self-adjoint matrix라고 한다.
$$ A = A^* $$

### 참고1
$\F = \R$이면 다음이 성립한다.
$$ \text{self-adjoint matrix} \Leftrightarrow \text{symmetric matrix} $$

### 참고2
$\F = \R$일 때, 다음이 성립한다.
$$ \text{symmetric matrix} \Rightarrow \text{normal matrix} $$

하지만 그 역은 성립하지 않는다.
$$ \text{normal matrix} \nRightarrow \text{symmetric matrix} $$

예를 들어, $A$가 skew-symmetric인 경우 ,i.e. $A^T = -A$, normal matrix이지만 symmetric matrix는 아니다.

### 참고3
$\F = \Complex$이면 self-adjoint matrix를 Hermitian matrix라고 부르기도 한다.

### 참고4
$\F = \Complex$일 떄, 다음이 성립한다.
$$ \text{self-adjoint matrix} \Rightarrow \text{normal matrix} $$

### 참고5
$\F = \Complex$일 떄, 다음이 성립한다.
$$ \text{symmetric matrix} \nRightarrow \text{normal matrix} $$

예를 들어 $A = \begin{bmatrix} i&i\\i&1 \end{bmatrix}$는 symmetric matrix이지만 normal matrix는 아니다.


