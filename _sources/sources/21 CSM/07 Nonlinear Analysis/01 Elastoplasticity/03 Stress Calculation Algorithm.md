# Prerequisite
1. 선형 탄성 재료에 J2 plasticity with combined hardening model을 사용한다.
2. $\sigma^k, \alpha^k, (\epsilon_e^p)^k, \Delta \epsilon$가 주어졌다.

# Objective
$\sigma^{k+1}, \alpha^{k+1},(\epsilon_e^p)^{k+1}, \tfrac{\partial\Delta\sigma}{\partial\Delta\epsilon}$를 계산한다.

# Algorithm
## 1. Elasticity assumption
J2 plasticity model의 가정에 의해 다음이 성립한다.

$$ \sigma = C : \epsilon^e $$

$$ \text{Where, } C \text{ is a linear stiffness tensor} $$

$\Delta \epsilon$가 더해진 후에도 elastic영역이여서 $\Delta \epsilon^p = 0$이라 가정한 상태를 trial state라고 하자.

$\sigma^k = C : (\epsilon^e)^k$라 할 때, trial state에서 다음이 성립한다.

$$ (\epsilon^e)^{tr} = (\epsilon^e)^k + \Delta \epsilon $$

따라서, elastic assumption에 의한 trial state의 변수들은 다음과같다.

$$ \begin{aligned} \sigma^{tr} &= C : ((\epsilon^e)^k + \Delta \epsilon) \\&= \sigma^k + C:\Delta \epsilon \\ \alpha^{tr} &= \alpha^k \\ (\epsilon_e^p)^{tr} &= (\epsilon_e^p)^k \end{aligned}  $$

### 명제1
$\sigma^{tr}$가 아래와 같이 주어졌다고하자.

$$ \sigma^{tr} = \sigma^k + C:\Delta \epsilon $$


$$ \text{Where, } C \text{ is a linear elastic stiffness tensor} $$

이 떄, 다음을 증명하여라.

$$ \tilde\sigma^{tr} = \tilde\sigma^k + 2 \mu \Delta \tilde\epsilon $$

**Proof**

$\tilde\epsilon$는 symmetric & deviatoric tensor임으로 다음이 성립한다.

$$ \begin{aligned} \tilde\sigma^{tr} &= \tilde\sigma^k + C:\Delta \tilde\epsilon \\&=  \tilde\sigma^k + \lambda\text{tr}(\tilde\epsilon)I + 2\mu\tilde\epsilon_{sym} \\&= \tilde\sigma^k + 2\mu\tilde\epsilon \quad\tiny\blacksquare \end{aligned} $$

## 2. Checking yield condition
Elasticity 가정이 옳바른지 판단하기 위해, trial state의 변수들이 yield condition을 만족하는지 확인한다.

J2 plasticity model의 yield function $f$는 다음과 같다.

$$ f(\sigma, \alpha, \epsilon_e^p) := \lVert \eta(\sigma,\alpha) \rVert - \sqrt{\frac{2}{3}}K(\epsilon_e^p) $$


$$ \text{Where, } \eta := \tilde\sigma - \alpha $$

이 때, yield condition은 다음과 같다.

$$ f(\sigma, \alpha, \epsilon_e^p) \le 0 $$

Trial state에서 yield condition을 만족하는 경우 elastic assumption은 참이 된다.

따라서 $\sigma^{k+1}, \alpha^{k+1},(\epsilon_e^p)^{k+1}$는 다음과 같은 값을 갖는다.

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} \\ \alpha^{k+1} &= \alpha^{tr} \\ (\epsilon_e^p)^{k+1} &= (\epsilon_e^p)^{tr} \\ \frac{\partial\Delta\sigma}{\partial\Delta\epsilon} &= C \end{aligned}$$

Yield condition을 만족하는 경우, 원하는 모든 값들을 찾았음으로 알고리즘을 종료한다.

### 명제1
Elasticity assumption이 참인 경우 다음을 증명하여라.

$$ \frac{\partial\Delta\sigma}{\partial\Delta\epsilon} = C $$

**Proof**

$\sigma^{k+1}$에 정의에 의해 다음이 성립한다.

