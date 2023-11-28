# Curvature
## 정의
$\R$의 open set $U$와 $U$에서 differentiable한 $r : U \rightarrow \R^n$이 있다고 하자.

$r$의 unit tangent vector를 $T$라고 할 때, $r$의 curvature는 $k:U\rightarrow\R$는 다음과 같이 정의한다.

$$ k:= \norm{\diff{T}{s}} $$

### 참고1

```{figure} _image/0401.png
```

곡선이 얼마나 빠르게 변하는지를 나타내는 곡률은 위 그림에서 볼 수 있듯이, 주변점의 tangent vector가 방향을 얼마나 빠르게 변하는지로 알 수 있다.

따라서, tangent vector의 방향만을 고려하기 위해서 unit tangent vector를 사용한다.

### 참고2
Parameter에 따른 tangent vector 방향의 변화율를 curvature의 정의로 할 경우 parameter에 depend하는 문제가 발생한다.

예를들어, 두 함수 $r,s$를 다음과 같이 정의하자.

$$ \begin{gathered} r(t) = (t,t^2) \\ s(u) = (e^u,e^{2u}) \end{gathered} $$

$t \in (1,2), u \in (0, \ln2)$일 때, 두 함수 모두 같은 parametric curve를 표현한다.

이 때, $r$에서 $\norm{\diff{T}{t}}$와 $s$에서 $\norm{\diff{T}{u}}$를 계산하면 다음과 같다.

$$ \begin{gathered} \norm{\diff{T}{t}} = \frac{1}{1+4t^2}\sqrt{64t^2+ (2-8t^2)^2} \\ \norm{\diff{T}{u}} = \frac{1}{1 +4e^{2u}}\sqrt{64e^{4u}+e^{2u}(1-4e^{2u})^2} \end{gathered} $$

동일한 점 $t=1,s=0$에서 계산하면 다음과 같다.

$$ \begin{gathered} \norm{\diff{T}{t}}_{t=1} = 2 \\ \norm{\diff{T}{u}}_{u=0} = \frac{\sqrt{73}}{5} \end{gathered} $$

동일한 curve의 같은 점에서 서로 다른 곡률값이 나오게 된다.

즉, 어떤 parameter를 쓰냐에 따라 곡률이 달라지는 문제가 생긴다.

따라서, parameter independent한 arc length function $s$에 따른 tangent vector 방향의 변화율을 $k$의 정의로 사용한다.