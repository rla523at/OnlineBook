# Line Integral
선적분(line integral)은 벡터장 또는 스칼라장을 어떤 **곡선을 따라 적분**하는 것이다.

1. **스칼라장에 대한 선적분**: 길이를 따라 스칼라 값을 더함
   예: 온도, 밀도, 에너지 등을 경로를 따라 누적

   $$
   \int_C f(x, y)\, ds
   $$

2. **벡터장에 대한 선적분**

## 벡터장에 대한 선적분
$$
\int_C \vec{F} \cdot d\vec{r} = \int_C P\,dx + Q\,dy
$$

여기서:
* $\vec{F} = (P, Q)$: 벡터장
* $d\vec{r} = (dx, dy)$: $C$ 의 접선 벡터 
   * 접선벡터? 경로를 따라 나아가는 방향의 미분

### 직관
* 선적분은 **힘 × 거리** 즉, 일(work)의 개념과 같다.
* 어떤 경로를 따라 벡터장이 작용할 때, 그 경로를 따라 벡터가 얼마나 '같은 방향으로 작용하느냐'를 측정한다

### 예제
다음 벡터장에 대해, 곡선 $C$: 단위 원 $x^2 + y^2 = 1$ (반시계 방향)을 따라 선적분을 구하라

$$
\vec{F}(x, y) = (-y, x)
$$

**SOLVE**
$$
\oint_C -y\,dx + x\,dy
$$

1. **경로 파라미터화**: 단위 원이므로

   $$
   x = \cos t,\quad y = \sin t,\quad t \in [0, 2\pi]
   $$

   $$
   dx = -\sin t\,dt,\quad dy = \cos t\,dt
   $$

2. **함수 대입:**

   $$
   -y\,dx + x\,dy = -\sin t \cdot (-\sin t) + \cos t \cdot \cos t = \sin^2 t + \cos^2 t = 1
   $$

3. **적분:**

   $$
   \int_0^{2\pi} 1\,dt = 2\pi
   $$

### $d\vec{r}$
경로 $C$는 일변수 함수로 파라미터화(parametrization)할 수 있다:

$$
\vec{r}(t) = (x(t), y(t)),\quad t \in [a, b]
$$

이 때, $d\vec{r}$ 는 다음과 같다.

$$
d\vec{r} = (dx, dy) = \left( \frac{dx}{dt}, \frac{dy}{dt} \right) dt
$$

* 이것은 **곡선을 따라 움직일 때의 순간적인 변화 벡터**다.
* 벡터장에 대해 **‘어떤 방향으로 움직이는가’를 나타내는 벡터**입니다.