# Governing Equation

## Momentum Equations
연속체 $\Omega$가 있다고 하자.

$\Omega$에 Euler의 운동방정식과 Euler-Cauchy stress principle을 적용하면 다음과 같다.

$$ \int_{\partial\Omega} \sigma n \thinspace dS + \int_\Omega \rho  f_b \thinspace dV = 0 $$

이를 `적분형(integral form)` 운동방정식이라 한다.


### 명제1(Differential form)
연속체 $\Omega$가 있다고 하자.

$\rho u, \sigma$가 충분히 매끄럽다고 할 때, 다음을 증명하여라.

$$ \begin{aligned} & \int_{\partial\Omega} \sigma n \thinspace dS + \int_\Omega \rho f_b \thinspace dV = 0 \\ \iff \enspace & \mathrm{div}(\boldsymbol\sigma) + \rho  f_b = 0 \end{aligned}  $$

**Proof**

$\rho u$가 충분히 매끄러움으로, Integral form $\frac{d}{dt} \int_\Omega \rho u \thinspace dV$에 Leibniz integral theorem을 적용하면 다음과 같다.

$$ \int_{\partial\Omega} \sigma n \thinspace dS + \int_\Omega \rho  f_b \thinspace dV = 0 $$

이 때, $\sigma$도 충분히 매끄러움으로 $\int_{\partial\Omega} \sigma n \thinspace dS$에 divergence theorem을 적용하면 다음과 같다.

$$ \int_{\Omega} \text{div} (\sigma) +\rho  f_b \thinspace dV = 0 $$

위 식은 임의의 $\Omega$에서 성립해야 함으로 다음이 성립한다.

$$ \mathrm{div}(\boldsymbol\sigma) + \rho  f_b = 0 $$

따라서, 위 식은 적분형 운동방정식과 동일하며 이를 미분형 운동방정식이라 한다.$\qed$

### Dispalcement based equations
Linear elastic material의 변형이 작다고 가정하자.

Dispaclement를 $d$라 할 떄, infinitesimal strain tensor $\epsilon(d)$은 다음과 같다.

$$ \epsilon(d) = \epsilon_{ij}(d)e_{ij} = \frac{1}{2} \bigg( \frac{\partial d_i}{\partial x_j} + \frac{\partial d_j}{\partial x_i} \bigg)e_{ij} $$

Linear stiffness tensor를 $C$라고 할 떄, constitutive equation에 의해 다음이 성립한다.

$$ \sigma(d) = C:\epsilon $$

위 관계식을 momentume equations에 대입하면 다음과 같다

$$ \begin{aligned} & \int_{\partial\Omega}\sigma(d) n \thinspace dS + \int_\Omega \rho f_b \thinspace dV = 0 \\ \iff \enspace & \mathrm{div}(\boldsymbol\sigma(d)) + \rho  f_b = 0 \end{aligned}  $$

이를 `displacement based momentum equations`라고 한다.