$$ \Delta \sigma = \sigma^{k+1} - \sigma^{k} = C : \Delta \epsilon $$

따라서 다음이 성립한다.

$$ \frac{\partial\Delta\sigma}{\partial\Delta\epsilon} = C \quad\tiny\blacksquare $$

## 3. Plasticity correction
Yield condition을 만족하지 않는 경우 elastic assumption은 거짓이 된다.

따라서 $\Delta \epsilon^p \neq 0$이 되고 plastic strain에 대한 correction이 필요하게 된다. 

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} + \sigma_c \\ \alpha^{k+1} &= \alpha^{tr} + \alpha_c \\ (\epsilon_e^p)^{k+1} &= (\epsilon_e^p)^{tr} + (\epsilon_e^p)_c \end{aligned}$$

### Time Integration algorithm
J2 plasticity with hardening model에서는 time rate form으로 주어져 있다.

하지만 주어진 $\Delta \epsilon$은 finite한 값임으로, Time integration을 해줘야 하고 
Time integration scheme으로 backward Euler method를 사용하자.

명제1-3에 의해 correction term은 다음과 같다.

$$ \begin{aligned} \sigma_c &= -2 \mu \Delta\gamma N^{k+1} \\  \alpha_c &= H \Delta\gamma N^{k+1} \\ (\epsilon_e^p)_c &= \sqrt{\frac{2}{3}} \Delta\gamma \end{aligned} $$


$$ \text {Where, } N^{k+1} := \frac{\eta^{k+1}}{\lVert \eta^{k+1} \rVert} $$

따라서 stress, back stress, yield stress는 다음과 같다.

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} -2 \mu \Delta\gamma N^{k+1} \\ \alpha^{k+1} &= \alpha^{tr} + H^{k+1} \Delta\gamma N^{k+1} \\ (\epsilon_e^p)^{k+1} &= (\epsilon_e^p)^{tr} + \sqrt{\frac{2}{3}} \Delta\gamma \\ \frac{\partial\Delta\sigma}{\partial\Delta\epsilon} &= C - 2\mu \bigg( \frac{\partial\Delta\gamma}{\partial \Delta\epsilon} \otimes N^{k+1} + \Delta\gamma\frac{\partial N^{k+1}}{\partial \Delta\epsilon} \bigg) \end{aligned} $$

### 명제1
선형 탄성 재료에 J2 plasticity model을 사용한다고 하자.

$\Delta \epsilon$이 주어졌을 때, $\sigma^{tr}$을 다음과 같이 가정하자.

$$ \sigma^{tr} = \sigma^k + C:\Delta \epsilon $$

Time integration scheme으로 backward Euler method를 사용하고 $\Delta \epsilon^p \neq 0$일 때, 다음을 증명하여라.

$$ \sigma^{k+1} = \sigma^{tr} - 2\mu \Delta \gamma N^{k+1} $$

**Proof**

J2 plasticity model의 flow rule에 의해 다음이 성립한다.

$$ \frac{\partial \epsilon^p}{\partial t} = \gamma N $$

또한 J2 plasticity model에 의해 다음이 성립한다.

$$ \begin{aligned} \sigma^{k+1} &= \sigma^k + C:\Delta \epsilon^e \\&= \sigma^k + C: (\Delta \epsilon - \Delta \epsilon^p ) \\&= \sigma^{tr} - C : \Delta \epsilon^p \end{aligned} $$

Implicit Euler method 의해 다음이 성립한다.

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} - C : \Delta \epsilon^p \\&= \sigma^{tr} - \Delta t \gamma C : N^{k+1} \\&= \sigma^{tr} - 2\mu \Delta \gamma N^{k+1} \quad\tiny\blacksquare \end{aligned} $$


### 명제2
선형 탄성 재료에 J2 plasticity model을 사용한다고 하자.

$\Delta \epsilon^p \neq 0$일 때, 다음을 증명하여라.

$$ \alpha^{k+1} = \alpha^k + H^{k+1} \Delta \gamma N^{k+1} $$

**Proof**

J2 plasticity model의 hardening rule과 flow rule에 의해 다음이 성립한다.

