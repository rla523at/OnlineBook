# Curvature
## Motivation
$\R$의 open set $U$와 $C^2$ function $r : U \rightarrow \R^n$이 있다고 하자.

$r$에 의해 주어진 parametric curve를 $C$라고 할 떄,  $C$의 휘어진 정도인 곡률(curvature) $\kappa$를 어떻게 정의할지 생각해보자.

### Remark
1. $r$이 chart라면, $C = \img(r)$임으로 $A=\Set{r}$은 $C$의 differentiable atlas이다.
2. $r$이 continuous bijective function이더라도 $r$은 homeomorphism이 아닐 수 있다.
   1. $f:[0,2\pi) \rightarrow S^1 \subseteq \R^2 \st t \mapsto (\cos t, \sin t)$은 continuous bijective function이지만, $f^{-1}$는 $(1,0)$ 근방에서 $2\pi$와 $0$이라는 불연속한 값을 갖음으로 continuous function이 아니고 $f$는 hoemomorphism이 아니다.
   2. $g(x) = \begin{cases} x & 0\le x \le 1 \\ x-1 & 2 < x\le 3 \end{cases}$는 continuous bijective function이고 $g^{-1}(y) = \begin{cases} y & 0\le y \le 1 \\ y+1 & 1 < y \le 2 \end{cases}$이다. 이 때, $y=1$에서 $g^{-1}(y)$의 값이 불연속한 값을 갖음으로 continuous function이 아니고 $g$는 homeomorphism이 아니다.
3. $C$가 꼬여 있으면 $r$이 bijective function이 될 수 없다. 그러나 $C$가 유한하게 꼬여있으면 정의역과 공역을 적절히 나우어서 bijective function들을 만들 수 있다.

```{figure} _image/0301.jpg
```

> Reference
> [math.stackexchange](https://math.stackexchange.com/questions/68800/functions-which-are-continuous-but-not-bicontinuous)  
   
## TRY1
```{figure} _image/0401.png
```

위 그림에서 보듯이, 곡선이 얼마나 휘어있는지는 곡선의 접선이 얼마나 빠르게 변하는지와 관련이 있어 보인다.


이 때, $r'(u)$가 $C$의 tangent vector임으로 $r'(u)$이 얼마나 빠르게 변하는지 즉, $r'(u)$의 변화량의 크기로 $\kappa$를 정의해보자.

$$ \kappa = \norm{r''(u)} $$

### Problem
하지만 위의 정의는 parametrization에 의존하는 문제를 가지고 있다. 다음 예시를 통해, parametrization에 의존한다는 의미가 무엇인지, 그리고 parametrizatipn에 의존하는것이 왜 문제점인지를 직관적으로 이해해보자. 

$\R^3$의 curve $C$가 서로 다른 두 parameter $u,v$로 다음과 같이 표현된다고 하자.

$$ r_1(u) = (u,u^2,2u), \enspace u\in(0,2), \quad r_2(v) = (2v,4v^2,4v), \enspace v\in(0,1) $$

$u=1$과 $v=0.5$일 때, $C$의 동일한 점을 나타내지만, TRY1에서 정의한 $\kappa$의 정의를 사용하면 $\kappa_1(1) = 2$이고 $\kappa_2(0.5) = 8$로 서로 다른 곡률을 가지게 됨을 알 수 있다.

이와 같이, parametrization에 따라서 값이 바뀐는 경우 그 값이 parametrization에 의존한다고 표현한다. 그리고 $\kappa$ 가 parametrization에 의존하는 것이 왜 문제이냐면 $r_1$과 $r_2$는 표현법만 다를 뿐 둘다 동일한 곡선 $C$를 나타내는데, 동일한 곡선의 $\kappa$가 parametrization에 따라 값이 다르게 나오는 것은 매우 이상하다.

다시 말해, parametrization이 달라지더라도 곡선 자체가 변하는 것은 아니기 때문에 parametrization에 의존하는 값들은 곡선의 성질을 나타내는 값이 아니기 때문에 곡선의 성질을 나타내는 $\kappa$를 parametrization에 의존하게 정의한 첫번째 시도는 옳바른 정의가 될 수 없다.

그러면 이번에는 조금더 추상화하여, parametrization에 의해서 얼마나 달라지는지 확인을 해보자.

임의의 curve $C$가 $C^2$ fucntion $r_1(u)$로 표현된다고 하자. 

$C^2$인 bijective function $f$가 있어서 $v = f(u)$일 때, $f$에 의해서 $v$로 reparametrization된 function을 $r_2$라고 하자.

$$ r_2 := r_1 \circ f^{-1} $$

$r_1 = r_2 \circ f$임으로 chain rule에 의해 다음이 성립한다.

$$ \diff{r_1}{u} = \diff{r_2}{v}\diff{f}{u} $$

다시 한번 chain rule에 의해 다음이 성립한다.

$$ \frac{d^2r_1}{du^2} = \frac{d^2r_2}{dv^2} \left(\diff{f}{u}\right)^2 + \diff{r_2}{v}\frac{d^2f}{du^2} $$

즉, $r'$과 $r''$은 서로 다른 parametrization에 사이에 위와 같은 차이가 존재하게 된다.

### Proposition

$C^2$인 bijective function $f$가 있어서 $v = f(u)$일 때, $f$에 의해서 $v$로 reparametrization된 function을 $r_2$라고 하자.

$$ r_2 := r_1 \circ f^{-1} $$

이 때, 다음을 증명하여라.

$$ r_1'(u) = r_2'(v)f', \quad r_1''(u) = r_2''(v)(f')^2 + r_2'f'' $$


