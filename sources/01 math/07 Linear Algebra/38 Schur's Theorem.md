# Schur's Theorem

## 한 줄 요약

Schur's theorem은 complex inner product space의 linear map이 어떤 orthonormal basis에서 upper triangular matrix로 표현됨을 보장한다.

## Motivation

Linear map의 matrix representation이 diagonal이면 작용과 eigenvalue를 쉽게 읽을 수 있지만 모든 linear map이 diagonalizable한 것은 아니다. Upper triangular matrix도 diagonal entry에서 eigenvalue를 읽을 수 있고 계산이 단순하므로, basis가 orthonormal하다는 조건을 유지하면서 적어도 upper triangular form을 얻을 수 있는지 묻게 된다.

Schur's theorem은 characteristic polynomial이 linear factor로 split되면 그런 orthonormal basis가 존재한다고 말한다. 특히 $\C$ 위에서는 fundamental theorem of algebra에 의해 모든 characteristic polynomial이 split되므로 모든 complex matrix에 적용된다.

증명에서는 $T$의 `adjoint` $T^*$를 사용한다. $T^*$는 선택한 inner product에 대해 다음 관계를 만족하는 유일한 linear map이다.

$$
B(Tx,y)=B(x,T^*y).
$$

Finite-dimensional inner product space에서 $T^*$의 존재성과 유일성은 [Riesz Representation Theorem](<42 Riesz Representation Theorem.md>)으로부터 얻을 수 있다.

## 정리

$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

$T$의 characteristic polynomial를 $\varphi_T(\lambda)$라 할 떄, 다음을 증명하여라.

$$ \varphi_T(\lambda) \text{is split} \implies \exist \text{ orthonormal basis } \beta \st \frak m_\beta^\beta(T) \text{ is an upper triangular matrix} $$

**Proof**

증명을 위해 수학적 귀납법을 사용한다.

먼저 $\dim(V) = 1$인 경우 자명하게 성립한다.

다음으로 $\dim(V) = n-1$일 때, 성립한다고 가정하고 $\dim(V) = n$이라고 하자.

$\lambda$를 $T$의 eigenvalue라 하면 adjoint의 성질에 의해 $\overline\lambda$는 $T^*$의 eigen value이다.

$\overline\lambda$의 크기가 1인 eigenvector를 $v$라하면,  orthogonal complement의 성질에 의해 다음이 성립한다.

$$ V = \span(v) \oplus \span(v)^\perp$$

보조명제3.1에 의해 $\span(v)^\perp$는 split됨으로, 귀납적 가정에 의해 다음이 성립한다.

$$ \exist \text{orthonormal basis } \gamma = \{\gamma_1, \cdots, \gamma_{n-1} \} \st \frak m_{\gamma}^{\gamma}(T|_{\span(v)^\perp}) \text{ be an upper triangular matrix.} $$

이 때, $V$이 기저 $\beta$를 다음과 같이 정의하자.

$$ \beta = \{ \gamma_1, \cdots, \gamma_{n-1}, v \}$$ 

그러면 다음이 성립한다.

$$ \begin{aligned} \frak m_{\beta}^{\beta}(T) &= \begin{bmatrix} \frak{m}_\beta(T(\gamma_1)) & \cdots & \frak{m}_\beta(T(\gamma_{n-1})) & \frak{m}_\beta(T(v)) \end{bmatrix} \\&= \begin{bmatrix} \begin{array}{c | c} \begin{array}{} \\ \frak m_\gamma^\gamma(T|_{\span(v)^\perp}) \\  \\ \hline 0 \end{array} & \begin{array}{} a_1 \\ \vdots \\ a_n \end{array} \end{array} \end{bmatrix} \end{aligned} $$

$$ \text{Where, } T(v) = a_1 \gamma_1 + \cdots + a_{n-1}\gamma_{n-1} + a_nv $$

이 때, $\frak m_{\gamma}^{\gamma}(T|_{\span(v)^\perp})$가 upper triangular matrix임으로 $\frak m_{\beta}^{\beta}(T)$도 upper trianular matrix가 된다. 

동시에 $\beta$를 이루고 있는 $\gamma$와 $v$는 direct sum 관계에 있는 두 공간의 기저임으로 $\beta$는 $V$의 orthonormal basis가 된다.$\qed$

### 보조명제3.1
다음을 증명하여라.

$$ \span(v)^\perp \text{ is split} $$

**Proof**

$V$에 대해 다음이 성립한다.

$$ V = \span(v) \oplus \span(v)^\perp$$

