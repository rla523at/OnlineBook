# Stress
## Principal Direction & Principal Stress
주어진 $x,t$에서 응력텐서 $\boldsymbol{\sigma}$가 주어졌다고 하자.

$\boldsymbol{\sigma}$의 eigen vectors를 `principal direction`, eigen values를 `principal stresses`라고 한다.

### 참고1
$\R^3$공간의 standard basis를 $\epsilon$이라 하고 $\frak m^\epsilon_\epsilon(\boldsymbol\sigma) = \sigma$라 하면 $\sigma$는 symmetric matrix이기 때문에 orthonormal한 3개의 eigen vector를 갖는다.

### 참고2
Principal direction을 normal vector로 갖는 평면을 `principal plane`이라고 한다.

principal plane에서는 $\boldsymbol{\sigma}$가 해당하는 principal stress만큼 scalar multiplication으로 작용한다. 다시 말해, principal stress $\sigma_1$를 갖는 principal direction $v$를 normal vector로 갖는 평면의 stress vector는 다음과 같다.

$$ t_v = \boldsymbol{\sigma}(v) = \sigma_1 v $$

즉, stress vector가 평면에 수직한 성분만 있으며, 평면에 접선방향 성분인 shearing stress는 존재하지 않는다.

### 명제1
주어진 $x,t$에서 응력텐서 $\boldsymbol{\sigma}$가 주어졌다고 하자.

$\sigma_1,\sigma_2,\sigma_3$를 principal stress라고 했을 때, 다음을 증명하여라.

$$ \sigma_1 = \sigma_2 = \sigma_3 = \sigma \Leftrightarrow \text{every basis is a principal directions} $$

**Proof**

주어진 principal direction을 $\beta$라 하고 $\R^3$공간의 임의의 기저를 $\gamma$라 하자.

$\beta \rightarrow \gamma$인 기저변환행렬을 $B$라 하면 다음이 성립한다.

$$ \begin{aligned} \frak m^\gamma_\gamma(\boldsymbol\sigma) &= B^{-1} \frak m^\beta_\beta(\boldsymbol\sigma) B \\ &= \sigma B^{-1} I B \\ &= \sigma I \end{aligned} $$

따라서, $\R^3$공간의 임의의 기저인 $\gamma$도 principal directions가 되며 그에 해당하는 principal stress는 $\sigma$다. $\quad {_\blacksquare}$

### 명제2(Maximum Shearing Stress)
주어진 $x,t$에서 응력텐서 $\boldsymbol{\sigma}$가 주어졌다고 하자.

$\lVert n \rVert = 1$을 만족하는 $n$을 normal vector로 갖는 평면의 stress vector를 $t_n$이라 할 때, 평면에 접하는 $t_n$의 성분을 shearing stress $s_s$라 하자.

$\sigma_1,\sigma_2,\sigma_3$를 principal stress라고 했을 때, 다음을 증명하여라.

$$ \max( \lVert s_s \rVert) = \max \Big( \frac{|\sigma_i-\sigma_j|}{2} \Big), \quad i,j = 1,2,3, \enspace i \neq j $$

**Proof**

$\beta$를 $\boldsymbol{\sigma}$의 principal direction이라고 하고 $n = n^i\beta_i$라 하면 $t_n$는 다음과 같다.

$$ \overset{n}{t}^i = \sigma_in^i, \quad i=1,2,3 $$

평면에 수직한 $t_n$의 성분인 normal stress $s_n$과 평면에 접하는 $t_n$의 성분인 shearing stress $s_s$는 다음과 같다.

$$ \begin{aligned} s_n &= (t_n \cdot n)n \\ s_s &= t_n - s_n \\ &= t_n - (t_n \cdot n)n \end{aligned} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} \lVert s_s \rVert ^2 &= (t_n - (t_n \cdot n)n) \cdot (t_n - (t_n \cdot n)n) \\ &= t_n \cdot t_n - s_n \cdot s_n \\ &= \sum_{i=1}^3 (\sigma_in^i)^2 - \bigg( \sum_{i=1}^3 \sigma_i(n^i)^2 \bigg) \end{aligned} $$

표기를 간단하게 하기 위해 다음과 같이 정의하자.

$$ \sigma_1 = a, \enspace \sigma_2 = b, \enspace \sigma_3 = c \\ n_1 = x, \enspace n_2 = y , \enspace n_3 = z \\ \lVert s_s \rVert^2 = f(x,y,z) = a^2x^2 + b^2y^2 + c^2z^2 - (ax^2 + by^2 + cz^2)^2 $$

Maximum shearing stress를 찾는 문제는 다음과 같다.

$$ \text{find }  \text{maximimum } f|_C $$


$$ \text{Where, } C = \{ (x,y,z) \in \R^3 \enspace | \enspace x^2 + y^2 + z^2 = 1 \}  $$

