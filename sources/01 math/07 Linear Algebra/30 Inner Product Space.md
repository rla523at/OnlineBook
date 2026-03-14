# Inner Product Space
## Inner Product
$n$차원 vector space $V/\F$가 있을 때, 다음 성질을 만족하는 함수 $B:V \times V \rightarrow \F$를 `내적(inner product)`라 한다.

$x,y,z \in V$이고 $c \in \F$일 때,
1. $$ B(x+y, z) = B(x,z) + B(y,z) $$
2. $$ B(cx,y) = cB(x,y) $$
3. $$ B(x,y) = \overline{B(y,x)} $$
4. $$ 0_F \le B(x,x) \quad (B(x,x) = 0_\F \text{ when } x = 0_V ) $$

### 참고1
1번 성질에 의해 $B$ 는 첫번째 인자에 대해서 선형성을 갖는다.

하지만 2번 성질에 의해 두번 째 인자에 대해서는 선형성을 갖지 않음으로 $B$는 symmetric하지 않고 bilinear하지 않다. 이렇게 scalar multiplcation은 conjugate되서 나오고 벡터의 덧셈은 보존될 때 이를 `Hermitian` 혹은 `conjugate linear`라고 한다.

$$ B(x,cy + z) = \bar c B(x,y) + B(x,z) $$

### 참고2
$\F = \R$이면 $B$ 는 두번째 인자에 대해서도 선형성을 갖는다.

$$ B(x,cy + z) = c B(x,y) + B(x,z) $$

즉, $B$는 symmetric하고 따라서 bilinear이다.

### 참고3
다음과 같은 표기법을 많이 사용한다.

$$ B(x,y) \equiv \lang x,y \rang $$

### 참고4(Gram Matrix)
$x,y \in V$ 와 $V$ 의 임의의 basis 에 $\beta, \gamma$ 가 있을 떄 다음이 성립한다.

$$ B(x,y) = x_iy_jB(\beta_i,\gamma_j) $$

$B(\beta_i,\gamma_j) = G_{ij}$ 라고 하면 다음이 성립한다.

$$ B(x,y) = ([x]_\beta)^T G [y]_\gamma $$

$G$ 를 `Gram Matrix` 라고 하며 inner product 의 matrix representation 이다.

Gram Matrix 는 inner product 의 선택과 basis 의 선택에 따라서 달라진다.

### 명제
$n$차원 vector space $V/\F$와 내적 $B$가 있다고 하자.

$v \in V$가 있을 때, 다음을 증명하여라.

$$ \forall w \in V, \quad B(v,w) = 0_\F \iff v = 0_V $$

**Proof**

$\forall w \in V$에 대해 성립함으로, $w = v$로 두면 다음이 성립한다.

$$ B(v,v) = 0_\F $$

이 떄, inner product의 4번 성질에 의해 $v = 0_V$이다. $\qed$

## Inner Product Space
$n$차원 vector space $V/\F$에 내적 $B$가 주어진 공간을 `내적 공간(inner product space)`라고 한다.

## Norm
$n$차원 inner product space $V/\F$가 있을 때, `norm`은 다음과 같이 정의된 함수이다.

$$ n : V \rightarrow \R \st v \mapsto \sqrt{ B(v,v)} $$

### 참고
다음과 같은 표기법을 많이 사용한다.

$$ n(v) \equiv \lVert v \rVert $$

## Distance
$n$차원 inner product space $V/\F$가 있을 때, `distance`는 다음과 같이 정의된 함수이다.

$$ d : V \times V \rightarrow \R \st (v,w) \mapsto \lVert v-w \rVert $$

## Angle
$n$차원 inner product space $V/\F$가 있을 때, `angle`는 다음과 같이 정의된 함수이다.

$$ a : V \times V \rightarrow \R \st (v,w) \mapsto \cos^{-1} \bigg(\frac{B(v,w)}{\lVert v \rVert \lVert w \rVert} \bigg) $$

### 참고1
두 벡터 $v,w \in V$가 주어졌을 때, 두 벡터 사이의 각도를 $\theta$라고 하면 다음이 성립한다.

