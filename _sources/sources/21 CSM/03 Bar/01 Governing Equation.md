# Governing Equation
단위길이당 분포하중 $p(x)$가 작용하고 단면적이 $A(x)$인 bar가 있다고 하자.

```{figure} _image/0101.png
```

## Deriving Governing Equation
Bar의 한 단면을 잘라 free body diagram을 그리면 다음과 같다.

```{figure} _image/0102.png
```

Force equilibrium equation은 다음과 같다.

$$ (\sigma A)(x+\Delta x) - (\sigma A)(x) + \int_x^{x+\Delta x} p\thinspace dx = 0 $$

이 때, $\Delta x$가 충분히 작아 선형 근사할 수 있다고 하면 명제1에 의해 다음과 같이 간단해진다.

$$ \frac{\partial}{\partial x}(\sigma A) + p = 0 $$

### 명제1
다음과 같이 식이 주어졌다고 하자.

$$ (\sigma A)(x+\Delta x) - (\sigma A)(x) + \int_x^{x+\Delta x} p\thinspace dx = 0 $$

$\Delta x$가 충분히 작아 선형 근사할 수 있을 때, 위 식을 선형근사하여 정리하면 다음과 같음을 증명하여라.

$$ \frac{\partial}{\partial x}(\sigma A) + p = 0 $$

**Proof**

이 때, $\Delta x$가 충분히 작아 선형 근사할 수 있다고 하면 다음이 성립한다.

$$ \begin{aligned} (\sigma A)(x+\Delta x) &= (\sigma A)(x) + \frac{\partial}{\partial x}(\sigma A)\Delta x \\ \int_x^{x+\Delta x} p\thinspace dx &= P(x+\Delta x) - P(x) \\&= P(x) + \frac{\partial P}{\partial x} \Delta x - P(x) \\&= p\Delta x \end{aligned} $$

이를 원래식에 대입하면 다음이 성립한다.

$$ \frac{\partial}{\partial x}(\sigma A)\Delta x + p\Delta x = 0 $$

양변을 $\Delta x$로 나눠주면 다음과 같다.

$$ \frac{\partial}{\partial x}(\sigma A) + p = 0 \qed $$

## Displacement Based Governing Equation
Displacement를 $u$라고 할 때, stress-strain-displacement relation은 다음과 같다.

$$ \sigma = E\pdiff{u}{x} $$

Governing equation에 stress-strain-displacement relation을 적용하면 다음과 같다.

$$ \pdiff{}{x}\left(EA\pdiff{u}{x}\right) + p = 0 $$

이 식이 1D bar의 `displacement based governing equation`이다.