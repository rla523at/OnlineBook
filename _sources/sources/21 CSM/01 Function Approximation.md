# Function Approximation
함수 $\phi : \R^n \rightarrow \R^m$와 $\phi$의 component functions $\phi^i, \enspace i=1,\cdots,m$가 있다고 하자.

$$ \phi^i : \R^n \rightarrow \R, \enspace i=1,\cdots,m $$

이 떄, 서로 다른 $p$개의 점 $x_1, \cdots x_p \in \R^n$에서 $\phi$의 값을 알고 있을 때, 풀고 싶은 문제는 다음과 같다.

$$ \text{find } \phi_h : \R^n \rightarrow \R^m \st \phi \approx \phi_h $$

이걸 component wise하게 풀어 쓰면 $i=1,\cdots,m$일 때 다음과 같다.

$$ \text{find } \phi_h^i : \R^n \rightarrow \R \st \phi^i \approx \phi_h^i $$

## $C^0$ Approximation
$\phi_h$는 $\phi$의 approximate function임으로 approximation의 기준을 정해야 되는데, 이 때 한가지 타당한 기준은 다음과 같다.

$$ \phi(x_j) = \phi_h(x_j), \enspace j=1,\cdots,p $$

즉, 값이 주어진 점들에서 approximate function이 원래 function과 정확히 일치하는 것을 approximation의 기준으로 삼는것이다.

이제, 기준이 결정되었으니 기준을 만족하는 $\phi_h$를 찾아보자.

### Search
Vector space $V/\R$를 다음과 같이 정의하자.

$$ V := \Set{f :\R^d \rightarrow \R} $$

그러면 풀고자 하는 문제는 $i=1,\cdots,m, \enspace j=1,\cdots,p$에 대해서 다음과 같다.

$$ \text{find } \phi^i_h \in V \st \phi^i_h(x_j) = \phi^i(x_j) $$

하지만 $V/\R$은 infinite dimensional vector space임으로 $V$를 전부 탐색하는 문제는 매우 어렵다.

따라서, 문제를 단순화 하기 위해 $V$의 linearly independent한 $l$개의 function들 $m^1,\cdots,m^l$을 선택하고 $V$의 finite dimensional subspace $\mathcal{M}$을 $k=1,\cdots,l$일 때, 다음과 같이 정의하자.

$$ \mathcal{M} = \span(\Set{m^k}) $$

따라서 단순화된 문제는 $i=1,\cdots,m, \enspace j=1,\cdots,p$에 대해서 다음과 같다.

$$ \text{find } \phi^i_h \in \mathcal{M} \st \phi^i_h(x_j) = \phi^i(x_j) $$

$i=1,\cdots,m, \enspace k=1,\cdots,l$에 대해서 $\phi^i_h \in \mathcal{M}$임으로 다음이 성립한다.

$$ \exist A^i_k \in \R \st \phi^i_h = A^i_km^k $$

따라서 탐색 문제는 $i=1,\cdots,m, \enspace j=1,\cdots,p, \enspace k=1,\cdots,l$에 대해서 다음과 같아진다.

$$ \text{find } A^i_k \in \R \st A^i_km^k(x_j) = \phi^i(x_j) $$

이를 matrix form으로 쓰면 다음과 같다.

$$ \begin{gathered} \begin{bmatrix} A_1^1 &\cdots& A_l^1 \\ \vdots \\ A_1^m &\cdots& A_l^m \end{bmatrix} \begin{bmatrix} m^1(x_1) &\cdots& m^1(x_p) \\ \vdots && \vdots \\ m^l(x_1) &\cdots& m^l(x_p) \end{bmatrix}  = \begin{bmatrix} \phi^1(x_1) &\cdots& \phi^1(x_p) \\ \vdots \\ \phi^m(x_1) &\cdots& \phi^m(x_p) \end{bmatrix} \\ AM =\Phi \end{gathered}  $$

$A$를 유일하게 결정하기 위해서는 $M$이 invertible해야 되고, 만약 $M$이 invertible하다면 다음이 성립한다.

$$ \begin{gathered} A = \Phi M^{-1} \end{gathered}  $$

