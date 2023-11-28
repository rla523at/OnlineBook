# FE Formulation
FE formulation은 물리적으로 essential BC에 대응되는 natural BC가 존재한다는 관측에서부터 시작한다.

다시 말해, 지지조건으로 displacement가 결정된 essential BC에는 지지력이 발생하게 되고, 이 지지력을 지지조건 대신에 부여해도 같은 해를 얻을 수 있다는 것이다. 

따라서 essential BC를 대응되는 natrual BC로 바꾸면 다음이 성립한다.

$$ \partial\Omega = \partial\Omega_N $$

> Reference  
> {cite}`bathe`p.161  

## Derivation

Essential BC를 natural BC로 바꾸면 solution function space와 test function space의 restriction이 완화되며 weak formulation은 다음과 같아진다.

$$ \text{find} \enspace d \in (\mathcal D_W)^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega} \sigma : \text{grad}(w) dV = \int _{\partial\Omega} t \cdot  w dS + \int _{\Omega} \rho f_b \cdot w dV $$


$$ \text{Where, } \mathcal{D}_W := \{ d_i \in C^1(\Omega) \} $$

$\Omega$를 서로 겹치지 않는 $N_E$개의 요소로 나눈 다음 각각의 요소 $\Omega_i \enspace i = 1, \cdots, N_E$ 에서 BVP를 만족한다고 하자.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega_i} \sigma : \text{grad}(w) \thinspace dV = \int _{\partial\Omega_i} t \cdot  w \thinspace dS + \int_{\Omega_i} \rho f_b \cdot w \thinspace dV $$


$$ \text{Where, } \mathcal{D}_{W_i} := \{ d_i \in C^1(\Omega_i) \} $$

명제1에 의해 다음이 성립한다.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega_i} \sigma_v \cdot \partial  w \thinspace dV = \int _{\partial\Omega_i} t \cdot  w \thinspace dS + \int_{\Omega_i} \rho f_b \cdot w \thinspace dV \quad (i=1,\cdots,N_E) $$

Bodunov-Galerkin method를 사용하면 명제2에 의해 BVP가 다음과 같아진다.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \int_{\Omega_i}  B_i^T\sigma_v \thinspace dV =  f_i \quad (i=1,\cdots,N_E) $$



### Elasticity material

Direct stiffness method를 사용하여 element equation을 global equation으로 assemble한다.

$$ \text{find} \enspace \hat{d} \in \R^{3N_N} \st K\hat{d} = f $$

### 명제1
다음을 증명하여라.

$$ \sigma : \text{grad}(w) = \sigma_v \cdot \partial  w $$


$$ \begin{aligned} \text{Where, } \sigma_v &= \begin{bmatrix} \sigma_{11} & \sigma_{22} & \sigma_{33} & 2\sigma_{23} & 2\sigma_{13} & 2\sigma_{12} \end{bmatrix}^T \\ \partial &= \begin{bmatrix} \frac{\partial}{\partial x_1} & 0 & 0 \\ 0 & \frac{\partial}{\partial x_2} & 0 \\ 0 & 0 & \frac{\partial}{\partial x_3} \\ 0 & \frac{\partial}{\partial x_3} & \frac{\partial}{\partial x_2} \\ \frac{\partial}{\partial x_3} & 0 & \frac{\partial}{\partial x_1} \\ \frac{\partial}{\partial x_2} & \frac{\partial}{\partial x_1} & 0 \end{bmatrix} \\ w &= \begin{bmatrix} w_1 & w_2 & w_3 \end{bmatrix}^T \end{aligned} $$

**Proof**

$\sigma$의 symmetry에 의해 $i \neq j$일 때, 다음이 성립한다.

$$ \sigma_{ij}\frac{\partial w_i}{\partial x_j} + \sigma_{ji}\frac{\partial w_j}{\partial x_i} = \sigma_{ij} \bigg( \frac{\partial w_i}{\partial x_j} + \frac{\partial w_j}{\partial x_i} \bigg) \enspace \text{(not summation)} $$

따라서, 다음이 성립한다.

$$ \sigma : \text{grad}(w) = \sigma_v \cdot \partial w $$

$$ \text{Where, } \sigma_v = \begin{bmatrix} \sigma_{11} & \sigma_{22} & \sigma_{33} & \sigma_{23} & \sigma_{13} & \sigma_{12} \end{bmatrix}^T $$

### 명제2(Bodunov-Galerkin method)
각각의 요소 $\Omega_i \enspace i = 1, \cdots, N_E$에서 BVP가 다음과 같이 주어졌다고 하자.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega_i} \sigma_v \cdot \partial  w \thinspace dV = \int _{\partial\Omega_i} t \cdot  w \thinspace dS + \int_{\Omega_i} \rho f_b \cdot w \thinspace dV $$

