# Differential Geometry

## Local Coordinates
Manifold $M$ 이 있고 chart $\phi: U \subset M \rightarrow \R^n$ 이 있을 때, $p \in U$ 에 대해서 $\phi(p)$ 를 $p$ 의 local coordinate 라고 한다.

참고로, local coordinate 는 chart 에 의존하는 값이기 때문에 chart 가 달라지면 local coordinate 도 달라진다. 

정의역이 겹치는 두 차트 $\phi, \psi$ 에 대해 전이함수(transition map) $\psi \circ \phi^{-1} : \phi(U \cap V) \rightarrow \R^n$ 가 잘 정의되어 서로 다른 local coordinates 는 오버랩 영역에서 전이 함수로 연결된다.

## Coordinate Function
Manifold $M$ 이 있고 chart $\phi: U \subset M \rightarrow \R^n$ 이 있을 때, Coordinate Function $x^i$ 는 chart 의 결과에 $i$ 번째 좌표값을 반환하는 함수이다.
$$
\forall p \in U, \quad  x^i(p) = \pi_i(\phi(p))
$$

## Coordinate Curve
Manifold $M$ 이 있고 chart $\phi: U \subset M \rightarrow \R^n$ 이 있을 때, $p \in U$ 에서 Coordinate Curve $\gamma_i|_p(t)$ 는 다음과 같이 정의된 $\R^n$ 의 curve $r_i(t)$ 를 $\phi^{-1}$ 로 옮겼을 때, $M$ 위에서의 curve 이다.

$$
\begin{gathered}
r_i(t) = \phi(p) + (0, \cdots, t, \cdots, 0) = (x^1(p), \cdots, x^i(p)+t, \cdots, x^n(p)) \\
\gamma_i|_p(t) = \phi^{-1}(r_i(t))  
\end{gathered} 
$$

즉, $\gamma_i|_p(t)$ 는 $\phi(p)$ 에서 $i$ 번째 좌표만 $t$ 만큼 바뀌어서 그려지는 curve 를 $M$ 위로 다시 가져왔을 떄 그려지는 Curve 이다.

### Example
* **대상 공간**: 단위 2-구 $S^2 \subset \mathbb{R}^3$
* **지도(chart)**: 북극 스테레오그래픽 투영 (북극 $(0,0,1)$에서 $\mathbb{R}^2$로 투영)
* **점**: $p = \left(\frac{1}{2}, \frac{1}{2}, \frac{1}{\sqrt{2}}\right) \in S^2$

1. **스테레오그래픽 투영 정의**
   북극 $(0,0,1)$에서 $S^2 \setminus \{(0,0,1)\} \to \mathbb{R}^2$로의 투영:

   $$
   (x, y, z) \mapsto \left( \frac{x}{1 - z}, \frac{y}{1 - z} \right)
   $$

2. **역 투영 (그래프화에 필요)**:

   $$
   (u, v) \mapsto \left( \frac{2u}{u^2 + v^2 + 1}, \frac{2v}{u^2 + v^2 + 1}, \frac{u^2 + v^2 - 1}{u^2 + v^2 + 1} \right)
   $$

![Stereographic Coordinate Curves](https://www.wolframcloud.com/obj/93973454-f92f-4635-9e7f-1654283375a4)

* 🔴 **빨간 곡선**: $x$-좌표 곡선 — $y$ 고정, $x$ 변화
* 🔵 **파란 곡선**: $y$-좌표 곡선 — $x$ 고정, $y$ 변화
* 🌐 **구형 반투명 도형**: 단위 구 $S^2$
