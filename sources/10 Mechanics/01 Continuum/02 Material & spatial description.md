# Lagrangian & Eulerian Description
운동하는 연속체의 어떤 물리량을 나타내는 텐서 $Q$가 있다고 하자. 

이 떄, $Q$를 다음 두가지 관점으로 서술할 수 있다.

## Lagrangian Description
$X$로 표현되는 연속체의 한 점의 $t$일 떄의 $Q$의 값을 함수로 나타내면 다음과 같다.

$$ Q = F(X,t) $$

이러한 서술 방법을 `물질 관점(material description)` 혹은 `Lagrangian description`이라고 한다.

이 때 주목할 점은, $t$가 변하면 $X$로 표현되는 연속체의 한 점에 위치도 변할 수 있다는 것이다. 

따라서 물질 관점에서 서술할 경우, 공간상의 한 점에서 연속체의 물리량이 어떻게 변하는지에 대한 정보를 직접적으로 제공하지는 않는다.

## Eulerian Description
$x$로 표현되는 공간상의 한 점에서 $t$일 떄의 $Q$의 값을 함수로 나타내면 다음과 같다.

$$ Q = G(x,t) $$

이러한 서술 관점을 `공간 관점(spatial description)` 혹은 `Eulerian description`이라고 한다.

이 때 주목할 점은, $t$가 변하면 $x$ 점에 존재하는 연속체의 한 점도 변할 수 있다는 것이다. 

따라서 spatial description에서 서술할 경우, 연속체의 한 점에 물리량이 어떻게 변하는지에 대한 정보를 직접적으로 제공하지는 않는다.

### 참고
Deformation $\varphi$을 이용하면, Eulerian description을 Lagrangian description으로 바꿀 수 있다.

$$ G(x,t) = G(\varphi(X,t),t) = F(X,t) $$

따라서, 앞으로 Eulerian description으로 주어진 함수 $G(x,t)$가 있을 때, $G(\varphi(X,t),t)$를 종종 $G_m(X,t)$로 표기한다.

> 참고  
[book] (Lai et al) Introduction to Continuum Mechanics Chapter3.2