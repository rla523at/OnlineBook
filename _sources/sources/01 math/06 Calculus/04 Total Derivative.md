# Total Derivative
## 정의
open subset $U \subset \R^n$과 함수 $f : U \rightarrow \R^m$이 있다고 하자.

$a \in U$와 linear map $L: \R^n \rightarrow \R^m $이 있다고 하자.

$L$이 다음을 만족할 경우, $L$을 $a$에서 $f$의  `total derivative`라고 한다.

$$ \lim_{h \rightarrow 0_n} \frac{1}{|h|}(f(a + h) - f(a) - L(h)) = 0_n $$

### 참고1
$a$에서 $f$의 total derivative를 $D^{f(a)} : \R^n \rightarrow \R^m$로 표기한다.

### 참고2
$D^{f(a)}$가 존재할 떄 $f$는 $a$에서 `미분가능(differentiable)`하다고 한다.


### 명제1
$\R^n$의 open subset $U$와 함수 $f : U \rightarrow \R^m$가 있다고 하자.

$\R^n$과 $\R^m$의 basis가 각 각 $\beta,\gamma$이고 $a \in U$일 때, 다음을 증명하여라.

$$ \exist D^{f(a)} : \R^n \rightarrow \R^m \implies  \mathfrak{m}_\beta^\gamma(D^{f(a)}) = \begin{bmatrix} \mathfrak{m}_\gamma(D_1^{f(a)}) &\cdots& \mathfrak{m}_\gamma(D_n^{f(a)}) \end{bmatrix}$$

**Proof**

$D^{f(a)}$이 linear map 임으로 행렬표현은 다음과 같다.

$$ \mathfrak{m}_\beta^\gamma(D^{f(a)}) = \begin{bmatrix} \mathfrak{m}_\gamma(D^{f(a)}(\beta_1)) & \cdots & \mathfrak{m}_\gamma(D^{f(a)}(\beta_n)) \end{bmatrix} $$

보조명제1.1에 의해 다음이 성립한다.

$$ \begin{aligned} \begin{bmatrix} \mathfrak{m}_\gamma(D^{f(a)}(\beta_1)) & \cdots & \mathfrak{m}_\gamma(D^{f(a)}(\beta_n)) \end{bmatrix} &= \begin{bmatrix} \mathfrak{m}_\gamma(D_1^{f(a)}) &\cdots& \mathfrak{m}_\gamma(D_n^{f(a)}) \end{bmatrix} \qed \end{aligned} $$

#### 보조명제1.1
다음을 증명하여라.

$$ D^{f(a)}(\beta_i) = D_i^{f(a)} $$

**Proof**

$h = t\beta_i$라 하면 total derivative의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{t \rightarrow 0} \frac{1}{\norm{t \beta_i}}(f(a+t\beta_i) - f(a) - D^{f(a)}(t\beta_i)) = 0_n \\\implies & \lim_{t\rightarrow 0} \frac{1}{t}(f(a + t \beta_i) - f(a)) -D^{f(a)}(\beta_i) = 0_n \\\implies & D_i^{f(a)} = D^{f(a)}(\beta_i) \qed \end{aligned} $$


#### 참고1
$L$의 형태가 결정되어 있음으로 $L$이 존재한다면 유일함을 알 수 있다.

즉, uniqueness가 보장된다.

#### 참고2
Total derivative $Df$의 행렬표현을 $f$의 `Jacobian matrix`라고 부르며 $J_f$로 표기한다.

$$ J_f(a) := \mathfrak{m}_\beta^\gamma(D^{f(a)}) = \begin{bmatrix} \mathfrak{m}_\gamma(D_1^{f(a)}) &\cdots& \mathfrak{m}_\gamma(D_n^{f(a)}) \end{bmatrix}$$

#### 참고3
$\mathfrak{m}_\beta^\gamma(D^{f(a)}) =J_f(a)$이려면 $f$가 differtiable해야 한다.

### 명제2
$\R^n$의 open set $U$와 linear map $f : U \rightarrow \R^m$가 있다고 하자.

$\R^n$과 $\R^m$의 basis가 각 각 $\beta,\gamma$이고 $a \in U$에서 $f$가 미분가능할 때, 다음을 증명하여라.

$$ D^{f(a)} = f $$

**Proof**

명제1에 의해 다음이 성립한다.