이를 풀기 위해 Lagrange Multiplier Method를 사용하자.

Constraint function $c$를 다음과 같이 정의하자.

$$ c(x,y,z) = x^2 + y^2 + z^2 $$

그러면 다음과 같은 4개의 연립방정식을 얻을 수 있다.

$$ \begin{aligned} \frac{\partial f}{\partial x} &= \lambda \frac{\partial c}{\partial x} \\ \frac{\partial f}{\partial y} &= \lambda \frac{\partial c}{\partial y} \\ \frac{\partial f}{\partial z} &= \lambda \frac{\partial c}{\partial z} \\ c &= 1 \end{aligned} $$

위 연립방정식을 만족하는 모든 $(x,y,z)$를 대입하여 그 중 최대 $f$값이 최대 $f|_C$가 된다.

먼저 위 연립방정식을 풀어 쓰면 다음과 같다.

$$ \begin{equation} \begin{aligned} x(a^2 - 2a(ax^2 + by^2 + cz^2)) &= \lambda x \\ y(b^2 - 2b(ax^2 + by^2 + cz^2)) &= \lambda y \\ z(c^2 - 2c(ax^2 + by^2 + cz^2)) &= \lambda z \\ x^2 + y^2 + z^2 &= 1 \end{aligned} \end{equation} $$

주어진 $a,b,c$를 경우에 따라 나눠 연립방정식을 풀어보자.

[$a = b = c$]  
이 때, $f$는 다음과 같이 간단해진다.

$$ \begin{aligned} f &= a^2x^2 + b^2y^2 + c^2z^2 - (ax^2 + by^2 + cz^2)^2 \\ &= a^2(x^2 + y^2 + z^2) - a^2(x^2 + y^2 + z^2)^2 \end{aligned} $$

식(2)는 다음과 같이 간단해진다.

$$ \begin{aligned} -a^2x &= \lambda x \\ -a^2y &= \lambda y \\ -a^2z &= \lambda z \\ x^2 + y^2 + z^2 &= 1 \end{aligned} $$

해는 다음과 같다.

$$\lambda = -a^2, \enspace \forall (x,y,z) \in C$$

따라서, $\forall (x,y,z) \in C$에 대해서 $f$는 다음과 같다.

$$ \begin{aligned} f &= a^2(x^2 + y^2 + z^2) - a^2(x^2 + y^2 + z^2)^2 \\ &= a^2 - a^2 \\ &= 0 \end{aligned} $$

즉, $a=b=c$인 경우에 모든 평면은  principal plane이 되며 shearing stress가 0이다.

[$a = b \neq c$]  
$f$는 다음과 같이 간단해 진다.

$$ \begin{aligned} f &= a^2x^2 + b^2y^2 + c^2z^2 - (ax^2 + by^2 + cz^2)^2 \\ &= a^2 + (c^2-a^2)z^2 - (a + (c-a)z^2)^2 \end{aligned}  $$

식(2)는 다음과 같이 간단해 진다.

$$ \begin{equation} \begin{aligned} x(-a^2 - 2a(c-a)z^2) &= \lambda x \\ y(-a^2 - 2a(c-a)z^2) &= \lambda y \\ z(c^2 -2ac - 2c(c-a)z^2) &= \lambda z \\ x^2 + y^2 + z^2 &= 1 \end{aligned} \end{equation} $$

-[$z = 0$]  
식(3)은 다음과 같이 간단해 진다.

$$ \begin{aligned} -a^2x &= \lambda x \\ -a^2y &= \lambda y \\ x^2 + y^2 &= 1 \end{aligned} $$

해는 다음과 같다.

$$\lambda = -a^2, \enspace \forall(x,y,0) \in C$$

따라서, $\forall (x,y,0) \in C$에서 $f$는 다음과 같다.

$$ \begin{aligned} f &= a^2 + (c^2-a^2)z^2 - (a + (c-a)z^2)^2 \\ &= a^2 - a^2 \\&= 0 \end{aligned} $$

즉, $(1,0,0)$과 $(0,1,0)$을 포함해서 $\forall(x,y,0) \in C$를 normal vector로 갖는 모든 평면은 principal plane이 되며 shearing stress가 0이다.

-[$z \neq 0$]  
--[$x=y=0$]  
식(3)은 다음과 같이 간단해 진다.

$$ \begin{aligned} -c^2z &= \lambda z \\ z^2 &= 1 \end{aligned} $$

해는 다음과 같다.

$$\lambda = -c^2, \enspace z = \pm 1$$

이 때, $(0,0, \pm 1)$에서 $f$는 다음과 같다.

$$ \begin{aligned} f &= a^2 + (c^2-a^2)z^2 - (a + (c-a)z^2)^2 \\ &= c^2 - c^2 \\&= 0 \end{aligned} $$

