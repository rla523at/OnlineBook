# Weak Formulation
Displacement based momentum equations로 서술되는 boundary value problem(BVP)이 다음과 같이 주어졌다고 하자.

$$ \text{find} \enspace d \in \mathcal D^3 \st \text{div}(\sigma(d)) + \rho f_b = 0 $$

$$ \text{Where, } \mathcal{D} := \{ d_i \in C^2(\Omega) \enspace | \enspace d_i \text{ satisfies boundary condition on } \partial\Omega \}  $$

## Weighted residual formulation

BVP의 Weighted residual formulation은 다음과 같다.

$$ \text{find} \enspace d \in \mathcal D^3 \st \forall w \in (C^\infty_c)^3, \quad \int_{\Omega} \text{div}(\sigma(d)) \cdot w \thinspace dV + \int_{\Omega} \rho f_b \cdot w \thinspace dV = 0 $$

> Q. test vector function과 내적한 식의 해가 연립방정식의 해와 동일한가?






## Weak formulation
명제1에 의해 weighted residual formulation은 다음과 같아진다.

$$ \text{find} \enspace d \in \mathcal D^3 \st \forall w \in (C^\infty_c)^3, \quad \int_{\Omega} \sigma : \text{grad}(w) dV = \int_{\partial\Omega} t \cdot  w dS + \int _{\Omega} \rho f_b \cdot w dV $$

Divergence theorem에 의해 solution의 미분항이 weight로 넘어갔음으로, solution space의 regularity를 완화시킬수 있다.

따라서, regularity relaxation에 의해 유도된 weak formulation은 다음과 같다.

$$ \text{find} \enspace d \in (\mathcal D_W)^3 \st \forall w \in \mathcal W^3, \quad \int_{\Omega} \sigma(d) : \text{grad}(w) dV = \int_{\partial\Omega} t \cdot  w dS + \int _{\Omega} \rho f_b \cdot w dV $$


$$ \begin{aligned} \text{Where, } \mathcal{D}_W &:= \{ d_i \in C^1(\Omega) \enspace | \enspace d_i \text{ satisfies boundary condition on } \partial\Omega_E \}  \\ \mathcal W &:= \{ w \in C^\infty(\Omega) \enspace | \enspace \forall  x \in \partial\Omega_E, \quad w( x) = 0 \} \end{aligned}  $$

### 참고1(Principle of Virtual Displacement)
Weak formulation의 test function $w$를 `가상 변위(virtual displacement)` $\delta$로 보면 다음이 성립한다.

$$ \text{find} \enspace d \in (\mathcal D_W)^3 \st \forall \delta \in \mathcal W^3, \quad \int_{\Omega} \sigma(d) : \text{grad}(\delta) \thinspace  dV = \int_{\partial\Omega} t \cdot  \delta \thinspace dS + \int _{\Omega} \rho f_b \cdot \delta \thinspace dV $$


$$ \begin{aligned} \text{Where, } \mathcal{D}_W &:= \{ d_i \in C^1(\Omega) \enspace | \enspace d_i \text{ satisfies boundary condition on } \partial\Omega_E \}  \\ \mathcal W &:= \{ \delta_i \in C^\infty(\Omega) \enspace | \enspace \forall  x \in \partial\Omega_E, \quad \delta_i( x) = 0 \} \end{aligned} $$

이 떄, $\delta(\delta  d)$를 변형률로써 해석하면, 좌측항은 물리적으로 `내부 가상 일(internal virtual work)`, 우측항은 `외부 가상 일(external virtual work)`로 볼 수 있기 때문에 가상 일 원리라고 한다.

> Reference  
> {cite}`bathe`p.156

### 명제1
$\sigma, w$가 충분히 매끄러울 때, 다음을 증명하여라.

$$ \int_{\Omega} \text{div}(\sigma) \cdot w \thinspace dV = \int _{\partial\Omega} t \cdot  w dS - \int_{\Omega} \sigma : \text{grad}(w) dV$$

**Proof**

Divergence theorem에 의해 다음이 성립한다.

$$ \begin{aligned} \int_{\Omega} \text{div}(\sigma) \cdot w \thinspace dV &=  \int_{\Omega} \mathrm{div}(w^T \sigma) - \sigma : \text{grad}(w) \thinspace dV \\&= \int _{\partial\Omega} w^T \sigma n dS - \int_{\Omega} \sigma : \text{grad}(w) dV \\&= \int _{\partial\Omega} t \cdot  w dS - \int_{\Omega} \sigma : \text{grad}(w) dV \end{aligned} $$