$\Omega_i$는 $N_{N_i}$개의 node로 구성되어 있으며, $\Omega_i$에서 축소된 solution function space $\mathcal D_i$는 다음과 같다. 

$$ \mathcal D_i = \text{span}(\{ (h_i)_1,\cdots,(h_i)_{N_{N_i}} \})$$


$$ \text{Where, } h_i \text{ is a shape function.} $$

Bodunov-Galerkin method를 사용하고 solution function의 approximation을 다음과 같이 하자.

$$ \begin{aligned} d &= d(x_1)(h_i)_1 + \cdots + d(x_{N_{N_i}})(h_i)_{N_{N_i}} \\&= H_i\hat{d}_i \end{aligned} $$


$$ \begin{aligned} \text{Where, } H_i &= \begin{bmatrix} (h_i)_1 & 0 & 0 & \cdots & (h_i)_{N_{N_i}} & 0 & 0 \\ 0 & (h_i)_1 & 0 & \cdots & 0 & (h_i)_{N_{N_i}} & 0 \\ 0 & 0 & (h_i)_1  & \cdots & 0 & 0 & (h_i)_{N_{N_i}}  \\\end{bmatrix} \\ \hat{d}_i &= \begin{bmatrix} d(x_1)_1 & d(x_1)_2 & d(x_1)_3 & \cdots & d(x_{N_{N_i}})_1 & d(x_{N_{N_i}})_2 & d(x_{N_{N_i}})_3 \end{bmatrix}^T \end{aligned} $$

이 때, BVP가 다음과 같아 짐을 증명하여라.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \int_{\Omega_i}  B_i^T\sigma_v \thinspace dV =  f_i $$


$$ \begin{aligned} \text{Where, } f_i &= \int _{\partial\Omega_i}  (H_i)^T  t \thinspace dS + \int _{\Omega_i}  (H_i)^Tf_b \thinspace dV \\ B_i &= \partial H_i \end{aligned} $$

**Proof**

Bodunov-Galerkin method를 사용하면 weight function을 다음과 같이 근사할 수 있다.

$$ w = H_i\hat{w} $$

그러면 다음이 성립한다.

$$ \begin{aligned} &\int_{\Omega_i} \sigma_v \cdot B_i\hat{w} \thinspace dV = \int_{\partial\Omega_i} t \cdot H_i\hat{w} \thinspace dS + \int_{\Omega_i} \rho f_b \cdot H_i\hat{w} \thinspace dV \\\implies&  \hat{w}^T \int_{\Omega_i} B_i^T\sigma_v \thinspace dV - \hat{w}^T\int_{\partial\Omega_i} (H_i)^T t \thinspace dS - \hat{w}^T\int_{\Omega_i} (H_i)^Tf_b \thinspace dV = 0 \\\implies& \hat{w}^T \left( \int_{\Omega_i} B_i^T\sigma_v \thinspace dV  - \int_{\partial\Omega_i} (H_i)^T t \thinspace dS - \int_{\Omega_i} (H_i)^Tf_b \thinspace dV \right) = 0 \end{aligned} $$

그러면 BVP는 다음과 같아진다.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall \hat{w} \in \R^{3N_{N_i}}, \enspace \hat{w}^T \left( \int_{\Omega_i} B_i^T\sigma_v \thinspace dV  - \int_{\partial\Omega_i} (H_i)^T t \thinspace dS - \int_{\Omega_i} (H_i)^Tf_b \thinspace dV \right) = 0 $$

$\forall \hat{w} \in \R^{3N_{N_i}}$에 대해서 위 식이 성립함으로 BVP가 다음과 같이 간단해진다.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \displaystyle\int_{\Omega_i}  B_i^T\sigma_v \thinspace dV = \int_{\partial\Omega_i} (H_i)^T t \thinspace dS + \int_{\Omega_i} (H_i)^Tf_b \thinspace dV $$

이 때, 우측항은 $\Omega_i$ element에 작용하는 외력을 나타냄으로 우측항을 $f_i$로 표기하면 다음과 같다.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \int_{\Omega_i}  B_i^T\sigma_v \thinspace dV =  f_i $$


---

# FE Formulation
FE formulation은 물리적으로 essential BC에 대응되는 natural BC가 존재한다는 관측에서부터 시작한다.