$$ \frac{\partial \alpha}{\partial t} = H(\epsilon_e^p) \ \gamma N$$

Implicit Euler integration method에 의해 다음이 성립한다.

$$ \begin{aligned} \alpha^{k+1} &= \alpha^k + \Delta t H^{k+1} \gamma N^{k+1} \\&= \alpha^k + H^{k+1} \Delta\gamma N^{k+1} \quad\tiny\blacksquare \end{aligned} $$

### 명제3
선형 탄성 재료에 J2 plasticity model을 사용한다고 하자.

$\Delta \epsilon^p \neq 0$일 때, 다음을 증명하여라.

$$ (\epsilon_e^p)^{k+1} = (\epsilon_e^p)^k + \sqrt{\frac{2}{3}}\Delta \gamma $$

**Proof**

J2 plasticity model의 hardening rule과 flow rule에 의해 다음이 성립한다.

$$ \frac{\partial \epsilon_e^p}{\partial t} = \sqrt{\frac{2}{3}} \gamma  $$

Implicit Euler method에 의해 다음이 성립한다.

$$ \begin{aligned} (\epsilon_e^p)^{k+1} &= (\epsilon_e^p)^k + \Delta t \sqrt{\frac{2}{3}} \gamma \\&= (\epsilon_e^p)^k + \sqrt{\frac{2}{3}}\Delta \gamma \quad\tiny\blacksquare\end{aligned} $$

### 명제4
stress, back stress가 다음과 같다고 하자.

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} -2 \mu \Delta\gamma N^{k+1} \\ \alpha^{k+1} &= \alpha^{tr} + H^{k+1} \Delta\gamma N^{k+1}, \end{aligned} $$

다음을 증명하여라.

$$ \eta^{k+1}  = \eta^{tr} - \Delta\gamma(2\mu + H)N^{k+1} $$

**Proof**

$N$은 deviatoric tensor임으로 다음이 성립한다.

$$ \begin{aligned} \tilde\sigma^{k+1} &= \tilde\sigma^{tr} -2 \mu \Delta\gamma \tilde N^{k+1} \\&= \tilde\sigma^{tr} -2 \mu \Delta\gamma N^{k+1} \end{aligned} $$

따라서 $\eta$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \eta^{k+1} &= \tilde\sigma^{k+1} - \alpha^{k+1} \\&= \eta^{tr} - \Delta\gamma (2 \mu  + H)N^{k+1} \\&= \eta^{tr} - \Delta\gamma (2 \mu  + H)N^{k+1} \quad\tiny\blacksquare \end{aligned} $$


### 명제5
stress, back stress, yield stress가 다음과 같이 주어졌을 때,

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} -2 \mu \Delta\gamma N^{k+1} \\ \alpha^{k+1} &= \alpha^{tr} + H^{k+1} \Delta\gamma N^{k+1} \\ (\epsilon_e^p)^{k+1} &= (\epsilon_e^p)^{tr} + \sqrt{\frac{2}{3}} \Delta\gamma, \end{aligned} $$

다음을 증명하여라.

$$ N^{k+1} = N^{tr} $$

**Proof**

명제4에 의해 다음이 성립한다.

$$ \begin{aligned} & \eta^{k+1}  = \eta^{tr} - \Delta\gamma(2\mu + H)N^{k+1} \\ \Rightarrow \enspace & (\lVert \eta^{k+1} \rVert + \Delta\gamma(2\mu + H)) N^{k+1} = \eta^{tr}\\ \Rightarrow \enspace & \eta^{tr} = C N^{k+1} \end{aligned} $$

이는 $\eta^{tr}$과 $N^{k+1}$이 평행하다는 의미이다.

따라서,  $\eta^{tr}$의 방향을 나타내는 $N^{tr}$과 같다.

$$ N^{k+1} = N^{tr} \quad\tiny\blacksquare $$

#### 명제5.1
다음을 증명하여라.

$$ \lVert \eta^{tr} + C N^{k+1} \rVert = \lVert \eta^{tr} \rVert + C $$

**Proof**

명제5에 의해 다음이 성립한다.

