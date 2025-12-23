# Change of Basis and Coordinate Matrix
## Change of Basis(Motivation)
$n$차원 벡터공간 $V/ \mathbb F$와 두 기저 $\beta, \gamma$가 있다고 하자.

$\gamma_i \in V$이기 때문에 $\beta$로 $\gamma_i$를 표현하면 유일한 계수들 $B_{ji}, \enspace j = 1, \cdots, n$로 표현된다.

$$ \gamma_i = B_{ji} \beta_j $$

이를 행렬형태로 나타내면 다음과 같다.

$$ \begin{bmatrix} \gamma_1 & \cdots & \gamma_n \end{bmatrix} = \begin{bmatrix} \beta_1 & \cdots & \beta_n \end{bmatrix} \begin{bmatrix} B_{11} & \cdots & B_{1n} \\ \vdots&&\vdots \\ B_{n1} & \cdots & B_{nn} \end{bmatrix} $$

이는 행렬 $B$에 의해 $\beta$가 $\gamma$로 변환되고 있다고 볼 수 있다.

그리고 $B$의 $i$ column은 $[\gamma_i]_\beta$임으로 $B = [id]^\beta_\gamma$이다.

## Change of Basis(Definition)
$n$차원 벡터공간 $V/ \mathbb F$와 두 기저 $\beta, \gamma$가 있다고 하자.

$V$위의 $\beta$에서 $\gamma$로 변환하는 `기저 변환 행렬(change of basis matrix)` $B$를 다음과 같이 정의한다.

$$ B := [id]^\beta_\gamma $$

### 명제1
$n$차원 vector space $V/\F$와 기저 $\beta, \gamma$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ ([id]_\gamma^\beta)^{-1} = [id]_\beta^\gamma  $$

**Proof**

linear map의 matrix representation의 성질에 의해 다음이 성립한다.

$$ 
\begin{gather*} 
[id\circ id]^\beta_\beta = [id]^\beta_\gamma[id]^\gamma_\beta = I_n \\ 
[id\circ id]^\gamma_\gamma = [id]^\gamma_\beta[id]^\beta_\gamma = I_n  
\end{gather*} 
$$

따라서, $[id]^\beta_\gamma$와 $[id]^\gamma_\beta$는 역행렬 관계에 있다. $\qed$

#### 참고1
1. 모든 change of basis matrix는 invertible matrix이다.
2. $\beta$를 $\gamma$로 바꾸는 change of basis matrix와 $\gamma$를 $\beta$로 바꾸는 change of basis matrix는 역행렬 관계를 갖는다.

#### 참고2
$\R^n$ 에서 임의의 basis $\beta,\gamma$ 가 주어졌을 때, change of basis matrix 를 구체적으로 계산하는 방법을 생각해보자.

$[id]^\beta_\gamma = [ [\gamma_1]_\beta, \cdots, [\gamma_n]_\beta]$ 인데 $[\gamma_i]_\beta$ 를 구하는게 쉽지 않다. 따라서, 다음과 같이 우회한다.

$$ [id]^\beta_\gamma = [id \circ id]^\beta_\gamma = [id]^\beta_\epsilon[id]^\epsilon_\gamma = ([id]^\epsilon_\beta)^{-1}[id]^\epsilon_\gamma $$

$[id]^\epsilon_{\beta,\gamma}$ 는 쉽게 계산할 수 있음으로 chagne of basis matrix 또한 쉽게 계산할 수 있다.

### 명제2
다음을 증명하여라.

$$ \text{Every invertible matrix is a change of basis matrix of } \F^n $$

**Proof**

임의의 가역행렬을 $A \in M_{n \times n}(\F)$라 하자.

$A$의 $i$ column을 $A_{*i}$라고 할 때, $A$는 invertible matrix임으로  $\beta=[A_{*1}, \cdots, A_{*n}]$은 $\mathbb F^n$의 기저이다.  

그러면 $A = [id]_\beta^{\epsilon}$이다. 즉, $A$는 $\epsilon$을 $\beta$로 바꿔주는 change of basis matrix이다. $\qed$

#### 보조정리
임의의 가역행렬을 $A \in M_{n \times n}(\F)$라 하자.

$A$의 column의 집합을 $\beta$라고 할 때, 다음을 증명하여라.

$$ \beta \text{ is a basis of } \F^n $$

**Proof**

$\beta$가 $n$개의 element를 갖음으로, $\beta$가 linearly independent set임만 보이면 된다.

