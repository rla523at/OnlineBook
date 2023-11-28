# Orthogonal Map
## 정의
Inner product space $V/\R$와 $T \in \End(V)$가 있다고 하자.

$V$의 임의의 element를 $v_1,v_2$라고 할 때, 다음하는 $T$를 `직교 변환(orthogonal map)`이라고 한다.

$$ B(v_1,v_2) = B(T(v_1),T(v_2)) $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Orthogonal_transformation)  
> [blog](https://m.blog.naver.com/qio910/221791116197)

### 참고
Orthogonal map은 inner product를 보존하는 변환이다.

### 명제1
Inner product space $V/\R$과 orthogonal map $T$가 있다고 하자.

$V$의 임의의 element를 $v$라고 할 떄, 다음을 증명하여라.

$$ \norm{v} = \norm{T(v)} $$

**Proof**

$$ \norm{u} = \sqrt{B(u,u)} = \sqrt{B(T(u),T(u))} = \norm{T(u)} $$

### 명제2
Inner product space $V/\R$과 orthogonal map $T$가 있다고 하자.

$V$의 임의의 element를 $v_1,v_2$라고 할 떄, $v_1,v_2$ 사이의 각을 $\theta$, $T(v_1),T(v_2)$사이의 각을 $\phi$라고 하자.

이 떄, 다음을 증명하여라.

$$ \theta = \phi $$

**Proof**

$$ \cos\theta = \frac{B(v_1,v_2)}{\norm{v_1}\norm{v_2}} = \frac{B(T(v_1),T(v_2))}{\norm{T(v_1)}\norm{T(v_2)}} = \cos\phi \qed $$

#### 참고
orthogonal map은 각도를 보존한다. 따라서 orthogonality도 보존된다.

### 명제3
$n$ 차원 inner product space $V/\R$과 orthogonal map $T$가 있다고 하자.

그리고 $V$의 orthonomral basis를 $\beta$라고 할 때, $A =\frak{m}^\beta_\beta(T)$라고 하자.

이 떄, 다음을 증명하여라.

$$ A A^T = A^T A = I $$

**Proof**

$T$의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} B(\beta_i,\beta_j) &= B(T(\beta_i),T(\beta_j)) \\ \delta_{ij} &= B(A^k_i\beta_k,A^l_j\beta_l) \\&= A^k_iA^l_jB(\beta_k,\beta_l) \\&= A^k_iA^k_j \end{aligned}  $$

즉, $A$의 $i$번째 column과 $j$번째 column을 componentwise하게 곱하면 $i=j$일 때는 1이고 $i\neq j$일 때는 0이라는 의미임으로 다음이 성립한다.

$$ A^TA =I $$

이 떄, $A$는 $n\times n$ square matrix임으로 다음이 성립한다.

$$ A^T = A^{-1} $$

따라서, 다음이 성립한다.

$$ A A^T = A^T A = I \qed $$

> Referemce  
> [math.stackexchange](https://math.stackexchange.com/questions/3613207/prove-the-matrix-of-an-orthogonal-linear-transformation-relative-to-an-orthonorm)   
> [youtube](https://www.youtube.com/watch?v=FM7u3jINbbA)  

#### 참고
$A^TA=I$임으로 A의 column을 coordinate로 갖는 vector들은 orthonormal하다.

$AA^T=I$임으로 $A$의 row을 coordinate로 갖는 vector들은 orthonormal하다.

### 명제4
$n$차원 inner product space $V/\R$과 orthogonal map $T$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ T \text{ is bijective} $$

**Proof**

[injective]  
$\ker(T)$의 임의의 element를 $v$라고 하면 다음이 성립한다.

$$ \begin{aligned} & \Braket{v,v} = \Braket{T(v),T(v)} = 0_\F \\\implies& v = 0_V \end{aligned} $$

$v$가 $0_V$일 수 밖에 없음으로 다음이 성립한다.

$$ \ker(T) = \Set{0_V} \implies \ker(T) \text{ is injective} \qed $$

[surjective]  
Dimension theorem에 의해 다음이 성립한다.

$$ \begin{aligned} & \rank(T) = \dim(V) - \nullity(T) \\\implies& \rank(T) = \dim(V) \\\implies& \img(T) = V \end{aligned} $$

따라서, $T$는 surjective 하다. $\qed$

### 명제5
$n$차원 inner product space $V/\R$과 orthogonal map $T$가 있다고 하자.

그리고 $V$의 orthonormal basis를 $\beta$라고 할 때, 다음을 증명하여라.

$$ T(\beta) \text{ is an orthonormal basis of } V $$

**Proof**

$T$가 bijective임으로 다음이 성립한다.

$$ T(\beta) \text{ is an basis of } V $$

$T(\beta)$의 임의의 element를 $T(\beta_i),T(\beta_j)$라고 하면 다음이 성립한다.

$$ \Braket{T(\beta_i),T(\beta_j)} = \Braket{\beta_i,\beta_j} = \delta_{ij} $$

따라서, 다음이 성립한다.

$$ T(\beta) \text{ is an orthonormal basis of } V \qed $$

### 명제6
$n$차원 inner product space $V/\R$과 orthogonal map $T$가 있다고 하자.

그리고 $V$의 orthonormal basis를 $\beta$라고 할 때,$A =\frak{m}^\beta_\beta(T)$라고 하자.

이 떄, 다음을 증명하여라.

$$ \Braket{\beta_i, T(\beta_j)} = A^i_j $$

**Proof**

$$ \Braket{\beta_i, T(\beta_j)} = \Braket{\beta_i, A^k_j\beta_k} = A^k_j\delta_{ik} = A^i_j \qed $$

#### 참고

$\beta_i$와 $T(\beta_j)$ 모두 orthonormal basis이기 때문에 두 vector의 inner product는 $\cos$값과 같다.

따라서, $A^i_j$는 $\beta_i$와 $T(\beta_j)$ 사이의 각도에 따른 $\cos$값이 됨으로 $A^i_j$를 기존의 $i$ basis와 새로운 $j$ basis 사이의 directional cosine이라고 부르기도 한다.


## Orthogonal Matrix
$A \in M_{nn}(\R)$가 있다고 하자.

이 떄, $A$가 다음 성질을 만족할 경우 `orthogonal matrix`라고 한다.

$$ AA^T = A^TA = I $$

### 참고
orthogonal map을 orthonormal basis로 표현하면 orthogonal matrix가 된다.