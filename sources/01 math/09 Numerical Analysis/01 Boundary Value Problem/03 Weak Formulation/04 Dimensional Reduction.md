# Dimensional Reduction

앞 문서의 weak formulation은 다음 문제를 다룬다.

$$
\text{find }u\in\mathcal U_{\mathrm{relax}}
\st
\forall w\in\mathcal W_{\mathrm{relax}},
\quad
B(w,u)=l(w)
$$

이 식은 무한차원 공간의 함수 $u$를 미지수로 두고 모든 test function에 대한 조건을 요구한다. 수치적으로 계산할 수 있는 approximation을 만들려면 finite-dimensional test space의 basis $w_1,\ldots,w_n$을 고정하고, essential boundary condition을 만족하는 finite-dimensional trial family를 유한 개 coefficient $a=(a^1,\ldots,a^n)$로 나타낸다. 이 candidate를 $u_h(a)$라고 쓰면 조건은

$$
B(w_i,u_h(a))=l(w_i),
\qquad
i=1,\ldots,n
$$

이라는 유한 개 equation으로 바뀐다. 따라서 질문도 infinite-dimensional weak solution을 정확히 찾는 것에서 coefficient $a$로 표현되는 approximation을 찾는 것으로 바뀐다. 아래에서는 이 전환을 test function space와 solution function space에 나누어 구성한다.

## Test Function Space Reduction
Linearly independent한 test functions

$$
w_1,\ldots,w_n\in\mathcal W_{\mathrm{relax}}
$$

을 선택하고 finite-dimensional test space를

$$
\mathcal W_f
:=
\operatorname{span}\{w_1,\ldots,w_n\}
\subseteq
\mathcal W_{\mathrm{relax}}
$$

로 둔다. 모든 $w\in\mathcal W_f$는 unique coefficient $x=(x^1,\ldots,x^n)\in\R^n$에 대해

$$
w=\sum_{i=1}^n x^iw_i
$$

로 표현된다. 따라서 test function space를 $\mathcal W_{\mathrm{relax}}$에서 $\mathcal W_f$로 제한한 weak formulation은

$$
\text{find }u\in\mathcal U_{\mathrm{relax}}
\st
\forall x\in\R^n,
\quad
\sum_{i=1}^n x^i\bigl(B(w_i,u)-l(w_i)\bigr)=0
$$

과 같다. 이 식에 standard basis vector $x=e_i$를 차례로 대입하면 다음 $n$개 equation을 얻는다.

$$
\text{find }u\in\mathcal U_{\mathrm{relax}}
\st
B(w_i,u)=l(w_i),
\qquad
i=1,\ldots,n
$$

### 참고
Test function space를 $\mathcal W_f$로 축소함으로써 $n$개의 `기저함수(basis function)`에 대해서만 확인하면 되는 문제로 단순화됐다.

## Solution Function Space Reduction
유한차원 함수 공간 $\mathcal{U_f}$를 다음과 같이 정의하자.

$$ \mathcal{U_f} := \operatorname{span}\{u_1,\ldots,u_n\}\subseteq C^1(\Omega) $$

$\mathcal{U_f}$의 subset $\mathcal{U_{reduc}}$를 다음과 같이 정의하자.

$$ \mathcal{U_{reduc}} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies essential BC}} $$

그러면 $\mathcal{U_{reduc}}$의 정의에 의해 다음이 성립한다.

$$ \mathcal{U_{reduc}} \text{ is subset of } \mathcal{U_{relax}} $$

이 때, $\mathcal{U_{relax}}$를 $\mathcal{U_{reduc}}$로 축소하자. 

### Restriction form
$\R^n_s \subseteq \R^n$을 다음과 같이 정의하자

$$ \R^n_s := \Set{a \in \R^n | \sum_{i=1}^n a^iu_i \text{ satisfies essential BC}} $$

그러면 test function space가 축소된 weak formulation는 다음과 같이 더 간단해 진다.