$$ \mathfrak{m}_\beta^\gamma(D^{f(a)}) = \begin{bmatrix} \mathfrak{m}_\gamma(D_1^{f(a)}) &\cdots& \mathfrak{m}_\gamma(D_n^{f(a)}) \end{bmatrix} $$

Partial derivative의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} \begin{bmatrix} \mathfrak{m}_\gamma(D_1^{f(a)}) &\cdots& \mathfrak{m}_\gamma(D_n^{f(a)}) \end{bmatrix} &= \begin{bmatrix} \mathfrak{m}_\gamma(f(\beta_1)) &\cdots& \mathfrak{m}_\gamma(f(\beta_n)) \end{bmatrix} \\&= \mathfrak{m}_\beta^\gamma(f) \end{aligned} $$

위의 결과를 종합하면 $\mathfrak{m}_\beta^\gamma(D^{f(a)}) = \mathfrak{m}_\beta^\gamma(f)$임으로 linear algebra에 의해 다음이 성립한다.

$$ D^{f(a)} = f \qed $$

#### 따름명제2.1
다음을 증명하여라.

$$ J_f(a) = \mathfrak{m}_\beta^\gamma(f) $$

**Proof**

명제2와 Jacobian의 정의에 의해 자명하다.

### 명제3(Chain Rule)
$\R^n$의 open set $U$와 $\R^m$의 open set $V$가 있다고 하자.

$f:U \rightarrow V$과 $g:V \rightarrow\R^p$가 있을 때, $a \in U$에서 $f$가 differentiable하고 $f(a) \in V$에서 $g$가 differentiable하다고 하자.

이 때, 다음을 증명하여라.

$$ \begin{gathered} g \circ f \text{ is diffrentiable at } a \\ D^{(g \circ f)(a)} = D^{g(f(a))} \circ D^{f(a)} \end{gathered} $$

**Proof**

함수 $r : \R^n \rightarrow \R^m$과 함수 $s : \R^m\rightarrow\R^p$를 다음과 같이 정의하자.

$$ \begin{gathered} r(h) = f(a+h) - f(a) - D^{f(a)}(h) \\ s(h) = g(f(a)+h) - g(f(a)) - D^{g(f(a))}(h) \end{gathered} $$

따라서, $r,s$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} g(f(a+h)) &= g(f(a) + r(h) + D^{f(a)}(h)) \\&= g(f(a) + \Delta f) \\&= g(f(a)) + D^{g(f(a))}(\Delta f) + s(\Delta f)  \end{aligned} $$

Total derivative의 정의에 의해 $D^{g(f(a))}$는 linear map임으로 다음이 성립한다.

$$ \begin{aligned} D^{g(f(a))}(\Delta f) &= D^{g(f(a))}(r(h) + D^{f(a)}(h))) \\&= D^{g(f(a))}(r(h)) + D^{g(f(a))}(D^{f(a)}(h)) \end{aligned} $$

따라서, 다음이 성립한다.

$$ g(f(a+h)) - g(f(a)) - D^{g(f(a))}(D^{f(a)}(h)) = D^{g(f(a))}(r(h)) + s(\Delta f) $$

이 때, 보조명제3.1에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}} \left( g(f(a+h)) - g(f(a)) - D^{g(f(a))}(D^{f(a)}(h)) \right) = 0_n $$

$D^{g(f(a))}$와 $D^{f(a)}$ 모두 linear map임으로 다음이 성립한다.

$$ D^{g(f(a))} \circ D^{f(a)} \text{ is an linear map} $$

따라서, total derivative의 정의에 의해 다음이 성립한다.

$$ \begin{gathered} f \circ g \text{ is diffrentiable at } a \\ D^{(g \circ f)(a)} = D^{g(f(a))} \circ D^{f(a)} \end{gathered} \qed $$

#### 보조명제3.1
다음을 증명하여라.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}} \left( g(f(a+h)) - g(f(a)) - D^{g(f(a))}(D^{f(a)}(h)) \right) = 0_n $$

**Proof**

$g(f(a+h)) - g(f(a)) - D^{g(f(a))}(D^{f(a)}(h)) = D^{g(f(a))}(r(h)) + s(\Delta f)$임으로 다음을 증명하자.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}} \left( D^{g(f(a))}(r(h)) + s(\Delta f) \right) = 0_n $$

[첫번째 항]  
다음이 성립한다. 

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}}\norm{D^{g(f(a))}(r(h))} \le \norm{D^{g(f(a))}}\lim_{h\rightarrow 0_n} \frac{1}{\norm{h}}\norm{r(h)} $$