즉, $(0,0, \pm 1)$을 normal vector로 갖는 모든 평면은 principal plane이 되며 shearing stress가 0이다.

--[$x \neq 0$]  
식(3)에 의해 다음이 성립한다.

$$ \begin{aligned} & \lambda = -a^2 - 2a(c-a)z^2 = c^2 -2ac - 2c(c-a)z^2 \\ \Leftrightarrow \enspace & 2(c-a)^2z^2 = c^2 - 2ac + a^2 \\ \Leftrightarrow \enspace & z^2 = \frac{1}{2} \end{aligned} $$

이 때, $\forall (x, y, \pm \frac{1}{\sqrt{2}}) \in C$에서 $f$는 다음과 같다.

$$ \begin{aligned} f &= a^2 + (c^2-a^2)z^2 - (a + (c-a)z^2)^2 \\ &= \frac{1}{2}(a^2 + c^2) - \frac{1}{4}(a + c)^2 = \frac{(a-c)^2}{4} = \frac{(b-c)^2}{4} \end{aligned} $$

[$a \neq b = c$]  
[$a = b \neq c$]경우와 동일한 과정을 거치면 다음 결과를 얻을 수 있다.

$\forall (0,y,z) \in C$에서 $f$는 다음과 같다.

$$ f=0 $$

$(\pm1,0,0) \in C$에서 $f$는 다음과 같다.

$$ f=0 $$

$\forall (x, y, \pm \frac{1}{\sqrt{2}}) \in C$에서 $f$는 다음과 같다.

$$ f = \frac{(b-a)^2}{4} = \frac{(c-a)^2}{4} $$

[$a = c \neq b$]  
[$a = b \neq c$]경우와 동일한 과정을 거치면 다음 결과를 얻을 수 있다.

$\forall (x,0,z) \in C$에서 $f$는 다음과 같다.

$$ f = 0 $$

$(0,\pm1,0) \in C$에서 $f$는 다음과 같다.

$$ f = 0 $$

$\forall (x, \pm \frac{1}{\sqrt{2}}, z) \in C$에서 $f$는 다음과 같다.

$$ f = \frac{(a-b)^2}{4} = \frac{(c-b)^2}{4} $$

[$a \neq b \neq c$]  
-[$x \neq 0, y \neq 0, z \neq 0$]  
식(2)로 부터 다음이 성립한다.

$$ \lambda = a^2 - 2a(ax^2 + by^2 + cz^2) =  b^2 - 2b(ax^2 + by^2 + cz^2) = c^2 - 2c(ax^2 + by^2 + cz^2) $$

두번째 등식으로 부터 다음이 성립한다.

$$ \begin{aligned} & a^2 - 2a(ax^2 + by^2 + cz^2) =  b^2 - 2b(ax^2 + by^2 + cz^2) \\ \Leftrightarrow \enspace & (a-b)(a+b) = 2(a-b)(ax^2 + by^2 + cz^2) \\ \Leftrightarrow \enspace & a + b = ax^2 + by^2 + cz^2 \end{aligned} $$

동일한 방식으로 다음이 성립한다.

$$ b + c = ax^2 + by^2 + cz^2 \\ a + c = ax^2 + by^2 + cz^2 $$

따라서, $a = b = c$가 되고 가정에 모순이 발생한다.

-[$x = 0$]  
$f$는 다음과 같이 간단해 진다.

$$ \begin{aligned} f &= a^2x^2 + b^2y^2 + c^2z^2 - (ax^2 + by^2 + cz^2)^2 \\ &= b^2y^2 + c^2z^2 - (by^2 + cz^2)^2 \end{aligned} $$

식(2)는 다음과 같이 간단해 진다.

$$ \begin{equation} \begin{aligned} y(b^2 - 2b(by^2 + cz^2)) &= \lambda y \\ z(c^2 - 2c(by^2 + cz^2)) &= \lambda z \\ y^2 + z^2 &= 1 \end{aligned} \end{equation} $$

식(4)로 부터 다음이 성립한다.

$$ \begin{aligned} & \lambda = b^2 - 2b(by^2 + cz^2) = c^2 - 2c(by^2 + cz^2) \\ \Leftrightarrow \enspace & b + c = 2(b + (c-b)z^2) \\ \Leftrightarrow \enspace & z^2 = \frac{1}{2} \end{aligned}  $$

이 때, $(0, \pm\frac{1}{\sqrt 2}, \pm\frac{1}{\sqrt 2})$에서 $f$는 다음과 같다.

$$ \begin{aligned} f &= b^2y^2 + c^2z^2 - (by^2 + cz^2)^2 \\ &= \frac{1}{2}(b^2 + c^2) - \frac{1}{4}(b+c)^2 \\ &= \frac{(b-c)^2}{4} \end{aligned} $$