$$ \begin{aligned} & \text{find } u \in \mathcal{U_{reduc}} \st B(w_i,u) = l(w_i), \enspace i = 1,\cdots,n \\ \iff \enspace & \text{find } a \in \R^n_s \st B(w_i,a^j u_j) = l(w_i), \enspace i = 1,\cdots,n \end{aligned} $$

### Affine form
$\mathcal{U_f}$의 subspace $\mathcal{U_0}$를 다음과 같이 정의하자.

$$ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u = 0 \text{ on } \partial\Omega_E} $$

$\phi \in \mathcal{U_{reduc}}$라 할 때, 다음이 성립한다.

$$ \forall u \in \mathcal{U_{reduc}}, \quad \exist u_0 \in \mathcal{U_0} \st u = \phi + u_0 $$

$\mathcal{U_0}$의 basis를 $\Set{u^0_i}$라고 하면, test function space가 축소된 weak formulation는 다음과 같이 간단해 진다.

$$ \begin{aligned} & \text{find } u \in \mathcal{U_{reduc}} \st B(w_i,u) = l(w_i), \enspace i = 1,\cdots,n \\ \iff \enspace & \text{find } a \in \R^n \st B(w_i,\phi + a^ju^0_j) = l(w_i), \enspace i = 1,\cdots,n \end{aligned} $$

#### 참고
명제1을 통해 알 수 있듯이 $\mathcal{U_{reduc}}$는 vector space가 아니다.

Affine form라고 부르는 이유는  위의 형태를 보면 알 수 있듯이 $\mathcal{U_{reduc}}$가 affine space이기 때문이다.

#### 명제1
다음을 증명하여라.

$$\mathcal{U_{reduc}} \text{ is not a vector space }$$

**Proof**

$\partial\Omega_E$에서 0이 아닌 BC $g$가 주어졌다고 가정하자.

$$ u = g \neq 0 \quad \text{on } \partial\Omega_E $$

그러면 $u_1, u_2 \in \mathcal{U_{reduc}}$가 있을 때, $\forall a \in \partial\Omega$에서 다음이 성립한다.  

$$ (u_1 + u_2)(x) = u_1(x) + u_2(x) = 2g(x) $$

따라서, 다음이 성립한다. 

$$ u_1 + u_2 \notin \mathcal{U_{reduc}} $$

즉 $\mathcal{U_{reduc}}$는 덧셈에 대해 닫혀있지 않기 때문에 vector space가 될 수 없다. 

> Reference  
> [Note - M. J. Zahr](https://mjzahr.github.io/content/ame40541/spr20/ch03-wres-solo.pdf)

### 참고
Dimensional reduction한 solution function space가 충분히 넓지 않은경우 weak formulation을 통해 구한 solution은 natural BC를 만족하지 않는다.

즉, 약하게 적영된 natural BC는 만족되지 않을 수도 있다.

그 예시는 model problem2번을 통해 확인해 볼 수 있다.


## Final Form
따라서, dimensional reduction된 weak formulation의 최종 형태는 다음과 같다.

### Restrition form
$$ \text{find } a \in \R^n_s \st B(w_i,a^j u_j) = l(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces

$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_f} := \operatorname{span}\{u_1,\ldots,u_n\}\subseteq C^1(\Omega) \\ \R^n_s := \Set{a \in \R^n | \sum_{i=1}^n a^iu_i \text{ satisfies essential BC}} \end{gathered} $$

### Affine form
$$ \text{find } a \in \R^n \st B(w_i,\phi + a^ju^0_j) = l(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_{reduc}} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies essential BC}} \\ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u=0 \text{ on } \partial\Omega_E} = \span(\Set{u_i^0}) \end{gathered} $$

## Linear Case
만약, $\mathcal P$가 linear operator면 $B$은 bilinear map이 된다.

따라서, dimensional reduction한 weak fomrulation는 다음과 같이 단순해 진다.