전제에 의해 $f$는 $a$에서 differentiable함으로 다음이 성립한다.

$$ \lim_{h\rightarrow 0_n} \frac{r(h)}{\norm{h}} = 0_n \iff \lim_{h\rightarrow 0_n} \frac{\norm{r(h)}}{\norm{h}} = 0 $$

따라서, 위의 결과를 종합하면 다음이 성립한다.

$$ \lim_{h\rightarrow 0_n} \frac{1}{\norm{h}}D^{g(f(a))}(r(h)) = 0_n \qed $$

[두번째 항]  
$\Delta f$를 풀어 쓰면 다음과 같다.

$$ s(\Delta f) = s(r(h) + D^{f(a)}(h))) $$

전제에 의해 $\lim_{h\rightarrow 0_n} \frac{r(h)}{\norm{h}} = 0_n$임으로 보조명제3.1.1에 의해 다음이 성립한다.

$$ \exist \delta_1 \in \R^+ \st \norm{h} < \delta_1 \implies \norm{r(h)} < \norm{h} $$

따라서, $\norm{h}<\delta_1$일 떄, 다음이 성립한다.

$$ \begin{aligned} \norm{r(h) + D^{f(a)}(h)} &\le \norm{r(h)} + \norm{D^{f(a)}(h)} \\&\le \norm{r(h)} + \norm{D^{f(a)}}\norm{h} \\&< \left(\norm{D^{f(a)}}+1\right)\norm{h} \end{aligned} $$

이 떄, 전제에 의해 $\lim_{h\rightarrow 0_n} \frac{s(h)}{\norm{h}} = 0_n$임으로 metric space의 limt의 정의에 의해 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \delta \in \R^+ \st \norm{h} < \delta \implies \norm{s(h)}<\epsilon\norm{h}$$

그럼으로 다음도 성립한다.

$$ \forall \epsilon \in \R^+, \quad \delta_\epsilon \in (0, \delta_1) \st \norm{h} < \delta \implies \norm{s(h)}<\epsilon\norm{h}$$

따라서, $\norm{h}<\delta_\epsilon$일 때, $\delta_\epsilon$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} s(r(h) + D^{f(a)}(h)) &\le \epsilon\norm{D^{f(a)}+r(h)} \\&< \epsilon\left(\norm{D^{f(a)}}+1\right)\norm{h} \\ \frac{s(r(h) + D^{f(a)}(h))}{\norm{h}} &< \epsilon\left(\norm{D^{f(a)}}+1\right) \end{aligned} $$

$\epsilon'$을 다음과 같이 정의하자.

$$ \epsilon' := \epsilon\left(\norm{D^{f(a)}}+1\right) $$

그러면 $\epsilon'$의 정의에 의해 다음이 성립한다.

$$ \forall \epsilon' \in \R^+, \quad \delta_\epsilon \st \norm{h} < \delta_\epsilon \implies \frac{s(r(h) + D^{f(a)}(h))}{\norm{h}}<\epsilon' $$

따라서, Metric space에서 limt의 정의에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0_n} \frac{s(r(h) + D^{f(a)}(h))}{\norm{h}} =0_n \qed $$


##### 보조명제3.1.1
$\lim_{h\rightarrow0_n} \frac{f(h)}{\norm{(h)}} = 0$일 때, 다음을 증명하여라.

$$ \forall\epsilon \in \R^+, \quad \exist \delta \st \norm{h} < \delta \implies \norm{f(h)} < \epsilon\norm{h} $$

**Proof**

Metric space에서 limt의 정의에 의해 $\lim_{h\rightarrow0_n} \frac{f(h)}{\norm{(h)}} = 0$면 다음이 성립한다.

$$ \forall\epsilon \in \R^+, \quad \exist \delta \st \norm{h} < \delta \implies \frac{\norm{f(h)}}{\norm{h}} < \epsilon $$

따라서, 다음이 성립한다.

$$ \forall\epsilon \in \R^+, \quad \exist \delta \st \norm{h} < \delta \implies \norm{f(h)} < \epsilon\norm{h} \qed $$

