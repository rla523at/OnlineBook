# Model Problem1
$$ \text{find } u \in \mathcal U_s \quad s.t. \quad \frac{du}{dx} - 2xu = 0 \quad \text{in} \enspace \Omega := [0,2] \subset \R $$

$$ \text{Where, } \mathcal U_s := \{ u \in C^2(\Omega) \enspace | \enspace u(0) = 2 \} $$

exact solution은 $u = 2e^{x^2}$이다.

## Weighted Residual Formulation
$$ \text{find } a \in \R^k \quad s.t. \quad B(w_i,\phi + a_ju_j) = 0 $$

$$ \begin{aligned} \text{Where, } & i = 1,\cdots,n, \enspace j = 1,\cdots,k \\& \phi \in \mathcal{U}_s \\& u_i \in \{ u \in C^1(\Omega) \enspace | \enspace u=0 \text{ on } \partial\Omega \}  \\& B(w,u) := \int_\Omega w \bigg( \frac{du}{dx} -2xu \bigg) \thinspace dV \end{aligned} $$

## Weak formulation
$$ \text{find } a \in \R^k \quad s.t. \quad B(w_i,\phi + a_ju_j) = 0 $$

$$ \begin{aligned} \text{Where, } & i = 1,\cdots,n, \enspace j = 1,\cdots,k \\& \phi \in \{ u \in C^0(\Omega) \enspace | \enspace u \text{ staisfy essential BC } \} \\& u_i \in \{ u \in C^0(\Omega) \enspace | \enspace u=0 \text{ on } \partial\Omega_E \}  \\& B(w,u) := wu \Big|_0^2 - \int_\Omega \bigg( \frac{dw}{dx} + 2xw \bigg) u \thinspace dV \\&  \end{aligned} $$

## Matrix form
$B$가 bi-linear 임으로
$$ \text{find } a \in \mathcal \R^k \quad s.t. \quad a_jB(w_i,u_j) = - B(w_i, \phi) $$

행렬식으로 나타내면
$$ \begin{bmatrix} B(w_1,u_1) & \cdots & B(w_1,u_n) \\ \vdots & \ddots & \vdots \\ B(w_n,u_1) & \cdots & B(w_n,u_n) \end{bmatrix} \begin{bmatrix} a_1 \\ \vdots \\ a_n \end{bmatrix} = \begin{bmatrix} b - B(w_1, \phi) \\ \vdots \\ b - B(w_n, \phi) \end{bmatrix} $$

## 참고
* WRF_Galerkin method의 경우 $\phi = 2, \enspace u_i = x^i$라 하면 $n=7$정도 되면 비슷함
* WRF_Galerkin method랑 WF_Galerkin method랑 같은 결과가 나옴