$$ \cos \theta =  \frac{B(v,w)}{\lVert v \rVert \lVert w \rVert} $$

### 참고2
두 벡터 $v,w \in V$가 주어졌을 때 $B(v,w) = 0_\F$ 면 $v,w$ 는 서로 `orthogonal`(직교) 하다고 한다.

$v,w$ 가 직교하는 경우 $v \perp w$ 라고 표기한다. 

### 정리1 (Pythagorean theorem)
$n$차원 inner product space $V/\F$ 와 $v,w \in V$ 가 있다고 하자.

이 떄, 다음을 증명하여라

$$ v \perp w \implies \norm{v+w}^2 = \norm{v}^2 + \norm{w}^2 $$

**Proof**

$v \perp w$ 임으로 $B(v,w) = B(w,v) = 0$ 이다.

따라서, 다음이 성립한다.

$$ \begin{aligned}
\norm{v+w}^2 &= B(v+w,v+w) \\
&= B(v,v) + B(v,w)+ B(w,v) + B(w,w) \\
&= B(v,v) + B(w,w) \\
&= \norm{v}^2 + \norm{w}^2 \qed
\end{aligned}$$

## Projection
$n$차원 inner product space $V/\F$ 와 $v,w \in V$ 가 있다고 하자.

$w$ 를 $v$ 방향으로 projection 시켜 얻은 $v$ 와 평행한 vector 를 $w^\parallel$ 라고 하자.

$w-w^\parallel$ 는 $v$ 와 orthogonal 한 부분만 남아 있어야 함으로 $w-w^\parallel \perp v$ 이고 $B(w-w^\parallel, v) = 0_\F$ 이 성립해야 한다.

이 떄, $w^\parallel$ 는 $v$ 와 평행한 vector 임으로 어떤 $\alpha \in \F$ 가 있어서 $w^\parallel = \alpha v$ 으로 표현할 수 있고 다음이 성립한다.

$$ \begin{aligned}
B(w-w^\parallel, v) &= B(w-\alpha v, v)  \\
&= B(w,v) - \alpha B(v,v) \\
\end{aligned}  $$

이를 $B(w-w^\parallel, v) = 0_\F$ 에 대입하고 $\alpha$ 에 대해 정리하면 다음과 같다.

$$ \alpha = \frac{B(w,v)}{B(v,v)} $$

따라서, 다음이 성립한다.

$$w^\parallel = \frac{B(w,v)}{B(v,v)}v $$

### 참고
일반적으로 $w$ 를 $v$ 방향으로 projection 시킨 vector 는 다음과 표기한다.

$$ \text{proj}_{v}(w) $$

## Orthogonal Property
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V$ 가 다음을 만족할 때, $S$ 가 `orthogonal property` 를 갖는다고 한다.

$$ i \neq j \implies B(s_i, s_j) = 0 $$

## Orthogonal Subset
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$ 가 orthogonal property 를 갖는 경우 `orthogonal subset`이라고 한다.

### 참고1
$0_V$ 를 포함하지 않는 Subset 이 orthogonal property 를 갖는 경우를 orthogonal subset 이라고 한다.

### 참고2
Orthogonal subset $S$ 가 다음을 만족할 때, $S$를 `orthonormal subset`이라고 한다.

$$ \norm{s_i} = 1 $$

즉, orthonormal subset 은 orthogonal subset 을 크기가 1이 되게 normalize 한 경우이다.

### 명제1
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 orthogonal subset일 때 다음을 증명하여라.

$$ S \text { is linearly independent} $$

**Proof**

어떤 $a_i \in \F, \quad i \in \Set{1,\cdots,k}$ 들에 대해서 $a_is_i = 0_V$ 라고하자.

임의의 $j \in \Set{1,\cdots,k}$ 을 고정하면 내적에 성질에 의해 다음이 성립한다.

$$ B(a_is_i, s_j) = a_j B(s_j,s_j) = 0_\F \quad (\text{no sum over } j) $$