$$ \begin{aligned} \lVert \eta^{tr} + C N^{k+1} \rVert &= \lVert \lVert \eta^{tr} \rVert(N^{tr}) + C N^{k+1} \rVert \\&= \lVert (\lVert \eta^{tr} \rVert + C) N^{k+1} \rVert \\&= \lVert \eta^{tr} \rVert + C \end{aligned} $$

### 명제6
함수 $f$가 다음과 같이 주어졌다고 하자.

$$ f(\sigma, \alpha,  \epsilon_e^p) = \lVert \eta \rVert - \sqrt{\frac{2}{3}}K(\epsilon_e^p) $$

stress, back stress, plastic equivalent stress가 다음과 같이 주어졌을 때,

$$ \begin{aligned} \sigma^{k+1} &= \sigma^{tr} -2 \mu \Delta\gamma N^{k+1} \\ \alpha^{k+1} &= \alpha^{tr} + H^{k+1} \Delta\gamma N^{k+1} \\ (\epsilon_e^p)^{k+1} &= (\epsilon_e^p)^{tr} + \sqrt{\frac{2}{3}} \Delta\gamma, \end{aligned} $$

다음을 증명하여라.

$$ f(\sigma^{k+1}, \alpha^{k+1}, (\epsilon_e^p)^{k+1}) = \lVert \eta^{k} \rVert - (H^{k+1} + 2\mu) \Delta\gamma - \sqrt{\frac{2}{3}} K^{k+1} $$

**Proof**

$f$에 주어진 값을 대입하면 다음과 같다.

$$ \begin{aligned} f(\sigma^{k+1}, \alpha^{k+1}, (\epsilon_e^p)^{k+1}) &= \lVert \eta^{k+1} \rVert - \sqrt{\frac{2}{3}} K^{k+1} \\ &= \lVert \eta^{tr} - (H^{k+1} + 2\mu) \Delta\gamma N^{k+1} \rVert - \sqrt{\frac{2}{3}}K^{k+1} \end{aligned}$$

명제5.1에 의해 다음이 성립한다.

$$ \begin{aligned} f(\sigma^{k+1}, \alpha^{k+1}, (\epsilon_e^p)^{k+1}) &= \lVert \eta^{tr} - (H^{k+1} + 2\mu) \Delta\gamma N^{k+1} \rVert - \sqrt{\frac{2}{3}}K^{k+1} \\&= \lVert \eta^{tr} \rVert - (H^{k+1} + 2\mu) \Delta\gamma - \sqrt{\frac{2}{3}}K^{k+1}\end{aligned}$$

### 명제7
$\Delta\epsilon^p \neq 0$인 경우 다음을 증명하여라.

$$ \frac{\partial \Delta\sigma}{\partial\Delta\epsilon} = C - 2\mu \bigg( \frac{\partial\Delta\gamma}{\partial \Delta\epsilon} \otimes N^{k+1} + \Delta\gamma\frac{\partial N^{k+1}}{\partial \Delta\epsilon} \bigg) $$

**Proof**

$\sigma^{k+1}$에 정의에 의해 다음이 성립한다.

$$ \Delta \sigma = \sigma^{k+1} - \sigma^{k} = C : \Delta \epsilon - 2 \mu \Delta\gamma N^{k+1} $$

따라서 다음이 성립한다.

$$ \frac{\partial \Delta\sigma}{\partial\Delta \epsilon} = C - 2\mu \bigg( \frac{\partial\Delta\gamma}{\partial\Delta\epsilon} \otimes N^{k+1} + \Delta\gamma\frac{\partial N^{k+1}}{\partial \Delta\epsilon} \bigg) $$

### 명제8
다음을 증명하여라.

$$ \frac{\partial N^{tr}}{\partial\Delta\epsilon} = \frac{2\mu}{\lVert\eta\rVert} (I_{dev} - N^{tr} \otimes N^{tr}) $$

**Proof**

