# Dual space
vetor space $V/\Bbb F$이 있을 때, $V$의 `쌍대공간(dual space)` $V^* / \mathbb F$는 다음과 같이 정의된 vetor space이다.

$$ V^* := L(V; \mathbb F) $$

즉, dual space는 linear form의 집합이다. 

linear form은 $V$와 $F$사이의 `homomorphism`임으로, $\hom(V;F)$이라고도 한다. 또한 함수 $f : V/\mathbb F \rightarrow \mathbb F$를 `범함수(functional)`라고 하며, linear form은 선형인 functional임으로 `linear functional`이라고도 한다.

> Reference  
> [Dual Space - Wiki](https://en.wikipedia.org/wiki/Dual_space) 

# Dual set
벡터공간 $V/\Bbb F$와 기저 $\beta$가 있을 때, 다음과 같이 정의된 함수 $\beta^i$의 집합을 $\beta$의 `쌍대 집합(dual set)` $\beta^*$이라 한다.

$$ \beta^i :V \rightarrow \mathbb F \quad s.t. \quad \beta_j \mapsto \delta^i_j $$

Dual set은 유한차원일 때, $V^*$의 기저임으로 `dual basis`라고도 하며 이는 명제2에서 증명한다.

### 명제1
벡터공간 $V/\Bbb F$와 기저 $\beta$가 있을 때, 다음을 증명하여라.

$$ \exist!\beta^* $$

**Proof**

$v \in V$일 때, dual set의 정의에 의해 다음이 성립한다.

$$ v = \beta^i(v)\beta_i $$

$\beta_i$가 $V$의 기저임으로, $\beta^i(v), \enspace i=1, \cdots, n$는 모든 $v \in V$마다 유일한 값을 갖음으로 모두 well-defined 함수이다. 따라서, $\beta^i, \enspace i=1, \cdots, n$의 집합인 $\beta^*$은 유일하게 존재한다.

### 명제2
벡터공간 $V/\Bbb F$와 기저 $\beta$가 있을 때, 다음을 증명하여라.

$$ \beta^* \text{ is linearly independent} $$

**Proof**

$\beta^*$의 원소중 임의로 $n$개의 원소를 선택해 $\beta^i \enspace i=1, \cdots, n$라 하자.

$a_i \in \mathbb F \enspace i=1, \cdots, n$가 있을 때, $a_i \beta^i  = 0_{V^*}$라 하자. 

따라서, $\beta_j \enspace j = 1, \cdots, n$에 대해 다음이 성립한다.

$$ a_i \beta^i(\beta_j) = a_j = 0_\mathbb F $$

결론적으로, $a_i = 0 \enspace \forall i$일 때만 $a_i \beta^i = 0_\mathbb F$을 만족함으로 $\beta^i \enspace i=1, \cdots, n$는 선형독립이다. 그리고 $\beta^i$는 $\beta^*$에서 유한개를 임의로 선택한 것이므로 $\beta^*$는 성형독립이다.  $\quad {_\blacksquare}$


### 명제3
$n$차원 벡터공간 $V/\Bbb F$와 기저 $\beta$가 있을 때, 다음을 증명하여라.

$$ \beta^i \text { is a basis of } V^* $$

**Proof**

명제2에의해 선형독립임이 증명되었음으로, $\text{span}(\beta^*) = V^*$만 확인하면 된다.

[$\text{span}(\beta^*) \subseteq V^*$]  
보조명제에 의해서 $\beta^* \subset V^*$임으로 $\text{span}(\beta^*) \subseteq V^*$이다. 

[$V^* \subseteq \text{span}(\beta^*)$]  
$v^* \in V^*$에 대해 다음이 성립한다고 하자.

$$ v^*(\beta_i) = a_i $$

$w^* = a_i\beta^i \in \text{span}(\beta^*)$는 다음을 만족한다.

$$ w^*(\beta_i) = a_j\beta^j(\beta_i) = a_i $$

즉, $v^* = w^*$임으로 $v^* \in \text{span}(\beta^*)$이다. $\quad {_\blacksquare}$

#### 보조명제
다음을 증명하여라.

$$ \beta^* \subset V^*  $$

**Proof**

$v_1 = b_i\beta_i, v_2 = c_i\beta_i \in V$일 때 다음이 성립한다.


$$ \begin{aligned} \beta^i(av_1 + v_2) &= \beta^i(ab_j\beta_j + c_k\beta_k) \\ &= \beta^i((ab_j+c_j)\beta_j) \\ &= ab_i + c_i \\ &= a \beta^i(v_1) + \beta^i(v_2) \end{aligned} $$

$\beta^i$은 linear form임으로  $\beta^i \in V^*$이다. 따라서 linear form의 집합인 $\beta^*$는 $V^*$의 부분집합이다. $\quad {_\blacksquare}$

#### 따름명제

$$ \dim(V) = \dim(V^*) $$

> Reference  
> [Dual Space - Wiki](https://en.wikipedia.org/wiki/Dual_space)  

#### 참고
$\dim(V) = \infty$면 일반적으로, $\dim(V) \neq \dim(V^*)$이다.

# Double dual
vector space $V/\Bbb F$가 있을 때, $V^*$ 또한 vector space임으로 $V^*$의 dual space인 double dual $V^{**}$을 정의할 수 있다.

### 명제
vector space $V/\Bbb F$가 있을 때, 함수 $\phi$를 다음과 같이 정의하자.

$$ \phi : V \rightarrow V^{**} \quad s.t \quad v \mapsto \phi(v) $$ 


$$\text{Where,} \quad \phi(v) : V^* \rightarrow \mathbb F \quad s.t. \quad v^* \mapsto v^*(v) $$

이 때, 다음을 증명하여라.

$$ \phi \text { is a vector space isomorphism} $$

**Proof**

[$\phi \in L(V;V^{**})$]  
$v_1,v_2 \in V, \enspace a \in \mathbb F, \enspace v^* \in V^*$라 하자.

$$ \begin{aligned} \phi (v_1 + av_2)(v^*) &= v^*(v_1 + av_2) \\ &= v^*(v_1) + av^*(v_2) \\ &= \phi(v_1) + a\phi(v_2) \end{aligned} $$

[bijective]  
정의에 의해 $\ker(\phi) = \{ 0_V \}$이고 $\dim(V) = \dim(V^*) = \dim(V^{**})$임으로 dimension theorem의 명제에 의해 $\phi$는 bijective이다. $\quad {_\blacksquare}$

#### 참고1
$\phi$는 basis의 선택에 의존하지 않는다.

> [Mathmatics - natural-isomorphism-in-linear-algebra](https://math.stackexchange.com/questions/234127/natural-isomorphism-in-linear-algebra)  

#### 참고2
$v \in V, \enspace v^{*} \in V^{*}$라 하면 다음이 성립한다.

$$ (\phi(v))(v^*) = v^{*}(v) $$

$v^* \in V^*, \enspace v^{**} \in V^{**}$라 하면 다음이 성립한다.

$$ v^*(\phi^{-1}(v^{**})) = v^*(v) = v^{**}(v^*) $$

> Reference  
> [note] (Garrett) Duals, naturality, bilinear forms

# Dual Map
vector space $V,W/\Bbb F$가 있을 때 $T \in L(V;W)$가 있다고 하자.

$T$의 `dual map` $T^*$은 다음과 같이 정의된 함수이다.

$$T^* \in L(W^*; V^*) \quad s.t. \quad w^* \mapsto w^* \circ T$$

### 명제
$n,m$차원 vector space $V,W/\Bbb F$가 있을 때 $T \in L(V;W)$가 있다고 하자.

다음을 증명하여라.

$$ \frak m^{\beta^*}_{\gamma^*}(T^*) = (\frak m_{\beta}^{\gamma}(T))^T $$

**Proof**


$$ \begin{aligned} \frak m^{\beta^*}_{\gamma^*}(T^*) &= \begin{bmatrix} \frak m_{\beta^*}(T^*(\gamma^*_1)) & \cdots & \frak m_{\beta^*}(T^*(\gamma^*_m)) \end{bmatrix} \\ &= \begin{bmatrix} (\gamma_1^* \circ T)(\beta_1) & \cdots & (\gamma_m^* \circ T)(\beta_1) \\ \vdots & & \vdots \\ (\gamma_1^* \circ T)(\beta_n) & \cdots & (\gamma_m^* \circ T)(\beta_n) \end{bmatrix} \\ &= \begin{bmatrix} \frak m_\gamma(T(\beta_1)) ^T \\ \vdots \\ \frak m_\gamma(T(\beta_n))^T \end{bmatrix}  \\ &= \begin{bmatrix} \frak m_\gamma(T(\beta_1)) & \cdots & \frak m_\gamma(T(\beta_n)) \end{bmatrix} ^T \end{aligned} $$

---

> Reference  
> [Dual Space - Wiki](https://en.wikipedia.org/wiki/Dual_space)  
> [note] (upenn) The Dual Space   
> [note] (Canez) Notes on dual spaces  
> [note] (Garrett) Duals, naturality, bilinear forms