$s_j \neq 0_V$ 임으로 inner product 의 정의에 의해 $0 < B(s_j,s_j)$ 임으로 $a_j = 0$ 다.

즉, 모든 $a_i = 0_\F$ 임으로 linearly independent하다. $\qed$

#### 참고
$S$ 가 linearly independent set 임으로 $\span(S)$ 의 basis 는 $S$ 이다. 

### 명제2
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 orthogonal subset일 때 다음을 증명하여라.

$$ y \in \span(S) \implies y = \sum_{i=1}^k \text{proj}_{s_i}(y) $$

**Proof** 

$y \in \span(S)$ 임으로 다음과 같이 표현할 수 있다.

$$ y = a_is_i, \quad a_i \in \F $$

임의의 $j \in \Set{1,\cdots,k}$ 을 고정하면 orthogonal subset 의 성질에 의해 다음이 성립한다.

$$ \begin{aligned}  
B(y, s_j) &= B(a_is_i, s_j) \\
&= a_j B(s_j, s_j) \quad (\text{no sum over } j) 
\end{aligned} $$

위를 $a_j$ 에 대해서 정리하면 

$$ a_j = \frac{B(y,s_j)}{B(s_j, s_j)}  \quad (\text{no sum over } j) $$

이를 이용해 $y$ 를 표현하면

$$ y = a_is_i = \frac{B(y,s_i)}{B(s_i, s_i)} s_i = \sum_{i=1}^k \text{proj}_{s_i}(y) \qed $$

#### 참고1
$S$ 을 span 한 공간에 있는 vector 를 $x$ 라고 하면 $x$ 는 $S$ 의 각 원소에 projection 시킨 vector 들로 표현된다.

#### 참고2
$S$ 을 span 한 공간에 basis 는 $S$ 이고 임의의 $x \in \span(S)$ 의 coordinate 를 `Fourier coefficient`라고 하며 다음과 같다. 

$$ x = \frac{B(x,s_i)}{B(s_i, s_i)} s_i $$

만약 $S$ 가 orthonormal subset 인 경우 Fourier coefficient 는 $B(y,s_i)$ 로 간단해 진다. 

### 명제3 (Gram-Schmidts Process)
$n$차원 inner product space $V / \F$가 있다고 하자.

$V$ 의 basis 이면서 동시에 orthogonal subset 인 집합을 orthogonal basis 라고 할 때, 다음을 증명하여라. 

$$ V \text{ has orthogonal basis}$$

**Proof**

$V$ 의 임의의 basis 를 $\beta$ 라고 하자.

이 때, $\beta'$ 을 다음과 같이 정의하자.

$$ \beta' = 
\begin{dcases}
\beta'_1 &= \beta_1 \\ 
\beta'_i &= \beta_i - \sum_{j=1}^{i-1} \text{proj}_{\beta'_j}(\beta_i), & (2 \le i)
\end{dcases} $$

보조명제에 의해 $\beta'$ 은 orthogonal subset 이다.

명제1에 의해 $\beta'$ 은 linear independet 이고 cardinallity 가 $n$ 임으로 $\beta'$ 은 basis 이다.

따라서 $\beta'$ 은 orthogonal basis 이다. $\qed$

#### 따름명제

$$ V \text{ has orthonormal basis} $$

**Proof**

명제 3에 의해 $V$ 가 orthogonal basis 를 갖음으로, orthonormal basis 를 normalize 하면 orthonormal basis 가 된다. $\qed$

#### 보조명제
$\beta'$ 이 orthogonal subset 임을 증명하여라.

**Proof**

[$\beta' \subset V - \Set{0_{\F}}$]

임의의 $1 \le i$ 에 대해서 $\beta'_i = 0_V$ 라고 하자.

그러면 

$$ 0_V = \beta_i - \sum_{j=1}^{i-1} \text{proj}_{\beta'_j}(\beta_i) = \beta_i - \sum_{j=1}^{i-1} \frac{B(\beta_i,\beta'_j)}{B(\beta'_j, \beta'_j)} \beta'_j $$

