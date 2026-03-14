# Model Problem2
$$ \text{find } u \in \mathcal U_s \quad s.t. \quad \frac{d^2u}{dx^2} + 1 = 0 \quad \text{in} \enspace \Omega := [0,2] \subset \R $$

$$ \text{Where, } \mathcal U_s := \{ u \in C^2(\Omega) \enspace | \enspace u(0) = 1 \enspace \land \enspace u'(2) = 1 \} $$

> 참고  
> [book] (Kelly) An Introduction to the FEM chapter 2.3.2

## Exact solution
$$ u = 1 + 3x - 0.5x^2 $$

## Weak Formulation
Functional $B_r,l_r$은 다음과 같다.
$$ \begin{aligned} B_r(w,u) &:= \int_\Omega \frac{dw}{dx}\frac{du}{dx} \thinspace dV \\ l_r(w) &:= \int_\Omega w \thinspace dV + w\frac{du}{dx} \bigg |_{x=2} \end{aligned} $$

### Restriction form
$$ \text{find } x \in \R_b^n \st B_r(w_i,u_j) x^j = l_r(w_i), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_f} := \span(\set{u_1,\cdots,u_n}) \subset C^1(\Omega))  \\ \R^n_b := \Set{x \in \R^n | x^iu_i \text{ satisfy essential BC}} \end{gathered} $$

#### Matrix form
$$ \begin{bmatrix} B_r(w_1,u_1) & \cdots & B_r(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B_r(w_n,u_1) & \cdots & B_r(w_n,u_n) \end{bmatrix} \begin{bmatrix} x^1 \\ \vdots \\ x^n \end{bmatrix} = \begin{bmatrix} l_r(w_1) \\ \vdots \\ l_r(w_n) \end{bmatrix} $$

### Affine form
$$ \text{find } x \in \R^n \st B_r(w_i,u^0_j)x^j = l_r(w_i) - B_r(w_i,\phi), \enspace i = 1,\cdots,n $$

#### Related function spaces
$$ \begin{gathered} \mathcal{W_f} := \span(\set{w_1,\cdots,w_n}) \\ \mathcal{U_b} := \Set{ u\in\mathcal{U_f}| u \text{ satisfies essential BC}} \\ \mathcal{U_0} := \Set{ u\in\mathcal{U_f}| u=0 \text{ on } \partial\Omega_E} = \span(\Set{u_i^0}) \end{gathered} $$

#### Matrix form
$$ \begin{bmatrix} B_r(w_1,u^0_1) & \cdots & B_r(w_1,u^0_n) \\ \vdots & \ddots & \vdots \\ B_r(w_n,u^0_1) & \cdots & B_r(w_n,u^0_n) \end{bmatrix} \begin{bmatrix} x^1 \\ \vdots \\ x^n \end{bmatrix} = \begin{bmatrix} l_r(w_1) - B_r(w_1, \phi) \\ \vdots \\ l_r(w_n) - B_r(w_n, \phi) \end{bmatrix} $$

## 참고
$n=2, u_i = x^{i-1}$로 하면 natural BC를 만족하지 않는다. 

즉, natural BC가 식에 포함된 형태로 약하게 적용되어있기 때문에 solution space가 충분히 크지 않을 경우 natural BC를 만족하지 않을 수 있다.
