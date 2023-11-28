# Inner Product
$n$차원 vector space $V/\F$가 있을 때, 다음 성질을 만족하는 함수 $B:V \times V \rightarrow \F$를 `내적(inner product)`라 한다.

$x,y,z \in V$이고 $c \in \F$일 때,
1. $$ B(x+y, z) = B(x,z) + B(y,z) $$
2. $$ B(cx,y) = cB(x,y) $$
3. $$ B(x,y) = \overline{B(y,x)} $$
4. $$ 0 \le B(x,x) \quad (B(x,x) = 0_\F \text{ when } x = 0_V ) $$

### 참고1
$x,y,z \in V$이고 $c \in \F$일 때,

$\F = \R$이면, 다음이 성립한다.
$$ B(x,cy + z) = c B(x,y) + B(x,z) $$

즉, $B$는 symmetric하고 따라서 bilinear이다.

하지만 $\F = \mathbb C$이면 다음이 성립한다.
$$ B(x,cy + z) = \bar c B(x,y) + B(x,z) $$

즉, $B$는 더이상 symmetric하지 않고 bilinear하지 않다. 이렇게 scalar multiplcation은 conjugate되서 나오고 벡터의 덧셈은 보존될 때 이를 `Hermitian` 혹은 `conjugate linear`라고 한다.

### 참고2
다음과 같은 표기법을 많이 사용한다.
$$ B(x,y) \equiv \lang x,y \rang $$

### 명제
$n$차원 vector space $V/\F$와 내적 $B$가 있다고 하자.

$v \in V$가 있을 때, 다음을 증명하여라.
$$ \forall w \in V, \quad B(v,w) = 0_\F \Leftrightarrow v = 0_V $$

**Proof**

$\forall w \in V$에 대해 성립함으로, $w = v$로 두면 다음이 성립한다.
$$ B(v,v) = 0_\F $$

이 떄, inner product의 4번 성질에 의해 $v = 0_V$이다. $\qed$

# Inner Product Space
$n$차원 vector space $V/\F$에 내적 $B$가 주어진 공간을 `내적 공간(inner product space)`라고 한다.

# Norm
$n$차원 inner product space $V/\F$가 있을 때, `norm`은 다음과 같이 정의된 함수이다.
$$ n : V \rightarrow \R \st v \mapsto \sqrt{ B(v,v)} $$

### 참고
다음과 같은 표기법을 많이 사용한다.
$$ n(v) \equiv \lVert v \rVert $$

# Angle
$n$차원 inner product space $V/\F$가 있을 때, `angle`는 다음과 같이 정의된 함수이다.
$$ a : V \times V \rightarrow \R \st (v,w) \mapsto \cos^{-1} \bigg(\frac{B(v,w)}{\lVert v \rVert \lVert w \rVert} \bigg) $$

### 참고
두 벡터 $v,w \in V$가 주어졌을 때, 두 벡터 사이의 각도를 $\theta$라고 하면 다음이 성립한다.
$$ \cos \theta =  \frac{B(v,w)}{\lVert v \rVert \lVert w \rVert} $$


# Distance
$n$차원 inner product space $V/\F$가 있을 때, `distance`는 다음과 같이 정의된 함수이다.
$$ d : V \times V \rightarrow \R \st (v,w) \mapsto \lVert v-w \rVert $$

# Orthogonal Subset
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 다음을 만족할 때, $S$를 `orthogonal subset`이라고 한다.
$$ i \neq j \Rightarrow B(s_i, s_j) = 0 $$

### 명제1
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 orthogonal subset일 때 다음을 증명하여라.
$$ S \text { is linearly independent} $$

**Proof**

다음과 같이 가정하자.
$$ a^is_i = 0_V $$

내적을 이용하면 다음이 성립한다.
$$ B(a^is_i, s_j) = a^j \lVert s_j \rVert = 0_\F $$

norm의 정의에 의해, 모든 $j$에 대해 $a^j = 0$임을 알 수 있다. 따라서 linearly independent하다. $\qed$

### 명제2
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 orthogonal subset일 때 다음을 증명하여라.
$$ y \in \span(S) \Rightarrow y = \frac{B(y,s_i)}{B(s_i,s_i)} s_i $$

**Proof** 

$y$는 다음과 같이 표현할 수 있다.
$$ y = a^is_i $$

내적을 이용하면 다음이 성립한다.
$$ \begin{aligned}  B(y, s_j) &= B(a^is_i, s_j) \\&= a^j B(s_j,s_j) \enspace(\text{not summation}) \\ \Rightarrow \enspace a^j &= \frac{B(y,s_j)}{B(s_j,s_j)} \enspace(\text{not summation}) \end{aligned}  $$