$A$의 $i$번째 column을 $A_{*i}$라고 할 때, 어떤 $a_i \in \F$에 대해서 다음이 성립한다고 하자.

$$ a_iA_{*i} = 0_{\F^n} $$

이를 행렬 형태로 다시 쓰면 다음과 같다.

$$ \begin{bmatrix} A_{*1} & \cdots & A_{*n} \end{bmatrix} \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} = \begin{bmatrix} 0_\F \\ \vdots \\ 0_\F \end{bmatrix} $$

이 때, $A$가 invertible matrix임으로 다음이 성립한다.

$$ a_i = 0_\F $$

따라서, $\beta$는 linearly independet set임으로 $\F^n$의 basis이다. $\qed$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1058555/do-the-columns-of-an-invertible-n-times-n-matrix-form-a-basis-for-mathbb-r)

## Change of Coordinate(Motivation)
$n$차원 벡터공간 $V/ \mathbb F$와 두 기저 $\beta, \gamma$가 있다고 하자.

$V$의 임의의 element를 $v$를  $v = a^i \beta_i = b^i \gamma_i$라 하자.

$\beta$를 $\gamma$로 바꾸는 change of basis matrix를 $B$라고 하면 다음이 성립한다.

$$ b^i\gamma_i = b^iB_{ji}\beta_j \implies a^i = B_{ij} b^j $$

이를 행렬 형태로 나타내면 다음과 같다.

$$ \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} = B \begin{bmatrix} b_1 \\ \vdots \\ b_n \end{bmatrix} \implies \begin{bmatrix} b_1 \\ \vdots \\ b_n \end{bmatrix} = B^{-1} \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} $$

이는 행렬 $B^{-1}$에 의해 $\beta$에 의한 좌표가 $\gamma$에 의한 좌표로 변환된다고 볼 수 있다.

그리고 $\beta \rightarrow \gamma$일 때, $B = [id]^\beta_\gamma$임으로  $B^{-1} = [id]^\gamma_\beta$이다.

## Change of Coordinate(Definition)
$n$차원 벡터공간 $V/ \mathbb F$와 두 기저 $\beta, \gamma$가 있다고 하자.

$V$의 임의의 element를 $v$라고 할 때, $[v]_\beta$를 $[v] _\gamma$로 변환시켜주는 `좌표 변환 행렬(change of coordinate matrix)` $C$를 다음과 같이 정의한다.

$$ C:= [id]^\gamma_\beta $$

그리고 $[v]_\beta$와 $[v]_\gamma$ 그리고 $C$는 다음 관계를 만족한다.

$$ [v]_\gamma = C [v]_\beta  $$

## Similar(Motivation)
$n$차원 벡터공간 $V/ \mathbb F$와 $V$의 임의의 element를 $v$가 있다고 하자.

$v = id(v)$임으로 $V$의 임의의 두 기저를 $\beta, \gamma$라고 할 때, linear map의 matrix representation에 의해 다음이 성립한다.

$$ [v]_\gamma = [id]^\gamma_\beta[v]_\beta $$

즉, 동일한 vector의 matrix representation간에 어떤 관계를 맺는지 알려준다.

그렇다면 $T \in \End(V)$가 있을 때, 위와 비슷한 방식으로 동일한 linear map의 matrix representation간에 어떤 관계를 맺는 지 살펴보자.

$T = id \circ T \circ id$임으로, linear map의 matrix representation에 의해 다음이 성립한다.

$$ [T]^\gamma_\gamma = [id]^\gamma_\beta[T]^\beta_\beta[id]^\beta_\gamma $$

$\beta \rightarrow \gamma$일 때, change of basis matrix $B = [id]^\beta_\gamma$임으로 다음이 성립한다.

$$ [T]^\gamma_\gamma = B^{-1}[T]^\beta_\beta B $$

$[T]^\gamma_\gamma$와 $[T]^\beta_\beta$는 서로 다른 matrix이지만, $T$라는 동일한 linear map을 표현하는 matrix representation이라는 공통점을 가지고 있다. 따라서, 두 matrix는 완전히 같지는 않지만 $T$를 표현하는 matrix라는 점에서 닮아 있다.

## Similar(Defintion)
$M,N \in M_{nn}(\F)$가 있다고 하자.

이 때, 다음이 성립하는 경우 $M$과 $N$이 서로 `닮았다(similar)`라고 하고 $M \sim N$라고 표기한다.

$$ \exist B \in M_{nn}(\F) \st M = B^{-1}NB  $$

