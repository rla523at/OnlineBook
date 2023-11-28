# Strong formulation
`경계값 문제(boudnary value problem; BVP)`가 다음과 같이 주어졌다고 하자.  

$$ \text{find } u \in \mathcal U \st \mathcal P(u) + f(\mathbf x) = 0 \quad \text{in } \Omega \subset \R^d$$

$$ \text{Where, } \mathcal U := \{ u \in C^m(\Omega) \enspace | \enspace u \text{ satisfies boundary condition on } \partial\Omega \} $$

이 때, $\mathcal P : C^m(\Omega) \rightarrow C^0(\Omega)$는 `계수(order)`가 $m$인 `미분 연산자(differential operator)`이며 $\mathcal U$는 `solution funtion space`이다.

`경계조건(boundary condition; BC)`은 $\partial\Omega_E \subset \partial\Omega$따라 essential BC와 $\partial\Omega_N \subset \partial\Omega$따라 natural BC가 주어져있으며 $\partial\Omega = \partial\Omega_E \cup \partial\Omega_N$ 이다.

식(1)을 PDE의 `strong formulation`이라 하는데 이는 strong formulation의 해가 $\Omega$의 모든 점에서 PDE를 만족시켜야하며 $m$번 미분 가능해야된다는 엄격한 `정규성(regularity)`을 만족시켜야하기 때문이다.

> Reference    
> [Function space - Wiki](https://en.wikipedia.org/wiki/Function_space#Functional_analysis)