이를 원래 표현식에 대입하면 다음을 얻을 수 있다.
$$ y = \frac{B(y,s_i)}{B(s_i, s_i)} s_i \qed$$

# Orthogonal Complement
$n$차원 inner product space $V/\F$가 있다고 하자.

$S \subseteq V$가 있을 때, $S$의 `orthogonal complement` $S^\perp$는 다음과 같이 정의된 집합이다.
$$ S^\perp := \{ v \in V \enspace | \enspace \forall s \in S, \quad  B(v,s) = 0_\F \}$$

### 명제1
$n$차원 inner product space $V/\F$가 있다고 하자.

$W \le V$가 있을 때, 다음을 증명하여라.
$$ W^\perp \le V $$

**Proof**

[기본 연산 법칙]  
$w \in W^\perp$면, $w \in V$이기 때문에 교환법칙 분배법칙등 $F-$가군의 성질들이 전부 성립한다. 

[연산에 닫힘]  
$w_1,w_2 \in W^\perp$, $a \in \Bbb F$이고 $s \in W$라 하면 다음이 성립한다.
$$ B(aw_1 + w_2, s) = aB(w_1,s) + B(w_2,s) = 0_\F  $$

따라서, $aw_1 + w_2 \in W^\perp$임으로 연산에 닫혀있다. $\qed$

[$+$연산 항등원의 존재성]  
$s \in W$라 하면 다음이 성립한다.
$$ B(0_V,s) =  0_\F $$

따라서 $0_V \in W^\perp$이다.

[$+$연산 역원의 존재성]  
상수곱이 정의되어 있음으로 환의 명제2에 의해 역원이 존재한다.


### 명제2
$n$차원 inner product space $V/\F$가 있다고 하자.

$W \le V$가 있을 때, 다음을 증명하여라.
$$ V = W \oplus W^\perp $$

**Proof**

[$V = W + W^\perp$]  
$W$의 orthonormal basis를 $\beta$라 하자.

$v \in V$가 있을 때, $u \in W$를 다음과 같이 정의하자.
$$ u = B(v,\beta_i)\beta_i $$

그러면, 다음이 성립한다.
$$ \begin{aligned} B(v-u, \beta_i) &= B(v,\beta_i) - B(v,\beta_j) B(\beta_j, \beta_i) \\ &= B(v,\beta_i) - B(v,\beta_i) \\ &= 0_\F \end{aligned} $$

따라서, $v-u \in W^\perp$임으로, $V = W + W^\perp$이다. $\qed$

[$W \cap W^\perp = \{ 0_V \}$]  
$w \in W \cap W^\perp$라 하자.

$w \in W$이면서 $w \in W^\perp$임으로, $W^\perp$의 정의에 의해 다음이 성립한다.
$$ B(w,w) = 0_\F $$

내적의 정의에 의해 $w = 0_V$이다. $\qed$

#### 참고1
$v \in V$면 $v = w + w^\perp, \enspace w \in W, w^\perp \in W^\perp$이 성립하고 direct sum의 성질에 의해 이러한 표현법이 유일하다.

#### 참고2
$v \in V$가 있을 때, $u \in W$를 다음과 같이 정의하자.
$$ u = B(v,\beta_i)\beta_i $$

$\forall w \in W$에 대해서 다음이 성립한다.
$$ \lVert v - u \rVert \le \lVert v - w \rVert $$

따라서 $u$는 $W$의 vector중 $u$와 가장 가까운 closest vector이다.

# Orthonormal Subset
$n$차원 inner product space $V/\F$가 있다고 하자.

Orthogonal subset $S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 다음을 만족할 때, $S$를 `orthonormal subset`이라고 한다.
$$ \norm{s_i} = 1 $$

### 명제1
$n$차원 inner product space $V/\F$가 있다고 하자.

Orthonormal subset $S$가 있을 때, $y \in \span(S)$에 대해 다음을 증명하여라.
$$ y = B(y,s_i)s_i $$

**Proof**

$S$는 orthogonal subset임으로, orthogonal subset의 성질에 의해 다음이 성립한다.
$$ y = \frac{B(y,s_i)}{B(s_i,s_i)} s_i $$

$S$는 orthonormal subset임으로, orthonormal subset의 정의에 의해 다음이 성립한다.
$$ \begin{aligned} & B(s_i,s_i) = \norm{s_i}^2 = 1 \\\implies& y = B(y,s_i)s_i \qed \end{aligned}  $$