$N^{tr}$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{\partial N^{tr}}{\partial\eta^{tr}} &= \frac{\partial}{\partial\eta^{tr}}\frac{\eta^{tr}}{\lVert\eta^{tr}\rVert} \\&= \frac{\lVert\eta^{tr}\rVert I_{diff} + \eta^{tr}\otimes N}{\eta^{tr}:\eta^{tr}} \\&= \frac{1}{\lVert\eta^{tr}\rVert}I_{diff} + \frac{1}{\lVert\eta^{tr}\rVert^3}\eta^{tr} \otimes \eta^{tr} \end{aligned} $$

$\eta^{tr}$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{\partial\eta^{tr}}{\partial\Delta\epsilon} &= \frac{\partial\eta^{tr}}{\partial\Delta\tilde\epsilon}\frac{\partial\Delta\tilde\epsilon}{\partial\Delta\epsilon} \\&= 2\mu I_{diff} : I_{dev} \\&= 2\mu I_{dev} \end{aligned} $$

위의 결과들과 보조명제8.1에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{\partial N^{tr}}{\partial\Delta\epsilon} &= \frac{\partial N^{tr}}{\partial\eta^{tr}}\frac{\partial\eta^{tr}}{\partial\Delta\epsilon} \\&= \frac{2\mu}{\lVert\eta\rVert} \bigg( I_{diff}:I_{dev} + \frac{1}{\lVert\eta^{tr}\rVert^2}\eta^{tr} \otimes \eta^{tr} : I_{dev} \bigg) \\&= \frac{2\mu}{\lVert\eta\rVert} \bigg( I_{dev} + \frac{1}{\lVert\eta^{tr}\rVert^2}\eta^{tr} \otimes \eta^{tr} \bigg) \\&= \frac{2\mu}{\lVert\eta\rVert} \bigg( I_{dev} + N \otimes N \bigg) \end{aligned} $$

#### 보조명제8.1
다음을 증명하여라.

$$ \eta^{tr} \otimes \eta^{tr} : I_{dev} = \eta^{tr} \otimes \eta^{tr} $$

**Proof**

$\eta^{tr}$가 symmetric & deviatroci tensor임으로 다음이 성립한다.

$$ \begin{aligned} \eta^{tr} \otimes \eta^{tr} : I_{dev} &= \eta^{tr} \otimes (\eta^{tr}_{sym} + \frac{1}{3}\text{tr}(\eta^{tr})I) \\&= \eta^{tr} \otimes \eta^{tr} \quad\tiny\blacksquare \end{aligned} $$

## 4. Calculating $\Delta \gamma$
$k+1$번째 값들은 plastic state에 있기 때문에 discrete plastic consistency condition을 만족한다.

$$ \begin{aligned} & f({\sigma}^{k+1}, {\alpha}^{k+1}, (\epsilon_e^p)^{k+1} ) = 0 \\ \Rightarrow \enspace & \lVert \eta^{k+1} \rVert - (H^{k+1} + 2\mu) \Delta\gamma - \sqrt{\frac{2}{3}}K^{k+1} = 0 \end{aligned} $$


$$\text {Where, } F((\epsilon_e^p)^{k+1}) \equiv F^{k+1} $$

이 때, ${\eta}^{tr},(\epsilon_e^p)^{tr}$는 전부 아는 값임으로 유일한 변수는 $\Delta\gamma$가 된다.

$$ f_{alg}(\Delta\gamma) := \lVert \eta^{tr} \rVert - (H^{k+1} + 2\mu) \Delta\gamma - \sqrt{\frac{2}{3}}K^{k+1}$$

따라서 $f_{alg}=0$이 되게 $\Delta\gamma$을 결정한다.

Nonlinear equation인 $f_{alg}=0$을 풀기 위해 newton-raphson method를 사용한다.

1. $n=0$으로 두고 초기 해 $\Delta\gamma_n = 0$으로 둔다.
2. $\Delta\gamma_n$에서 $f$를 선형근사한다.

$$ y= f_{alg}'(\Delta\gamma_n)(\Delta\gamma - \Delta\gamma_n) + f_{alg}(\Delta\gamma_n) $$


$$ \text{Where, } f_{alg}'(\Delta\gamma) := \frac{\partial f_{alg}}{\partial \Delta\gamma} $$
3. 선형근사한 함수의 해 $\Delta\gamma_{n+1}$을 찾는다.

