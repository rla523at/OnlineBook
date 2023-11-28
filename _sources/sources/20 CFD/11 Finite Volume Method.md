# Finite Volume Method
## FVM Discretization

$\Omega$가 일정하다고 하자.

그러면 다음이 성립한다.

$$ \begin{aligned} & \diff{}{t} \int_{\Omega} q \thinspace dV + \int_{\partial\Omega} f \cdot dS = 0 \\\implies& \diff{}{t} \frac{1}{\Omega} \int_{\Omega} q \thinspace dV + \frac{1}{\Omega}\int_{\partial\Omega} f \cdot dS = 0 \\\implies& \diff{\bar{q}}{t} = - \frac{1}{\Omega}\int_{\partial\Omega} f \cdot dS  \end{aligned} $$

이 때, $\partial\Omega$가 $n_f$개의 face $\partial\Omega_j, j=1,\cdots,n_f$로 이루어져 있고 각 face마다 고유의 normal vector $n$이 있고 $n$을 기준으로 face에서 $n$방향에 있는 cell을 neigbor cell, 그 반대 방향에 있는 cell을 owner cell이라고 할 때, 어떤 함수 $f_{numerical}$가 존재해서 다음이 성립한다고 하자.

$$ \int_{\partial\Omega} f \cdot dS = \sum_{j=1}^{n_f} f_{numerical}(\bar{q}_{owner},\bar{q}_{neighbor},n)\norm{\partial\Omega_j} $$

그러면 conservation form을 다음과 같이 표현할 수 있다.

$$ \diff{\bar{q}}{t} = - \frac{1}{\Omega}\sum_{j=1}^{n_f} f_{numerical}(\bar{q}_{owner},\bar{q}_{neighbor},n)\norm{\partial\Omega_j} $$

### 참고1
$f_{numerical}$이 $\bar{q}_{owner},\bar{q} _{neighbor},n$을 인자로 사용하기 때문에 surface를 따라 상수 값을 갖는다. 

즉 $f_{numerical}$도 $f$의 face에서의 평균값을 의미한다.

정리하면, $\bar{q}$는 $q$의 cell 에서의 평균값이며 $f_{numerical}$은 $f$의 face에서의 평균값이다.

### 참고2
FVM은 공간에 대해 평균을 냄으로써 복잡한 $q(x,t)$를 구해야 되는 문제가 아니라 공간에 대한 정보가 사라진 $\bar{q}(t)$를 구하는 문제로 간단해 진다.


### 참고2
solution distribution이 cell average된 경우 $\partial\Omega$에서 discontinuity가 나타난다.