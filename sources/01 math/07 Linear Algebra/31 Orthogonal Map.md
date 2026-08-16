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

그리고 $V$의 orthonomral basis를 $\beta$라고 할 때, $A =\mathfrak{m}^\beta_\beta(T)$라고 하자.

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

#### 보조정리
$A^TA=I$ 일 떄, 다음을 증명하여라.

$$ A \text{ is invertible } $$

**Proof**

$\det$ 의 성질에 의해 다음이 성립한다.

$$ \det(A^TA) = \det(A^T) \det(A) = \det(A)^2 = 1 $$

따라서, $\det(A) = \pm 1 $ 임으로 다음이 성립한다. 

$$ \det(A) \neq 0 \implies A \text{ is invertible } \qed $$

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

### Orthogonal group과 special orthogonal group

$n\times n$ orthogonal matrix 전체의 집합을 orthogonal group $O(n)$이라고 한다.

$$
O(n)
:=
\{Q\in\mathbb R^{n\times n}\mid Q^{\mathsf T}Q=I\}
$$

$Q\in O(n)$이면 다음이 성립한다.

$$
1
=
\det(Q^{\mathsf T}Q)
=
\det(Q)^2
$$

따라서 orthogonal matrix의 determinant는 $+1$ 또는 $-1$이다.

$$
\det Q\in\{-1,+1\}
$$

Determinant가 $+1$인 orthogonal matrix만 모은 subgroup을 special orthogonal
group $SO(n)$이라고 한다.

$$
SO(n)
:=
\{R\in O(n)\mid\det R=1\}
$$

$SO(3)$의 element는 3차원 orientation을 보존하는 rotation matrix다.
$O(3)$에는 rotation뿐 아니라 determinant가 $-1$인 orientation-reversing
transformation도 포함된다. 따라서 $Q^{\mathsf T}Q=I$만 확인하고 $Q$를 rotation이라고
부르면 reflection을 구분하지 못한다.

3차원 rotation의 geometric 의미와 group operation은
[Rotation Matrix and SO(3)](<../08 Geometry/22 Rotation Matrix and SO(3).md>)에서
설명한다.

### 명제
$M\in GL(3,\mathbb R)$이고 대각행렬의 집합을 $D(3)$라고 하자. 다음 두 조건은
동치다.

1. $MM^{\mathsf T}\in D(3)$이다.
2. Positive diagonal matrix $S\in D(3)$와 $Q\in O(3)$가 존재해 $M=SQ$다.

**Proof**

[$\implies$]

$MM^{\mathsf T}\in D(3)$이면 $M$의 서로 다른 row는 orthogonal하다. $M$이
invertible이므로 각 row의 norm $s_1,s_2,s_3$는 0보다 크다. 다음 matrix를 정의하자.

$$ S = \begin{bmatrix}
  s_1 & 0  & 0  \\
  0  & s_2 & 0  \\
  0  & 0  & s_3 \\
\end{bmatrix} $$

$Q=S^{-1}M$이라고 하면 $MM^{\mathsf T}=S^2$이므로 다음이 성립한다.

$$
QQ^{\mathsf T}
=
S^{-1}MM^{\mathsf T}S^{-\mathsf T}
=
S^{-1}S^2S^{-1}
=I
$$

따라서 $Q\in O(3)$이고 $M=SQ$다.

[$\impliedby$]

$M=SQ$, $S\in D(3)$, $Q\in O(3)$이면:

$$
MM^{\mathsf T}
=
SQQ^{\mathsf T}S^{\mathsf T}
=
S^2
\in D(3)
\qed
$$

#### 따름명제1
$\mathfrak T:=\{M\in GL(3,\mathbb R)\mid MM^{\mathsf T}\in D(3)\}$라고 하자.

$M \in \mathfrak{T}$ 일 때, $M^{-1} \notin \mathfrak{T}$ 일 수 있다.

#### 따름명제2
$\mathfrak T:=\{M\in GL(3,\mathbb R)\mid MM^{\mathsf T}\in D(3)\}$라고 하자.

$M,N \in \mathfrak{T}$ 일 때, $MN \notin \mathfrak{T}$ 일 수 있다.
