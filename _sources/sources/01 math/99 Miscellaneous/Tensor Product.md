# Tensor Products
두 벡터공간 $V,W / \mathbb F$이 있을 때, $V$와 $W$의 `텐서 곱(tensor product)` $V \otimes W$는 아래와 같은 보편 성질을 갖는 bilinear map  $B \in L(V\times W; V \otimes W)$과 함께 정의된 벡터공간이다. 

임의의 벡터공간 $X / \mathbb F$가 있을 때, 
$$ H \in L(V\times W;X) \Rightarrow \exist! T \in L(V \otimes W;X) \quad s.t. \quad H = T \circ B  $$

### 명제1
두 벡터공간 $V,W / \mathbb F$이 있을 때, $V \otimes W$가 존재함을 증명하여라.

**Proof**

집합 $V \otimes W$에 대해서 함수 $B$를 다음과 같이 정의하자.
$$ B:V \times W \rightarrow V \otimes W \quad s.t. \quad (v,w) \mapsto v \otimes w $$

$$ \text{Where, } \quad v \otimes w \in L^2(V^*,W^*; \mathbb F) \quad s.t. \quad (v^*,w^*) \mapsto v^*(v)w^*(w) $$

임의의 벡터공간 $X / \mathbb F$와 $H \in L(V\times W;X)$가 있을 때, $V,W$의 기저 $\beta,\gamma$에 대해 다음과 같이 동작하는 linear map $T$를 정의하자.
$$T \in L(V \otimes W;X) \quad s.t. \quad \beta_i \otimes \gamma_j \mapsto H(\beta_i,\gamma_j)$$

위의 정의에 의해 $T$는 유일하게 존재하며, $(\beta_i ,\gamma_j)$에서 $H = T \circ B$이다. 

위의 정의한 $B$는 $T$ 의해 보편 성질을 갖으며 보조명제1에 의해 $B\in L(V\times W; V\otimes W)$이다. 또한, 보조명제2에 의해 위와 같이 정의된 함수 $B$에 대해서, $V \otimes W = L^2(V^*, W^*, \mathbb F)$임으로, vector space이다.

따라서 $V \otimes W$가 최소한 한개 이상 존재한다. $\quad {_\blacksquare}$

#### 따름명제
두 벡터공간 $V,W / \mathbb F$이 있을 때, $L^2(V^*, W; \mathbb F),L(V\times W^*, \mathbb F),L(V\times W; \mathbb F)$도 $V \otimes W$임을 증명하여라.

#### 보조명제1
다음을 증명하여라.
$$ B \in L(V\times W; V \otimes W) $$

**Proof**

$a \in \mathbb F, \enspace v_1,v_2 \in V, \enspace w \in W$가 있을 때, $v^* \in V^*, \enspace w^* \in W^*$에 대해 다음이 성립한다.
$$ \begin{aligned} B(av_1+v_2,w)(v^*,w^*) &= ((av_1+v_2) \otimes w)(v^*,w^*) \\ &= v^*(av_1+v_2)w^*(w) \\ &= av^*(v_1)w^*(w) + v^*(v_2)w^*(w) \\ &= a(v_1 \otimes w)(v^*,w^*) + (v_2 \otimes w)(v^*,w^*) \\ &= aB(v_1,w)(v^*,w^*) + B(v_2,w)(v^*,w^*) \end{aligned}  $$ 

위와 비슷하게 $B$는 두번째 항에 대해서도 linear map임을 보일 수 있다. $\quad {_\blacksquare}$

##### 따름명제
$$ a^i b^j \beta_i \otimes \beta_j = a^i\beta_i \otimes b^j\beta_j $$


#### 보조명제2
다음을 증명하여라.
$$ V \otimes W = L^2(V^*, W^*, \mathbb F) $$

**Proof**

$\beta,\gamma$를 각 각 $V,W$의 기저라고 하자.

[$L^2(V^*, W^*, \mathbb F) \subseteq V \otimes W$]  
$v^* = a_i\beta^i \in V^*, \enspace w^* = b_i\gamma^i \in W^*$가 있을 때, $f \in L^2(V^*, W^*; \mathbb F)$에 대해 다음이 성립한다.
$$ \begin{aligned} & f(v^*,w^*) = a_ib_jf(\beta^i,\gamma^j) \\ \Rightarrow \enspace & \exist v = c^i\beta_i \in V, \enspace w = d^i\gamma_i \in W \quad s.t. \quad c^id^j = f(\beta^i, \gamma^j) \\ \Rightarrow \enspace & f = v \otimes w \in V \otimes W \end{aligned}  $$

[$V \otimes W \subseteq L^2(V^*, W^*, \mathbb F)$]  
$v = a^i\beta_i, \enspace w = b^i\gamma_i$가 있을 때, $v \otimes w \in V \otimes W$ 대해 다음이 성립한다.
$$ \begin{aligned} & B(v,w) = a^ib^jB(\beta_i,\gamma_j) \\ \Rightarrow \enspace & v \otimes w = a^ib^j \beta_i \otimes \gamma_j \\ \Rightarrow \enspace & \exist f \in L^2(V^*, W^*, \mathbb F) \quad s.t. \quad a^ib^j = f(\beta^i, \gamma^j) \\ \Rightarrow \enspace & v \otimes w = f \in L^2(V^*, W^*, \mathbb F) \quad {_\blacksquare} \end{aligned}  $$