$$ \Delta\gamma_{n+1} = \Delta\gamma_n - \frac{f_{alg}(\Delta\gamma_n)}{f_{alg}'(\Delta\gamma_n)}$$
4. $\Delta\gamma_{n+1}$이 수렴 조건을 만족하는지 확인한다.

$$ f_{alg}(\Delta\gamma_{n+1}) \le \epsilon \enspace \land \enspace N \le k + 1  $$   
5. 만족하지 않는 경우 $k = k +1$로 두고 과정2로 돌아간다.

### 명제1
함수 $f_{alg}$가 다음과 같이 주어졌다고 하자.

$$ f_{alg}(\Delta\gamma) := \lVert \eta^{tr} \rVert - (H^{k+1} + 2\mu) \Delta\gamma - \sqrt{\frac{2}{3}}K^{k+1}$$

이 때, 다음을 증명하여라.

$$ \frac{\partial f_{alg}}{\partial \Delta\gamma} = -\bigg( 2\mu + H^{k+1} + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p}\Delta\gamma + \frac{2}{3}\frac{\partial K^{k+1}}{\partial \epsilon_e^p} \bigg)$$

**Proof**

$\eta^{tr}$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \dfrac{\partial \eta^{tr}}{\partial \Delta \gamma} &= \dfrac{\partial }{\partial \Delta \gamma}(\sigma^k - \alpha^k + 2\mu\Delta\tilde\epsilon) \\&=0 \end{aligned} $$

또한 $(\epsilon_e^p)^{k+1}$의 정의에 의해 임의의 함수 $F(\epsilon_e^p)$에 대해 다음이 성립한다.

$$ \begin{aligned} \dfrac{\partial F^{k+1}}{\partial \Delta \gamma} &= \dfrac{\partial F^{k+1}}{\partial (\epsilon^p_e)^{k+1}}\dfrac{\partial(\epsilon^p_e)^{k+1}}{\partial\Delta \gamma} \\&= \sqrt{\frac{2}{3}}\dfrac{\partial F^{k+1}}{\partial\epsilon_e^p} \end{aligned} $$

위 두 결과를 이용해서 계산하면 성립함을 보일 수 있다.$\quad\tiny\blacksquare$ 

### 명제2
함수 $f_{alg}$가 다음과 같이 주어졌다고 하자.

$$ f_{alg}(\Delta\gamma) := \lVert \eta^{tr} \rVert - (H^{k+1} + 2\mu) \Delta\gamma - \sqrt{\frac{2}{3}}K^{k+1}$$

이 때, 다음을 증명하여라.

$$ \frac{\partial f_{alg}}{\partial\Delta\epsilon} = 2\mu N^{tr} - \bigg( 2\mu + H^{k+1} + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta\gamma + \frac{2}{3} \frac{\partial K^{k+1}}{\partial \epsilon_e^p} \bigg)\frac{\partial \Delta \gamma}{ \partial\Delta\epsilon} $$

**Proof**

$f_{alg}$의 정의에 의해 다음이 성립한다.

$$ \frac{\partial f_{alg}}{\partial\Delta\epsilon} = \frac{\partial \lVert \eta^{tr} \rVert}{\partial\Delta\epsilon} - (H^{k+1} + 2\mu)  \frac{\partial \Delta\gamma}{\partial\Delta\epsilon} - \frac{\partial H^{k+1}}{\partial\Delta\epsilon} \Delta\gamma - \sqrt{\frac{2}{3}} \frac{\partial K^{k+1}((\epsilon_e^p)^{k+1})}{\partial\Delta\epsilon} $$

