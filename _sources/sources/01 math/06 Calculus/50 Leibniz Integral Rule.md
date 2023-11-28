# Libniz Integral Rule
## 1D

> Referebce  
> [Wiki - Leibniz integral rule ](https://en.wikipedia.org/wiki/Leibniz_integral_rule)  

## Multi Dimension
다차원에서 일반화된 Leibniz integral rule은 다음과 같다.

$$ \frac{d}{dt} \int_{\Omega(t)} f(x,t) \thinspace dV = \int_{\Omega(t)} \frac{\partial f}{\partial t} \thinspace dV + \int_{\partial\Omega(t)} (v(x,t) \cdot n(x,t))f \thinspace dS$$

이 때, $n$은 $\partial\Omega(t)$의 outward unit normal vector이며 $v$는 $x$에서의 $\Omega(t)$의 velocity vector이다.

### Derivation
Reference figure $\Omega_0$와 deforemd firuge $\Omega$가 있다고 하자.

Deformation $\varphi$를 다음과 같이 정의하자.

$$ \varphi : \Omega_0 \times \R^+ \rightarrow \Omega \quad s.t. \quad (X,t) \mapsto x $$

따라서 deformation gradient $F$는 다음과 같다.

$$ F(X,t) = \nabla_X \varphi $$

Deformation gradient의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} \int_{\Omega} f(x,t) \thinspace dV &= \int_{\Omega_0} f(\varphi(X,t),t) \det(F) \thinspace dV_0 \\&= \int_{\Omega_0} f_m J \thinspace dV_0 \end{aligned} $$


$$ \text {Where, } f_m(X,t) := f(\varphi(X,t),t), \enspace J := \det(F)$$

미분의 정의에 의해, 다음이 성립한다.

$$ \begin{aligned} \frac{d}{dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \lim_{\Delta t \rightarrow 0} \frac{1}{\Delta t} \left( \int_{\Omega(t + \Delta t)} f(x,t + \Delta t) \thinspace dV  - \int_{\Omega(t)} f(x,t) \thinspace dV \right) \\&= \lim_{\Delta t \rightarrow 0} \frac{1}{\Delta t} \left( \int_{\Omega_0} f_m(X,t + \Delta t) J(X, t + \Delta t) - f_m J \thinspace dV_0 \right) \\&= \int_{\Omega_0} \left( \lim_{\Delta t \rightarrow 0} \frac{ f_m(X,t + \Delta t) J(X, t + \Delta t) - f_m J \thinspace dV_0}{\Delta t} \right) \thinspace dV_0 \\&= \int_{\Omega_0} \frac{\partial}{\partial t} (f_m J) \thinspace dV_0  \end{aligned} $$

$F$의 시간변화율의 성질에 의해, 다음이 성립한다.

$$ \begin{aligned} \frac{d}{dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \int_{\Omega_0} \frac{\partial}{\partial t} (f_m J) \thinspace dV_0 \\&= \int_{\Omega_0} \left( J\frac{\partial f_m}{\partial t} + f_m\frac{\partial J}{\partial t} \right) \thinspace dV_0 \\&= \int_{\Omega_0} \left( J\frac{\partial f_m}{\partial t} + f_m J (\nabla_x \cdot v) \right) \thinspace dV_0 \\&= \int_{\Omega_0} \left( \frac{\partial f_m}{\partial t} + f_m (\nabla_x \cdot v) \right) J \thinspace dV_0 \\&= \int_{\Omega} \left( \frac{Df}{Dt} + f (\nabla_x \cdot v) \right) \thinspace dV \end{aligned} $$

Material derivative의 성질에 의해, 다음이 성립한다.

$$ \begin{aligned} \frac{d}{dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \int_{\Omega} \left( \frac{Df}{Dt} + f (\nabla_x \cdot v) \right) \thinspace dV \\&= \int_{\Omega} \left( \frac{\partial f}{\partial t} + (\nabla_x f) v + f (\nabla_x \cdot v) \right) \thinspace dV \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\Omega} \left( (\nabla_x f) v + f (\nabla_x \cdot v) \right) \thinspace dV \end{aligned} $$

Tensor product의 identity와 divergence theorem에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{d}{dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \int_{\Omega} \frac{\partial f}{\partial t}   \thinspace dV + \int_{\Omega} \left( (\nabla_x f) v + f (\nabla_x \cdot v) \right) \thinspace dV \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\Omega} \nabla_x \cdot (f \otimes v) \thinspace dV \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\partial\Omega} (f \otimes v)  n \thinspace dS \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\partial\Omega}  (v \cdot n) f \thinspace dS \end{aligned} $$

> Reference  
> [Wiki - Reynolds transport theorem](https://en.wikipedia.org/wiki/Reynolds_transport_theorem)  


### Material volume & Control volume
$\Omega_0$를 reference figure라고 하고 Material volume $\Omega_m(t)$와 control volume $\Omega_c$를 다음과 같이 정의하자.

$$ \Omega_m(t) := \{ x(X,t) \enspace | \enspace X \in \Omega_0 \} \\ \Omega_c := \Omega_m(t_0) $$

$\Omega = \Omega_m$이라 하면 $t = t_0$에서 Leibniz integral rule은 다음과 같이 표현할 수 있다.

$$ \begin{aligned} & \frac{d}{dt} \int_{\Omega_m} f \thinspace dV \bigg|_{t= t_0} = \int_{\Omega_c} \frac{\partial}{\partial t} f \bigg|_{t= t_0} \thinspace dV + \int_{\partial\Omega_c} f(v_s \cdot n) \thinspace dS \end{aligned} $$

이 때, $v_s(x,t) = v_m(X,t)$는 $\Omega_m$에 속한 물질의 속도이다.

> [Book] (Lai et al) Introduction to Continuum Mechanics chap 7.4  
