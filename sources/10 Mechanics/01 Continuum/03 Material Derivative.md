# Material Derivative
물리량을 나타내는 tensor $\phi$가 다음과 같이 Lagrangian관점과 Eulerian관점으로 표현되었다고 하자.

$$ \phi = f(X, t) = g(x, t) $$

이 떄, 연속체 한 점의 물리량이 시간에 따라 변하는 정도를 `물질미분(material derivative)`이라고 한다.

$$ \pdiff{f(X,t)}{t} $$

Eulerian 관점은 deformation $\varphi$을 이용해서 Lagragian 관점으로 변환할 수 있음으로, Eulerian 관점 함수의 material derivative는 다음과 같다.

$$ \begin{aligned}
\pdiff{g(x,t)}{t} \bigg |_{X=\text{const}} &= \pdiff{g(\varphi(X,t),t)}{t} \\
                                           &= \pdiff{g}{\varphi} \pdiff{\varphi}{t} + \pdiff{g}{t} \\
                                           &= v_m \nabla g + \pdiff{g}{t} 
\end{aligned} $$

이 때, Eulerian 관점 함수의 material derivative를 간단하게 $D/Dt$로 표현한다.

$$ \frac{Dg(x,t)}{Dt} \equiv \pdiff{g(x,t)}{t} \bigg |_{X=\text{const}} = \pdiff{g(\varphi(X,t),t)}{t} $$

위를 종합하면 다음과 같다.

$$ \pdiff{f(X,t)}{t} = \frac{Dg(x,t)}{Dt} = v_m \nabla g  + \pdiff{g}{t} $$

> Reference    
> {cite}`lai` Chapter3.3  