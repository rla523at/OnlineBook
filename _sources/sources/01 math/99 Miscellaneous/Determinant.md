

### 명제1
$A \in M_{33}(\R)$이라 하자.

이 때, 다음을 증명하여라.
$$ \det(A) = A_{1*} \cdot (A_{2*} \times A_{3*}) $$

**Proof**

직접 계산하면 동일함을 알 수 있다. $\quad {_\blacksquare}$

#### 참고1
$\det(A) = \det(A^T)$임으로 행이 아닌 열에 대해서도 성립한다.
$$ \det(A) = A_{*1} \cdot (A_{*2} \times A_{*3}) $$

#### 참고2
Scalar triple product는 circular shift에 대해서 교환법칙이 성립함으로 다음이 성립한다.
$$ \det(A) = A_{1*} \cdot (A_{2*} \times A_{3*}) = A_{2*} \cdot (A_{3*} \times A_{1*}) = A_{3*} \cdot (A_{1*} \times A_{2*}) $$

> Referece  
> [Mathematics - Scalar triple product and determinant](https://math.stackexchange.com/questions/314275/scalar-triple-product-why-equivalent-to-determinant)  
> [Wiki - Triple product](https://en.wikipedia.org/wiki/Triple_product)

### 명제2
$$\det(A) = \det(A^T)$$

### 명제3
$$\det(AB) = \det(A)\det(B)$$

### 명제4
$A \in M_{nn}(\mathbb F), B \in M_{nm}(\mathbb F), D \in M_{mm}(\mathbb F)$이 있을 때, 다음을 증명하여라.

$$ \det\begin{bmatrix} A & B \\ 0 & D \end{bmatrix} = \det(A)\det(D)  $$

> Reference  
> [Wiki - Determinant](https://en.wikipedia.org/wiki/Determinant#Block_matrices)  
> [blog](https://www.statlect.com/matrix-algebra/determinant-of-block-matrix)  

### 명제5
$A \in M_{nn}(\mathbb F), B \in M_{nm}(\mathbb F), D \in M_{mm}(\mathbb F)$이 있다고 하자.

$D$가 invertible matrix일 때, 다음을 증명하여라.

$$ \det\begin{bmatrix} A & B \\ C & D \end{bmatrix} = \det (A-BD^{-1}C)\det(D)  $$

> Reference  
> [Wiki - Determinant](https://en.wikipedia.org/wiki/Determinant#Block_matrices)  
> [blog](https://www.statlect.com/matrix-algebra/determinant-of-block-matrix) 

### 명제5
Upper trianglular matrix $A \in M_{nn}(\mathbb F)$가 있다고 하자.

이 떄, 다음을 증명하여라.
$$ \det(A) = \prod_{i=1}^n A_{ii} $$

**Proof**

증명을 위해 수학적 귀납법을 사용한다.

$n=1$일 떄는 자명하게 성립한다.

$n = k-1$일 때 성립한다고 가정하고, $n=k$인 경우를 보자.

$A$가 upper triangular matrix임으로 $M_{ij}$을 $A$의 minor matrix라고 할 때, 다음이 성립한다.
$$ \det(A) = \sum_{i=1}^{k} A_{ki}M_{ki} = A_{kk}M_{kk} $$

이 떄, $M_{kk}$는 귀납법 가정에 의해 다음과 같다.
$$ M_{kk} = \prod_{i=1}^{k-1} A_{ii} $$

따라서, 다음이 성립한다.
$$ \det(A) = \prod_{i=1}^{k} A_{ii} \quad {_\blacksquare} $$

### 명제6
$A \in M_{nn}(\mathbb F)$가 있을 때, 다음을 증명하여라.
$$ \det(I + hA) = 1 + h \text{tr}(A) + O(h^2)$$

**Proof**

$A$의 eigen value들을 $\lambda_1, \cdots, \lambda_n$이라 하자.

Similar의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} & I+hA \sim I+hD_A \\ \Rightarrow \enspace & \det(I+hA) = \det(I + hD_a) \\ \Rightarrow \enspace & \det(I+hA) = (1+h\lambda_1) \cdots (1+h\lambda_n) \\ \Rightarrow \enspace & \det(I+hA) = 1 + h(\lambda_1 + \cdots + \lambda_n) + O(h^2) \\ \Rightarrow \enspace & \det(I+hA) = 1 + h \text{tr}(A) + O(h^2) \quad {_\blacksquare} \end{aligned} $$

#### 따름명제
$A \in M_{nn}(\mathbb F)$가 있을 때, 다음을 증명하여라.
$$ \lim_{h \rightarrow 0} \frac{\det(I + hA) - \det(I)}{h} = \text{tr}(A) $$

**Proof**

명제 6에 의해 다음이 성립한다.
$$ \begin{aligned} \lim_{h \rightarrow 0} \frac{\det(I + hA) - \det(I)}{h} &= \text{tr}(A) + \lim_{h \rightarrow 0} \frac{O(h^2)}{h} \\&= \text{tr}(A) \quad {_\blacksquare} \end{aligned} $$


### 명제7(Jacobi's formula)
집합 $M$을 다음과 같이 정의하자.
$$ M := \{ m \in M_{nn}(\mathbb F) \enspace | \enspace \det(m) \neq 0 \} $$

미분가능한 함수 $A : \mathbb F \rightarrow M$이 있을 때, 다음을 증명하여라.
$$ \frac{d}{dt} \det(A(t)) = \det(A(t)) \text{tr} ( A(t)^{-1} A'(t) ) $$

$$ \text{Where, } A'(t) \equiv \frac{dA}{dt} $$

**Proof**

Chain rule의 성질에 의해 다음이 성립한다.
$$ \lim_{h \rightarrow 0} \frac{\det(A(t+h)) - \det(A(t))}{h} = \lim_{h \rightarrow 0} \frac{\det(A(t) + hA'(t)) - \det(A(t))}{h} $$

명제3과 명제6에 의해 다음이 성립한다.
$$ \begin{aligned} \det ( A + h A' ) &= \det(A) \det ( I + h A^{-1} A' ) \\&= \det(A)(I + h \text{tr}(A^{-1}A') + O(h^2)) \end{aligned} $$

따라서, 다음이 성립한다. 
$$ \begin{aligned} \frac{d}{dt} \det(A(t)) &= \lim_{h \rightarrow 0} \frac{\det(A(t+h)) - \det(A(t))}{h} \\&= \lim_{h \rightarrow 0} \frac{\det(A(t) + hA'(t)) - \det(A(t))}{h} \\&= \lim_{h \rightarrow 0} \frac{h \det(A)\text{tr}(A^{-1}A')}{h} \\&= \det(A)\text{tr}(A^{-1}A') \qed \end{aligned} $$

> Reference  
> [Blog - Matrix identities](https://terrytao.wordpress.com/2013/01/13/matrix-identities-as-derivatives-of-determinant-identities/)  

### 명제8
$A \in M_{nn}(\mathbb F)$이 있을 때, 다음을 증명하여라.
$$ \det(A^*) = \overline{\det(A)} $$

> Reference  
> [Wiki - conjugate transpose](https://en.wikipedia.org/wiki/Conjugate_transpose#Properties_of_the_conjugate_transpose)

---

### 참고1

$$ A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}, \quad \det(A) = ad - bc $$

$\det(A)$는 $A$의 행 벡터들에 의해 생성되는 부호있는 부피이다.

따라서 $\det(A) = 0$이라는 말은 행 벡터들이 선형 독립이 아니라서 의미있는 부피(차원)을 만들어내지 못한다는 말이다.




### 라플라스 전개

$\det(A) = \sum _{k=1}^n a_{kj}M_{kj}$