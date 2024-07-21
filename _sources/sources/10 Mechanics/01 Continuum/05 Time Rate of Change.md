# Time Rate of Change
## Time Rate of Deformation
Reference figure $\Omega_0$와 deforemd firuge $\Omega$ 그리고 deformation $\varphi$가 있다고 하자.

$\varphi$를 시간으로 편미분하면 연속체의 한 점의 시간에 따른 변화량, 즉 `속도(velocity)` $v_m$를 얻게 된다.

$$ v_m(X,t) := \pdiff{\varphi}{t}(X,t) $$

### 참고
$v_m(X,t)$를 spatial description으로 나타낸 함수를 $v(x,t)$라고 하자.

그러면, 다음이 성립한다.

$$  v_m(X,t) = v(\varphi(X,t),t) $$

> Reference    
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter 3.12

## Time Rate of Deformation Gradient
### 명제1
Reference figure $\Omega_0$와 deforemd firuge $\Omega$ 그리고 deformation $\varphi$가 있다고 하자.

Deformation gradient $F$와 $\varphi$에 따른 속도 $v_m(X,t)$가 있을 때, 다음을 증명하여라.

$$ \frac{\partial F}{\partial t} = \nabla_m v_m  $$

**Proof**

$F$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} 
\frac{\partial F_{ij}}{\partial t} &= \pdiff{}{t}\pdiff{\varphi_i}{X_j} \\
                                   &= \pdiff{}{X_j}\pdiff{\varphi_i}{t} \\
                                   &= \pdiff{(v_m)_i}{X_j} \qed 
\end{aligned} $$

### 명제2
Deformation $\varphi$와 이에 따른 속도 $v(x,t)$가 있다고 하자.

Deformation gradient $F$가 있을 때, 다음을 증명하여라.
$$ \frac{\partial F}{\partial t} = F \nabla v $$

**Proof**

명제1과 $v$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned}
\pdiff{F_{ij}}{t} &= \pdiff{(v_m)_i}{X_j} \\
                  &= \pdiff{}{X_j}v(\varphi(X,t),t)_i \\
                  &= \pdiff{v_i}{\varphi_k}\pdiff{\varphi_k}{X_j} \\
                  &= \frac{\partial v_i}{\partial x_k} F_{kj} \qed 
\end{aligned} $$

#### 따름명제2.1
Deformation $\varphi$와 이에 따른 속도 $v_m(X,t), v(x,t)$가 있다고 하자.

이 때, 다음을 증명하여라.
$$ \nabla_mv_m = \nabla Fv $$

**Proof**

명제1과 2에의해 성립한다.

### 명제3
deformation $\varphi$와 deformation gradient $F$ 그리고 그에 따른 속도 $v(x,t)$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \pdiff{}{t} \det(F) = \det(F) \nabla \cdot v  $$

**Proof**

Jacobi's theorem과 $F$의 시간변화율에 의한 성질에 의해 다음이 성립한다.

$$ \begin{aligned} 
\pdiff{}{t} \det(F) &= \det(F)\text{tr}(F^{-1} \pdiff{F}{t} ) \\
                    &= \det(F)\text{tr}(F^{-1} F \nabla v) \\
                    &= \det(F)\text{tr}(\nabla v) \\
                    &= \det(F)\nabla \cdot v \qed 
\end{aligned} $$

> Reference  
> [wikipedia - Jacobi's formula](https://en.wikipedia.org/wiki/Jacobi%27s_formula)  

## Other Time Rates

### 명제1
Deformation $\varphi$와 이에 따른 속도 $V(X,t)$가 있다고 하자.

점 $X \in \Omega_0$와 vector $\Delta X$가 있을 때, 점 $x \in \Omega$와 vector $\Delta x$를 다음과 같이 정의하자.

$$ \begin{aligned} 
       x &:= \varphi(X,t) \\ 
\Delta x &:= \varphi(X + \Delta X, t) - \varphi(X, t) 
\end{aligned} $$

$\Delta X$가 충분히 작을 때, 다음을 증명하여라.

$$ \pdiff{}{t} \Delta x = (\nabla_mv_m) \Delta X $$

**Proof**

$\Delta x$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} 
\pdiff{}{t} \Delta x &= \pdiff{}{t} \varphi(X + \Delta X, t) - \pdiff{}{t} \varphi(X, t) \\ 
                     &= v_m(X + \Delta X,t) - v_m(X,t) 
\end{aligned} $$

$\Delta X$가 충분히 작아 $V(X + \Delta X,t)$를 선형으로 근사할 수 있다고 가정하면 다음과 같다.

$$ \begin{aligned} 
\pdiff{}{t} \Delta x &= v_m(X + \Delta X,t) - v_m(X,t) \\
                     &= v_m(X,t) + \nabla_X v_m \Delta X - v_m(X,t) \\
                     &= (\nabla_m v_m) \Delta X \qed 
\end{aligned} $$

> Reference    
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter 3.12

### 명제2
Deformation $\varphi$와 이에 따른 속도 $v(x,t)$가 있다고 하자.

점 $X \in \Omega_0$와 vector $\Delta X$가 있을 때, 점 $x \in \Omega$와 vector $\Delta x$를 다음과 같이 정의하자.

$$ \begin{aligned} 
       x &:= \varphi(X,t) \\ 
\Delta x &:= \varphi(X + \Delta X, t) - \varphi(X, t) 
\end{aligned} $$

$\Delta x$가 충분히 작을 때, 다음을 증명하여라.

$$ \pdiff{}{t} \Delta x = (\nabla v) \Delta x $$

**Proof1**