이 때, $\beta'_j$ 는 $\beta_1,\cdots,\beta_j$ 의 선형결합으로 표현됨으로 $\beta_i$ 는 $\beta_1,\cdots,\beta_{i-1}$ 의 선형결합으로 표현된다.

이는 $\beta$ 가 basis 라는, 정확히는 linear independent set 이라는 사실에 모순이 발생한다.

따라서 proof by contradiction 에 의해 임의의 $1 \le i$ 에 대해서 $\beta'_i \neq 0_V$ 이다. $\qed$

[orthogonal property]

수학적 귀납법을 이용해서 증명한다.

$\Set{\beta'_1}$ 은 자명하게 orthogonal subset 이다.

이 때, 임의의 $2 \le i$ 에 대해서 $\Set{\beta'_1,\cdots,\beta'_{i-1}}$ 이 orthogonal subset 이라고 가정하자.

이 때, $\Set{\beta'_1,\cdots,\beta'_{i}}$ 이 orthogonal subset 임을 증명하자.

임의의 $k < i$ 에 대해서 다음이 성립한다.

$$ \begin{aligned}
B(\beta'_i,\beta'_k) &= B(\beta_i - \sum_{j=1}^{i-1} P_{\beta'_j}{\beta_i}, \beta'_k) \\
&= B(\beta_i, \beta'_k) - \sum_{j=1}^{i-1}B(P_{\beta'_j}{\beta_i}, \beta'_k) 
\end{aligned}  $$


$P_{\beta'_j}{\beta_i}$ 는 $\beta'_j$ 와 평행한 vector 이고 $\Set{\beta'_1,\cdots,\beta'_{i-1}}$ 이 orthogonal subset 임으로 다음이 성립한다.

$$ j \neq k \implies  B(P_{\beta'_j}{\beta_i}, \beta'_k) = 0 $$

이를 위에 식에 대입해서 정리하면

$$ \begin{aligned}
B(\beta'_i,\beta'_k) &= B(\beta_i, \beta'_k) - \sum_{j=1}^{i-1}B(P_{\beta'_j}{\beta_i}, \beta'_k) \\
&= B(\beta_i, \beta'_k) - B(P_{\beta'_k}{\beta_i}, \beta'_k) \\
&= B(\beta_i, \beta'_k) - \frac{B(\beta_i,\beta'_k)}{B(\beta'_k, \beta'_k)}B( \beta'_k, \beta'_k) \\
&= 0_{\F} \qed
\end{aligned}  $$

#### 참고1
subset $S \subset V$ 가 주어졌을 떄, 위와 같이 $S'$ 을 정의하는 방식을 `Gram-Schmidts Process` 라고 한다.

Gram-Schmidts Process 는 임의의 Subset 이 주어졌을 때 Orthogonal Property 를 갖는 Subset 으로 바꾸는 구체적인 방법이다.

만약 linear independet subset 인 경우 Gram-Schmidts Process 에 결과로 주어지는 Subset 은 orthogonal subset 이 된다.

#### 참고2
Orthogonal Basis 는 inner product 의 선택에 따라 달라진다.

## Orthogonal Complement
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
$n$차원 inner product space $V/\F$ 와 $W \le V$ 가 있을 떄, 다음을 증명하여라.

$$ W \cap W^{\perp} = \set{0_V} $$

**Proof**

$w \in W \cap W^{\perp}$ 라고 하면 다음이 성립한다.

$$ B(w,w) = 0_V \implies w = 0_V \qed $$

### 명제3
$n$차원 inner product space $V / \F$와 $k \le n$ 차원 subspace $W \le V$ 가 있다고 하자.

$W$ 의 orthogonal basis 를 $\beta$ 라고 할 때, 임의의 $v \in V$ 에 대해 다음을 증명하여라.

$$ v - \sum_{i=1}^k P_{\beta_i}(v) \in W^{\perp} $$

**Proof**

임의의 $\beta_i$ 에 대해서 다음이 성립한다.

$$ \begin{aligned}
B(v - \sum_{j=1}^k P_{\beta_j}(v), \beta_i) &= B(v, \beta_i) - \sum_{j=1}^k \frac{B(v,\beta_j)}{B(\beta_j, \beta_j)} B(\beta_j, \beta_i) \\
&= B(v, \beta_i) - \frac{B(v,\beta_i)}{B(\beta_i, \beta_i)} B(\beta_i, \beta_i) \\
&= 0_\F 
\end{aligned} $$

따라서, $\forall w \in W$ 에 대해서 다음이 성립한다.

$$ \begin{aligned}
B(v - \sum_{j=1}^k P_{\beta_j}(v),w) &= a_j B(v - \sum_{j=1}^k P_{\beta_j}(v), \beta_j) \\
&= 0_\F \qed
\end{aligned} $$

### 명제4
$n$차원 inner product space $V / \F$와 $k \le n$ 차원 subspace $W \le V$ 가 있다고 하자.

$W$ 의 orthogonal basis 를 $\beta, \gamma$ 라고 할 때, 임의의 $v \in V$ 에 대해 다음을 증명하여라.

$$ \sum_{i=1}^k P_{\beta_i}(v) = \sum_{i=1}^k P_{\gamma_i}(v) $$

**Proof**

$\sum_{i=1}^k P_{\gamma_i}(v), \sum_{i=1}^k P_{\beta_i}(v) \in W$ 임으로 다음이 성립한다.

$$ \sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) \in W $$

그리고 명제3에 의해 $v - \sum_{i=1}^k P_{\beta_i}(v),v - \sum_{i=1}^k P_{\gamma_i}(v) \in W^\perp$ 임으로 다음이 성립한다.

$$ \sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) = \left( v - \sum_{i=1}^k P_{\gamma_i}(v) \right) - \left( v - \sum_{i=1}^k P_{\beta_i}(v) \right) \in W^\perp $$

그러면 $\sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) \in W \cap W^\perp$ 임으로 명제2에 의해 다음이 성립한다.