## TRY2
tangent vector의 변화율로 $\kappa$를 나타내는 것은 매우 자연스러운 선택임으로 tangent vector를 계속 활용하되 
이번에는 unit tanget vector $T(u)$를 활용해서 $\kappa$를 정의해보자.

$$ \kappa := \norm{T'(u)} $$

이 정의가 parametrization에 의존하는 정의인지 확인해보자.

임의의 curve $C$가 $C^2$ fucntion $r_1(u)$로 표현된다고 하자. 

$C^2$인 bijective function $f$가 있어서 $v = f(u)$일 때, $f$에 의해서 $v$로 reparametrization된 function을 $r_2$라고 하자.

$$ r_2 := r_1 \circ f^{-1} $$

그러면 $r'_1 = r'_2f'$이고 $\norm{r'_1} = \norm{r'_2}|f'|$임으로 다음이 성립한다.

$$ T_1(u) = T_2(v) \text{ sgn}(f') $$

그리고 chain rule에 의해 다음이 성립한다.

$$ T_1'(u) = T_2'(v) f'\text{ sgn}(f') = T_2'(v)|f'| $$

따라서, 다음이 성립한다.

$$ \kappa_1(u) = \norm{T_1'(u)} = \norm{T_2'(v)}|f'| =\kappa_2(v)|f'| $$

그럼으로, unit tangent vector를 활용한 정의도 서로 다른 parametrization 사이에 $|f'|$에 해당하는 차이가 존재함으로 parametrization에 의존하는 값이 되고, TRY1과 동일한 이유로 $\kappa$의 옳바른 정의가 될 수 없다.

## TRY3
임의의 curve $C$가 $C^2$ fucntion $r_1(u)$로 표현된다고 하자. 

$C^2$인 bijective function $f$가 있어서 $v = f(u)$일 때, $f$에 의해서 $v$로 reparametrization된 function을 $r_2$라고 하자.

$$ r_2 := r_1 \circ f^{-1} $$

이 때, $\norm{r'_1} = \norm{r'_2}|f'|$이고 $\norm{T_1'(u)} = \norm{T_2'(v)}|f'|$임을 알았다.

따라서, $\kappa$를 다음과 같이 정의하면 parametrization에 의존하지 않으면서도, tangent vector의 변화량을 곡선의 곡률의 정의로써 활용하겠다는 직관을 잘 표현할 수 있게 된다.

$$ \kappa := \frac{\norm{T'}}{\norm{r'}} $$

## Definition(curvature)
$\R$의 open set $U$와 $C^2$ function $r : U \rightarrow \R^n$이 있다고 하자.

$r$에 의해 주어진 parametric curve를 $C$라고 할 떄,  $C$의 curvature $\kappa$를 다음과 같이 정의한다.

$$ \kappa := \frac{\norm{T'}}{\norm{r'}} $$

### Example

### Arc length parametrization

> Reference  
> https://math.stackexchange.com/questions/199417/how-and-why-would-i-reparameterize-a-curve-in-terms-of-arclength

# Arc Length
## 정의
$\R$의 open set $U$와 $U$에서 differentiable한 $r : U \rightarrow \R^n$이 있다고 하자.

$r$로 표현되는 curve를 $C$라고 했을 때, $C$의 `arc length function` $s$를 다음과 같이 정의한다.

$$ s(t) := \int^t_a \norm{r'(x)}\thinspace dx $$

이 때, $a= \inf(U)$이다.

### 참고1
$r$이 $C$를 여러번 지나갈 경우 arc length function은 $C$의 길이를 나타내지 않는다.

예를 들어 $U = (0,2\pi)$이고 $r$이 다음과 같이 주어졌다고 하자.

$$ r(t) =  (3\cos(4t),3\sin(4t)) $$

그러면 $s(\pi)$는 다음과 같다.

$$ s(\pi) = \int^\pi_0 12 \thinspace dx = 12\pi $$

하지만 실제로 $C$가 나타내는 curve는 반지름이 3인 원이고 둘레는 6$\pi$가 된다. 하지만 주어진 $U$에서 $r$이 $C$를 2번 지나감으로 arc length function이 2배의 값이 나오게 된다.

이처럼 $C$를 $r$이 여러번 지나가는 경우 arc length function의 값이 arc length가 아닐 수 있다.

### 명제1
$\R$의 open set $U$와 $U$에서 differentiable한 $r : U \rightarrow \R^n$이 있다고 하자.

$r$의 arc length function을 $s$라고 할 때, 다음을 증명하여라.

$$ \diff{s}{t} = \norm{r'(t)} $$

**Proof**

$s$의 정의와 fundamental theorem of calculus에 의해 자명하다. $\qed$

### 명제2
$\R$의 open set $U$와 $U$에서 differentiable한 $r : U \rightarrow \R^n$이 있다고 하자.

$r$의 arc length function을 $s$라고 할 때, 다음을 증명하여라.

$$ s \text{ is bijective} $$

**Proof**

$s$는 정의에 의해 monotonic increase 함수임으로 자명하다. $\qed$

### 명제3
다음을 증명하여라.

$$ \text{Arc length is parametric invariant} $$

**Proof**

$\R$의 open set $U,V$와 $r_1:U\rightarrow\R^n \in C^1$과 $r_2:V\rightarrow\R^n \in C^1$이 있을 때, Curve $C$가 $r_1,r_2$로 표현된다고 하자.

$C$가 $r_1,r_2$로 표현됨으로 다음이 성립한다.

$$ \begin{gathered} \exist \text{bijective } f:U\rightarrow V \st  \forall x \in U, \quad  r_1(x) = r_2(f(x)) \end{gathered} $$

따라서, chain rule에 의해 다음이 성립한다.

$$ \begin{aligned} s(t) &= \int_{U} \norm{r_1'(x)}\thinspace dx \\&= \int_{U} \norm{r_2'(f(x))\diff{f}{x}}\thinspace dx \\&= \int_{U} \norm{r_2'(f(x))}\norm{\diff{f}{x}}\thinspace dx \end{aligned} $$

이 때, $0 \le df/dx$이라 가정하고 $u = f(x)$로 두면 다음이 성립한다.

$$ \begin{aligned} \int_{U} \norm{r_2'(f(x))}\norm{\diff{f}{x}}\thinspace dx &= \int_{f(U)} \norm{r_2'(u)}\thinspace du \end{aligned} $$

즉, 다음이 성립한다.

$$ \int_{U} \norm{r_1'(x)}\thinspace dx = \int_{V} \norm{r_2'(u)}\thinspace du $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/137410/arc-length-under-change-of-parameter)