### Restrition form
$$ \text{find } a \in \R^n_s \st B(w_i,u_j) a^j = l(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_f} := \operatorname{span}\{u_1,\ldots,u_n\}\subseteq C^1(\Omega) \\ \R^n_s := \Set{a \in \R^n | \sum_{i=1}^n a^iu_i \text{ satisfies essential BC}} \end{gathered} $$

#### Matrix form
$$ \begin{bmatrix} B(w_1,u_1) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_1) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} a^1 \\ \vdots \\ a^n \end{bmatrix} = \begin{bmatrix} l(w_1) \\ \vdots \\ l(w_n) \end{bmatrix} $$

#### Consider BC
Essential BC를 만족하기 위해 $a \in \R^n_s$여야 한다.

이를 위해 2가지 방식을 취할 수 있다.

##### Predefined DOF
만약 solution function space가 다음과 같다고 하자.

$$ u_i = x^{i-1} $$

그러면, $u(0) = u_0$를 만족하기 위해서는 다음이 성립한다.

$$ a^1 = u_0 $$

그러면 matrix form은 다음과 같아진다.

$$ \begin{bmatrix} B(w_1,u_1) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_1) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} u_0 \\ \vdots \\ a^n \end{bmatrix} = \begin{bmatrix} l(w_1) \\ \vdots \\ l(w_n) \end{bmatrix} $$

$u_0$는 이미 알고 있는 값임으로, 관련된 항을 우측으로 옮기면 다음과 같아진다.

$$ \begin{bmatrix} B(w_1,u_2) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_2) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} a_1 \\ \vdots \\ a^n \end{bmatrix} = \begin{bmatrix} l(w_1) \\ \vdots \\ l(w_n) \end{bmatrix} - u_0 \begin{bmatrix} B(w_1,u_1) \\ \vdots \\ B(w_n,u_1) \end{bmatrix} $$

이렇게 되면, 식은 $n$개 미지수는 $n-1$개인 overdetermined system이 된다.

Overdetermined system을 풀기위해서 식 하나를 제거해야 된다.

예를 들어, $n$번째 식을 제거하는 경우, 미지수를 나타내는 $\begin{bmatrix} a_1 & \cdots & a^n \end{bmatrix}^T$를 제외하고 전부 $n$번째 행을 제거하면 된다.

##### Additinoal Equation
Essential BC를 만족하기 위해서는 다음 식이 성립해야 한다.

$$ a^iu_i|_{x=0} = u_0 $$

따라서 이를 matrix form에 추가하면 다음과 같다.

$$ \begin{bmatrix} u_1(0) & \cdots & u_n(0) \\ B(w_1,u_1) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_1) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} a_1 \\ \vdots \\ a^n \end{bmatrix} = \begin{bmatrix} u_0 \\ l(w_1) \\ \vdots \\ l(w_n) \end{bmatrix} $$

이렇게 되면, 식은 $n+1$개 미지수는 $n$개인 overdetermined system이 된다.

Overdetermined system을 풀기위해서 식 하나를 제거해야 되며, 첫번째 식은 BC를 고려하기 위한 식임으로, 나머지 $n$개의 식중에 하나를 제거해야 된다.

##### 참고
BC를 고려하기 위해 식을 제거하는 경우, 그 식에 해당하는 solution function space의 basis를 고려하지 못하게 된다.

### Affine form
$$ \text{find } a \in \R^n \st B(w_i,u^0_j)a^j = l(w_i) - B(w_i,\phi), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_{reduc}} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies essential BC}} \\ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u=0 \text{ on } \partial\Omega_E} = \span(\Set{u_i^0}) \end{gathered} $$

#### Matrix form
$$ \begin{bmatrix} B(w_1,u^0_1) & \cdots & B(w_1,u^0_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u^0_1) & \cdots & B(w_n,u^0_n) \end{bmatrix} \begin{bmatrix} a^1 \\ \vdots \\ a^n \end{bmatrix} = \begin{bmatrix} l(w_1) - B(w_1, \phi) \\ \vdots \\ l(w_n) - B(w_n, \phi) \end{bmatrix} $$

