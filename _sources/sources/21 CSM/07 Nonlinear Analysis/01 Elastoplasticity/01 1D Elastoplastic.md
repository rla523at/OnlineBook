# 1D Elastoplasticity
`Elastoplastic material`이란 elastic deformation과 plastic deformation이 전부 발생하는 재료다.

이 떄, small deformation상황에서 elastoplastic material을 modeling할 것이기 때문에 elastic과 strain hardening 현상만 모델링 한다.

## Stress-Strain Relation in Tension
Tensile test를 통해서 low-carbon steel의 stress-strain curve를 그리면 다음과 같다.

<p align = "center">
<img src = "./_image/0101.png">
</p>

> Reference  
> [wiki-yield](https://en.wikipedia.org/wiki/Yield_(engineering)#Definition)  

그래프를 그릴 때 사용한 stress와 strain은 초기 단면적$A$와 초기 길이 $L$를 사용하는 engineering stress와 engineering strain을 사용한다.

초기의 일정 하중(3번) 전까지는 elastic deformation이 발생하며, elastic deformation이 발생한 뒤 `unloading`(제하)하면 원래 상태로 돌아간다. elastic deformation이 발생하는 동안 stress-strain curve가 elastic modulus $E$의 기울기를 갖는다. 

계속 하중을 증가시켜서 plastic deformation이 발생하면 영구변형이 생긴다. 영구 변형은 금속 내 결정체들의 전이(`dislocation`)라 불리는 미끄러짐에 기인한다. plastic deformation이 발생한 뒤에 unloading하면 그림에 점선으로 표시된 경로를 따라서 stress-strain curve가 그려지며 $E$의 기울기를 갖는다. 이 후 하중을 완전히 제거 했을 때 영구변형에 의해 발생한 변형률을 `plastic strain` $\epsilon_p$라고 한다.

`Plastic deformation`이 발생하기 시작하는 stress-strain curve 상의 점을 `elastic limit` (3번,4번)이라고 하고 Elastic limit에서의 stress를 `yield stress`라고 한다.

plastic deformation이 발생한 뒤에 unloading했다가 reloading을 하면 yield stress가 증가하는 `strain hardening` 현상이 나타난다. 그 이유는 이미 dislocation된 결정체는 추가적인 dislocation에 저항하려는 성질이 있기 때문에 dislocation의 증가가 둔화되고 따라서 추가적인 dislocation을 발생시키려면 이전 보다 더 큰 하중이 필요하게 되고 yield stress가 증가하는 현상이 나타난다. 그 결과 재료의 강성이 증가된다.


> Reference  
> [wiki-yield](https://en.wikipedia.org/wiki/Yield_(engineering)#Definition)  
> [wiki-tensile testing](https://en.wikipedia.org/wiki/Tensile_testing)  
> [wiki-stress-strain-curve](https://en.wikipedia.org/wiki/Stress%E2%80%93strain_curve)
> [wiki-plasticity](https://en.wikipedia.org/wiki/Plasticity_(physics))
> [반디통](https://www.banditong.com/cae-dict/strain_hardening)  

## Hardening Model
Elastic limit 이후에 strain-hardening 현상을 나타내는 stress-strain curve의 기울기를 `tangent modulus` $E_t$라고 한다.

strain-hardening을 linear로 modeling하여 $E_t$가 상수인 간단한 두 모델 `kinematic hardening model`과 `isotropic hardening model`을 살펴보자.

<p align = "center">
<img src = "./_image/2022.09.16_2.png">
</p>


`인장-압축(tension-compression)`이 반복되는 cyclic loading이 발생하는 경우, yield stress를 어떻게 결정하는지에 따라 다른 hardening model을 사용한다.

### Kinematic hardening model
* Elastic range가 initial yeild stress의 2배이다.
* Elastic range의 중심점이 원점을 지나고 기울기가 tangent modulus인 직선(그림상 실점선)을 따라 움직인다.
* Elastic range가 항상 initial yeild stress의 2배임으로 elastic range는 일정하다.
  * Bauschinger effect를 잘 표현한다.

> Reference  
> [Bauschinger effect](https://www.banditong.com/cae-dict/bauschinger_effect)

### Isotropic hardening model
* Elastic range가 current yeild stress의 2배이다.
  * $|\sigma_b| = |\sigma_e|$이고 $|\sigma_f| = |\sigma_g|$이다.
* Elastic range의 중심점이 strain축을 따라 움직인다.
* Plastic deformation이 발생할 때 마다 yeild stress가 증가함으로 elastic range도 증가한다.

## Plastic Strain
위에서 하중을 완전히 제거 했을 때 영구변형에 의해 발생한 변형률을 plastic strain $\epsilon_p$로 정의했다. 이와 비슷하게 unloading하는 과정에서 줄어드는 변형률을 elastic strain $\epsilon_e$라고 정의하자.

그러면 Total strain을 $\epsilon$이라고 할 때, small deformation 가정에 의해 다음 관계가 성립한다.

$$ \epsilon = \epsilon_e + \epsilon_p $$


일반적으로 displacement의 gradient로 표현되는 strain은 비선형임으로 $\epsilon$의 각 component가 $\epsilon_e, \epsilon_p$의 각 component의 합으로 표현되지 않지만 small deformation 가정에 의해 linear in the displacements임으로 위 식이 성립한다.

> Reference  
> 1969 [Paper] (Lee) Elastic-Plastic Deformation at Finite Strain

### ?
$\epsilon_p$는 정의상 stress에 영향을 주지 않기 때문에 현재 stress를 결정하기 위해서는 $\epsilon_e$가 얼마인지에 알아야한다. 

따라서 elastoplastic material에서는 $\epsilon_p$를 계산해야 stress를 구할 수 있으며, $\epsilon_p$를 계산하기 위해서는 현재까지 얼마나 plastic deformation이 발생했는지 알아야 한다.

이러한 특성을 `path(history) dependent`라고 한다.

### Effective Plastic Strain
현재 상태의 plastic strain을 $\epsilon_p^n$이라고 하고 이전 상태에서 현재 상태로 변하면서 plastic deformation에 의해 발생한 plastic strain을 $\Delta\epsilon_p$라고 하자.

$\epsilon_p$의 경우 압축에서 plastic deformation이 발생할 경우 $\Delta\epsilon_p < 0$ 일 수 있다. 즉, $\epsilon_p$는 감소할 수 있다. 하지만 isotropic hardening model의 경우 compression에서 plastic deformation이 발생하더라도 yield stress $\sigma_Y$는 계속 증가한다.

이를 위해, plastic strain의 크기를 누적한 값을 effective plastic strain $\epsilon^e_p$라고 정의하자.

$$ (\epsilon^e_p)^n = (\epsilon^e_p)^{n-1} + \norm{\Delta\epsilon_p} $$

### Definition(Plastic Modulus)
Elastoplastic material이 있을 때, strain increment $\Delta \epsilon$가 주어졌다고 하자.

이 떄, `plastic modulus` $H$를 다음과 같이 정의한다.

$$ H := \frac{\Delta \sigma}{\Delta \epsilon_p} $$

그러면 stress increment $\Delta \sigma$는 다음과 같다.

$$ \Delta \sigma = E \Delta \epsilon_e = E_t \Delta \epsilon = H\Delta \epsilon_p $$

#### 참고
Elastic phase인 경우, $\Delta\epsilon_p = 0$인데 $\Delta\sigma \neq 0$임으로 $H=\infty$이다. 

### 명제1
Elastoplastic material이 plastic phase에 있을 때 strain increment $\Delta \epsilon$가 주어졌다고 하자.

다음을 증명하여라.

$$ \frac{1}{E_t} = \frac{1}{E} + \frac{1}{H} $$

#### 따름명제1
다음을 증명하여라.

$$ H = \frac{EE_t}{E-E_t} $$

#### 따름명제2
다음을 증명하여라.

$$ E_t = \frac{EH}{H + E} $$

##### 참고
$H$가 $\infty$면 $E_t = E$이다.

### 명제2
Elastoplastic material이 plastic phase에 있을 때 strain increment $\Delta \epsilon$가 주어졌다고 하자.


다음을 증명하여라.

$$ \Delta \epsilon_p = \frac{\Delta \epsilon}{1 + H /E} $$

### 참고(아닐듯?)
명제1과 2는 Elastoplastic material이 plastic phase에 있는 상황에서 strain increment가 주어졌을 때 성립한다.

# FE Formulation for Elastoplasticity
Material nonlinearity를 고려해서 constitutive equation이 다음과 같이 주어졌다고 하자.

$$ \sigma_v = C^{ep}(d)\epsilon_v $$

linear relation이 아닌 stress-strain relation 고려하기 위해  elasticity tensor $C^{ep}$가 상수가 아니고 displacement의 함수가 되며 $C^{ep}(d)$를 `elastoplastic tangent modulus`라 한다. 그러면 $C^{ep}(d)$의 정의에 의해 다음이 성립한다.

$$C^{ep}(d) = \pdiff{\sigma_v}{\epsilon_v}$$

따라서, small deformation을 가정할 떄, load $F$에 대한 static structural equilibrium의 FE formulation은 다음과 같다.

$$ \int_\Omega B^T \sigma_v(d) \thinspace dV - F = 0 $$

# Algorithm
incremental force method를 사용하고 $n$번째 load increment까지 해석이 완료되었다고 하자.

$$ K_t({}^{n}d) {}^{n}d - {}^{n}F = 0 $$

$R(d)$를 다음과 같이 정의하자.

$$R(d) := K_t(d) d - {}^{n+1}F $$

이 때, $R(d) = 0$을 만족하는 $d$를 구하여서, $n+1$ load increment과 equilibrium을 이루는 ${}^{n+1}d$를 찾아보자.

이를 위해, Newton-Raphson method를 사용하자.

1. 초기 값을 결정하고 $k=0$으로 둔다.

$$ {}^{n+1}d^0 = {}^{n}d $$   

2. 선형화 한 뒤 해를 구한다.

$$ {}^{n+1}d^{k+1} = {}^{n+1}d^k - J_R({}^{n+1}d^k)^{-1} R({}^{n+1}d^k) $$

3. ${}^{n+1}d^{k+1}$이 convergence criterion을 만족하는지 확인한다.

$$ \lVert R({}^{n+1}d^{k+1}) \rVert \le \epsilon \enspace \land \enspace N \le k $$
4. convergence criterion을 만족하는 경우 ${}^{n+1}d = {}^{n+1}d^{k+1}$로 두고 algorithm을 종료한다. convergence criterion을 만족하지 않는 경우, $k = k+1$로 두고 step2로 돌아간다.

---

이 때, `tangent stiffness matrix` $K_t$를 다음과 같이 정의하자.

$$ K_t(d) := \int_\Omega B^T \sigma_v \thinspace dV $$

그러면 FE Formulation for elastoplasticity는 다음과 같아진다.

$$ K_t(d)d - F = 0 $$

# Algorithm
incremental force method를 사용하고 $n$번째 load increment까지 해석이 완료되었다고 하자.

$$ K_t({}^{n}d) {}^{n}d - {}^{n}F = 0 $$

$R(d)$를 다음과 같이 정의하자.

$$R(d) := K_t(d) d - {}^{n+1}F $$

이 때, $R(d) = 0$을 만족하는 $d$를 구하여서, $n+1$ load increment과 equilibrium을 이루는 ${}^{n+1}d$를 찾아보자.

이를 위해, Newton-Raphson method를 사용하자.

1. 초기 값을 결정하고 $k=0$으로 둔다.

$$ {}^{n+1}d^0 = {}^{n}d $$   

2. 선형화 한 뒤 해를 구한다.

$$ {}^{n+1}d^{k+1} = {}^{n+1}d^k - J_R({}^{n+1}d^k)^{-1} R({}^{n+1}d^k) $$

3. ${}^{n+1}d^{k+1}$이 convergence criterion을 만족하는지 확인한다.

$$ \lVert R({}^{n+1}d^{k+1}) \rVert \le \epsilon \enspace \land \enspace N \le k $$
4. convergence criterion을 만족하는 경우 ${}^{n+1}d = {}^{n+1}d^{k+1}$로 두고 algorithm을 종료한다. convergence criterion을 만족하지 않는 경우, $k = k+1$로 두고 step2로 돌아간다.

## Stress calculation
Newton-Raphson method의 step2를 보면 $R({}^{n+1}d^k)$를 계산해야 한다.

$$ \begin{aligned} R({}^{n+1}d^k) &= \Big( \int_\Omega B^T C^{ep}B \thinspace dV \Big) {}^{n+1}d^k - {}^{n+1}F \\&= \int_\Omega B^T ({}^{n+1}\sigma^k) \thinspace dV - {}^{n+1}F \end{aligned} $$

${}^{n+1}F$는 주어진 값임으로 추가적으로 계산할 필요가 없으나 displacement가 ${}^{n+1}d^k$일 때 발생하는 stress ${}^{n+1}\sigma^k$를 계산해야 한다.

이 떄, ${}^{n+1}\sigma^k = {}^{n}\sigma + \Delta \sigma$이고, ${}^{n}d$과 ${}^{n}\sigma$는 이미 알고 있는 값임으로 $\Delta \epsilon = B({}^{n+1}d^k - {}^{n}d)$가 주어졌을 때, $\Delta \sigma$가 얼마인지 구하면 된다.

이 때, $\Delta \sigma$ 계산은 hardening model에 따라 달라진다.

### Isotropic Hardening Model
다음과 같은 값들이 주어졌다고 하자.
* elastic modulus $E$
* plastic modulus $H$
* $n$번째 load increment에서 stress ${}^n\sigma$
* $n$번째 load increment에서 yield stress ${}^n\sigma_Y$
* strain increment $\Delta \epsilon$

이 떄, ${}^{n+1}\sigma, {}^{n+1}\sigma_Y$를 구하는 방법은 다음과 같다.

#### 1. Elasiticy assumption
$\Delta \epsilon$가 더해진 후에도 elastic영역이여서 $\Delta \epsilon_p = 0$이라 가정하자.

$\Delta \epsilon = \Delta \epsilon_e$임으로 elasticity 가정에 의한 trial stress와 trial yield stress는 다음과 같다.

$$ \begin{aligned} {}^{tr}\sigma &= {}^n\sigma + E \Delta \epsilon \\ {}^{tr}\sigma_Y &= {}^n\sigma_Y \end{aligned}  $$

#### 2. Checking yield condition
Elasticity 가정이 옳바른지 판단하기 위해, 현재 stress들이 yield condition을 만족하는지 확인한다.

Isotropic hardening model에서 yield condition은 stress가 yield stress보다 항상 작거나 같아야 한다는 조건이다. 

$$ |\sigma| \le {}^{}\sigma_Y $$

따라서, 다음이 성립해야한다.

$$ \begin{equation} \begin{aligned} & |{}^{tr}\sigma| \le {}^{tr}\sigma_Y \\ \Leftrightarrow \enspace & |{}^n\sigma + E \Delta \epsilon| \le {}^{n}\sigma_Y \end{aligned} \end{equation} $$

식(1)이 성립하는 경우 elastic assumption은 참이 되고, stress와 plastic strain은 다음과 같다.

$$ \begin{gathered} {}^{n+1}\sigma = {}^{tr}\sigma \\ {}^{n+1}\sigma_Y = {}^{n}\sigma_Y \end{gathered}$$

#### 3. Plasticity correction
식(1)이 성립하지 않는 경우 ealstic assumption은 거짓이 되고 이는 $\Delta \epsilon_p \neq 0$이 되어 plastic strain에 대한 correction이 필요하게 된다. 

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma + \sigma_c \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y + {\sigma_Y}_c \end{aligned}$$

Plastic strain에 의한 strain-hardening 현상을 고려한 stress correction term $\sigma_c$와 yield stress correction term ${\sigma_Y}_c$는 다음과 같다.

$$ \begin{aligned} \sigma_c &= -\text{sgn}({}^{tr}\sigma) E \Delta\epsilon_p \\  {\sigma_Y}_c &=  H \Delta\epsilon_p \end{aligned}  $$

따라서 stress와 yield stress는 다음과 같다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\sigma) E \Delta\epsilon_p \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y + H \Delta \epsilon_P \end{aligned} $$

이 과정은 trial stress에 plastic strain increment의 영향을 반영하기 때문에 `plastic corrector`라고하며, trial stress에서 yield stress로 돌아온다는 의미에서 `return-mapping`이라고 한다.

##### 3-1 Calculating plastic strain increment
$\Delta\epsilon_p$을 계산하기 위해 `plastic consistency condition`을 이용한다.

Isotropic hardening model에서 plastic consistency condition은 plastic phase라면, stress가 yield stress여야 한다는 조건이다. 

$$ |\sigma| = \sigma_Y $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & |{}^{n+1}\sigma| = {}^{n+1}\sigma_Y \\ \Leftrightarrow \enspace & |{}^{tr}\sigma -\text{sgn}({}^{tr}\sigma) E \Delta\epsilon_p | = {}^n\sigma_Y + H\Delta\epsilon_p \\ \Leftrightarrow \enspace & |{}^{tr}\sigma| - E \Delta\epsilon_p  = {}^n\sigma_Y + H\Delta\epsilon_p \\ \Leftrightarrow \enspace & \Delta\epsilon_p = \frac{|{}^{tr}\sigma| - {}^n\sigma_Y}{H+E} \end{aligned}$$

결론적으로, stress와 yield stress는 다음과 같다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\sigma) \frac{E}{H+E}(|{}^{tr}\sigma| - {}^n\sigma_Y) \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y + \frac{H}{H+E}(|{}^{tr}\sigma| - {}^n\sigma_Y) \end{aligned} $$

위 식을 $\Delta\epsilon$으로 나타내기 위해 아래 그림을 고려해보자.

<p align = "center">
<img src = "./_image/2022.09.16_3.png">
</p>

위 그림과 같이 $\Delta \epsilon$중 순수하게 elastic 변형만 나타나는 부분의 비율을 $R$이라 하자.

$\triangle abc \sim \triangle dec$에 의해 다음이 성립한다.

$$ \begin{aligned} & \frac{(1-R)\Delta \epsilon}{\Delta \epsilon} = \frac{|{}^{tr}\sigma| - {}^{n}\sigma_Y}{|{}^{tr}\sigma - {}^{n}\sigma|} \\ \Leftrightarrow \enspace &  R = 1 - \frac{|{}^{tr}\sigma| - {}^{n}\sigma_Y}{|{}^{tr}\sigma - {}^{n}\sigma|} \end{aligned} $$

이를 활용하면 다음이 성립한다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\sigma)\frac{E}{H+E}(|{}^{tr}\sigma| - {}^n\sigma_Y) \\ &=  {}^{tr}\sigma -\text{sgn}({}^{tr}\sigma) \frac{E(1-R)}{H+E} |{}^{tr}\sigma - {}^{n}\sigma| \\ &= {}^{tr}\sigma -\text{sgn}({}^{tr}\sigma) \frac{E^2(1-R)}{H+E} |\Delta \epsilon| \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y  + \frac{HE(1-R)}{H+E} |\Delta \epsilon| \end{aligned} $$

### Kinematic hardening model
다음과 같은 값들이 주어졌다고 하자.
* elastic modulus $E$
* plastic modulus $H$
* $n$번째 load increment에서 stress ${}^n\sigma$
* $n$번째 load increment에서 back stress ${}^n\alpha$
* strain increment $\Delta \epsilon$

이 떄, ${}^{n+1}\sigma, {}^{n+1}\alpha$를 구하는 방법은 다음과 같다.

#### 1. Elasiticy assumption
$\Delta \epsilon$가 더해진 후에도 elastic영역이여서 $\Delta \epsilon_p = 0$이라 가정하자.

$\Delta \epsilon = \Delta \epsilon_e$임으로 elasticity 가정에 의한 trial stress와 trial back stress는 다음과 같다.

$$ \begin{aligned} {}^{tr}\sigma &= {}^n\sigma + E \Delta \epsilon \\ {}^{tr}\alpha &= {}^n\alpha \end{aligned}  $$

stress에서 back stress를 뺀 값을 `shifted stress` $\eta$라 할 떄, trial shifted stress는 다음과 같다.

$$ \begin{aligned} {}^{tr}\eta &= {}^{tr}\sigma - {}^{tr}\alpha \\ &= {}^n\sigma + E \Delta \epsilon - {}^n\alpha \end{aligned}  $$

#### 2. Checking yield condition
Elasticity 가정이 옳바른지 판단하기 위해, 현재 stress들이 yield condition을 만족하는지 확인한다.

Kinematic hardening model에서 yield condition은 shifted stress가 initial yield stress보다 항상 작거나 같아야 한다는 조건이다. 

$$ |\eta| \le {}^{0}\sigma_Y $$

따라서, 다음이 성립해야한다.

$$ \begin{equation} \begin{aligned} &|{}^{tr}\eta| \le {}^{0}\sigma_Y \\ \Leftrightarrow \enspace & |{}^n\sigma + E \Delta \epsilon - {}^n\alpha| \le {}^{0}\sigma_Y \end{aligned} \end{equation} $$

식(3)이 성립하는 경우 elastic assumption은 참이 되고, stress와 back stress는 다음과 같다.

$$ \begin{gathered} {}^{n+1}\sigma = {}^{tr}\sigma \\ {}^{n+1}\alpha = {}^{tr}\alpha \end{gathered}$$

#### 3. Plasticity correction
식(3)이 성립하지 않는 경우 ealstic assumption은 거짓이 되고 이는 $\Delta \epsilon_p \neq 0$이 되어 plastic strain에 대한 correction이 필요하게 된다. 

$$ \begin{gathered} {}^{n+1}\sigma = {}^{tr}\sigma + \sigma_c \\ {}^{n+1}\alpha = {}^{tr}\alpha + \alpha_c \end{gathered}$$

Plastic strain에 의한 strain-hardening 현상을 고려한 stress correction term $\sigma_c$와 back stress correction term $\alpha_c$는 다음과 같다.

$$ \begin{aligned} \sigma_c &= -\text{sgn}({}^{tr}\eta) E \Delta\epsilon_p \\  \alpha_c &= \text{sgn}({}^{tr}\eta) H \Delta\epsilon_p \end{aligned}  $$

따라서 stress와 back stress는 다음과 같다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\eta) E \Delta\epsilon_p \\ {}^{n+1}\alpha &= {}^{tr}\alpha + \text{sgn}({}^{tr}\eta) H \Delta\epsilon_p \end{aligned} $$

##### 참고
<p align = "center">
<img src = "./_image/2022.09.16_4.png" width = 400>
</p>

위 그림의 음영처리된 영역에 ${}^{tr}\sigma$값이 있는 경우, $\text{sgn}({}^{tr}\eta)$과 $\text{sgn}({}^{tr}\sigma)$의 부호가 반대가 된다.

이 현상은 elasticity region이 일정한 kinematic hardening model에서만 나타나며  correction term의 부호를 $\eta$로 결정하는 이유이기도 하다.

##### 3-1 Calculating plastic strain increment
$\Delta\epsilon_p$을 계산하기 위해 plastic consistency condition을 이용한다.

Kinematic hardening model에서 plastic consistency condition은 plastic phase라면, shifted stress가 initial yield stress여야 한다는 조건이다. 

$$ |\eta| = {}^{0}\sigma_Y $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & |{}^{n+1}\sigma - {}^{n+1}\alpha| = {}^0\sigma_Y \\ \Leftrightarrow \enspace & |{}^{tr}\sigma - {}^{tr}\alpha - \text{sgn}({}^{tr}\eta) (E + H) \Delta\epsilon_p | = {}^0\sigma_Y \\ \Leftrightarrow \enspace & |{}^{tr}\sigma - {}^{tr}\alpha| - (E+H) \Delta\epsilon_p  = {}^0\sigma_Y \\ \Leftrightarrow \enspace & \Delta\epsilon_p = \frac{|{}^{tr}\eta| - {}^0\sigma_Y}{H+E} \end{aligned}$$

결론적으로, Stress와 back stress는 다음과 같다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\eta) \frac{E}{H+E}(|{}^{tr}\eta| - {}^0\sigma_Y) \\ {}^{n+1}\alpha &= {}^{tr}\alpha + \text{sgn}({}^{tr}\eta) \frac{H}{H+E} (|{}^{tr}\eta| - {}^0\sigma_Y) \end{aligned} $$

### Combined hardening model
다음과 같은 값들이 주어졌다고 하자.
* elastic modulus $E$
* plastic modulus $H$
* $n$번째 load increment에서 stress ${}^n\sigma$
* $n$번째 load increment에서 back stress ${}^n\alpha$
* $n$번째 load increment에서 yield stress ${}^n\sigma_Y$
* strain increment $\Delta \epsilon$

이 떄, ${}^{n+1}\sigma, {}^{n+1}\alpha, {}^{n+1}\sigma_Y$를 구하는 방법은 다음과 같다.

#### 1. Elasiticy assumption
$\Delta \epsilon$가 더해진 후에도 elastic영역이여서 $\Delta \epsilon_p = 0$이라 가정하자.

$\Delta \epsilon = \Delta \epsilon_e$임으로 elasticity 가정에 의한 trial stress, trial back stress, trial yield stress는 다음과 같다.

$$ \begin{aligned} {}^{tr}\sigma &= {}^n\sigma + E \Delta \epsilon \\ {}^{tr}\alpha &= {}^n\alpha \\ {}^{tr}\sigma_Y &= {}^n\sigma_Y \end{aligned}  $$

Stress에서 back stress를 뺀 값을 shifted stress $\eta$라 할 떄, trial shifted stress는 다음과 같다.

$$ \begin{aligned} {}^{tr}\eta &= {}^{tr}\sigma - {}^{tr}\alpha \\ &= {}^n\sigma + E \Delta \epsilon - {}^n\alpha \end{aligned}  $$

#### 2. Checking yield condition
Elasticity 가정이 옳바른지 판단하기 위해, 현재 stress들이 yield condition을 만족하는지 확인한다.

Combined hardening model에서 yield condition은 shifted stress가 yield stress보다 항상 작거나 같아야 한다는 조건이다. 

$$ |\eta| \le \sigma_Y $$

따라서, 다음이 성립해야한다.

$$ \begin{equation} \begin{aligned} &|{}^{tr}\eta| \le {}^{tr}\sigma_Y \\ \Leftrightarrow \enspace & |{}^n\sigma + E \Delta \epsilon - {}^n\alpha| \le {}^{n}\sigma_Y \end{aligned} \end{equation} $$

식(3)이 성립하는 경우 elastic assumption은 참이 되고, stress, back stress yield stress는 다음과 같다.

$$ \begin{gathered} {}^{n+1}\sigma = {}^{tr}\sigma \\ {}^{n+1}\alpha = {}^{tr}\alpha \\ {}^{n+1}\sigma_Y = {}^{tr}\sigma_Y \end{gathered}$$

#### 3. Plasticity correction
식(3)이 성립하지 않는 경우 ealstic assumption은 거짓이 되고 이는 $\Delta \epsilon_p \neq 0$이 되어 plastic strain에 대한 correction이 필요하게 된다. 

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma + \sigma_c \\ {}^{n+1}\alpha &= {}^{tr}\alpha + \alpha_c \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y + {\sigma_Y}_c \end{aligned}$$

Plastic strain에 의한 strain-hardening을 고려한 stress correction term $\sigma_c$, back stress correction term $\alpha_c$, yield stress correction term ${\sigma_Y}_c$는 다음과 같다.

$$ \begin{aligned} \sigma_c &= -\text{sgn}({}^{tr}\eta) E \Delta\epsilon_p \\  \alpha_c &= \text{sgn}({}^{tr}\eta) \beta  H \Delta\epsilon_p \\ {\sigma_Y}_c &= (1 - \beta) H \Delta\epsilon_p \end{aligned}  $$

따라서 stress, back stress, yield stress는 다음과 같다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\eta) E \Delta\epsilon_p \\ {}^{n+1}\alpha &= {}^{tr}\alpha + \text{sgn}({}^{tr}\eta) \beta H \Delta\epsilon_p \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y +(1-\beta) H \Delta\epsilon_p \end{aligned} $$

##### 3-1. Calculating plastic strain increment
$\Delta\epsilon_p$을 계산하기 위해 plastic consistency condition을 이용한다.

Combined hardening model에서 plastic consistency condition은 plastic phase라면, shifted stress가 yield stress여야 한다는 조건이다. 

$$ |\eta| = \sigma_Y $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & |{}^{n+1}\sigma - {}^{n+1}\alpha| = {}^{n+1}\sigma_Y \\ \Leftrightarrow \enspace & |{}^{tr}\sigma - {}^{tr}\alpha - \text{sgn}({}^{tr}\eta) (E + \beta H) \Delta\epsilon_p | = {}^{tr}\sigma_Y +(1-\beta) H \Delta\epsilon_p \\ \Leftrightarrow \enspace & |{}^{tr}\sigma - {}^{tr}\alpha| - (E + \beta H) \Delta\epsilon_p  = {}^{tr}\sigma_Y +(1-\beta) H \Delta\epsilon_p \\ \Leftrightarrow \enspace & \Delta\epsilon_p = \frac{|{}^{tr}\eta| - {}^{tr}\sigma_Y}{H+E} \end{aligned}$$

결론적으로, Stress와 back stress는 다음과 같다.

$$ \begin{aligned} {}^{n+1}\sigma &= {}^{tr}\sigma -\text{sgn}({}^{tr}\eta) \frac{E}{H+E}(|{}^{tr}\eta| - {}^{tr}\sigma_Y) \\ {}^{n+1}\alpha &= {}^{tr}\alpha + \text{sgn}({}^{tr}\eta) \frac{H\beta}{H+E} (|{}^{tr}\eta| - {}^{tr}\sigma_Y) \\ {}^{n+1}\sigma_Y &= {}^{tr}\sigma_Y + \frac{H(1-\beta)}{H+E}(|{}^{tr}\eta| - {}^{tr}\sigma_Y) \end{aligned} $$

## Tangent stiffness matrix calculation
Stress calculation 과정에서 구한 elastoplastic tangent modulus를 `algorithmic tangent modulus` $D^\text{alg}$라고 한다.

식(2)를 이용해서 $D^\text{alg}$을 계산하면 다음과 같다.

$$ \begin{aligned} D^\text{alg} &= \frac{\partial \Delta \sigma}{\partial \Delta \epsilon} \\&= \frac{\partial {}^{n+1}\sigma}{\partial \Delta \epsilon} - \frac{\partial {}^{n}\sigma}{\partial \Delta \epsilon} \\&= \frac{\partial {}^{n+1}\sigma}{\partial \Delta \epsilon} \\&= \frac{\partial {}^{tr}\sigma}{\partial \Delta \epsilon} -\text{sgn}({}^{tr}\sigma) E \frac{\partial \Delta\epsilon_p}{\partial \Delta \epsilon} \end{aligned} $$

$\Delta\epsilon_p = 0$일 때, $D^\text{alg}$는 다음과 같다.

$$ D^\text{alg} = E $$

$\Delta\epsilon_p \neq 0$일 때, $\frac{\partial \Delta\epsilon_p}{\partial \Delta \epsilon}$는 다음과 같다.

$$ \begin{aligned} \frac{\partial \Delta\epsilon_p}{\partial \Delta \epsilon} &= \frac{1}{E + H} \frac{\partial}{\partial \Delta \epsilon} \Big( |{}^{tr}\sigma| - {}^{n}\sigma_Y \Big) \\&= \frac{1}{E + H} \frac{\partial}{\partial \Delta \epsilon} \Big( |{}^{n}\sigma + E \Delta \epsilon| - {}^{n}\sigma_Y \Big) \\ &=  \frac{E}{E + H} \text{sgn}({}^{tr}\sigma) \end{aligned} $$

따라서, $\Delta\epsilon_p \neq 0$일 때, $D^\text{alg}$는 다음과 같다.

$$ D^\text{alg} = E - \frac{E^2}{E + H} = E_t $$