### 참고
$M,N$이 어떤 linear map의 두 기저에 대한 matrix representation이라고 할 때, $\exist B \in M_{nn}(\F) \st M = B^{-1}NB$가 성립하는지보자. 두 기저 사이의 change of basis matrix $B$가 존재하고 motivation에서 $M = B^{-1}NB$를 만족한다는 것을 보았다. 따라서, $\exist B \in M_{nn}(\F) \st M = B^{-1}NB$가 성립한다.

이번에는 반대로 $\exist B \in M_{nn}(\F) \st M = B^{-1}NB$일 때, $M,N$이 어떤 linear map의 두 기저에 대한 matrix representation인지 보자. 모든 invertible matrix는 change of basis matrix임으로, $\beta \rightarrow \gamma$ change of basis matrix가 $B$가 되게 하는 $\beta,\gamma$를 임의로 결정하자. 그리고 $N = [T]_\beta^\beta$가 되게 $T$를 결정하면 $M = B^{-1}NB = [id]^\gamma_\beta[T]^\beta_\beta[id]^\beta_\gamma = [T]_\gamma^\gamma$가 된다. 따라서, $M,N$은 $T$의 서로다른 기저에서의 matrix representation이 됨을 볼 수 있다.

### 명제1
$M,N \in M_{nn}(\F)$가 있을 때, 다음을 증명하여라.

$$ M \sim N \iff \exist \beta \quad s.t. \quad [L_N]^\beta_\beta = M $$

**Proof**

[$\implies$]  
전제에 의해 다음이 성립한다.

$$ \exist B \in M_{n\times n}(\F) \st M = B^{-1}NB $$

$B$의 $i$ column을 $B_{*i}$라고 할 때, $\beta = \Set{B_{*1}, \cdots, B_{*n}}$라고 하면 다음이 성립한다.

$$ B = [id]^\epsilon_\beta $$

따라서, 다음이 성립한다.

$$ [L_N]_\beta^\beta = [id]^\beta_\epsilon [L_N]_\epsilon^\epsilon [id]^\epsilon_\beta = B^{-1}NB = M \qed $$

[$\impliedby$]  
$$ 
\begin{align*}  
M &= [L_N]^\beta_\beta \\ 
& = [id_V]^\beta_\epsilon [L_N]^\epsilon_\epsilon [id_V]_\beta^\epsilon \\ 
&= B^{-1}NB \qed  
\end{align*} 
$$

### 명제2
$M,N \in M_{nn}(\F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.

$$ \det(M) = \det(N) $$

**Proof**

$\det(AB) = \det(BA)$ 임으로 다음이 성립한다.

$$ \det(M) = \det(B^{-1}NB) = \det(B^{-1})\det(N)\det(B) = \det({B^{-1}B})\det(N) = \det(N) \qed $$

### 명제3
$M,N \in M_{nn}(\F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.

$$ \tr(M) = \tr(N) $$

**Proof**

$\tr(AB) = \tr(BA)$ 임으로 다음이 성립한다.

$$ \tr(M) = \tr(B^{-1}NB) = \tr(BB^{-1}N) = \tr(N) \qed  $$

### 명제4
$M,N \in M_{nn}(\F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.

$$ \tr(M^2) = \tr(N^2) $$

**Proof**

$$ \tr(M^2) = \tr(B^{-1}NBB^{-1}NB) = \tr(B^{-1}N^2B) = \tr(N^2)  \qed  $$

### 명제5
$M,N \in M_{nn}(\F)$가 있을 때, $M \sim N$라 하자.

$c_1,c_2 \in \F$에 대해, 다음을 증명하여라.

$$ c_1M + c_2I \sim c_1N + c_2I $$

**Proof**

$M \sim N$이기 때문에 다음을 만족하는 $B \in M_{nn}(\F)$가 존재한다.

$$ M = B^{-1}NB $$

따라서, 다음이 성립한다.

$$
\begin{align*} 
B^{-1}(c_1N + c_2I)B &= c_1B^{-1}NB + c_2I \\
&= c_1M + c_2I \qed 
\end{align*} 
$$

#### 따름명제
$X_m = \tr(X) / 3$로 정의할 떄, 다음을 증명하여라.

$$ (M - M_mI) \sim (N - N_m I) $$

**Proof**

$M \sim N$이기 때문에 다음이 성립한다.

$$ M_m = N_m = k $$

따라서, 명제5에 의해 성립한다. $\qed$

즉, 동일한 linear map의 matrix representation간에는 위와 같은 관계가 성립한다.


