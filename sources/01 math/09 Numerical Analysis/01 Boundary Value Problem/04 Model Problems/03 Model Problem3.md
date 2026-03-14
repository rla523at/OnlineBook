# Model Problem3
$$ \text{find } u \in \mathcal U_s \quad s.t. \quad \frac{d^2u}{dx^2} + u - x = 0 \quad \text{in} \enspace \Omega := [0,2] \subset \R $$

$$ \text{Where, } \mathcal U_s := \{ u \in C^2(\Omega) \enspace | \enspace u(0) = 0 \enspace \land \enspace u(2) = 5 \} $$

exact solution은 $u = \frac{3}{\sin 2}\sin  x + x$이다.

## Weighted residual formulation
$$ \text{find } a \in \R^k \quad s.t. \quad B(w_i,\phi + a_ju_j) = l(w_i) $$

$$ \begin{aligned} \text{Where, } & i = 1,\cdots,n, \enspace j = 1,\cdots,k \\& \phi \in \mathcal{U}_s \\& u_i \in \{ u \in C^2(\Omega) \enspace | \enspace u=0 \text{ on } \partial\Omega_E \} \\& B(w,u) := \int_\Omega w \bigg( \frac{d^2u}{dx^2} + u \bigg) \thinspace dV \\& l(w) := \int_\Omega wx \thinspace dV\end{aligned} $$

## Weak formulation
$$ \text{find } a \in \R^k \quad s.t. \quad B(w_i,\phi + a_ju_j) = 0 $$

$$ \begin{aligned} \text{Where, } & i = 1,\cdots,n, \enspace j = 1,\cdots,k \\& \exist w_i \quad s.t. \quad w(2) \neq 0 \\& \phi \in \{ u \in C^1(\Omega) \enspace | \enspace u \text{ staisfy essential BC } \} \\& u_i \in \{ u \in C^1(\Omega) \enspace | \enspace u=0 \text{ on } \partial\Omega_E \} \\& B(w,u) := \int_\Omega wu -\frac{dw}{dx}\frac{du}{dx} \thinspace dV + w\frac{du}{dx} \bigg |^{2}_{x=0} \\& l(w) := \int_\Omega wx \thinspace dV \end{aligned} $$

## Matrix form
$B$가 bi-linear 임으로
$$ \text{find } a \in \mathcal \R^k \quad s.t. \quad a_jB(w_i,u_j) = l(w_i) - B(w_i, \phi) $$

행렬식으로 나타내면
$$ \begin{bmatrix} B(w_1,u_1) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_1) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} = \begin{bmatrix} l(w_1) - B(w_1, \phi) \\ \vdots \\ l(w_n) - B(w_n, \phi) \end{bmatrix} $$