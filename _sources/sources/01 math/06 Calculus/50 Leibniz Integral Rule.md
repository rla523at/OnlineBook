# Libniz Integral Rule
## 1D

> Referebce  
> [Wiki - Leibniz integral rule ](https://en.wikipedia.org/wiki/Leibniz_integral_rule)  

## Multi Dimension
다차원에서 일반화된 Leibniz integral rule은 다음과 같다.

$$ \begin{aligned} 
     & \frac{D}{Dt} \int_{\Omega(t)} f(x,t) \thinspace dV \\
    =& \int_{\Omega(t)} \left( \frac{Df}{Dt} + f (\nabla \cdot v) \right) \thinspace \\ 
    =& \int_{\Omega(t)} \pdiff{f}{t} \thinspace dV + \int_{\partial\Omega(t)} f(v(x,t) \cdot n(x,t)) \thinspace dS 
\end{aligned} $$

이 때, $n$은 $\partial\Omega(t)$의 outward unit normal vector이며 $v$는 $x$에서의 $\Omega(t)$의 velocity vector이다.

### Derivation
Reference figure $\Omega_0$, deforemd firuge $\Omega$, Deformation $\varphi$ 그리고 deformation gradient $F$가 있다고 하자.

Deformation gradient의 성질과 change of variable에 의해 다음이 성립한다.

$$ \begin{aligned} 
\int_{\Omega} f(x,t) \thinspace dV &= \int_{\Omega_0} f(\varphi(X,t),t) \det(F) \thinspace dV_0 \\
                                   &= \int_{\Omega_0} f_m J \thinspace dV_0 
\end{aligned} $$

$$ \text {Where, } f_m(X,t) := f(\varphi(X,t),t), \enspace J := \det(F)$$

미분의 정의에 의해, 다음이 성립한다.

$$ \begin{aligned} 
\frac{D}{Dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \lim_{\Delta t \rightarrow 0} \frac{1}{\Delta t} \left( \int_{\Omega(t + \Delta t)} f(x,t + \Delta t) \thinspace dV  - \int_{\Omega(t)} f(x,t) \thinspace dV \right) \\
&= \lim_{\Delta t \rightarrow 0} \frac{1}{\Delta t} \left( \int_{\Omega_0} f_m(X,t + \Delta t) J(X, t + \Delta t) - f_m J \thinspace dV_0 \right) \\
&= \int_{\Omega_0} \left( \lim_{\Delta t \rightarrow 0} \frac{ f_m(X,t + \Delta t) J(X, t + \Delta t) - f_m J \thinspace dV_0}{\Delta t} \right) \thinspace dV_0 \\
&= \int_{\Omega_0} \frac{\partial}{\partial t} (f_m J) \thinspace dV_0  
\end{aligned} $$

$F$의 시간변화율의 성질에 의해, 다음이 성립한다.

$$ \begin{aligned} 
\frac{D}{Dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \int_{\Omega_0} \frac{\partial}{\partial t} (f_m J) \thinspace dV_0 \\
&= \int_{\Omega_0} \left( J\frac{\partial f_m}{\partial t} + f_m\frac{\partial J}{\partial t} \right) \thinspace dV_0 \\
&= \int_{\Omega_0} \left( J\frac{\partial f_m}{\partial t} + f_m J (\nabla \cdot v) \right) \thinspace dV_0 \\
&= \int_{\Omega_0} \left( \frac{\partial f_m}{\partial t} + f_m (\nabla \cdot v) \right) J \thinspace dV_0 \\
&= \int_{\Omega_0} \left( \frac{Df}{Dt} + f (\nabla \cdot v) \right) J \thinspace dV_0 \\
&= \int_{\Omega} \left( \frac{Df}{Dt} + f (\nabla \cdot v) \right) \thinspace dV 
\end{aligned} $$

Material derivative의 성질에 의해, 다음이 성립한다.

$$ \begin{aligned} 
\frac{D}{Dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \int_{\Omega} \left( \frac{Df}{Dt} + f (\nabla \cdot v) \right) \thinspace dV \\
&= \int_{\Omega} \left( \frac{\partial f}{\partial t} + (\nabla f) v + f (\nabla \cdot v) \right) \thinspace dV \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\Omega} \left( (\nabla f) v + f (\nabla \cdot v) \right) \thinspace dV \end{aligned} $$

Tensor product의 identity와 divergence theorem에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{D}{Dt} \left( \int_{\Omega} f(x,t) \thinspace dV \right) &= \int_{\Omega} \frac{\partial f}{\partial t}   \thinspace dV + \int_{\Omega} \left( (\nabla f) v + f (\nabla \cdot v) \right) \thinspace dV \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\Omega} \nabla \cdot (f \otimes v) \thinspace dV \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\partial\Omega} (f \otimes v)  n \thinspace dS \\&= \int_{\Omega} \frac{\partial f}{\partial t} \thinspace dV + \int_{\partial\Omega}  (v \cdot n) f \thinspace dS \end{aligned} $$

> Reference  
> [Wiki - Reynolds transport theorem](https://en.wikipedia.org/wiki/Reynolds_transport_theorem)  

#### 참고
deformation $\varphi$에 의해서 다음이 성립한다

$$ f(x,t) = f(\varphi(X,t),t) = f_m(X,t) $$

하지만 이 때, 햇갈리지 말아야 하는 것은 다음이다.

$$ \pdiff{f}{t} \neq \pdiff{f_m}{t} $$

위가 다른 이유는 $\pdiff{}{t}$를 할 떄, 무엇을 고정하는지를 잘 봐야 한다.

$$ \pdiff{f}{t} \bigg|_{x= \text{const}} \neq \pdiff{f_m}{t} \bigg|_{X= \text{const}} $$

두 함수 모두 $X=\text{const}$로 두는 material derivate를 할 경우 다음 관계식이 성립한다.

$$ \pdiff{f_m}{t}  =  \frac{Df}{Dt} = \nabla fv + \pdiff{f}{t} $$


### Material volume & Control volume
$\Omega_0$를 reference figure라고 하고 deformed figure $\Omega(t)$가 있다고 하자.

이 떄, 특정 순간의 deformed figure의 volume과 surface를 각각 control volume과 constrol surface라고 한다.

예를 들어, $t = t_1$일 때의 control volume과 control surface는 다음과 같다.

$$ \Omega_c := \Omega(t_1), \quad \partial\Omega_c := \partial\Omega(t_1) $$

따라서, $t = t_1$에서 Leibniz integral rule은 다음과 같이 표현할 수 있다.

$$ \frac{D}{Dt} \left( \int_{\Omega_m} f \thinspace dV \right) \bigg|_{t= t_1} = \int_{\Omega_c} \pdiff{f}{t} \bigg|_{t= t_1} \thinspace dV + \int_{\partial\Omega_c} f(v_s \cdot n) \thinspace dS $$

이 때, $v_s(x,t) = v_m(X,t)$는 $\Omega_m$에 속한 물질의 속도이다.

#### 참고



> Reference  
> [Book] (Lai et al) Introduction to Continuum Mechanics chap 7.4   
