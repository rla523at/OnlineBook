# Dimensional Reduction

앞 문서의 WRF는 다음 exact problem을 다룬다.

$$
\text{find }u\in\mathcal U
\st
\forall w\in C_c^\infty(\Omega),
\quad
B_r(w,u)=l_r(w)
$$

이 식은 무한차원 공간의 함수 $u$를 미지수로 두고 모든 test function에 대한 조건을 요구한다. 이를 유한한 계산으로 근사하려면 질문을 exact solution을 찾는 문제에서, 유한 개 coefficient로 나타낸 candidate 중 조건을 만족하는 approximation을 찾는 문제로 바꾸어야 한다.

Finite-dimensional test space의 basis $w_1,\ldots,w_n$과 trial functions $u_1,\ldots,u_n$을 고정하고, coefficient가 필요한 boundary condition을 만족하도록 제한한 candidate를

$$
u_h=\sum_{j=1}^n x^ju_j
$$

로 두면, 미지수는 함수 $u$ 자체가 아니라 coefficient $x^1,\ldots,x^n$이 된다. 또한 test function에 대한 조건은 basis별 equation

$$
B_r(w_i,u_h)=l_r(w_i),
\qquad
i=1,\ldots,n
$$

으로 바뀐다. 이처럼 test function space와 solution function space를 유한차원으로 제한하는 과정을 각각 살펴보자. 제한된 문제의 해는 일반적으로 original WRF의 exact solution이 아니라 approximation이다.

## Test Function Space Reduction
Linearly independent한 test functions

$$
w_1,\ldots,w_n\in C_c^\infty(\Omega)
$$

을 선택하고 finite-dimensional test space를

$$
\mathcal W_f
:=
\operatorname{span}\{w_1,\ldots,w_n\}
\subseteq
C_c^\infty(\Omega)
$$

로 둔다. 모든 $w\in\mathcal W_f$는 unique coefficient $c=(c^1,\ldots,c^n)\in\R^n$에 대해

$$
w=\sum_{i=1}^n c^iw_i
$$

로 표현된다. 따라서 test function space를 $C_c^\infty(\Omega)$에서 $\mathcal W_f$로 제한한 WRF는

$$
\text{find }u\in\mathcal U
\st
\forall c\in\R^n,
\quad
\sum_{i=1}^n c^i\bigl(B_r(w_i,u)-l_r(w_i)\bigr)=0
$$

과 같다. 이 식에 standard basis vector $c=e_i$를 차례로 대입하면 다음 $n$개 equation을 얻는다.

$$
\text{find }u\in\mathcal U
\st
B_r(w_i,u)=l_r(w_i),
\qquad
i=1,\ldots,n
$$

### 참고
Test function space를 $\mathcal W_f$로 축소함으로써 $n$개의 `기저함수(basis function)`에 대해서만 확인하면 되는 문제로 단순화됐다.

## Solution Function Space Reduction
유한차원 함수 공간 $\mathcal{U_f}$를 다음과 같이 정의하자.

$$ \mathcal{U_f} := \operatorname{span}\{u_1,\ldots,u_n\}\subseteq C^m(\Omega) $$

$\mathcal{U_f}$의 subset $\mathcal{U_{b}}$를 다음과 같이 정의하자.

$$ \mathcal{U_b} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies BC}} $$

그러면 $\mathcal{U_b}$의 정의에 의해 다음이 성립한다.

$$ \mathcal{U_b} \text{ is subset of } \mathcal{U} $$

이 때, $\mathcal{U}$를 $\mathcal{U_b}$로 축소하자. 

### Restriction form
$\R^n_b \subseteq \R^n$을 다음과 같이 정의하자

$$ \R^n_b := \Set{x \in \R^n | \sum_{i=1}^n x^iu_i \text{ satisfies BC}} $$

그러면 test function space가 축소된 WRF는 다음과 같이 더 간단해 진다.

$$ \begin{aligned} & \text{find } u \in \mathcal U_b \st B_r(w_i,u) = l_r(w_i), \enspace i = 1,\cdots,n \\ \iff \enspace & \text{find } x \in \R_b^n \st B_r(w_i,x^j u_j) = l_r(w_i), \enspace i = 1,\cdots,n \end{aligned} $$

### Affine form
$\mathcal{U_f}$의 subspace $\mathcal{U_0}$를 다음과 같이 정의하자.

$$ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u=0 \text{ on } \partial\Omega} $$

$\phi \in \mathcal{U_b}$라 할 때, 다음이 성립한다.

$$ \forall u \in \mathcal{U_b}, \quad \exist u_0 \in \mathcal{U_0} \st u = \phi + u_0 $$

$\mathcal{U_0}$의 basis를 $\Set{u^0_i}$라고 하면, test function space가 축소된 WRF는 다음과 같이 간단해 진다.