-[$y = 0$]  
[$x = 0$]의 경우와 동일하게 반복하면 다음 결과를 얻을 수 있다.
$(\pm\frac{1}{\sqrt 2}, 0, \pm\frac{1}{\sqrt 2})$에서 $f$는 다음과 같다.

$$ f = \frac{(a-c)^2}{4} $$

-[$z = 0$]  
[$x = 0$]의 경우와 동일하게 반복하면 다음 결과를 얻을 수 있다.
$(\pm\frac{1}{\sqrt 2}, \pm\frac{1}{\sqrt 2}, 0)$에서 $f$는 다음과 같다.

$$ f = \frac{(a-b)^2}{4} $$

이로써 모든 경우의 수를 따져봤으며 그 결과를 통해 다음이 성립한다.

$$ \begin{aligned} & \max(\sqrt f) = \max( \frac{|a-b|}{2}, \frac{|b-c|}{2}, \frac{|a-c|}{2}) \\ \Rightarrow \enspace & \max( \lVert s_s \rVert) = \max \Big( \frac{|\sigma_i-\sigma_j|}{2} \Big), \quad i,j = 1,2,3, \enspace i \neq j \quad {_\blacksquare} \end{aligned} $$

> Reference  
> [book] (J. Stewart) Calculus chap 14.8  
> [book] (Lai et al) Introduction to Continuum Mechanics Chap4.6


## Principal Invariants of Second Rank Tensor
Second rank tensor $A: \R^3 \rightarrow \R^3$가 있다고 하자.

$A$의 principal invariants $I_{1,2,3}$은 다음과 같다.

$$ \begin{aligned} I_1 &= \text{tr}(A) \\ I_2 &= \frac{1}{2}(\text{tr}(A)^2 - \text{tr}(A\circ A)) \\ I_3 &= \det(A) \end{aligned}  $$

### 명제1
Second rank tensor $A: \R^3 \rightarrow \R^3$가 있다고 하자.

$\R^3$의 임의의 기저를 $\beta$라 할 때, $\frak m_\beta^\beta(A) = A$이다.

이 떄, 다음을 증명하여라.

$$ I_2 = A_{11}A_{22} + A_{22}A_{33} + A_{33}A_{11} - A_{12}A_{21} - A_{23}A_{32} - A_{13}A_{31} $$

### 명제2
Second rank tensor $A: \R^3 \rightarrow \R^3$가 있다고 하자.

$A$의 eigen value를 $\lambda_{1,2,3}$이라 할 떄, 다음을 증명하여라.

$$ \begin{aligned} I_1 &= \lambda_1 + \lambda_2 + \lambda_3 \\ I_2 &= \lambda_1\lambda_2 + \lambda_2\lambda_3 + \lambda_3\lambda_1 \\ I_3 &= \lambda_1\lambda_2\lambda_3 \end{aligned}  $$

### 명제3
Symmetric second rank tensor $A: \R^3 \rightarrow \R^3$가 있다고 하자.

$\frac{1}{3}\text{tr}(A) = A_m$ 할 때, $A' = A - A_m id$의 principal invariants를 $I_{1,2,3}$이라 하자.

$A$의 eigen value를 $\lambda_{1,2,3}$이라 할 떄, 다음을 증명하여라.

$$ I_2 = -\frac{1}{6} ((\lambda_1 - \lambda_2)^2 + (\lambda_2 -\lambda_3)^2 + (\lambda_3 - \lambda_1)^2)  $$

**Proof**

$\R^3$의 임의의 기저를 $\beta$에 대해 $\frak m_\beta^\beta(A) = A$라하고, $A$의 eigen value로 이루어진 diagonal matrix를 $\Lambda$라고 하면 다음이 성립한다.

$$ \begin{aligned} I_2 &= \frac{1}{2}(\text{tr}(A')^2 - \text{tr}(A' \circ A')) \\ &= - \frac{1}{2} \text{tr}(A' \circ A')) \\&= - \frac{1}{2} \text{tr}((A - A_mI)^2) \\&= - \frac{1}{2} \text{tr}((\Lambda - A_mI)^2) \\&= - \frac{1}{2} (\Lambda - A_mI) : (\Lambda - A_mI) \\&= - \frac{1}{2} ((\lambda_1 - A_m)^2 + (\lambda_2 - A_m)^2 + (\lambda_3 - A_m)^2) \\ &= - \frac{1}{6}((\lambda_1 - \lambda_2)^2 + (\lambda_2 - \lambda_3)^2 + (\lambda_3 - \lambda_1)^2)) \quad {_\blacksquare} \end{aligned} $$

> Reference  
> [Wiki - Invaraints of tensors](https://en.wikipedia.org/wiki/Invariants_of_tensors)