#### 참고1
$M$이 invertible하기 위해서는 $l=p$여야 한다.

즉, $\phi_h$를 유일하게 결정하기 위한 필요조건은 주어진 점의 개수와 동일한 수의 basis를 선택하는 것이다.

#### 명제1
$M$이 invertible matrix일 때, 다음을 증명하여라.

$$ \phi \in \mathcal{M} \implies \phi = \phi_h $$

**Proof**

$i=1,\cdots,m, j=1,\cdots,l$일 떄,  $\phi$와 $\phi_h$를 다음과 같이 표현하자.

$$ \phi^i = A^i_jm^j, \enspace \phi_h^i = B^i_jm^j $$

이 때, $\phi^i(x_j) = \phi_h^i(x_j)$임으로 다음이 성립한다.

$$ AM = BM $$

$M$이 invertible matrix이면 다음이 성립한다.

$$ A = B $$

따라서, 다음이 성립한다.

$$ \phi = \phi_h \qed $$


### Shape Function
계산의 편의성을 위해 행렬 $m$을 다음과 같이 정의하자.

$$ m := \begin{bmatrix} m^1(x) \\ \vdots \\ m^l(x)  \end{bmatrix} $$  

그러면 $\phi_h$를 다음과 같이 표현할 수 있다.

$$ \phi_h = Am $$

이 때, $M$이 invertible하다고 하면 $l=p$이고 다음이 성립한다.

$$ \begin{aligned} \phi_h &= Am \\&= \Phi M^{-1} m \end{aligned} $$

이 때, 행렬 $n$을 다음과 같이 정의하자.

$$ n := M^{-1}m $$

그러면 $j=1,\cdots,p$에 대해서 다음이 성립한다.

$$ \begin{aligned} \phi_h &= \Phi n \\&= \phi(x_j)n^j \end{aligned} $$

이 때, $n^j, \enspace j=1,\cdots,p$을 `shape function`이라고 한다.

#### 명제1
다음을 증명하여라.

$$ \text{set of shape function is an linearly independent set} $$

**Proof**

다음을 만족하는 $c_j \in \R, j=1,\cdots, p$을 찾아보자.

$$ c_jn^j = 0_V $$

Shape function의 정의에 의해 다음이 성립한다.

$$ c_j((M^{-1})^j_im^i ) = 0_V  $$

이 떄, $c_j', \enspace i,j=1,\cdots,p$을 다음과 같이 정의하자.

$$ c_j' := c_i(M^{-1})^i_j $$

그러면 다음이 성립한다.

$$ c_j'm_j = 0 $$

$\Set{m^i}$는 linearly independet set임으로 다음이 성립한다.

$$ c_j' = 0, \enspace i=1,\cdots,n $$

이를 행렬식으로 나타내면 다음과 같다.

$$ \begin{bmatrix} (M^{-1})^1_1 & \cdots & (M^{-1})^1_p \\ \vdots && \vdots \\ (M^{-1})^p_1 &\cdots&  (M^{-1})^p_p \end{bmatrix} \begin{bmatrix} c_1 \\ \vdots \\ c_p \end{bmatrix} = 0_p $$

따라서, $c := \begin{bmatrix} c_1 & \cdots& c_p \end{bmatrix}^T$일 떄, 다음이 성립한다.

$$ \begin{aligned} M^{-1}c &= 0_p \\ c &= 0_p \\ c_j &= 0, \enspace j=1,\cdots,p \end{aligned}  $$

따라서, linearly independent set의 정의에 의해 다음이 성립한다.

$$ \Set{n^j} \text{ is an linearly independent set} \qed $$

##### 따름명제
다음을 증명하여라.

$$ \text{ set of shape function is a basis of } \mathcal{M} $$

**Proof**

$\mathcal{M}$의 dimension과 linearly independent set $\Set{n^j}$의 cardinality가 같음으로 $\Set{n^j}$는 basis이다.

#### 명제2
다음을 증명하여라.

$$ n^i(x_j) = \delta^i_j, \enspace i,j=1,\cdots,p $$

**Proof**

