# Numerical Flux
Model problem으로 $n$차원 linear advection 문제를 살펴보자.

advection speed $a \in \R^n$이 주어졌을 떄, linear advection은 다음과 같은 conservatin form의 PDE다.

$$ \text{find } q(x,t):\Omega \times \R^+ \rightarrow\R \st \diff{}{t} \int_{\Omega} q \thinspace dV + \int_{\partial\Omega} (aq) \cdot dS = 0 $$

FVM discretization에 의해 다음과 같이 간단하게 표현할 수 있다.

$$ \text{find } \bar{q}(t) :\R^+\rightarrow\R \st \diff{\bar{q}}{t} = - \frac{1}{\Omega}\sum_{j=1}^{n_f} f_{numerical}(\bar{q}_{owner},\bar{q}_{neighbor},n)\norm{\partial\Omega_j} $$

## Upwind Flux
$f_{upwind}$는 다음과 같다.

$$ f_{upwind} = \frac{1}{2}(a\cdot n + |a\cdot n|)\bar{q}_{owner} + \frac{1}{2}(a\cdot n - |a\cdot n|)\bar{q}_{neighbor} $$

### 참고1
linear advection같은 경우에는 정보의 전달이 advection speed 따라서 전파된다.

따라서 $f_{upwind}$처럼 특정 방향의 정보만 사용을 하더라도 물리적인 정보의 전달 방향 즉, advection 방향의 정보만 전달이 된다면 문제가 없다.

### 참고2
advection 방향과 $n$이 같은 방향인 경우($0 < a\cdot n$) $\bar{q}_{owner}$에 대한 정보가 advection되고 advection 방향과 $n$이 다른 방향인 경우($a\cdot n < 0$) $\bar{q}_{neighbor}$에 대한 정보가 advection된다.