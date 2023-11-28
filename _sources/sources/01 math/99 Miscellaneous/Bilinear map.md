# Vector Space Bilinear Map
벡터공간 $V,W,X / \mathbb F$이 있을 때, 다음과 같이 정의된 함수 $T$가 있다고 하자.
$$ T : V \times W \rightarrow X $$

모든 $w \in W$에 대해서 함수 $T_w : V \rightarrow X \quad s.t. \quad v \mapsto T(v,w)$가 linear map이고 동시에 모든 $v \in V$에 대해서 함수 $T_v : W \rightarrow X \quad s.t. \quad w \mapsto T(v,w)$가 linear map일 때, $T$를 `vectorspace bilinear map`이라고 한다.

만약 $X = \mathbb F$일 경우에는 `vectorspace bilinear form`이라고 한다.

이 때, $V,W$의 원소를 인자로 받아 $X$의 원소로 mapping 시키는 모든 bilinear map의 집합을 $L^2(V,W; X)$라 한다.



> 참고  
> [Bilinear map - Wiki](https://en.wikipedia.org/wiki/Bilinear_map)


### 명제1
벡터공간 $V,W,X / \mathbb F$이 있을 때, $L^2(V,W; X)$는 다음과 같이 정의된 연산이 있을 때, $\mathbb F$위의 벡터공간임을 보여라.
$$ \begin{aligned} + := & L^2(V,W;X) \times L^2(V,W;X) \rightarrow L^2(V,W;X) \quad s.t. \quad (\varphi, \psi) \mapsto \varphi + \psi \\ & \text{satisfying } (\varphi + \psi)(v,w) = \varphi(v,w) + \psi(v,w) \\ \cdot := & \mathbb F \times L^2(V,W;X) \rightarrow L^2(V,W;X) \quad s.t. \quad (a, \psi) \mapsto a \psi \\ & \text{satisfying } (a\psi)(v,w) = a\psi(v,w) \end{aligned}  $$

### 명제2
벡터공간 $V,W / \mathbb F$과 bilinear form $T \in L^2(V, W; \mathbb F)$가 있을 때, $v \in V$에 대해 함수 $T(v, \cdot)$를 다음과 같이 정의하자.
$$ T(v, \cdot) \in W^* \quad s.t. \quad w \mapsto T(v,w) $$

$\beta, \gamma$를 각 각 $V,W$의 기저라 할 때, 다음을 증명하여라.
$$ v = a^i \beta_i \Rightarrow T(v,\cdot) = c_i\gamma^i $$
$$ \text{where, } c_i = a^jT(\beta_j,\gamma_i) $$

**Proof**

$w = b^i \gamma_i \in W$라 하면 다음이 성립한다.
$$ T(v,\cdot)(w) = T(v,w) = a^ib^jT(\beta_i,\gamma_j) $$

$T(v,\cdot) \in W^*$임으로 $T(v, \cdot) = c_i \gamma^i$이고 다음이 성립한다.
$$ T(v,\cdot)(w) = b^j c_i \gamma^i(\gamma_j) = b^j c_i \delta^i_j = b^i c_i $$

두 식을 비교함으로써 다음 관계식을 얻을 수 있다.
$$ c_i = a^jT(\beta_j,\gamma_i) $$

> 참고  
> [피그티 기초 물리 네이버](https://m.blog.naver.com/PostView.naver?blogId=defxgenh&logNo=50191387615)  
> [피그티 기초 물리 tistory](https://elementary-physics.tistory.com/155)  

#### 따름명제
$$ T(\beta_i, \cdot) = T(\beta_i,\gamma_j)\gamma^j $$

# Non-degeneracy
벡터공간 $V,W,X / \mathbb F$과 $T \in L^2(V,W,X)$가 있을 때, 다음 성질을 만족하는 경우를 `Non-degenerate` bilinear map이라고 한다.
$$ \begin{aligned} & v \in V - \{ 0_V \} \Rightarrow \exist w \in W \quad s.t. \quad T(v,w) \neq 0_X \\ \land \enspace & w \in W - \{ 0_w \} \Rightarrow \exist v \in W \quad s.t. \quad T(v,w) \neq 0_X \end{aligned} $$

혹은
$$ \begin{aligned} & \forall w \in W, \quad  T(v,w) = 0_X \Leftrightarrow v = 0_V \\ \land \enspace & \forall v \in V, \quad  T(v,w) = 0_X \Leftrightarrow w = 0_W \end{aligned} $$

> 참고  
> [note] (Garrett) Duals, naturality, bilinear forms

### 명제1
벡터공간 $V/ \mathbb F$과 non-degenerate bilinear form $T \in L^2(V,V;\mathbb F)$가 있고 $v \in V$가 있을 때, 함수 $T(v, \cdot)$를 다음과 같이 정의하자.
$$ T(v, \cdot) \in L(V ;\mathbb F) \quad s.t. \quad w \mapsto T(v,w) $$

이 때, 다음과 같이 정의된 함수 $\varphi$가 vector space isomorphism임을 보여라.
$$ \varphi : V \rightarrow V^* \quad s.t. \quad v \mapsto T(v, \cdot) $$

**Proof**

[$\varphi$ is linear map]  
$v_1,v_2 \in V, \enspace a \in F$라 하자.
$$ \varphi(av_1 + v_2) = T(av_1+v_2, \cdot) = aT(v_1, \cdot)+T(v_2, \cdot) = a\varphi(v_1) + \varphi(v_2)  $$

[$\varphi$ is bijective]  
$T$가 non-degenerate bilinear form임으로 $T(v,\cdot) = 0_{V^*} \Leftrightarrow v = 0_V$이다. 따라서 $\ker(\varphi) = \{ 0_V \}$이다.

$\ker(\varphi) = \{ 0_V \}$이고 $\dim(V) = \dim(V^*)$임으로 선형대수 Dimension Theorem의 명제1에 의해 $\varphi$는 bijective이다. $\quad {_\blacksquare}$

> 참고  
> [note] (Garrett) Duals, naturality, bilinear forms

#### 따름명제
$V$의 기저를 $\beta$라 했을 때 다음을 증명하여라.
$$ [\varphi]^{\beta^*}_{\beta} = T_{ij} $$
$$ \text{Where, } T_{ij} = T(\beta_i, \beta_j) $$


### 명제2
벡터공간 $V/ \mathbb F$과 non-degenerate bilinear form $T \in L^2(V,V;\mathbb F)$가 있고 $v \in V$가 있을 때, 다음과 같은 함수를 정의하자.
$$ T(v, \cdot) : V \rightarrow \mathbb F \quad s.t. \quad w \mapsto T(v,w) $$
$$ \varphi : V \rightarrow V^* \quad s.t. \quad v \mapsto T(v, \cdot) $$
$$ H : V^* \times V^* \rightarrow \mathbb F \quad s.t. \quad (v^1,v^2) \mapsto B(\varphi^{-1}(v^1), \varphi^{-1}(v^2)) $$

다음을 증명하여라.
$$ H \in L^2(V^*,V^*; \mathbb F) $$

> 참고  
> [inner-product-in-dual-space - Mathematics](https://math.stackexchange.com/questions/3486532/inner-product-in-dual-space)