# Similar
$n$차원 벡터공간 $V/ \mathbb F$와 $V$의 두 기저 $\beta,\gamma$가 있다고 하자.

$T \in \text{End}(V)$가 있을 때, $T = id_V \circ T \circ id_V$임으로 다음이 성립한다.
$$ \frak m^\gamma_\gamma(T) = \frak m^\gamma_\beta(id_V) \frak m^\beta_\beta(T) \frak m_\gamma^\beta(id_V)$$

기저 $\beta$를 $\gamma$로 바꿔주는 기저변환행렬을 $\mathbf B$라고 하면 다음과 같다.
$$ \frak m^\gamma_\gamma(T) = \mathbf B^{-1} \frak m^\beta_\beta(T) \mathbf B$$

두 행렬 $\frak m^\gamma_\gamma(T), \frak m^\beta_\beta(T)$는 동일한 선형변환인 $T$를 서로 다른 기저에서 표현한것으로, 동일한 선형변환 $T$를 표현한다는 맥락에서 두 행렬은 '닮았다'라고 볼 수 있다.

따라서, $M,N \in M_{nn}(\mathbb F)$가 있을 때, $M = B^{-1}NB$을 만족하는 가역행렬 $B \in M_{nn}(\mathbb F)$가 존재하는 경우 $M$이 $N$과 `닮았다(similar)`라고 하고 $M \sim N$라고 표기한다.

### 명제1
$M,N \in M_{nn}(\mathbb F)$가 있을 때, 다음을 증명하여라.
$$ M \sim N \Leftrightarrow \exist \beta \quad s.t. \quad \frak m^\beta_\beta(L_N) = M $$
$$ \text{Where, } L_N : \mathbb F^n \rightarrow \mathbb F^n \quad s.t. \quad x \mapsto Nx $$

**Proof**

[$\Rightarrow$]  
$$ \begin{aligned} & M \sim N \\ \Rightarrow \enspace & M = B^{-1}NB = \frak m^\beta_\epsilon(id_V) \frak m^\epsilon_\epsilon(L_N) \frak m_\beta^\epsilon(id_V) = \frak m_\beta^\beta(L_N)\\ & \text{Where, } \beta_i \text{ is i-th column vector of B} \quad {_\blacksquare}  \end{aligned} $$

[$\Leftarrow$]  
$$ \begin{aligned}  M &= m^\beta_\beta(L_N) \\ & = \frak m^\beta_\epsilon(id_V) \frak m^\epsilon_\epsilon(L_N) \frak m_\beta^\epsilon(id_V) \\ &= B^{-1}NB \quad {_\blacksquare}  \end{aligned} $$

### 명제2
$M,N \in M_{nn}(\mathbb F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.
$$ \det(M) = \det(N) $$

**Proof**

$$ \det(M) = \det(B^{-1}NB) = \det(B^{-1})\det(N)\det(B) = \det({B^{-1}B})\det(N) = \det(N) \quad {_\blacksquare} $$

### 명제3
$M,N \in M_{nn}(\mathbb F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.
$$ \text{tr}(M) = \text{tr}(N) $$

**Proof**

$$ \mathrm{tr}(M) = \mathrm{tr}(B^{-1}NB) = \mathrm{tr}(BB^{-1}N) = \mathrm{tr}(N) \quad (\because \mathrm{tr}(M_1M_2) = \mathrm{tr}(M_2M_1)) \quad {_\blacksquare}  $$

### 명제4
$M,N \in M_{nn}(\mathbb F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.
$$ \text{tr}(M^2) = \text{tr}(N^2) $$

**Proof**

$$ \mathrm{tr}(M^2) = \mathrm{tr}(B^{-1}NBB^{-1}NB) = \mathrm{tr}(B^{-1}N^2B) = \mathrm{tr}(N^2)  \quad {_\blacksquare}  $$

### 명제5
$M,N \in M_{nn}(\mathbb F)$가 있을 때, $M \sim N$라 하자.

$c_1,c_2 \in \mathbb F$에 대해, 다음을 증명하여라.
$$ c_1M + c_2I \sim c_1N + c_2I $$

**Proof**

$M \sim N$이기 때문에 다음을 만족하는 $B \in M_{nn}(\mathbb F)$가 존재한다.
$$ M = B^{-1}NB $$

따라서, 다음이 성립한다.
$$\begin{aligned} B^{-1}(c_1N + c_2I)B &= c_1B^{-1}NB + c_2I \\&= c_1M + c_2I \quad {_\blacksquare} \end{aligned} $$

#### 따름명제
$X_m = \text{tr}(X) / 3$로 정의할 떄, 다음을 증명하여라.
$$ M - M_mI \sim N - N_m I $$

**Proof**

$M \sim N$이기 때문에 다음이 성립한다.
$$ M_m = N_m = k $$

따라서, 명제5에 의해 성립한다. $\quad {_\blacksquare}$