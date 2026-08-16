# Riesz Representation Theorem

## 한 줄 요약

Finite-dimensional inner product space의 모든 linear functional은 어떤 vector와의 inner product로 유일하게 표현된다.

## Motivation

[Dual Space](<41 Dual Space.md>)에서 정의한 linear functional $g\in V^*$는 vector를 scalar로 측정한다. 한편 [Inner Product Space](<30 Inner Product Space.md>)의 inner product에서 두 번째 인자를 $v$로 고정한 함수

$$
B(\cdot,v):V\rightarrow\F
$$

도 첫 번째 인자에 대해 linear하므로 linear functional이다. 그렇다면 반대로 모든 linear functional을 어떤 vector와의 inner product로 표현할 수 있는지, 그리고 그 vector가 유일한지 묻게 된다. Finite-dimensional inner product space에서는 Riesz representation theorem이 이 질문에 답한다.

## 정리

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

### 참고1
$g\in V^*$가 있을 때, Riesz representation theorem에 의해 유일하게 결정되는 $v_g$를 $g$의 `Riesz representation`이라고 한다.

이 theorem은 inner product가 주어지면 linear functional을 vector로 표현할 수 있음을 보여준다. Gram matrix를 이용하면 그 vector의 coordinate도 구체적으로 계산할 수 있다.

### 참고2
$f\in V^*$와 $f$의 Riesz representation $v_f\in V$의 인자 순서를 다음과 같이 바꿔보자.

$$ \forall x \in V, \quad f(x) = B(v_f, x) $$

$\F=\C$이면 이 문서의 convention에서 inner product는 두 번째 인자에 대해 conjugate linear하므로 오른쪽 함수는 일반적으로 linear functional이 아니다. 따라서 complex inner product space에서는 $f(x)=B(x,v_f)$라는 인자 순서가 중요하다. $\F=\R$이면 conjugation이 값을 바꾸지 않으므로 이 차이가 사라진다.

### 참고3
$\beta$가 orthonormal basis이면 $G=I$이므로 Riesz representation은 다음과 같이 단순해진다.

$$
a=\overline f
\qquad\Longrightarrow\qquad
v_g=\overline{g(\beta_i)}\beta_i.
$$

Riesz representation은 유일한 vector이므로 basis 선택에 의존하지 않는다.

따라서 $\gamma$ 가 $V$ 의 임의의 basis 일 때, $v_g$ 의 matrix representation 은 구체적으로 다음과 같이 적을 수 있다.

$$ [v_g]_\gamma = [id]^\gamma_\beta [v_g]_\beta $$

### 따름명제1
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

그러면 Riesz representation theorem에 의해 다음이 성립한다.

$$ \exist v_f \st B^\flat(v_f) = f \qed $$

#### 참고
$B^\flat$의 역함수를 $B^\sharp$로 표기한다.

> Reference  
> [math.stackexchange - Inner product in dual space](https://math.stackexchange.com/questions/3486532/inner-product-in-dual-space)

### 따름명제2
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

#### 참고
$B^\flat$은 coordinate basis의 선택에는 의존하지 않지만, inner product $B$의 선택에는 의존한다. 따라서 vector space structure만으로 주어지는 canonical isomorphism은 아니며, 선택한 real inner product가 $V$와 $V^*$를 식별해 주는 isomorphism이다.

> Reference  
> [math.stackexchange - Inner product in dual space](https://math.stackexchange.com/questions/3486532/inner-product-in-dual-space)  
> [math.stackexchange - natural isomorphism in linear algebra](https://math.stackexchange.com/questions/234127/natural-isomorphism-in-linear-algebra)