#### 참고1
이 때, $B(y,s_i)$를 `Fourier coefficient`라고 한다.

### 명제2(Riesz representation)
$n$차원 inner product space $V/\F$가 있다고 하자.

$\forall g \in V^*$에 대해 다음을 증명하여라.
$$ \exist! v_g \in V \st g(\cdot) = B(\cdot, v_g)$$

**Proof**

$\beta$를 $V$의 orthonormal basis라 하자.

[uniquness]  
다음을 가정하자.
$$ \forall g \in V^*, \quad \exist v_g \in V \st g(\cdot) = B(\cdot, v_g)$$

그러면 Orthonormal basis의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} v_g &= B(v_g,\beta_i)\beta_i \\&= \overline{B(\beta_i, v_g)}\beta_i \\&= \overline{g(\beta_i)}\beta_i \end{aligned} $$

이 때, $g$는 $V^*$의 원소로 well-defined 함수임으로, 각각의 $\beta_i$마다 $\overline{g(\beta_i)}$가 유일한 값을 갖는다. 

따라서, $v_g$가 존재한다면 반드시 $v_g=\overline{g(\beta_i)}\beta_i$ 형태로 유일하게 결정된다. $\qed$

[existence]  
$v_g \in V$를 다음과 같이 정의하자.
$$ \forall g \in V^*, \quad v_g := \overline{g(\beta_i)}\beta_i $$

$x = a^i\beta_i \in V$이 있을 때, Orthonormal basis의 성질에 의해 다음이 성립한다.
$$ \begin{aligned} B(x,v_g) &= a^i g(\beta_j) B(\beta_i,\beta_j) \\&= a^i g(\beta_i) \\&= g(x) \end{aligned}$$

위의 두 결과를 조합하면, 다음과 같다.
$$ \forall g \in V^*, \quad \exist v_g \in V \st g(\cdot) = B(\cdot,v_g) \qed $$

#### 따름명제2.1
$n$차원 inner product space $V/\F$가 있다고 하자.

함수 $B^\flat$를 다음과 같이 정의하자.

$$ B^\flat : V \rightarrow V^* \st v \mapsto B(\cdot,v)$$

다음을 증명하여라.

$$ B^\flat \text{ is a bijective} $$

**Proof**

[injective]  
$V$의 임의의 element를 $v_1,v_2$라고 하자. 

$V$의 임의의 element를 $v$라고 하면 다음이 성립한다.

$$ \begin{aligned} & (B^\flat(v_1))(v) = (B^\flat(v_2))(v) \\\implies& B(v,v_1) = B(v,v_2) \\\implies& B(v, v_1-v_2) = 0_W \end{aligned} $$

임의의 $v$에서 위가 성립함으로, inner product의 성질에 의해 다음이 성립한다.

$$ v_1 - v_2 = 0_V \implies v_1 = v_2 \qed $$

[surjective]  
$V^*$의 임의의 element를 $f$라고 하자.

그러면 Reise representation theorem에 의해 다음이 성립한다.

$$ \exist v_f \st B^\flat(v_f) = f \qed $$

##### 참고
$B^\flat$의 역함수를 $B^\sharp$로 표기한다.

