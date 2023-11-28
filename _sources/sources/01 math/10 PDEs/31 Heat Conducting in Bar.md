# Heat Conducting in Bar
## Modeling
bar의 축방향으로는 heat conducting이 발생하고 각 단면 내에서는 heat conducting이 없는 bar가 있다고 하자.

$q$를 heat flux, $g$를 generation rate라고 하면 bar의 한단면을 다음과 같이 모델링 할 수 있다.  

```{figure} _image/3101.png
```

이 떄, $c$를 specific heat rate, $\rho$를 density라고하면 conservation law of energy에 의해 다음과 같은 식을 얻을 수 있다.

$$ (Aq)(x,t) + \int_x^{x+\Delta x} Ag \thinspace dx  - (Aq)(x+\Delta x,t) = \int_x^{x+\Delta x} \rho A c \pdiff{T}{x} \thinspace dx  $$

$\Delta x$가 충분히 작아 $x$에 대한 함수들을 전부 선형 근사할 수 있다고 가정하면 명제1에 의해 다음과 같이 식이 간단해진다.

$$ \rho A c \pdiff{T}{x} + \frac{\partial(Aq)}{\partial x} = Ag $$

이 떄, $\kappa$를 thermal conductivity라고 하면 Fourier의 heat conduction law에 의해 다음 관계식이 성립한다.

$$ q = -\kappa\pdiff{T}{x} $$

이를 식에 대입하면 다음과 같은 heat equation을 얻을 수 있다.

$$ \rho A c \pdiff{T}{x} =  \frac{\partial}{\partial x}\left(A\kappa\pdiff{T}{x}\right) + Ag $$


> Reference
> {cite}`powers` chapter2

### 참고1
$g$는 표면을 통해 heat radiation이나 heat convection이 일어나는 경우 또는 전기,화학적 작용으로 단면 내에서 heat이 발생하는 경우등을 고려하는 값이다. 

경우에 따라서 $g$는 $x,t$ 또는 $T$의 함수이며 $g$의 단위는 $W/m^3$으로 단위 부피, 단위 시간당 발생하는 열에너지이다.


> Reference
> {cite}`powers` chapter2.1

### 참고2
$q$의 단위는 $W/m^2=J/sm^2$로 단위 면적$(m^2)$, 단위 시간$(s)$당 전달되는 에너지$(J)$이다.

따라서, $Aq$는 단위시간당 (열에 의해) 전달되는 energy를 나타낸다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Heat_flux)

### 참고3
$c$의 단위는 $J/Kkg$로 단위 온도$(K)$, 단위 질량$(kg)$당 흡수하는 에너지$(J)$이다.

그럼으로 $A\Delta x \rho c$는 단위 온도변화당 물체가 흡수하는 energy를 나타내고, $\pdiff{T}{t}$는 단위 시간당 온도변화를 나타낸다.

따라서 $A\Delta x \rho c \pdiff{T}{x}$는 단위 시간당 (온도변화에 의해) 물체가 흡수하는 energy를 나타낸다.


### 명제1
다음과 같이 식이 주어졌다고 하자.

$$ (Aq)(x,t) + \int_x^{x+\Delta x} Ag \thinspace dx  - (Aq)(x+\Delta x,t) = \int_x^{x+\Delta x} \rho A c \pdiff{T}{x} \thinspace dx  $$

$\Delta x$가 충분히 작아 선형 근사할 수 있을 때, 위 식을 선형근사하여 정리하면 다음과 같음을 증명하여라.

$$ \rho A c \pdiff{T}{x} + \frac{\partial (Aq)}{\partial x} = Ag $$


**Proof**

이 때, $\Delta x$가 충분히 작아 선형 근사할 수 있다고 하면, 임의의 $f(x)$에 대해 다음이 성립한다.

$$ \begin{aligned} f(x+\Delta x) &= f(x) + \frac{\partial f}{\partial x}\Delta x \\ \int_x^{x+\Delta x} f\thinspace dx &= F(x+\Delta x) - F(x) \\&= F(x) + \frac{\partial F}{\partial x} \Delta x - F(x) \\&= f\Delta x \end{aligned} $$

이를 원래식에 대입하면 다음이 성립한다.

$$ Ag\Delta x - \frac{\partial (Aq)}{\partial x}\Delta x = \rho A c \pdiff{T}{x} \Delta x $$

양변을 $\Delta x$로 나눠주고 정리하면 다음과 같다.

$$ \rho A c \pdiff{T}{x} + \frac{\partial (Aq)}{\partial x} = Ag \qed $$

## Simplify
bar의 축을 따라 $A,\kappa$가 일정하고 $g=0$인 bar에서는 heat equation이 다음과 같이 간단해 진다.

$$ \frac{\rho c}{\kappa} \pdiff{T}{x} = \frac{\partial^2 T}{\partial x^2} $$

이 때, thermar diffusivity $k$를 다음과 같이 정의하자.

$$ k := \frac{\kappa}{\rho c} $$

그러면 heat equation을 다음과 같이 표현할 수 있다.

$$ \frac{1}{k} \pdiff{T}{x} = \frac{\partial^2 T}{\partial x^2} $$