명제1과 $v$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} 
\pdiff{}{t}\Delta x &= v_m(X + \Delta X,t) - v_m(X,t) \\
                    &= v (\varphi(X + \Delta X,t), t) - v(\varphi(X,t),t) \\
                    &= v(x + \Delta x, t) - v(x, t)  
\end{aligned} $$

$\Delta x$가 충분히 작아 $v(x + \Delta x,t)$를 선형으로 근사할 수 있음으로 다음이 성립한다

$$ \begin{aligned} 
\pdiff{}{t} \Delta x  &= v(x + \Delta x,t) - v(x,t) \\
                      &= v(x,t) + \nabla v \Delta x - v(x,t) \\
                      &= \nabla v \Delta x \qed 
\end{aligned} $$

**Proof2**

명제1과 velocity gradient의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} 
\pdiff{}{t}\Delta x &= \nabla_m v_m \Delta X \\
                    &= \nabla v F \Delta X \\
                    &= \nabla v \Delta x \qed
 \end{aligned} $$

> Reference    
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter 3.12

### 명제3
Reference figure $\Omega_0$, deformation $\varphi$, defromation gradient $F$가 있다고 하자.

점 $X \in \Omega_0$와 vector $\Delta X$가 있을 때, 점 $x \in \Omega$와 vector $\Delta x$를 다음과 같이 정의하자.

$$ \begin{aligned} 
       x &:= \varphi(X,t) \\ 
\Delta x &:= \varphi(X + \Delta X, t) - \varphi(X, t) 
\end{aligned} $$

이 때, vector $\Delta x$를 크기 $l$과 단위 vector $m$으로 나타내자.

$$ \Delta x = lm $$

 $\Delta X$가 충분히 작을 때, 다음을 증명하여라.

$$ \frac{1}{l} \pdiff{l}{t} = m \cdot (\nabla v)m $$

**Proof1**

$\Delta x$의 정의에 의해 다음이 성립한다. 

$$ \begin{aligned} 
          & l^2 = \Delta x \cdot \Delta x \\ 
\implies  & 2 l \pdiff{l}{t} = 2 \Delta x \cdot \pdiff{}{t} \Delta x \\ 
\implies  & l \pdiff{l}{t} = l^2 m \cdot (\nabla v) m \\ 
\implies  & \frac{1}{l} \pdiff{l}{t} = m \cdot (\nabla v) m \qed \end{aligned} $$

**Proof2**

vector $\Delta X$를 크기 $l_0$와 단위 vector $n$으로 나타내자.
$$ \Delta X = l_0n $$

그러면 deformation gradient의 성질에 의해 다음이 성립한다.
$$ l = \lVert Fn \rVert l_0, \enspace m = \frac{Fn}{\lVert Fn \rVert} $$

따라서, 다음이 성립한다.
$$ \begin{aligned} \frac{\partial l}{ \partial t} &= l_0 \pdiff{}{t} (Fn \cdot Fn)^{1/2} \\&= l_0 \frac{1}{2} (Fn \cdot Fn)^{-1/2} \pdiff{}{t} (Fn \cdot Fn) \\&= l_0 \frac{1}{\lVert Fn \rVert} Fn \cdot (\pdiff{}{t} F)n \\&= l_0m \cdot \nabla _x vFn \\&= l m \cdot (\nabla_xv) m \\ \frac{1}{l} \pdiff{l}{t} &= m \cdot (\nabla_xv)m \qed \end{aligned} $$

### 명제4
deformation $\varphi$와 그에 따른 속도 $v(x,t)$가 있다고 하자.

$X \in \Omega_0$와 vector $\Delta X_{1,2,3}$가 있을 때, $x \in \Omega$와 vector $\Delta x_{1,2,3}$를 다음과 같이 정의하자.
$$ \begin{aligned} x &:= \varphi(X,t) \\ \Delta x_{1,2,3} &:= \varphi(X + \Delta X_{1,2,3}, t) - \varphi(X, t) \end{aligned} $$

이 떄, vector $\Delta x_{1,2,3}$로 이루어진 부피를 $\rm V$ 표현하자.
$$ \rm V:= \Delta  x_1 \cdot \Delta  x_2 \times \Delta  x_2$$

$\Delta X_{1,2,3}$가 충분히 작을 때, 다음을 증명하여라.
$$ \frac{1}{\rm V} \frac{\partial \rm V}{\partial t} = \text{div}(v) $$

**Proof**

Vector $\Delta X_{1,2,3}$로 이루어진 부피를 $\rm V_0$라 하자.
$$ \mathrm V_0 := \Delta X_1 \cdot \Delta X_2 \times \Delta X_3 $$

Deformation gradient를 $F$라 할 때, $\Delta X_{1,2,3}$가 충분히 작기 때문에 다음이 성립한다.
$$ \begin{aligned} \mathrm V &= \det(F) \mathrm V_0 \\ \implies \frac{\partial \rm V}{\partial t} &= \frac{\partial \det(F)}{\partial t} \mathrm V_0 \end{aligned}  $$

Jacobi's Theorem과 $F$의 시간변화율에 대한 성질에 의해 다음이 성립한다.
$$ \begin{aligned} \frac{\partial \rm V}{\partial t} &= \frac{\partial \det(F)}{\partial t} \mathrm V_0 \\&= \mathrm V \text{tr} \bigg( \frac{\partial F}{\partial t} F^{-1} \bigg) \\ \frac{1}{\mathrm V}\frac{\partial \rm V}{\partial t} &= \text{tr}(\nabla_x v) \\&= \text{div}(v) \qed \end{aligned}  $$