보조명제2.1-3에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{\partial f_{alg}}{\partial\Delta\epsilon} &= \frac{\partial \lVert \eta^{tr} \rVert}{\partial\Delta\epsilon} - (H^{k+1} + 2\mu)  \frac{\partial \Delta\gamma}{\partial\Delta\epsilon} - \frac{\partial H^{k+1}}{\partial\Delta\epsilon} \Delta\gamma - \sqrt{\frac{2}{3}} \frac{\partial K^{k+1}((\epsilon_e^p)^{k+1})}{\partial\Delta\epsilon} \\&= 2\mu N - (H^{k+1} + 2\mu)  \frac{\partial \Delta\gamma}{\partial\Delta\epsilon} - \sqrt{\frac{2}{3}} \Delta \gamma \frac{\partial H^{k+1}}{\partial \epsilon_e^p} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon} - \frac{2}{3} \frac{\partial K^{k+1}}{\partial \epsilon_e^p} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon} \\&= 2\mu N - \bigg( 2\mu + H^{k+1} + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta\gamma + \frac{2}{3} \frac{\partial K^{k+1}}{\partial \epsilon_e^p} \bigg)\frac{\partial \Delta \gamma}{ \partial\Delta\epsilon} \end{aligned} $$


#### 보조명제2.1
선형 탄성 재료에 J2 plasticity model을 사용한다고 할 때, 다음을 증명하여라.

$$ \frac{\partial \lVert \eta^{tr} \rVert}{\partial\Delta\epsilon} = 2\mu N^{tr} $$

**Proof**

$\eta^{tr}$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{\partial \eta^{tr}}{\partial \Delta\tilde\epsilon} &= \frac{\partial }{\partial \Delta \tilde \epsilon} ({}^n \tilde \sigma + 2 \mu \Delta \tilde \epsilon - {}^n\alpha) \\&= 2\mu I_{diff} \end{aligned}  $$

따라서, chain rule에 의해 다음이 성립한다.

$$ \begin{aligned} \frac{\partial \lVert \eta^{tr} \rVert}{\partial\Delta\epsilon} &= \frac{\partial\lVert\eta^{tr}\rVert}{\partial\eta^{tr}}\frac{\partial\eta^{tr}}{\partial\Delta\tilde\epsilon}\frac{\partial\Delta\tilde\epsilon}{\partial\Delta\epsilon} \\&= 2\mu N^{tr}:I_{diff}:I_{dev} \\&= 2\mu N^{tr}:I_{dev} \\&= 2\mu N^{tr} \quad\tiny\blacksquare \end{aligned} $$

#### 보조명제2.2
다음을 증명하여라.

$$ \frac{\partial H^{k+1}}{\partial\Delta\epsilon} = \sqrt{\frac{2}{3}} \frac{\partial H^{k+1}}{\partial \epsilon_e^p} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon}  $$

**Proof**

Chain rule에 의해서 다음이 성립한다.

$$ \begin{aligned} \frac{\partial H^{k+1}}{\partial\Delta\epsilon} &= \frac{\partial H^{k+1}}{\partial ((\epsilon_e^p)^{k+1})} \frac{\partial ((\epsilon_e^p)^{k+1})}{\partial \Delta \gamma} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon} \\&= \sqrt{\frac{2}{3}}  \frac{\partial H(\epsilon_e^p)}{\partial \epsilon_e^p} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon} \quad\tiny\blacksquare \end{aligned}  $$

#### 보조명제2.3
다음을 증명하여라.

$$ \frac{\partial K^{k+1}((\epsilon_e^p)^{k+1})}{\partial\Delta\epsilon} = \sqrt{\frac{2}{3}} \frac{\partial K^{k+1}}{\partial \epsilon_e^p} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon}  $$

**Proof**

Chain rule에 의해서 다음이 성립한다.

$$ \begin{aligned} \frac{\partial K^{k+1}((\epsilon_e^p)^{k+1})}{\partial\Delta\epsilon} &= \frac{\partial K^{k+1}((\epsilon_e^p)^{k+1})}{\partial ((\epsilon_e^p)^{k+1})} \frac{\partial ((\epsilon_e^p)^{k+1})}{\partial \Delta \gamma} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon} \\&= \sqrt{\frac{2}{3}}  \frac{\partial K^{k+1}(\epsilon_e^p)}{\partial \epsilon_e^p} \frac{\partial \Delta \gamma}{\partial\Delta\epsilon} \quad\tiny\blacksquare \end{aligned}  $$

## Discrete Plastic Consistency Requirement
선형 탄성 재료에 J2 plasticity model을 기반으로한 알고리즘을 사용한다고 하자.