$$ \sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) = 0_\F \qed $$

#### 참고
$v \in V$ 에 대해서 $W$ 로의 projection $P_W(v)$ 를 다음과 같이 정의한다.

$$ P_W(v) = \sum_{i=1}^k P_{\beta_i}(v), \quad \beta \text{ is any orthogonal basis of } W $$

### 명제5
$n$차원 inner product space $V / \F$와 $k \le n$ 차원 subspace $W \le V$ 가 있을 때, 다음을 증명하여라.

$$ \forall w \in W, \quad \norm{v-P_W(v)} \le \norm{v-w} $$

**Proof**

$v-w = (v-P_W(v)) - (P_W(v)-w)$ 이고 $v-P_W(v) \in W^\perp$, $P_W(v)-w \in W$ 임으로 다음이 성립한다.

$$ (v-P_W(v)) \perp (P_W(v)-w) $$

따라서, Pythagorean theorem 에 의해 다음이 성립한다.

$$ \norm{v-w}^2 = \norm{v-P_W(v)}^2 + \norm{P_W(v)-w}^2 $$

inner product 의 성질에 의해 $0_F \le \norm{P_W(v)-w}^2$ 이고 $0_F \le \norm{\cdot}$ 임으로 다음이 성립한다.

$$ \norm{v-P_W(v)}^2 \le \norm{v-w}^2 \implies \norm{v-P_W(v)} \le \norm{v-w} \qed $$

#### 참고
$P_W(v)$ 는 $W$ 의 모든 vector 중 가장 $v$ 에 가깝다. 따라서 $P_W(v)$ 를 `closest vector` 라고 한다.

### 명제6
$n$차원 inner product space $V/\F$가 있다고 하자.

$W \le V$가 있을 때, 다음을 증명하여라.

$$ V = W \oplus W^\perp $$

**Proof**

[$V = W + W^\perp$]  
임의의 $v\in V$ 에 대해 $P_W(v) \in W$ 이고 $v-P_W(v) \in W^\perp$ 임으로 $V = W + W^\perp$이다. $\qed$

