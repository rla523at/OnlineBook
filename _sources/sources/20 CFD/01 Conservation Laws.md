# Conservation Laws
## Definition
Conservation Law는 특정 물리량이 새롭게 생성되거나 제거 되지 않고 보존되는 현상을 나타내는 법칙이다.

새롭게 생성되거나 제거되지 않기 때문에 특정 영역안에서 물리량이 변화했다면 영역의 표면(surface)을 통해 흘러 들어오거나 나가는 양에 의해 변화가 발생하게 된다.

먼저, 특정 영역은 고정된 영역일 필요는 없으며 시간 $t$에 따라 변화할 수 있음으로 $\Omega(t)$라고 하고 $\Omega$의 surface를 $\partial\Omega(t)$라고 하자.

그리고 $\Omega$안에서 단위 부피당 어떤 물리량을 나타내는 값을 $q(x,t)$, $q$의 속도를 $v_q(x,t)$라고 하고 $\partial\Omega$의  속도를 $v_b(x,t)$라고 하면 conservation law는 다음과 같이 표현된다.

$$ \diff{}{t} \int_{\Omega} q \thinspace dV + \int_{\partial\Omega} q (v_q-v_b) \cdot dS = 0 $$

> Reference  
> [wikipedia](https://en.wikipedia.org/wiki/Continuum_mechanics#Balance_laws)

### 참고1
$v_q-v_b$는 $\partial\Omega$를 기준으로 바깥으로 나가는 $q$의 상대속도이다.

### 참고2
$q(v_q-v_b)$는 물리량 $\times$ $\partial\Omega$를 통해 유출되는 속도를 나타낸다.

따라서, $f(x,t) = q(v_q-v_b)$를 $q$의 `유량(flux)`이라고 한다.

그러면 conservation laws는 다음과 같은 형태를 갖는다.

$$ \diff{}{t} \int_{\Omega} q \thinspace dV + \int_{\partial\Omega} f \cdot dS = 0 $$

이러한 형태를 `conservation form`라고 한다.

> Reference  
> [wikipedia](https://en.wikipedia.org/wiki/Continuity_equation#Definition_of_flux)

### 참고3
$dS$는 $\Omega$를 기준으로 바깥으로 나가는 방향임으로 $(v_q-v_b) \cdot dS$는 바깥으로 나가는 양은 $+$ 안으로 들어오는 양은 $-$ 값을 갖는다.

그럼으로  $\int_{\partial\Omega} q (v_q-v_b) \cdot dS$는 "바깥으로 나가는 양 - 안으로 들어오는 양" 즉, 순 유출양을 의미한다.

즉, 보존되는 물리량의 변화량과 surface를 통해 순유출된 양의 합은 $0$이 된다는 의미를 갖는다.

### 참고4
conservation laws를 다음과 같이 표현하자.

$$ \diff{}{t} \int_{\Omega} q \thinspace dV = - \int_{\partial\Omega} q (v_q-v_b) \cdot dS $$

$-\int_{\partial\Omega} q (v_q-v_b) \cdot dS$는 순 유입양을 의미한다.

따라서, 보존되는 물리량은 surface를 통해 순유입된 양만큼 변화한다는 의미를 갖는다.

## Balance Law
Balance Law는 conservation law에서 물리량이 새롭게 생성되거나 제거되는 현상을 포함하는 법칙이다.

따라서, 특정 영역안에서 물리량이 변화했다면 영역의 surface를 통해 흘러 들어오거나 나가는 양과 물리량이 새롭게 생성되거나 제거되는 양의 합이 될 것이다.

$\Omega$안에서 단위 부피당 생성되는 물리량을 $S_V$, $\partial\Omega$에서 단위 면적당 생성되는 물리량을 $S_S$라고 하면 balance law는 다음과 같이 표현할 수 있다.

$$ \diff{}{t} \int_{\Omega} q \thinspace dV + \int_{\partial\Omega} q (v_q-v_b) \cdot dS = \int_{\Omega} S_V \thinspace dV + \int_{\partial\Omega} S_S \cdot dS $$

> Reference  
> [wikipedia](https://en.wikipedia.org/wiki/Continuum_mechanics#Balance_laws)

### 참고1
$S_V,S_S$가 양수일 경우 source 음수일 경우 sink라고 하며 통칭하여 그냥 source라고 부르기도 한다.

---

> Reference  
> [wikiversity](https://en.wikiversity.org/wiki/Continuum_mechanics)