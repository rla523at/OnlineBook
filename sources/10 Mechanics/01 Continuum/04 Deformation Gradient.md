# Deformation Gradient
## Definition
Reference figure $\Omega_0$와 deforemd firuge $\Omega$ 그리고 Deformation $\varphi$가 있다고 하자.

`Deformation gradient` $F$는 다음과 같이 정의한다.

$$ F:= \frac{\partial \varphi}{\partial X} = \nabla_m\varphi $$

### 명제1
Deformation gradient $F$가 있다고 하자.

Displacment vector를 $d$라 할 떄, 다음을 증명하여라.

$$ F = I + \nabla d  $$

**Proof**

$F$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} 
F &= \frac{\partial \varphi}{\partial X} \\
  &= \frac{\partial (X + d)}{\partial X} \\
  &= \frac{\partial X}{\partial X} + \frac{\partial d}{\partial X} \\
  &= I + \nabla d \quad {_\blacksquare}
\end{aligned} $$

### 명제2
Reference figure $\Omega_0$와 deforemd firuge $\Omega$ 그리고 Deformation $\varphi$가 있다고 하자.

점 $X \in \Omega_0$와 vector $\Delta X$가 있을 때, 점 $x \in \Omega$와 vector $\Delta x$를 다음과 같이 정의하자.

$$ \begin{aligned} x &:= \varphi(X,t) \\ \Delta x &:= \varphi(X + \Delta X, t) - \varphi(X, t) \end{aligned} $$

$\Delta X$가 충분히 작을 떄, 다음을 증명하여라.

$$ \Delta  x = F \Delta X $$

**Proof**

$\Delta X$가 충분히 작아 $\varphi (X + \Delta X, t)$를 선형으로 근사할 수 있다고 가정하면 $\Delta  x$는 다음과 같다.

$$ \begin{aligned} \Delta  x & \approx \nabla \varphi \Delta X \\&=  F \Delta X \end{aligned} $$

### 명제3(Change of length)
Reference figure $\Omega_0$, deformation $\varphi$, defromation gradient $F$가 있다고 하자.

$X \in \Omega_0$와 vector $\Delta X$가 있을 때, $x \in \Omega$와 vector $\Delta x$를 다음과 같이 정의하자.

$$ \begin{aligned} x &:= \varphi(X,t) \\ \Delta x &:= \varphi(X + \Delta X, t) - \varphi(X, t) \end{aligned} $$

이 때, $\Delta X, \Delta x$를 크기 $l_0, l$과 단위 vector $n,m$으로 표현하자.

$$ \begin{aligned} \Delta X &= l_0n  \\ \Delta x &= lm \end{aligned} $$

$\Delta X$가 충분히 작을 때, 다음을 증명하여라.

$$ l = \lVert Fn \rVert l_0, \enspace m = \frac{Fn}{\lVert Fn \rVert} $$

**Proof**

$\Delta X$가 충분히 작음으로 명제2에 의해 다음이 성립한다.

$$ \begin{aligned} & \Delta  x =  F \Delta X \\ \Rightarrow \enspace &  l  m = l_0  F  n \end{aligned} $$

$m$은 $Fn$과 평행하고 단위방향 벡터임으로 다음이 성립한다.

$$  m = \frac{ F  n}{\lVert  F  n \rVert} $$

이를 활용하면 다음이 성립한다.

$$ \begin{aligned} & lm = l_0Fn \\ \Rightarrow \enspace & lFn = \lVert  F  n \rVert l_0  F  n \\ \Rightarrow \enspace & l = \lVert  F  n \rVert l_0 \quad {_\blacksquare} \end{aligned} $$

### 명제4(Change of area)
$X \in \Omega_0$와 vector $\Delta X_{1,2}$가 있을 때, $x \in \Omega$와 vector $\Delta x_{1,2}$를 다음과 같이 정의하자.

