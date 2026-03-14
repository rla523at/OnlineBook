# Deformation
## Reference Figure
어떤 기준이 되는 시점 $t_0$에 연속체를 이루고 있는 점의 집합 $\Omega_0$을 `reference figure`이라 한다.

이후 $t_0$로부터 $t \in \R^+$의 시간이 흘러 변형된 연속체를 이루고 있는 점의 집합 $\Omega(t)$을 `deformed figure`라 한다.

## Material Coordinate
Reference figure $\Omega_0$가 있을 때, $X \in \Omega_0$를 `물질좌표(material coordinates)`라고 한다.

### 참고1
연속체의 한 점을 기준 시간 $t_0$라는 기준을 가지고 표현하는 방법이다.

## Deformation
연속체의 모든 점마다 시간에 따라 어떤 점으로 가는지 표현하는 함수가 유일하게 존재한다고 하자.

연속체의 모든 점은 material coordinate $X$로 유일하게 표현할 수 있음으로, $X$가 시간에 따라 어떤 점으로 가는지를 표현한 함수를 $\varphi_X$라고 하자.

그러면 연속체가 시간에 따라 어떤 deformed figure을 갖는지 나타내는 함수 $\varphi$를 다음과 같이 정의할 수 있다.

$$ \varphi : \Omega_0 \times \R^+ \rightarrow \Omega(t) \st (X,t) \mapsto \varphi_X(t) $$

이렇게 정의된 $\varphi$를 `변형(deformation)`이라고 한다.

> Reference   
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter3.1

### 참고(연속체 변형 가정)
연속체의 변형이 물리적으로 타당하기 위해서 다음 두가지를 가정한다.
1. 한 점은 시간이 지나도 한 점이며, 한 점은 하나의 위치만 차지해야 한다.
2. 연속체의 변형이 물리적으로 타당하기 위해, 하나의 위치에는 하나의 점만 존재 할 수 있다.

### 명제1
다음을 증명하여라.

$$ \varphi \text{ is a well defined} $$

**Proof**

연속체의 변형의 가정1에 의해 $\varphi_X$는 well-defined 함수이다.

$$ t_1 = t_2 \implies \varphi_X(t_1) = \varphi_X(t_2) $$

연속체의 모든 점마다 위치를 나타내는 함수가 유일하게 존재함으로 다음이 성립한다.

$$ X_1 = X_2 \implies \varphi_{X_1} = \varphi_{X_2} $$

이 둘을 조합하면, 다음이 성립한다.

$$ (X_1,t_1) = (X_2,t_2) \implies \varphi(X_1,t_1) = \varphi(X_2,t_2)  $$

즉, $\varphi$는 well-defined 함수이다.

### 명제2
$t \in \R^+$가 있을 떄, 함수 $\varphi_t$를 다음과 같이 정의하자.

$$ \varphi_t : \Omega_0 \rightarrow \Omega_t \st X \mapsto \varphi_X(t) $$

이 때, 다음을 증명하여라.

$$ \varphi_t \text{ is a bijective} $$

**Proof**

[well-defined]  
$\varphi$는 well defined 함수임으로 다음이 성립한다.

$$ X_1 = X_2 \implies  \varphi_t(X_1) = \varphi(X_1,t) = \varphi(X_2,t) = \varphi_t(X_2) $$

[injective]  
연속체의 변형의 가정2에 의해 다음이 성립한다.

$$ \varphi_t(X_1) = \varphi_t(X_2) \implies X_1 = X_2 $$

[surjective]  
$\Omega_t$의 정의에 의해 다음이 성립한다.

$$ \forall x \in \Omega_t, \quad \exist X \in \Omega_0 \st \varphi_t(X) = X $$

따라서, $\varphi$는 bijective 함수이다. $\qed$

#### 참고1
$\varphi_t$는 $X \in \Omega_0$를 기준시간으로 부터 $t$가 지났을 때의 위치로 보내주는 함수이다.

#### 참고2
$\varphi_{t_0}=id_{\Omega_0}$이다.

## Displacement
연속체 한 점이 시간에 따라 얼마나 변했는지를 나타내는 vector $d$를 `변위(displacement)`라고 정의한다.

$$  d(X, t) := \varphi(X, t) - X $$