Stress calculation 과정에서 plastic state일 때, yield surface에 머물러 있어야 함으로 다음이 성립해야 한다.

$$ \begin{aligned} &\frac{\partial f_{alg}}{\partial\Delta\epsilon} = 0 \\ \Rightarrow \enspace & 2\mu N^{tr} - \bigg( 2\mu + H^{k+1} + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta\gamma + \frac{2}{3} \frac{\partial K^{k+1}}{\partial \epsilon_e^p} \bigg) \frac{\partial \Delta \gamma}{ \partial\Delta\epsilon} = 0 \\ \Rightarrow \enspace & \frac{\partial \Delta \gamma}{ \partial\Delta\epsilon} = \frac{2\mu N^{tr}}{2\mu + H^{k+1} + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta\gamma + \frac{2}{3} \frac{\partial K^{k+1}}{\partial \epsilon_e^p}}  \end{aligned} $$

### 명제1
선형 탄성 재료에 J2 plasticity model을 사용한다고 하자.

Plastic state일 때, 알고리즘을 통해 계산된 $\Delta \sigma$가 있다고 할 때, 다음을 증명하여라.

$$ \frac{\partial\Delta\sigma}{ \partial\Delta\epsilon} = C - \frac{4 \mu^2 N^{tr} \otimes N^{tr}}{2\mu + H + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta \gamma + \frac{2}{3}\frac{\partial K^{k+1}}{\partial \epsilon_e^p}} - \frac{4 \mu^2 \Delta \gamma}{\lVert \eta^{tr} \rVert}(I_{dev} - N^{tr} \otimes N^{tr})   $$

**Proof**

$\Delta\epsilon^p \neq 0$인 경우 다음이 성립한다.

$$ \frac{\partial \Delta\sigma}{\partial\Delta\epsilon} = C - 2\mu \bigg( \frac{\partial\Delta\gamma}{\partial \Delta\epsilon} \otimes N^{k+1} + \Delta\gamma\frac{\partial N^{k+1}}{\partial \Delta\epsilon} \bigg) $$

discrete plastic consistency requirement와 plastic correction 명제5,8에 의해 다음이 성립한다.

$$ \frac{\partial\Delta\sigma}{\partial\Delta\epsilon} = C - \frac{4 \mu^2 N^{tr} \otimes N^{tr}}{2\mu + H^{k+1} + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta\gamma + \frac{2}{3}\frac{\partial K^{k+1}}{\partial \epsilon_e^p}} - \frac{4 \mu^2 \Delta \gamma}{\lVert \eta^{tr} \rVert}(I_{dev} - N^{tr} \otimes N^{tr}) \quad\tiny\blacksquare $$

## Tangent Stiffness Tensor
Algorithmic tangent stiffness tensor를 다음과 같이 정의하자.

$$ C^{alg} := \frac{\partial\Delta\sigma}{\partial\Delta\epsilon} $$

그러면 checking yield condition의 명제1과 discrete plastic consistency requirement의 명제1에 의해 다음과 같다.

$$ C^{alg} = \begin{dcases} C & \text{elastic state} \\  C - \frac{4 \mu^2 N^{tr} \otimes N^{tr}}{2\mu + H + \sqrt{\frac{2}{3}}\frac{\partial H^{k+1}}{\partial \epsilon_e^p} \Delta \gamma + \frac{2}{3}\frac{\partial K^{k+1}}{\partial \epsilon_e^p}} - \frac{4 \mu^2 \Delta \gamma}{\lVert \eta^{tr} \rVert}(I_{dev} - N^{tr} \otimes N^{tr}) & \text{plastic state} \end{dcases} $$

### 명제
$\Delta t \rightarrow 0$일 때, 다음을 증명하여라.

$$ C^{alg} = C^{ep} $$

**Proof**

$\Delta t \rightarrow 0$일 때, 다음이 성립한다.

$$ \Delta \gamma \rightarrow 0 $$

따라서 다음이 성립한다.

$$ \lim_{\Delta t \rightarrow 0} C^{alg} = C^{ep} \quad\tiny\blacksquare $$