임의의 $i \in [1,p]$가 있을 때, $\phi = n^i$라고 하자.

그러면 $n^i \in \mathcal{M}$임으로 다음이 성립한다.

$$ \begin{aligned} & \phi = \phi(x_j)n^j \\ \implies& n^i = n^i(x_j)n^j \end{aligned} $$

명제1에 의해 $\Set{n^j}$는 linearly independent set임으로 다음이 성립한다.

$$ n^i(x_j) = \delta^i_{j} \qed $$

> Reference  
> [blog1](http://what-when-how.com/the-finite-element-method/fundamentals-for-finite-element-method-part-1/)  
> [blog2](http://what-when-how.com/the-finite-element-method/fundamentals-for-finite-element-method-part-2/)  

#### 참고
$\phi_h = \Phi n$에서 $\Phi$를 벡터 형태로 쓰면 다음과 같은 형태를 갖는다.

$$ \phi_h = \begin{bmatrix} n_1 \enspace \underbrace{0 \cdots 0}_{m-1}  && n_2 \enspace \underbrace{0 \cdots 0}_{m-1} && \cdots && n_n \enspace \underbrace{0 \cdots 0}_{m-1} \\ 0 \enspace n_1 \enspace \underbrace{0 \cdots 0}_{m-2} && 0 \enspace n_2 \enspace \underbrace{0 \cdots 0}_{m-2} && \cdots && 0 \enspace n_n \enspace \underbrace{0 \cdots 0}_{m-2} \\ && \qquad  \vdots \\ \underbrace{0 \cdots 0}_{m-1} \enspace  n_1 && \underbrace{0 \cdots 0}_{m-1} \enspace n_2 && \cdots && \underbrace{0 \cdots 0}_{m-1} \enspace n_n \end{bmatrix} \begin{bmatrix} \phi^1(x_1) \\\vdots\\ \phi^m(x_1) \\\vdots\\ \phi^1(x_k) \\\vdots\\ \phi^m(x_k) \end{bmatrix} $$

$$ \phi_h = N {\hat\phi} $$


# Interpolation


## $C^1$ 보간
### $\phi : \R \rightarrow \R$
함수 $\phi : \R \rightarrow \R$를 $2n$개의 `단항식(monomial)`을 갖는 `다항식(polynomial)`으로 근사하면 다음과 같이 표현할 수 있다.

$$ \phi(x) \approx \mathbf a \cdot \mathbf m(x) $$

이 떄, 서로 다른 $n$개의 $x_i$에서 $\phi(x_i), \phi'(x_i)$값을 알고 있다면 $\phi(x_i) = \mathbf a \cdot \mathbf m(x_i)$와 $\phi'(x_i) = \mathbf a \cdot \mathbf m'(x_i)$를 만족하도록 $\mathbf a$를 결정할 수 있다.

위의 조건을 행렬 형태로 나타내면 다음과 같다.

$$ \boldsymbol{\hat{\phi}} = \mathbf M^T \mathbf a \\ \text{where,} \quad \mathbf M =  \begin{bmatrix} \mathbf m(x_1) & \mathbf m'(x_1) & \cdots & \mathbf m(x_n) & \mathbf m'(x_n) \end{bmatrix}, \quad \boldsymbol{\hat{\phi}} = \begin{bmatrix} \phi(x_1) \\ \phi'(x_1) \\ \vdots \\ \phi(x_n) \\ \phi'(x_n) \end{bmatrix} $$

따라서 $\bf a = (M^T)^{-1} \boldsymbol{\hat{\phi}}$이고 이를 원래 식에 대입하면 다음과 같다. 

$$ \begin{aligned} \phi &= \mathbf {a \cdot m} \\ &= \mathbf {m \cdot a} \\ &= \mathbf m^T (\mathbf M^T)^{-1} \boldsymbol{\hat{\phi}} \\ &= \mathbf n \cdot \boldsymbol{\hat{\phi}} \quad  \end{aligned} $$

이와 같이 각 점에서 값과 기울기 값을 만족하게 보간하는 방법을 `에르미트 보간(Hermitian interpolation)`이라고 한다.