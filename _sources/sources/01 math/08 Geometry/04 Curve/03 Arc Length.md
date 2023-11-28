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