> Reference  
> [math.stackexchange - Inner product in dual space](https://math.stackexchange.com/questions/3486532/inner-product-in-dual-space)

#### 따름명제2.2
$n$차원 inner product space $V/\R$가 있다고 하자.

함수 $B^\flat$를 다음과 같이 정의하자.

$$ B^\flat : V \rightarrow V^* \st v \mapsto B(\cdot,v)$$

다음을 증명하여라.

$$ B^\flat \text{ is a vector space isomorphism} $$

**Proof**

따름명제2.1에 의해 $B^\flat$은 bijective임으로 linear map인지만 증명하면 된다.

[$B^\flat \in L(V,V^*)$]  
$V$의 임의의 element를 $v_1,v_2$, $\R$의 임의의 element를 $a$라고 하면 다음이 성립한다.

$$ \begin{aligned} B^\flat(v_1+av_2) &= B(\cdot, v_1 +av_2) \\&= B(\cdot,v_1) + aB(\cdot,v_2) \\&= B^\flat(v_1) + aB^\flat(v_2) \qed \end{aligned}  $$

[$B^\sharp \in L(V^*,V)$]  
$V^*$의 임의의 element를 $g_1,g_2$, $\R$의 임의의 element를 $a$라고 하자.

$V$의 임의의 element를 $v$라고 하면 다음이 성립한다.

$$ (g_1 + ag_2)(v) = g_1(v) + ag_2(v) $$

따라서 다음이 성립한다.

$$ B^\sharp(g_1+ag_2) = B^\sharp(g_1) + aB^\sharp(g_2) \qed $$

##### 참고
$V$와 $V^*$ 사이에 isomorphism은 basis에 무관함으로 natural isomorphism이다. 

> Reference  
> [math.stackexchange - Inner product in dual space](https://math.stackexchange.com/questions/3486532/inner-product-in-dual-space)  
> [math.stackexchange - natural isomorphism in linear algebra](https://math.stackexchange.com/questions/234127/natural-isomorphism-in-linear-algebra)

#### 참고1
$g \in V^*$가 있을 때, Reisz representation theorem에 의해 결정되는 $v_g$를 $g$에 대한 Reisz representation이라고 한다.

#### 참고2
$f \in V^*$와 $f$의 Riesz representation $v_f \in V$의 관계를 다음과 같이 바꿔보자
$$ \forall x \in V, \quad f(x) = B(v_f, x) $$

이럴 경우, inner product는 두번째 component에 대해서 conjugate linear이기 때문에 $f$는 conjugate linear map이 된다.

따라서, $f \notin V^*$이 되는 모순이 발생한다.

### 명제3(Schur's theorem)
$n$차원 inner product space $V/\F$와 $T \in \End(V)$가 있다고 하자.

$T$의 characteristic polynomial를 $\varphi_T(\lambda)$라 할 떄, 다음을 증명하여라.
$$ \varphi_T(\lambda) \text{is split} \implies \exist \text{ orthonormal basis } \beta \st \frak m_\beta^\beta(T) \text{ is an upper triangular matrix} $$

**Proof**

증명을 위해 수학적 귀납법을 사용한다.

먼저 $\dim(V) = 1$인 경우 자명하게 성립한다.

다음으로 $\dim(V) = n-1$일 때, 성립한다고 가정하고 $\dim(V) = n$이라고 하자.

$\lambda$를 $T$의 eigenvalue라 하면 adjoint의 성질에 의해 $\overline\lambda$는 $T^*$의 eigen value이다.

$\overline\lambda$의 크기가 1인 eigenvector를 $v$라하면,  orthogonal complement의 성질에 의해 다음이 성립한다.
$$ V = \span(v) \oplus \span(v)^\perp$$

보조명제3.1에 의해 $\span(v)^\perp$는 split됨으로, 귀납적 가정에 의해 다음이 성립한다.
$$ \exist \text{orthonormal basis } \gamma = \{\gamma_1, \cdots, \gamma_{n-1} \} \st \frak m_{\gamma}^{\gamma}(T|_{\span(v)^\perp}) \text{ be an upper triangular matrix.} $$

이 때, $V$이 기저 $\beta$를 다음과 같이 정의하자.
$$ \beta = \{ \gamma_1, \cdots, \gamma_{n-1}, v \}$$ 

그러면 다음이 성립한다.
$$ \begin{aligned} \frak m_{\beta}^{\beta}(T) &= \begin{bmatrix} \frak{m}_\beta(T(\gamma_1)) & \cdots & \frak{m}_\beta(T(\gamma_{n-1})) & \frak{m}_\beta(T(v)) \end{bmatrix} \\&= \begin{bmatrix} \begin{array}{c | c} \begin{array}{} \\ \frak m_\gamma^\gamma(T|_{\span(v)^\perp}) \\  \\ \hline 0 \end{array} & \begin{array}{} a_1 \\ \vdots \\ a_n \end{array} \end{array} \end{bmatrix} \end{aligned} $$

$$ \text{Where, } T(v) = a_1 \gamma_1 + \cdots + a_{n-1}\gamma_{n-1} + a_nv $$

이 때, $\frak m_{\gamma}^{\gamma}(T|_{\span(v)^\perp})$가 upper triangular matrix임으로 $\frak m_{\beta}^{\beta}(T)$도 upper trianular matrix가 된다. 

동시에 $\beta$를 이루고 있는 $\gamma$와 $v$는 direct sum 관계에 있는 두 공간의 기저임으로 $\beta$는 $V$의 orthonormal basis가 된다.$\qed$

#### 보조명제3.1
다음을 증명하여라.
$$ \span(v)^\perp \text{ is split} $$

**Proof**

$V$에 대해 다음이 성립한다.
$$ V = \span(v) \oplus \span(v)^\perp$$

$\span(v)^\perp$의 임의의 기저를 $\gamma = \{ \gamma_1, \cdots, \gamma_{n-1} \}$이라 할 때, $V$의 기저 $\beta$을 다음과 같이 정의하자
$$ \beta = \{ \gamma_1, \cdots, \gamma_{n-1}, v \}$$ 

보조명제3.1.1에 의해서 $\span(v)^\perp$는 $T$ invariant임으로 다음이 성립한다.
$$ \begin{aligned} \frak m_{\beta}^{\beta}(T) &= \begin{bmatrix} \frak{m}_\beta(T(\gamma_1)) & \cdots & \frak{m}_\beta(T(\gamma_{n-1})) & \frak{m}_\beta(T(v)) \end{bmatrix} \\&= \begin{bmatrix} \begin{array}{c | c} \begin{array}{} \\ \frak m_\gamma^\gamma(T|_{\span(v)^\perp}) \\  \\ \hline 0 \end{array} & \begin{array}{} a_1 \\ \vdots \\ a_n \end{array} \end{array} \end{bmatrix} \end{aligned} $$

$$ \text{Where, } T(v) = a_1 \gamma_1 + \cdots + a_{n-1}\gamma_{n-1} + a_nv $$

따라서, $\det(T - a_nI) = 0$임으로 $T$가 split된 1차식 중에는 $(a_n - \lambda)$가 포함되어 있다.

이 때, detrminant의 block matrix에 대한 성질에 의해 다음이 성립한다.
$$\det(\frak m_{\beta}^{\beta}(T) - \lambda I_n) = \det\Big( m_\gamma^\gamma(T|_{\span(v)^\perp}) - \lambda I_{n-1}\Big)(a_n - \lambda)$$

따라서, $T$의 split 된 1차식들 중 $(a_n - \lambda)$제외한 나머지 1차식들로 $T|_{\span(v)^\perp}$이 구성되어 있다.

그럼으로, $T|_{\span(v)^\perp}$는 split 된다. $\qed$


##### 보조명제3.1.1
다음을 증명하여라.
$$ \span(v)^\perp \text{ is a } T \text{ invariant} $$

**Proof**

$x \in \span(v)^\perp$라 하면 다음이 성립한다.
$$ B(T(x),v) = B(x, T^*(v)) = B(x, \overline\lambda v) = \lambda B(x, v) = 0_\F $$

임의의 $x \in \span(v)^\perp$에 대해, $T(x)$와 $v$가 수직함으로, $T(x) \in \span(v)^\perp$이다.

따라서, $\span(v)^\perp$는 $T \text{ invariant}$이다. $\qed$

###### 참고
$\span(v)$는 일반적으로 $T$ invariant가 아니다.

#### 따름명제3.1
다음을 증명하여라.
$$ \text{ every complex matrix is similar to an upper triangular matrix} $$

**Proof**

$A \in M_{nn}(\C)$가 있다고 하자.

Fundamental theorem of algebra에 의해 $\varphi_A(\lambda)$는 항상 split된다. 

따라서, Schur's theorem에 의해 $\frak m_\beta^\beta(L_A)$가 upper triangular matrix이 되는 orthonormal basis $\beta$가 존재한다.

그럼으로, 다음이 성립한다.
$$ \begin{aligned} \frak m_\beta^\beta(L_A) &= \frak m_\epsilon^\beta(id) \frak m_\epsilon^\epsilon(L_A) \frak m_\beta^\epsilon(L_A) \\&= C^{-1}AC \end{aligned}  $$

즉, $\frak m_\beta^\beta(L_A) \sim A$이다. $\qed$

#### 참고1
$\frak m_\beta^\beta(T)$가 다음과 같은 upper triangular matrix로 주어진다고 하자.
$$ \frak m_\beta^\beta(T) = \begin{bmatrix} a_1 & \cdots & & * \\ & a_2 \\ & & \ddots & \vdots \\ 0 & & & a_n \end{bmatrix} $$

$T(\beta_1) = a_1\beta_1$이 되기 때문에 $\beta_1$은 eigen vector, $a_1$은 eigen value가 된다.

#### 참고2
$\frak m_\beta^\beta(T)$가 다음과 같은 upper triangular matrix로 주어진다고 하자.
$$ \frak m_\beta^\beta(T) = \begin{bmatrix} a_1 & \cdots & & * \\ & a_2 \\ & & \ddots & \vdots \\ 0 & & & a_n \end{bmatrix} $$

upper triangular matrix의 determinant 성질에 의해 다음이 성립한다.
$$ \det(T-\lambda I) = \prod_{i=1}^n (a_i - \lambda) $$ 

따라서, $a_1, \cdots, a_n$은 eigenvalue가 된다.