$$ \begin{aligned} x &:= \varphi(X,t) \\ \Delta x_1 &:= \varphi(X + \Delta X_1, t) - \varphi(X, t) \\ \Delta x_2 &:= \varphi(X + \Delta X_2, t) - \varphi(X, t) \end{aligned} $$

이 떄, vector $\Delta X_{1,2}$로 이루어진 면과 $\Delta x_{1,2}$로 이루어진 면을 크기 $A_0,A$와 단위 vector $n,m$으로 표현하자.

$$ \Delta X_1 \times \Delta X_2 = A_0  n, \enspace \Delta  x_1 \times \Delta  x_2 = A  m$$

$\Delta X$가 충분히 작을 때, 다음을 증명하여라.

$$ A = \lVert  F^{-T}  n \rVert \det( F) A_0, \enspace  m = \frac{{F^{-T}n}}{\lVert {F^{-T}n} \rVert} $$

**Proof**

$\Delta X$가 충분히 작음으로 명제2에 의해 다음이 성립한다.

$$ \begin{aligned} & A  m = ( F \Delta X_1) \times ( F \Delta X_2) \\ \Rightarrow \enspace &  A  F  n \cdot   m =  F  n \cdot ( F \Delta X_1) \times ( F \Delta X_2) \\ \Rightarrow \enspace &  A  n \cdot  F^T  m = \det(  {FX}) \\ \Rightarrow \enspace &  A   F^T  m = \det( F)\det( X)  n \\ \Rightarrow \enspace &  A   m = \det( F) A_0  F^{-T}  n \end{aligned} $$

$$ \text{Where, }  X = \begin{bmatrix}  n & \Delta X_1 & \Delta X_2 \end{bmatrix} $$

$m$은 $F^{-T}n$과 평행하고 단위방향 벡터임으로 다음이 성립한다.

$$  m = \frac{ F^{-T}  n}{\lVert  F^{-T}  n \rVert} $$

이를 활용하면 다음이 성립한다.

$$ \begin{aligned} &  A  m = A_0 \det( F)  F^{-T}  n \\ \Rightarrow \enspace & A  F^{-T}  n = \lVert  F^{-T}  n \rVert \det( F) A_0  F^{-T}  n \\ \Rightarrow \enspace & A = \lVert  F^{-T}  n \rVert \det( F) A_0 \quad {_\blacksquare} \end{aligned} $$

### 명제5(Change of volume)
$X \in \Omega_0$와 vector $\Delta X_{1,2,3}$가 있을 때, $x \in \Omega$와 vector $\Delta x_{1,2,3}$를 다음과 같이 정의하자.

$$ \begin{aligned} x &:= \varphi(X,t) \\ \Delta x_{1,2,3} &:= \varphi(X + \Delta X_{1,2,3}, t) - \varphi(X, t) \end{aligned} $$

이 떄, vector $\Delta X_{1,2,3}$로 이루어진 부피와 $\Delta x_{1,2,3}$로 이루어진 부피를 크기 $V_0,V$ 표현하자.

$$ \Delta X_1 \cdot \Delta X_2 \times \Delta X_3 = V_0 , \enspace \Delta  x_1 \cdot \Delta  x_2 \times \Delta  x_2 = V$$

$\Delta X$가 충분히 작을 때, 다음을 증명하여라.

$$ V = \det( F) V_0 $$

**Proof**

$\Delta X$가 충분히 작음으로 명제 2에 의해 다음이 성립한다.

$$ \begin{aligned} V &= (F\Delta X_1) \cdot (F\Delta X_2) \times (F\Delta X_3) \\ &=  \det({FX}) \\&= \det(F)\det(X) \\ &= \det( F) V_0 \quad {_\blacksquare} \end{aligned} $$

$$ \text{Where, }  X = \begin{bmatrix} \Delta X_1 & \Delta X_2 & \Delta X_3 \end{bmatrix} $$