$\span(v)^\perp$의 임의의 기저를 $\gamma = \{ \gamma_1, \cdots, \gamma_{n-1} \}$이라 할 때, $V$의 기저 $\beta$을 다음과 같이 정의하자

$$ \beta = \{ \gamma_1, \cdots, \gamma_{n-1}, v \}$$ 

보조명제3.1.1에 의해서 $\span(v)^\perp$는 $T$ invariant임으로 다음이 성립한다.

$$ \begin{aligned} \frak m_{\beta}^{\beta}(T) &= \begin{bmatrix} \frak{m}_\beta(T(\gamma_1)) & \cdots & \frak{m}_\beta(T(\gamma_{n-1})) & \frak{m}_\beta(T(v)) \end{bmatrix} \\&= \begin{bmatrix} \begin{array}{c | c} \begin{array}{} \\ \frak m_\gamma^\gamma(T|_{\span(v)^\perp}) \\  \\ \hline 0 \end{array} & \begin{array}{} a_1 \\ \vdots \\ a_n \end{array} \end{array} \end{bmatrix} \end{aligned} $$

$$ \text{Where, } T(v) = a_1 \gamma_1 + \cdots + a_{n-1}\gamma_{n-1} + a_nv $$

따라서, $\det(T - a_nI) = 0$임으로 $T$가 split된 1차식 중에는 $(a_n - \lambda)$가 포함되어 있다.

이 때, detrminant의 block matrix에 대한 성질에 의해 다음이 성립한다.

$$\det(\frak m_{\beta}^{\beta}(T) - \lambda I_n) = \det\Big( m_\gamma^\gamma(T|_{\span(v)^\perp}) - \lambda I_{n-1}\Big)(a_n - \lambda)$$

따라서, $T$의 split 된 1차식들 중 $(a_n - \lambda)$제외한 나머지 1차식들로 $T|_{\span(v)^\perp}$이 구성되어 있다.

그럼으로, $T|_{\span(v)^\perp}$는 split 된다. $\qed$


#### 보조명제3.1.1
다음을 증명하여라.

$$ \span(v)^\perp \text{ is a } T \text{ invariant} $$

**Proof**

$x \in \span(v)^\perp$라 하면 다음이 성립한다.

$$ B(T(x),v) = B(x, T^*(v)) = B(x, \overline\lambda v) = \lambda B(x, v) = 0_\F $$

임의의 $x \in \span(v)^\perp$에 대해, $T(x)$와 $v$가 수직함으로, $T(x) \in \span(v)^\perp$이다.

따라서, $\span(v)^\perp$는 $T \text{ invariant}$이다. $\qed$

##### 참고
$\span(v)$는 일반적으로 $T$ invariant가 아니다.

### 따름명제3.1
다음을 증명하여라.

$$ \text{ every complex matrix is similar to an upper triangular matrix} $$

**Proof**

$A \in M_{nn}(\C)$가 있다고 하자.

Fundamental theorem of algebra에 의해 $\varphi_A(\lambda)$는 항상 split된다. 

따라서, Schur's theorem에 의해 $\frak m_\beta^\beta(L_A)$가 upper triangular matrix이 되는 orthonormal basis $\beta$가 존재한다.

그럼으로, 다음이 성립한다.

$$ \begin{aligned} \frak m_\beta^\beta(L_A) &= \frak m_\epsilon^\beta(id) \frak m_\epsilon^\epsilon(L_A) \frak m_\beta^\epsilon(L_A) \\&= C^{-1}AC \end{aligned}  $$

즉, $\frak m_\beta^\beta(L_A) \sim A$이다. $\qed$

### 참고1
$\frak m_\beta^\beta(T)$가 다음과 같은 upper triangular matrix로 주어진다고 하자.

$$ \frak m_\beta^\beta(T) = \begin{bmatrix} a_1 & \cdots & & * \\ & a_2 \\ & & \ddots & \vdots \\ 0 & & & a_n \end{bmatrix} $$

$T(\beta_1) = a_1\beta_1$이 되기 때문에 $\beta_1$은 eigen vector, $a_1$은 eigen value가 된다.

### 참고2
$\frak m_\beta^\beta(T)$가 다음과 같은 upper triangular matrix로 주어진다고 하자.

$$ \frak m_\beta^\beta(T) = \begin{bmatrix} a_1 & \cdots & & * \\ & a_2 \\ & & \ddots & \vdots \\ 0 & & & a_n \end{bmatrix} $$

upper triangular matrix의 determinant 성질에 의해 다음이 성립한다.

$$ \det(T-\lambda I) = \prod_{i=1}^n (a_i - \lambda) $$ 

따라서, $a_1, \cdots, a_n$은 eigenvalue가 된다.
