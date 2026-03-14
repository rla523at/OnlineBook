# Divergence Theorem

## 명제
다음을 증명하여라.
$$ \int_{\Omega} w_i\frac{\partial F_{ij}}{\partial x_j} \thinspace dV = \int_{\partial\Omega} w_i F_{ij} n_j \thinspace dS -\int_{\Omega} F_{ij} \frac{\partial w_i}{\partial x_j} \thinspace dV $$


**Proof**

$$ \frac{\partial}{\partial x_j} (w_i F_{ij}) =  F_{ij} \frac{\partial w_i}{\partial x_j} + w_i \frac{\partial  F_{ij}}{\partial x_j} $$

따라서, 다음이 성립한다.
$$ \begin{aligned} \int_\Omega \frac{\partial}{\partial x_j} (w_i F_{ij}) dV &= \int_{\Omega}  F_{ij} \frac{\partial w_i}{\partial x_j} + w_i \frac{\partial  F_{ij}}{\partial x_j} \thinspace dV \\ \Rightarrow \enspace \int_{\Omega} w_i \frac{\partial  F_{ij}}{\partial x_j} \thinspace dV &= \int_{\partial\Omega} w_i F_{ij}n_j \thinspace dS - \int_{\Omega}  F_{ij} \frac{\partial w_i}{\partial x_j} \thinspace dV \end{aligned} $$