다시 말해, 지지조건으로 displacement가 결정된 essential BC에는 지지력이 발생하게 되고, 이 지지력을 지지조건 대신에 부여해도 같은 해를 얻을 수 있다는 것이다. 

따라서 essential BC를 대응되는 natrual BC로 바꾸면 다음이 성립한다.

$$ \partial\Omega = \partial\Omega_N $$

> Reference  
> {cite}`bathe`p.161  

## Derivation

Essential BC를 natural BC로 바꾸면 solution function space와 test function space의 restriction이 완화되며 weak formulation은 다음과 같아진다.

$$ \text{find} \enspace d \in (\mathcal D_W)^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega} \sigma : \text{grad}(w) dV = \int _{\partial\Omega} t \cdot  w dS + \int _{\Omega} \rho f_b \cdot w dV $$


$$ \text{Where, } \mathcal{D}_W := \{ d_i \in C^1(\Omega) \} $$

$\Omega$를 서로 겹치지 않는 $N_E$개의 요소로 나눈 다음 각각의 요소 $\Omega_i \enspace i = 1, \cdots, N_E$ 에서 BVP를 만족한다고 하자.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega_i} \sigma : \text{grad}(w) \thinspace dV = \int _{\partial\Omega_i} t \cdot  w \thinspace dS + \int_{\Omega_i} \rho f_b \cdot w \thinspace dV $$


$$ \text{Where, } \mathcal{D}_{W_i} := \{ d_i \in C^1(\Omega_i) \} $$

명제1에 의해 다음이 성립한다.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega_i} C \epsilon_v \cdot \partial  w \thinspace dV = \int _{\partial\Omega_i} t \cdot  w \thinspace dS + \int_{\Omega_i} \rho f_b \cdot w \thinspace dV $$

Bodunov-Galerkin method를 사용하면 명제2에 의해 BVP가 다음과 같아진다.

$$ \text{find} \enspace \hat{d}_i \in \R^{3N_{N_i}} \st K_i\hat{d}_i = f_i \quad (i=1,\cdots,N_E) $$

Direct stiffness method를 사용하여 element equation을 global equation으로 assemble한다.

$$ \text{find} \enspace \hat{d} \in \R^{3N_N} \st K\hat{d} = f $$

### 명제1
다음을 증명하여라.

$$ \sigma : \text{grad}(w) = C\epsilon_v \cdot \partial  w $$


$$ \begin{aligned} \text{Where, } C &:= \text{elasticity matrix} \\  \epsilon_v &= \begin{bmatrix} \epsilon_{11} & \epsilon_{22} & \epsilon_{33} & 2\epsilon_{23} & 2\epsilon_{13} & 2\epsilon_{12} \end{bmatrix}^T \\ \partial &= \begin{bmatrix} \frac{\partial}{\partial x_1} & 0 & 0 \\ 0 & \frac{\partial}{\partial x_2} & 0 \\ 0 & 0 & \frac{\partial}{\partial x_3} \\ 0 & \frac{\partial}{\partial x_3} & \frac{\partial}{\partial x_2} \\ \frac{\partial}{\partial x_3} & 0 & \frac{\partial}{\partial x_1} \\ \frac{\partial}{\partial x_2} & \frac{\partial}{\partial x_1} & 0 \end{bmatrix} \\ w &= \begin{bmatrix} w_1 & w_2 & w_3 \end{bmatrix}^T \end{aligned} $$

**Proof**

$\sigma$의 symmetry에 의해 $i \neq j$일 때, 다음이 성립한다.

$$ \sigma_{ij}\frac{\partial w_i}{\partial x_j} + \sigma_{ji}\frac{\partial w_j}{\partial x_i} = \sigma_{ij} \bigg( \frac{\partial w_i}{\partial x_j} + \frac{\partial w_j}{\partial x_i} \bigg) \enspace \text{(not summation)} $$

따라서, 다음이 성립한다.

$$ \sigma : \text{grad}(w) = \sigma_v \cdot \partial w $$


$$ \text{Where, } \sigma_v = \begin{bmatrix} \sigma_{11} & \sigma_{22} & \sigma_{33} & \sigma_{23} & \sigma_{13} & \sigma_{12} \end{bmatrix}^T $$

이 때, 구성방정식에 의하여 다음이 성립한다.

$$ \sigma_v = C \epsilon_v $$

따라서, 다음이 성립한다. 

$$ \sigma : \text{grad}(w) = C\epsilon_v \cdot \partial  w \qed$$

### 명제2(Bodunov-Galerkin method)
각각의 요소 $\Omega_i \enspace i = 1, \cdots, N_E$에서 BVP가 다음과 같이 주어졌다고 하자.

