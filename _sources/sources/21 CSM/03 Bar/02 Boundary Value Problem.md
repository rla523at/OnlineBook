# Boundary Value Problem
1D Bar의 displacement based governing equation는 $u$의 2계 미분방정식임으로 이를 풀기 위해서는 두개의 `경계조건(boundary condition; BC)`이 필요하다.

다음과 같은 model을 고려해보자.

```{figure} _image/0201.png
```

그림에 나타난 model의 경계조건은 다음과 같다.

$$ \begin{aligned} x=0 \quad& u=0 \\ x=L \quad& \sigma_{xx}A = EA \frac{\partial u}{\partial x} = f \end{aligned} $$

이 때, Modeling domain을 $\Omega := [0,L] \subseteq \R$라고 하자.