[$W \cap W^\perp = \{ 0_V \}$]  
$w \in W \cap W^\perp$라 하자.

$w \in W$이면서 $w \in W^\perp$임으로, $W^\perp$의 정의에 의해 다음이 성립한다.

$$ B(w,w) = 0_\F $$

내적의 정의에 의해 $w = 0_V$이다. $\qed$

#### 참고1
$v \in V$면 $v = w + w^\perp, \enspace w \in W, w^\perp \in W^\perp$이 성립하고 direct sum의 성질에 의해 이러한 표현법이 유일하다.

### 명제7(Riesz representation)
$n$차원 inner product space $V/\F$가 있다고 하자.

$\forall g \in V^*$에 대해 다음을 증명하여라.
$$ \exist! v_g \in V \st g(\cdot) = B(\cdot, v_g) $$

**Proof**

[existence]  
$V$ 의 임의의 basis 를 $\beta$라고 할 때, 어떤 $v_g = a_i\beta_i \in V$ 가 있어 임의의 $w = b_j\beta_j$ 에 대해 다음을 만족한다고 하자.

$$ \begin{aligned}
g(w) &= B(w,v_g) \\
b_j g(\beta_j) &= b_j\overline{a_i} B(\beta_j,\beta_i) \\ 
\end{aligned} $$

임의의 $b_j$ 에 대해서 위가 성립해야 됨으로 다음이 성립한다. 

$$ g(\beta_i) = B(\beta_i, \beta_j )\overline{a_j} $$

$G_{ij} = B(\beta_i,\beta_j)$, $f_i = g(\beta_i)$  라고하면 $\beta$ 는 linearly independent set 임으로 $G$ 는 invertible 하다. 따라서, 다음이 성립한다.

$$ a = \overline{G^{-1}f} $$

즉, $[v_g]_\beta = a$ 면 $g(\cdot) = B(\cdot, v_g)$ 를 만족하는 vector 이다. $\qed$ 

[uniquness]  
$u,v \in V$ 가 임의의 $w \in V$ 에 대해 $g(w) = B(w, u) = B(w, v)$ 름 만족하면 다음이 성립한다.

$$ \begin{aligned}
B(w, u) - B(w, v) = 0_\F \\
B(w, u-v) = 0_\F \\
\end{aligned}  $$

임의의 $w$ 에 대해서 만족해야 함으로 $u-v = 0_V$ 여야 하고, $u=v$다. $\qed$

#### 참고1
$g \in V^*$가 있을 때, Reisz representation theorem에 의해 결정되는 $v_g$를 $g$에 대한 Reisz representation이라고 한다.

Reisz represnetation theorem 은 inner proudct space 에서 linear functional 은 vetor 로 나타내는 방법을 알려주는 theorem 이다.

그리고 Reisz representation theorem 이 강력한 이유는 정확히 어떤 vector 로 나타낼 수 있는지 구체적으로 계산할 수 있다는 점이다.

#### 참고2
$f \in V^*$와 $f$의 Riesz representation $v_f \in V$의 관계를 다음과 같이 바꿔보자
$$ \forall x \in V, \quad f(x) = B(v_f, x) $$

이럴 경우, inner product는 두번째 component에 대해서 conjugate linear이기 때문에 $f$는 conjugate linear map이 된다.

따라서, $f \notin V^*$이 되는 모순이 발생한다.

#### 참고3
$\beta$ 가 orthonormal basis 면 $G = I$ 임으로 Riesz representation 은 다음과 같이 단순해진다.

$$ a = \overline{f} \implies v_g = g(\beta_i)\beta_i $$

그리고 Riesz representation 은 유일한 vector 임으로 basis 선택에 의존하지 않는다. 

따라서 $\gamma$ 가 $V$ 의 임의의 basis 일 때, $v_g$ 의 matrix representation 은 구체적으로 다음과 같이 적을 수 있다.

$$ [v_g]_\gamma = [id]^\gamma_\beta [v_g]_\beta $$

#### 따름명제1
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

#### 따름명제2
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