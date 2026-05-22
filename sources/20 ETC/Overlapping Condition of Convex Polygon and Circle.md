# ConvexPolygon 과 원의 겹침 판단

$n$ 개의 Point 로 정의된 ConvexPolygon $CP$ 가 있고 각 Point 의 Index 가 반시계 방향으로 주어졌다고 하자.

$P_i$ 에서 $P_{i+1}$ 로가는  edge vector $e_i$ 라고 하자.

## Convex Polygon 과 원이 겹치지 않을 조건

원의 중심점 $C$ 와 반지름 $r$ 이 주어졌을 때, $CP$ 와 겹치지 않으려면 $CP$ 와 $C$ 의 최단거리를 $m$ 이라고 할 때 다음을 만족해야 한다.

$$ (C \notin CP) \land (r < m) $$

즉 원의 중심점이 Convex Polygon 의 외부에 있어야하고 반지름보다 더 멀리 떨어져 있어야 한다. 

## Convex Polygon 의 외부에 있을 조건

$e_i$ 를 direction vector 로 갖는 직선과 임의의 점을 $A$ 라고 하자.

$P_i$ 에서 $A$ 로가는 벡터를 $v_i$ 라고 하고 $e_i$ 와 수직이면서 CP 의 바깥방향을 가르키는 벡터를 $n_i$ 라고 하면 직선과 $A$ 를 포함하는 평면에서 $A$ 가 직선의 우측면에 있을 조건은

$$ v_i \cdot n_i > 0 $$

이다. 이 때, $n_i = (e_{i,y}, -e_{i,x})$ 임으로 $v_i \cdot n_i$ 는

$$ v_{i,x}e_{i,y} - v_{i,y}e_{i,x} $$

가 된다.

$CP$ 의 외부 영역은 각 edge vector 를 direction vector 로 갖는 모든 직선의 우측면의 합집합이기 때문에 원의 중심점 $C$ 가 $CP$ 의 외부에 있으려면 $P_i$ 에서 $C$ 로가는 벡터를 $v_i$ 라고 할 때 다음을 만족해야 한다.

$$ \exist i \in [0,n-1] \quad s.t \quad v_i \cdot n_i >0 $$

즉, 하나의 edge 에서라도 $v_i \cdot n_i > 0$ 이어야 $C$ 는 $CP$ 외부의 영역에 위치하게 된다.

## Convex Polygon 과의 최단 거리

$CP$ 와 $C$ 의 최단거리를 찾는 일은 각 Point 로 정의된 $n$ 개의 선분 $\overline{P_0P_1}, \cdots, \overline{P_{n-1}P_0}$ 과 $C$ 의 최단거리들 중에 최솟값을 찾는 것과 같다.

선분 $\overline{P_iP_{i+1}}$ 과 $C$ 의 최단거리를 구하는건  직선 $\overleftrightarrow{P_iP_{i+1}}$ 위의 점 중 $C$ 와 가장 가까운 점 $H$의 위치에 따라 달라진다.

$H = P_i + te_i$ 라고 표현하면 $\overline{P_iP_{i+1}}$ 과 $C$ 의 최단거리 $m$ 은

$$ m = \begin{cases}
P_i \text{에서 } C\text{까지의 거리} & t \le 0 \\
H \text{에서 } C\text{까지의 거리} & 0 < t < 1 \\
P_{i+1} \text{에서 } C\text{까지의 거리} & t \ge 1
\end{cases} $$

이 때, $t e_i$ 는 $v$ 를 $e$ 로 projection 시킨 vector 임으로

$$ te_i = \frac{v \cdot e}{(e \cdot e)^2} e_i $$

이다.


따라서, 선분 위의 점 중 $C$ 와 가장 가까운점을 $\bar{H}$ 라고 하면 

$$ \bar{H} = P_i + \operatorname{clamp}(t,0,1)e_i $$

가 됨으로 $\bar{H}$ 에서 $A$ 로 가는 vector 를 $h$ 라고 하면

$$ h = v - \operatorname{clamp}(t,0,1)e_i $$

이고 $m = \lVert h \rVert$ 다. 

이 때, $r,m$ 모두 양수이기 때문에 $r < m \iff r^2 < m^2$  임으로 불필요한 $sqrt$ 연산을 줄이기 위해 제곱값을 구한다.

$$ m^2 = h \cdot h $$

## $e_i$ 와 $1 / (e_i \cdot e_i)^2$ 미리 계산하기

동일한 $CP$ 에 대해서 여러번 계산함으로 $CP$ 가 결정되면 정해지는 값들인 $e_i$ 와 $1 / (e_i \cdot e_i)^2$ 는 미리 계산해놓으면 연산량을 줄일 수 있다.

$e_i$ 는 다음과 같이 계산할 수 있다.

$$ e_i = (P_{i+1,x} - P_{i,x}, P_{i+1,y} - P_{i,y} ) $$

$1 / (e_i \cdot e_i)^2$ 는 역수형태를 미리 계산하여서 실제 계산할 때에는 무거운 나눗셈이아니라 곱셈으로 연산할 수 있게 한다.
