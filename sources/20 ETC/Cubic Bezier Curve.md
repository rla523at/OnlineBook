# Cubic Bezier Curve Length

## Preliminary
### Cubic Bezier Curve
4 개의 점 $P_0,C_0,C_1,P_1 \in \R^3$ 가 주어졌을 떄, cubic bezier curve $B(t) : [0,1] \rightarrow \R^3$ 는 다음 수식으로 정의되는 parametric curve이다.

$$ \begin{aligned}
B &= (1-t)^3P_0 + 3(1-t)^2tC_0 + 3(1-t)t^2C_1 + t^3P_1 \\
B_t &= 3(-(1-t)^2P_0 + (1-3t)(1-t)C_0 + t(2-3t)C_1 + t^2P_1) \\ 
B_{tt} &= 6((1-t)P_0 + (2-3t)C_0 + (1-3t)C_1 + tP_1) \\
B_{ttt} &= 6(-P_0 + 3C_0 - 3C_1 + P_1)
\end{aligned} $$

아래 첨자가 하나씩 늘어날때마다 $t$ 로 한번 미분한 값이다.

> Reference   
> [wikipedia - Bézier curve](https://en.wikipedia.org/wiki/B%C3%A9zier_curve)  

### Arc Length of Parametric Curve
Parametric curve 를 $B(t)$ 라고 할 때, $t \in [t_0,t_1]$ 의 arc length 는 다음과 같다.

$$ \int^{t_1}_{t_0} \sqrt{ {B_t \cdot B_t}} dt $$

> Refernece  
> [wikipedia - Arc_length](https://en.wikipedia.org/wiki/Arc_length)  

### Change of Variable in Integral
$t = T(x)$ 이고 $t_0 = T(a), t_1 = T(b)$일 떄, 다음이 성립한다.

$$ \int^{t_1}_{t_0} f(t) dt = \int^{b}_{a} f(T(x)) \diff{T}{x} dx $$

> Reference  
> [wikipedia - Integration_by_substitution](https://en.wikipedia.org/wiki/Integration_by_substitution)  

### Newton-Raphson Method
$g(t) = 0$ 을 만족하는 $t$ 값을 수치적으로 찾는 방법이다.

초기 값 $t_0$ 가 주어졌을 때, 다음  iteration 을 반복해서 값을 구한다.

$$ t_i = t_{i-1} - \frac{g(t_{i-1})}{g_t(t_{i-1})} $$

> Reference  
> [wikipedia - Newton's method](https://en.wikipedia.org/wiki/Newton%27s_method)

### Gaussian Quadrature Method
$n$ 개의 point 를 이용하여 $[-1,1]$ 범위의 적분을 근사하는 방법이다.

$$ \int^{1}_{-1}f(x)dx \approx \sum_{i=1}^n f(x_i)w_i $$

> Reference   
> [wikipedia - Gaussian_quadrature](https://en.wikipedia.org/wiki/Gaussian_quadrature)  
> [pomax.github - Gaussian Quadrature Table](https://pomax.github.io/bezierinfo/legendre-gauss.html)

## 알고리즘 소개
임의의 $[t_0,t_1]$ 구간에서 Cubic Bezier Curve 의 Arc Length 를 구하기 위해 다음 두가지 단계를 거친다.

1. Heuristic 한 방법으로 2개의 Split Point 를 구해 $[t_0,t_1]$ 을 수치적분 하기 좋은 구간으로 나눈다.
2. 각 구간별로 Arc Length 를 수치적분해서 구한다. 

### Calculate Split Point
Gaussain Quadrature Method 는 Integrand 가 다항함수로 근사가 잘 될 수록 정확하게 적분 값을 구할 수 있다.

따라서, 적분하고자 하는 $l(t)$ 에 아래와 같은 특이점이 있는 경우에는 특이점을 기준으로 적분 구간을 나누어서 계산해야 오차를 줄일 수 있다.

```{figure} _image/0001.png
```

다양한 $l(t)$ 에 대해 관측한 결과 특이점과 관련하여 다음과 같은 규칙을 따르는것을 확인하였다.
1. $\diff{(B_t \cdot B_t)}{t} = 0$ 을 만족하는 $t$ 에서 특이점이 발생한다.
2. $B_t \cdot B_t$ 는 4차 다항식임으로 위를 만족하는 점은 3개가 나오며, 그 중 첫번째 점과 세번째 점에서 특이점이 나온다.
3. 특이점이 나타나더라도 Integrand 의 함수값이 충분히 크지 않으면 수치적분 오차가 크지 않다.

위의 규칙을 왜 따르는지에 대한 논리적인 이유는 찾지 못하였지만, $l(t)$ 가 4차 다항식 $B_t \cdot B_t$ 에 루트를 씌운 형태이기 때문에 위와 같은 규칙을 따르는것으로 추측된다.

1번에 의해 Split Point 를 찾는 문제는 Newton Raphson Method 를 사용하여 해결할 수 있다. 

수치적분 오차를 가능한 줄일 수 있는 split point 를 찾는게 목적임으로 따라서, $\diff{(B_t \cdot B_t)}{t} = 0$ 의 해를 $t=0.0$ 을 시작점으로 하여 Newton Raphson Method 로 찾은 값을 첫번째 split point $t_{sp1}$ 로 두고 $t=1.0$ 을 시작점으로 하여 찾은 값을 두번째 split point $t_{sp2}$ 로 둔다. 단, split point 가 t0 보다 작거나 t1 보다 큰 경우 start point 로 둔다. $t_{sp1}$ 과 $t_{sp2}$에 따라 $[t_0, t_{sp1}]$, $[t_{sp1}, t_{sp2}]$, $[t_{sp2}, t_1]$ 또는 $[t_0, t_{sp2}]$, $[t_{sp2}, t_{sp1}]$, $[t_{sp1}, t_1]$ 으로 적분 구간을 나눈다.

아래의 규칙은 다시 한번 생각해 봐야 한다.
* 2번에 의해 $t=0.0$ 을 시작점으로 하면 첫번째 점으로 부터 나오는 특이점을 찾을 수 있다. 단, $t=0.0$ 에서 기울기가 음수가 아닌 경우 첫번째 점으로 부터 나오는 특이점이 주어진 구간에 없음을 의미한다. 마찬가지로 $t=1.0$ 을 시작점으로 하면 세번째 점으로 부터 나온은 특이점을 찾을 수 있으며, 단, $t=1.0$ 에서 기울기가 양수가 아닌 경우 세번째 점으로 부터 나오는 특이점이 주어진 구간에 없음을 의미한다. 

$g = \diff{(B_t \cdot B_t)}{t}$ 라고 하면 다음이 성립한다.

$$ \begin{aligned}
g &= 2B_{tt}\cdot B_t \\ 
g_t &= 2(B_{ttt}\cdot B_t + B_{tt} \cdot B_{tt} )
\end{aligned}  $$

따라서, $i$ 번 째 iteration 으로 구한 $g=0$ 을 만족하는 $t_i$ 는 다음과 같다.

$$ t_i = t_{i-1} - \frac{g(t_{i-1})}{g_t(t_{i-1})} $$

이 때, iteration 수를 제한하기 위해 최대 interation 수를 정해놨으며, $t_i - t_{i-1}$ 의 차이가 작은 경우에는 iteration 을 탈출 할 수 있게 하였다.

3번 조건에 의해 Integrand 의 함수값이 충분히 크지 않은 경우 Split Point 를 계산하지 않는다.

이 떄, 충분히 크지 않은 값은 경험적으로 결정하였다.

### Calculate Arc Length By Numerical Integration
cubic bezier curve 의 arc length 를 구하기 위해 다음 식을 수치적분한다.

$$ \int^{t_1}_{t_0} \sqrt{ {B_t \cdot B_t}} dt $$

이 때, 수치적분은 $[-1,1]$ 범위의 적분을 근사함으로 change of variable 을 통해 적분 범위를 맞춘다.

change of variable 함수는 다음을 사용한다.

$$ t = T(x) = \frac{t_1-t_0}{2}(x+1) + t_0 $$ 

$l(t) = \sqrt{ {B_t \cdot B_t}}$ 라고 하면 change of variable 에 의해 적분식은 다음과 같이 바뀐다.

$$ \frac{t_1-t_0}{2} \int^{1}_{-1} l(T(x)) dx $$

이제 Gaussian Quadarature Method 를 이용해 수치적분 하면 다음과 같이 근사할 수 있다.

$$ \frac{t_1-t_0}{2} \int^{1}_{-1} l(T(x)) dx \approx \frac{t_1-t_0}{2} \sum_{i=1}^n l(T(x_i))w_i $$

