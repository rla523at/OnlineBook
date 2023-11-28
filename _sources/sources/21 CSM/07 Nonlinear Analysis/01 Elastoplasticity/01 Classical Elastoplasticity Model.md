# Classical Elastoplasticity Model
Classical elastoplasticity model에서는 다음과 같은 가정을 한다.

## 1.Additive decomposition
Small deformation의 경우 strain을 elastic part와 plastic part의 합으로 표현된다고 가정한다. 
$$ \epsilon = \epsilon^e + \epsilon^p $$

## 2. Linear elastic 
$\sigma$와 $\epsilon^e$가 선형 탄성관계를 만족한다고 가정한다.

그러면 strain energy density function $W$가 존재해서 다음 관계를 만족한다.
$$ \begin{aligned} \sigma(x,t) &= \frac{\partial W(x, \epsilon^e(x,t))}{\partial \epsilon^e} \\&= C \epsilon^e \end{aligned}  $$

이 떄, $C$는 linear elastic stiffness tensor이다.

## 3. Flow rule
비가역적인 plastic strain의 시간에 대한 변화율이 flow rule을 따른다고 가정한다.
$$ \frac{\partial \epsilon^p}{\partial t} = \gamma r(\sigma, q) $$

## 4. Hardening rule
Internal plastic variables의 시간에 대한 변화율이 hardening rule을 따른다고 가정한다.
$$ \frac{\partial q}{\partial t} = \gamma h(\sigma, q) $$

> Reference  
> [Book] (Simo & Hughes) Computational Inelasticity chap 2.2.2