> Reference  
> {cite}`hubbard` Appendix4  
> [illinois note](https://faculty.math.illinois.edu/~carty/ChainRuleNotesSlides.pdf)

### 참고2
Total derivative의 의미는 $a\in\R^n$에서 함수 $f: \R^n \rightarrow \R^m$에 대한 best linear approximation이다.

따라서 위 정의는 $f$의 domain과 codomain이 Euclidean vector space일때 만 성립하는 것이 아니라, 임의의 vector space에 대해서도 성립한다.

$f$의 total erivative를 계산하는데 있어서, 정의를 사용하는것보다 $J_f$를 사용하는 것이 더 간단한 경우가 많다. 

하지만 domain과 codomain이 Euclidean vector space가 아닌 vector space인 경우에는 vector space isomorphism을 이용하여 Euclidean vector space에서 Jacobian matrix를 구하는 형태로 계산을 해야 되는데, 이 방법이 최선의 선택이 아닌 경우도 있다.

#### 예시
함수 $f$가 다음과 같이 정의되었다고 하자.

$$f : M_{nn} \rightarrow M_{nn} \st A \mapsto A^2 $$

##### $J_f$를 사용하는 경우
$M_{nn}$과 $\R^{nn}$은 다음 관계에 의해 vector space isomorphic이다.

$$ \begin{CD} M_{nn} @>f>> M_{nn} \\ @V{\varphi}VV @VV{\varphi}V \\ \mathbb \R^{nn} @>>g> \mathbb \R^{nn}  \end{CD} $$

$f,g$의 total derivative를 각 각 $L_f,L_g$라하고 $M_{nn}$과 $\R^{nn}$의 basis를 각 각 $\beta, \gamma$라 할 때, $\varphi$의 성질에 의해 다음이 성립한다.

$$ \mathfrak{m_\beta^\beta}(L_f) = \mathfrak{m_\gamma^\gamma}(L_g) $$

$f$의 정의에 의해 $a = a^{ij}\gamma_{ij} \in \R^{nn}$가 있을 떄, $g(v)$는 다음과 같다.

$$ g(v) = a^{ik}a^{kj}\gamma_{ij} $$

따라서, 다음이 성립한다.

$$ \mathfrak{m_\gamma^\gamma}(L_g) = J_g(a) = \begin{bmatrix} D_{11}g^{11}( a) & \cdots & D_{nn}g^{11}(a) \\ \vdots & & \vdots \\ D_{11}g^{nn}( a) & \cdots & D_{nn}g^{nn}( a) \end{bmatrix} $$


$$ \text{Where, }D_{ij}g^{kl}(a) = \frac{\partial a^{kr}a^{rl}}{\partial a^{ij}} $$

그럼으로 $\mathfrak{m_\beta^\beta}(L_f)$를 알고 있기 때문에 $L_f$에 대해 알 수 있다.

예를 들어 $m \in M_{nn}$에 대해서, $L_f(m)$은 다음과 같다.

$$ \begin{aligned} L_f(m) &= \varphi^{-1}(\mathfrak{m_\beta}(L_f(m))) \\&= \varphi^{-1}(\mathfrak{m_\beta^\beta}(L_f)\mathfrak{m_\beta}(m)) \end{aligned} $$

##### 정의를 사용하는 경우
Total derivative의 정의를 통해 $L_f$을 구해보자.

$$ \begin{aligned} & \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}(f(A + H) - f(A) - \mathit L_f(H)) = 0_{M_{nn}} \\ \implies & \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}(AH + HA + H^2 - \mathit L_f(H)) = 0_{M_{nn}} \end{aligned} $$

$f$가 $A$에서 미분가능하기 위해서는 위의 식이 만족되어야 함으로 $\norm{H}$로 나눠주면 $0_{M_{nn}}$이 되지 않는 선형 항들을 제거하기 위해 linear map $L_f$를 다음과 같이 정의하자.

$$ L_f : M_{nn} \rightarrow M_{nn} \st H \mapsto AH + HA $$

위와 같이 정의한 $L_f$에 의해 다음이 만족된다.

$$ \begin{aligned} \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}(AH + HA + H^2 - \mathit L(H)) &= \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}H^2 \\&\le \lim_{H \rightarrow 0_{M_{nn}}} \frac{1}{\norm{H}}\norm{H}\norm{H} \\ &= 0_{M_{nn}} \end{aligned} $$

따라서, $f$의 total derivative는 $L_f(A)$이다.

##### 결론
두 경우를 비교해보면 알 수 있듯이, 정의를 사용하는 경우가 훨씬 간단할수도 있다.

> Reference  
> {cite}`hubbard` p.132