### 명제2
두 벡터공간 $V,W / \mathbb F$이 있을 때, 집합 $V \otimes W$에 대해서 함수 $B$를 다음과 같이 정의하자.
$$ B:V \times W \rightarrow V \otimes W \quad s.t. \quad (v,w) \mapsto v \otimes w $$

$$ \text{Where, } \quad v \otimes w \in L^2(V^*,W^*; \mathbb F) \quad s.t. \quad (v^*,w^*) \mapsto v^*(v)w^*(w) $$

이 때, 다음을 증명하여라.
$$\beta_i \otimes \gamma_j \text{ are bases of } V \otimes W$$

**Proof**

[linearly independent]  
$v^* = a_i\beta^i \in V^*, \enspace w^* = b_i \gamma^i \in W^*$라 할 때, 다음이 성립한다.
$$ \begin{aligned} & c^{ij} \beta_i \otimes \gamma_j = 0_V \otimes W \\ \Rightarrow \enspace & c^{ij}(\beta_i \otimes \gamma_j)(v^*, w^*) = 0_{\mathbb F} \\ \Rightarrow \enspace & a_kb_lc^{ij} \beta^k(\beta_i)\gamma^l(\gamma_j) \\ \Rightarrow \enspace &a_k b_l c^{ij} \delta^k_i \delta^l_j \\ \Rightarrow \enspace & a_i b_j c^{ij}  = 0 \end{aligned} $$

이 떄, 임의의 모든 $a_i, b_j$에 대해서 만족해야 함으로 모든 $c^{ij} =0$이다. $\quad {_\blacksquare}$

[$\text{Span}(\{ \beta_i \otimes \gamma_j\}) = V \otimes W$]   
$v = a^i\beta_i, \enspace w = b^i\gamma_i$라 하면 다음이 성립한다.
$$ B(v,w) = B(a^i \beta_i, b^j \gamma_j) = a^i \beta_i \otimes b^j \gamma_j = a^i b^j \beta_i \otimes \gamma_j \quad {_\blacksquare} $$

### 명제3
두 벡터공간 $V,W / \mathbb F$이 있을 때, $(V \otimes W, \phi)_1, (V \otimes W, \phi)_2$를 두개의 $V \otimes W$라고 하자. 이 때, 다음과 같이 정의된 vector space isomorphism $T$가 유일하게 존재함을 증명하여라
$$ T \in L( (V \otimes W)_1; (V \otimes W)_2) \quad s.t. \quad \phi_2 = T \circ \phi_1  $$

**Proof**

Tensor product의 정의에서 $V \otimes W = (V \otimes W)_1, \enspace B = \phi_1$라 두고, $X = (V \otimes W)_2, \enspace H = \phi_2$라하면 Tensor product의 정의에 의해, 다음이 성립한다.
$$ \exist! T \in L( (V \otimes W)_1; (V \otimes W)_2) \quad s.t. \quad \phi_2 = T \circ \phi_1 $$

이번에는 반대로 역할을 바꿔 Tensor product의 정의에서 $V \otimes W = (V \otimes W)_2, \enspace B = \phi_2$라 두고, $X = (V \otimes W)_1, \enspace H = \phi_1$라하면 Tensor product의 정의에 의해, 다음이 성립한다.
$$ \exist! S \in L( (V \otimes W)_2; (V \otimes W)_1) \quad s.t. \quad \phi_1 = S \circ \phi_2 $$

이 때, 위의 관계식에 의해 다음이 성립힌다.
$$ S \circ T \circ \phi_1 = S \circ \phi_2 = \phi_1 $$

이번에는 Tensor product의 정의에서 $V \otimes W = (V \otimes W)_1, \enspace B = \phi_1$라 두고, $X = (V \otimes W)_1, \enspace H = \phi_1$라하면 Tensor product의 정의에 의해, 다음이 성립한다.
$$ \exist! R \in L( (V \otimes W)_1) \quad s.t. \quad \phi_1 = R \circ \phi_1 $$

$S,T$는 모두 linear map임으로 $S \circ T$는 linear map이고 따라서 $S \circ T$와 $id_{L((V\otimes W)_1)}$ 모두 $R$의 역활을 할 수 있다. 이 떄, $R$은 유일함으로 $S \circ T = id_{L((V\otimes W)_1)}$이다. 동일한 논리로 $T \circ S = id_{L((V\otimes W)_2)}$이다.

$T$와 $S$는 역함수 관계임으로 $T$는 vector space isomorphism이다. 그리고 $T$는 Tensor product의 정의에 의해 유일하게 존재한다. $\quad {_\blacksquare}$

> Reference  
> [note] (Kamnitzer) Tensor products    
> [tensor product - wiki](https://en.wikipedia.org/wiki/Tensor_product)  
> [blog](https://algebrology.github.io/tensor-products/)