$$ \text{find} \enspace d \in (\mathcal D_{W_i})^3 \st \forall w \in (C^\infty)^3, \quad \int_{\Omega_i} C \epsilon_v \cdot \partial  w \thinspace dV = \int _{\partial\Omega_i} t \cdot  w \thinspace dS + \int_{\Omega_i} \rho f_b \cdot w \thinspace dV $$

$\Omega_i$는 $N_{N_i}$개의 node로 구성되어 있으며, $\Omega_i$에서 축소된 solution function space $\mathcal D_i$는 다음과 같다. 

$$ \mathcal D_i = \text{span}(\{ (h_i)_1,\cdots,(h_i)_{N_{N_i}} \})$$


$$ \text{Where, } h_i \text{ is a shape function.} $$

Bodunov-Galerkin method를 사용하고 solution function의 approximation을 다음과 같이 하자.

$$ \begin{aligned} d &= d(x_1)(h_i)_1 + \cdots + d(x_{N_{N_i}})(h_i)_{N_{N_i}} \\&= H_i\hat{d}_i \end{aligned} $$


$$ \begin{aligned} \text{Where, } H_i &= \begin{bmatrix} (h_i)_1 & 0 & 0 & \cdots & (h_i)_{N_{N_i}} & 0 & 0 \\ 0 & (h_i)_1 & 0 & \cdots & 0 & (h_i)_{N_{N_i}} & 0 \\ 0 & 0 & (h_i)_1  & \cdots & 0 & 0 & (h_i)_{N_{N_i}}  \\\end{bmatrix} \\ \hat{d}_i &= \begin{bmatrix} d(x_1)_1 & d(x_1)_2 & d(x_1)_3 & \cdots & d(x_{N_{N_i}})_1 & d(x_{N_{N_i}})_2 & d(x_{N_{N_i}})_3 \end{bmatrix}^T \end{aligned} $$

이 때, BVP가 다음과 같아 짐을 증명하여라.

$$ \text{find} \enspace \hat{d}_i \in \R^{3N_{N_i}} \st K_i\hat{d}_i = f_i $$


$$ \begin{aligned} \text{Where, } K_i &= \int_{\Omega_i} (B_i)^T C_i B_i \thinspace dV \\ f_i &= \int _{\partial\Omega_i}  (H_i)^T  t \thinspace dS + \int _{\Omega_i}  (H_i)^Tf_b \thinspace dV \\ B_i &= \partial H_i \end{aligned} $$

**Proof**

Bodunov-Galerkin method를 사용하면 solution function과 weight function을 다음과 같이 근사할 수 있다.

$$ d = H_i\hat{d}_i, \enspace w = H_i\hat{w} $$

그러면 다음이 성립한다.

$$ \epsilon_v = \partial d = \partial H_i\hat{d}_i = B_i\hat{d}_i $$

그러면 BVP는 다음과 같아진다.

$$ \def\arraystretch{2}\begin{array}{ll} &\text{find} \enspace \hat{d}_i \in \R^{3N_{N_i}} \st \forall \hat{w} \in \R^{3N_{N_i}}, \quad \displaystyle \int_{\Omega_i} C \epsilon_v \cdot B_i\hat{w} \thinspace dV = \int_{\partial\Omega_i} t \cdot H_i\hat{w} \thinspace dS + \int_{\Omega_i} \rho f_b \cdot H_i\hat{w} \thinspace dV \\ \implies& \text{find} \enspace \hat{d}_i \in \R^{3N_{N_i}} \st \forall \hat{w} \in \R^{3N_{N_i}}, \quad \displaystyle \hat{w}^T \left( \int_{\Omega_i} B_i^TCB \thinspace dV \hat{d}_i  - \int_{\partial\Omega_i} (H_i)^T t \thinspace dS - \int_{\Omega_i} (H_i)^Tf_b \thinspace dV \right) = 0 \end{array} $$

$\forall \hat{w} \in \R^{3N_{N_i}}$에 대해서 위 식이 성립함으로 BVP가 다음과 같이 간단해진다.

$$ \def\arraystretch{2}\begin{array}{ll} & \text{find} \enspace \hat{d}_i \in \R^{3N_{N_i}} \st \displaystyle\int_{\Omega_i}  B_i^TCB \thinspace dV \hat{d}_i = \int_{\partial\Omega_i} (H_i)^T t \thinspace dS + \int_{\Omega_i} (H_i)^Tf_b \thinspace dV \\ \implies& \text{find} \enspace \hat{d}_i \in \R^{3N_{N_i}} \st K_i \hat{d}_i = f_i \qed \end{array} $$
