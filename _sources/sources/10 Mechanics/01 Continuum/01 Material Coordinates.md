# Material Coordinates
## Definition
$t_0$일 때, 연속체를 이루는 모든 점들이 차지하는 위치의 집합을 $\Omega_0 \subseteq \R^n$이라고 하자.

연속체의 모든 점마다 시간에 따른 위치를 나타내는 위치 함수가 유일하게 존재하고 그 함수의 집합을 $\varPhi$라고 하면

$X \in \Omega_0$를 `물질좌표(material coordinates)`라고 하며, $X$는  $t_0$일 때, $X$에 위치한 연속체의 특정점을 나타내는 고유한 값이 될 수 있음으로 다음이 성립한다.

$$ \forall X \in \Omega_0, \quad \exist! \varphi_X \in \varPhi \st \varphi_X(t_0) = X $$

그러면 전체 연속체의 시간에 따른 위치를 나타내는 함수 $\phi$를 다음과 같이 정의할 수 있다.

$$ \phi : \Omega_0 \times \R^+ \rightarrow \R^3 \st (X,t) \mapsto \varphi_X(t) $$


> Reference   
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter3.1

### 참고1
$\Omega_0$를 `undeformed configuration` 혹은 `reference configuration`이라고 한다.

> Reference  
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter3.1

### 참고2
$X$는 기준 시간 $t_0$때 위치를 가지고 연속체의 한 점을 표현하기 위해 도입한 좌표이다.

예를 들어, $X=(X_1,X_2,X_3)$라고 한다면, $t_0$일 떄, $(X_1,X_2,X_3) \in \R^3$에 있었던 연속체 내부의 점이라는 의미를 갖는다.

따라서 $\phi(X, t_1)$은 $t_0$일 때, $X$에 있었던 연속체 내부의 점이 $t_1$일 때의 위치를 의미한다.

### 참고3
$t \in \R^+$가 있을 떄, 함수 $\phi(\cdot, t)$를 다음과 같이 정의하자.

$$ \phi(\cdot, t) : \Omega_0 \rightarrow \R^n \st X \mapsto \varphi_X(t) $$

그러면 $\phi(\cdot, t)$는 $X \in \Omega_0$를 $t$일 떄 위치로 함수로 옮겨주는 함수이다.

따라서, 다음이 성립한다.

$$\phi(\cdot, t_0) =id_{\Omega_0}$$