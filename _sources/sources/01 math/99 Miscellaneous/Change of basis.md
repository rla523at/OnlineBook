# Basis Change Matrix
## 정의
$n$차원 벡터공간 $V/ \mathbb F$와 두 기저 $\beta, \gamma$가 있다고 하자.

$\gamma_i \in V$이기 때문에 $\beta$로 $\gamma_i$를 표현하면 유일한 계수들 $B_i^j, \enspace j = 1, \cdots, n$로 표현된다.

$$ \gamma_i = B^j_i \beta_j $$

이를 행렬형태로 나타내면 다음과 같다.

$$ \begin{bmatrix} \gamma_1 & \cdots & \gamma_n \end{bmatrix} = \begin{bmatrix} \beta_1 & \cdots & \beta_n \end{bmatrix} \begin{bmatrix} B^1_1 & \cdots & B^1_n \\ \vdots&&\vdots \\ B^n_1 & \cdots & B^n_n \end{bmatrix} $$

행렬 $B$에 의해 $\beta$가 $\gamma$로 변환되고 있음으로, $B$를 $\beta$에서 $\gamma$로 변환하는 `기저 변환 행렬(basis change matrix)`이라고 한다.

### 참고
$B_{*i}$는 $\gamma_i$의 $\beta$에 대한 coordinate임으로 다음이 성립한다.

$$ \begin{aligned} B &= \begin{bmatrix} \mathfrak{m}_\beta(\gamma_1) &\cdots& \mathfrak{m}_\beta(\gamma_n) \end{bmatrix} \\&= \begin{bmatrix} \mathfrak{m}_\beta(id(\gamma_1)) &\cdots& \mathfrak{m}_\beta(id(\gamma_n)) \end{bmatrix} \end{aligned} $$

따라서, $B = \mathfrak{m}^\beta_\gamma(id)$이다.


# Coordinate Change
$n$차원 벡터공간 $V/ \mathbb F$와 두 기저 $\beta, \gamma$가 있을 때,  $v = a^i \beta_i = b^i \gamma_i \in V$라 하자.

$v= id_V(v)$임으로 다음 관계식이 성립한다.

$$  b^i = C^i_j a^j $$


$$ \text{Where, } [id_V]_\beta^\gamma = (C^1_1, \cdots, C^n_1, \cdots, C^1_n, \cdots, C^n_n) $$

이 때,  $[id_V]_\beta^\gamma$의 행렬표현 $\mathbf C$는 다음과 같다.

$$ \mathbf C := \begin{bmatrix} C^1_1 & \cdots & C^1_n \\ \vdots & \ddots &\vdots \\ C^n_1 & \cdots & C^n_n  \end{bmatrix} $$

$b^i = C^i_j a^j$를 행렬형태로 나타내면 다음과 같다.

$$ \begin{bmatrix} b_1 \\ \vdots \\ b_n \end{bmatrix} = \mathbf C \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} $$

이를 행렬 $\mathbf C$에 의해 $\beta$에 의한 좌표가 $\gamma$에 의한 좌표로 변환된다고 볼 수 있다.

따라서, $\mathbf C$를 $\beta$에 의한 좌표를 $\gamma$에 의한 좌표로 변환하는 `좌표 변환 행렬(coordinate change matrix)`이라고 한다.

### 참고
변수변환이 $x = Pv$형태로 이루어졌다고 하자. 

이 때, $x$는 기저 $\beta$에 대한 coordinate이고 $v$는 기저 $\gamma$에 대한 coordinate로 보면 $P$는 $\beta \rightarrow \gamma$인  coordinate change matrix이다.

### 명제1
$n$차원 벡터공간 $V/F$와 $V$의 두 기저 $\beta,\gamma$가 있을 때, $\beta \rightarrow \gamma$기저변환 행렬을 $\mathbf B$ 좌표변환 행렬을 $\mathbf C$라 할 때, 다음을 증명하여라.

$$ \mathbf C = \mathbf B^{-1} $$

**Proof**

함수 $f$를 $f := id_V \circ id_V$로 정의하면 다음이 성립한다.

$$ [f]_\beta = [id_V]^\beta_\gamma [id_V]^\gamma_\beta \land [f]_\gamma = [id_V]^\gamma_\beta [id_V]^\beta_\gamma $$

이 때,  $[f]_\beta = [id_V]_\beta = [f]_\gamma = [id_V]_\gamma = I_n$임으로 $[id_V]^\gamma_\beta = \left( [id_V]^\beta_\gamma \right)^{-1}$이다. 

따라서, $\mathbf C = \mathbf B^{-1}$이다. $\quad {_\blacksquare}$

### 명제2
모든 `가역행렬(invertible matrix)`은 좌표변환행렬임을 증명하여라.

**Proof**

임의의 가역행렬을 $A \in \mathbb F^{n \times n}$라 하자.   
가역행렬의 모든 열 벡터는 선형 독립임으로 열 벡터의 집합 $\beta=[A_{*1}, \cdots, A_{*n}]$은 $\mathbb F^n$의 기저이다.  
그러면 $A = [id_{\mathbb F^n}]_\beta^{\epsilon_n}$이다. 즉, $A$는 $\beta$에 대한 좌표를 기준 기저$\epsilon_n$에 대한 좌표로 바꿔주는 좌표변환행렬이다. $\quad {_\blacksquare}$