$$ \begin{aligned} & \text{find } u \in \mathcal U_b \st B_r(w_i,u) = l_r(w_i), \enspace i = 1,\cdots,n \\ \iff \enspace & \text{find } x \in \R^n \st B_r(w_i,\phi + x^ju^0_j) = l_r(w_i), \enspace i = 1,\cdots,n \end{aligned} $$

#### 참고
명제1을 통해 알 수 있듯이 $\mathcal{U_b}$는 vector space가 아니다.

Affine form라고 부르는 이유는  위의 형태를 보면 알 수 있듯이 $\mathcal{U_b}$가 affine space이기 때문이다.

#### 명제1
다음을 증명하여라.

$$\mathcal{U_b} \text{ is not a vector space }$$

**Proof**

$\partial\Omega$에서 0이 아닌 BC $g$가 주어졌다고 가정하자.

$$ u = g \neq 0 \quad \text{on } \partial\Omega $$

그러면 $u_1, u_2 \in \mathcal{U_b}$가 있을 때, $\forall x \in \partial\Omega$에서 다음이 성립한다.  

$$ (u_1 + u_2)(x) = u_1(x) + u_2(x) = 2g(x) $$

따라서, 다음이 성립한다. 

$$ u_1 + u_2 \notin \mathcal{U_b} $$

즉 $\mathcal{U_b}$는 덧셈에 대해 닫혀있지 않기 때문에 vector space가 될 수 없다. 

> Reference  
> [Note - M. J. Zahr](https://mjzahr.github.io/content/ame40541/spr20/ch03-wres-solo.pdf)

## Final Form
따라서, WRF의 최종 형태는 다음과 같다.

### Restrition form
$$ \text{find } x \in \R_b^n \st B_r(w_i,x^j u_j) = l_r(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces

$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_f} := \span(\set{u_1,\cdots,u_n}) \\ \R^n_b := \Set{x \in \R^n | x^iu_i \text{ satisfy BC}} \end{gathered} $$

### Affien form
$$ \text{find } x \in \R^n \st B_r(w_i,\phi + x^ju^0_j) = l_r(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_b} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies BC}} \\ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u=0 \text{ on } \partial\Omega} = \span(\Set{u_i^0}) \end{gathered} $$

### 참고
Dimensional reduction된 WRF는 탐색하는 function space가 축소됐기 때문에 더이상 strong formulation과 동치가 아니다.

따라서, dimensional reduction된 WRF를 만족하는 해도 strong formulation의 solution이 아닐 수 있다.

Dimensional reduction된 WRF의 해는 strong formulation의 approximated solution이다

## Linear Case
만약, $\mathcal P$가 linear operator면 $B_r$은 bilinear map이 된다.

따라서, dimensional reduction한 WRF는 다음과 같이 단순해 진다.

### Restrition form
$$ \text{find } x \in \R_b^n \st B_r(w_i,u_j) x^j = l_r(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_f} := \span(\set{u_1,\cdots,u_n}) \\ \R^n_b := \Set{x \in \R^n | x^iu_i \text{ satisfy BC}} \end{gathered} $$

#### Matrix form
$$ \begin{bmatrix} B(w_1,u_1) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_1) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} x^1 \\ \vdots \\ x^n \end{bmatrix} = \begin{bmatrix} l(w_1) \\ \vdots \\ l(w_n) \end{bmatrix} $$

### Affine form
$$ \text{find } x \in \R^n \st B_r(w_i,u^0_j)x^j = l_r(w_i) - B_r(w_i,\phi), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_b} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies BC}} \\ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u=0 \text{ on } \partial\Omega} = \span(\Set{u_i^0}) \end{gathered} $$

#### Matrix form
$$ \begin{bmatrix} B(w_1,u^0_1) & \cdots & B(w_1,u^0_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u^0_1) & \cdots & B(w_n,u^0_n) \end{bmatrix} \begin{bmatrix} x^1 \\ \vdots \\ x^n \end{bmatrix} = \begin{bmatrix} l(w_1) - B(w_1, \phi) \\ \vdots \\ l(w_n) - B(w_n, \phi) \end{bmatrix} $$

### 참고

만약 $u^0_j$의 $m$계 도함수가 0이라면 $B(w_i,u_j) = 0$이 된다.

그러면 행렬은 0으로만 이루어진 열을 갖는 singular matrix가 된다. 

이는 기저 함수 $u_j$가 해를 근사하는데에 어떠한 기여도 하지 않는다는 것을 의미하며, 그로인해 $u_j$의 계수 $x_j$가 어떤 값을 갖더라도 같은 근사가 됨을 의미한다.

> Reference  
> [Note - M. J. Zahr](https://mjzahr.github.io/content/ame40541/spr20/ch03-wres-solo.pdf)
