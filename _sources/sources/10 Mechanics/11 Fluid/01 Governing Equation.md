# Governing Equation
## Conservation of Mass
질량이 다음과 같이 밀도의 함수로 주어진다고 하자.

$$ m = \int_{\Omega(t)} \rho(x,t) dV $$

이 때, 질량 보존을 방정식으로 나타내면 다음과 같다.

$$ \frac{D}{Dt} \int_{\Omega(t)} \rho(x,t) dV = 0 $$

Reynold's tranport theorem에 의해 다음과 같이 표현할 수 있다.

$$ \int_{\Omega(t)} \left( \Diff{\rho}{t} + \rho (\nabla \cdot v) \right) \thinspace dV = 0 $$

$$ \int_{\Omega(t)} \pdiff{\rho}{t} \thinspace dV + \int_{\partial\Omega(t)} \rho(v \cdot n) \thinspace dS = 0 $$

### Differential Form
임의의 $\Omega(t)$에서 위의 적분식이 항상 성립해야 됨으로, 다음이 성립한다.

$$ \Diff{\rho}{t} + \rho (\nabla \cdot v) = 0 $$

만약, $\rho v$가 미분가능하다면, 다음과 같이 표현할 수 있다.

$$ \pdiff{\rho}{t} + \nabla \cdot (\rho v) = 0 $$ 

### Incompressible Fluid
비압축성 유체의 경우, 밀도가 일정함으로 연속 방정식을 간단하게 나타낼 수 있다.

$$ \nabla \cdot v = 0 $$

### 참고
유체의 질량 보존을 나타내는 방정식을 `연속 방정식(Continuity Equation)`이라고도 한다.

## Conservation of Linear Momentum
Newton의 제 2 운동법칙에 의해 다음과같다.

$$ \Diff{}{t} \int_{\Omega(t)} \rho v dV = F_b + F_s $$

Reynold's tranport theorem에 의해 다음과 같이 표현할 수 있다.

$$ \int_{\Omega(t)} \left( \Diff{\rho v}{t} + \rho v (\nabla \cdot v) \right) \thinspace dV = F_b + F_s $$

$$ \int_{\Omega(t)} \pdiff{\rho v}{t} \thinspace dV + \int_{\partial\Omega(t)} \rho v(v \cdot n) \thinspace dS = F_b + F_s $$

### Differential Form
임의의 $\Omega(t)$에서 위의 적분식이 항상 성립해야 됨으로, 다음이 성립한다.

$$ \Diff{\rho v}{t} + \rho v (\nabla \cdot v) = F_b + F_s $$

만약, $\rho v$가 미분가능하다면, 다음과 같이 표현할 수 있다.

$$ \pdiff{\rho v}{t} + \nabla \cdot (\rho v v) = F_b + F_s $$ 

### Incompressible Fluid
비압축성 유체의 경우, 밀도가 일정함으로 방정식을 간단하게 나타낼 수 있다.

$$ \rho\Diff{v}{t} + \rho v (\nabla \cdot v) = F_b + F_s $$

이 때, continuity equation을 적용하면 $\nabla \cdot v = 0 $ 임으로 다음과 같다.

$$ \rho\Diff{v}{t} = F_b + F_s $$

