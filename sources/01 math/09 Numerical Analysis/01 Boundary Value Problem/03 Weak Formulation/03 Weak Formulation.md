# Weak Formulation
WRF의 왼쪽 첫번째 항에 부분적분법을 적용하면 다음과 같다.  

$$ \text{find } u \in \mathcal U \quad s.t. \quad \forall w \in C^\infty_c(\Omega), \quad \int_\Omega a\frac{dw}{dx}\frac{du}{dx} + wcu \thinspace dV = wa\frac{du}{dx} \bigg|_{x = 0}^L + \int_\Omega wf \thinspace dV $$

Natural BC를 적용하면 다음과 같다.  

$$ \text{find } u \in \mathcal U \quad s.t. \quad \forall w \in C^\infty_c(\Omega), \quad \int_\Omega a\frac{dw}{dx}\frac{du}{dx} + wcu \thinspace dV = w(L)Q_L - wa\frac{du}{dx} \bigg|_{x = 0} + \int_\Omega wf \thinspace dV  $$

## Test Function Space Relaxation
Test funtion space를 살펴보자.

$\forall w \in C^\infty_c(\Omega)$에서 $w(L)=0$임으로 natural BC를 적용할 수 없게 된다.

따라서, natural BC를 equation에 적용하기 위해서 natrual BC에서는 0이 아니고 essential BC에서만 0이 되어야 한다.

따라서, test function space의 요구조건을 완화해서 $C^\infty_c(\Omega)$를 확장한 $\mathcal{W_{relax}}$는 다음과 같이 정의된다.

$$ \mathcal{W_{relax}} := \Set{w \in C^\infty(\Omega) | w = 0 \text{ on } \partial\Omega_E} $$

확장된 후에도, 여전히 $C^\infty_c(\Omega) \subseteq \mathcal{W_{relax}}$임으로 strong formulation과 동치이다.

## Solution Function Space Relaxation
부분 적분법에 의해 미분항이 $w$로 옮겨갔음으로 solution을 $C^2(\Omega)$가 아닌 $C^1(\Omega)$에서 찾을 수 있다. 

또한, weak formulation에 natural BC가 적용되어 있음으로 solution function space에서 natural BC와 관련된 요구조건도 완화할 수 있다.

따라서, Solution function space의 요구조건을 완화해서 $\mathcal U$를 확장한 $\mathcal{U_{relax}}$는 다음과 같이 정의된다.

$$ \mathcal{U_{relax}} := \Set{u \in C^1(\Omega) | u \text{ satisfies essential BC}} $$

## Weak Form
Test function space와 solution function space를 relaxation하여 얻은 weak form은 다음과 같다.

$$ \text{find } u \in \mathcal{U_{relax}} \quad s.t. \quad \forall w \in \mathcal{W_{relax}}, \quad \int_\Omega a\frac{dw}{dx}\frac{du}{dx} + wcu \thinspace dV = w(L)Q_L + \int_\Omega wf \thinspace dV$$

### 참고1
Test function space의 모든 원소가 $\partial\Omega_E$에서 $0$임으로, $- wa\frac{du}{dx} \bigg|_{x = 0}$항이 없어졌다.

### 참고2
Weak formulation이라고 불리는 이유는 바로 test function space와 solution function space의 regularity 요구사항을 약화시켰기 때문이다. 

### 참고3
$\mathcal{U_{relax}}$는 $\mathcal U$보다 더 약한 regularity를 요구하기 때문에 strong formulation과 동치가 아니며 weak formulation이 보다 더 일반적인 형태이다. 

예를들어, weak formulation의 해가 $u \notin C^2(\Omega)$이면 strong formulation은 정의조차 되지 않는다.

> Reference  
> [Note - M. J. Zahr](https://mjzahr.github.io/content/ame40541/spr20/ch03-wres-solo.pdf)

### 참고4
Solution function space에 natural BC를 적용할 경우 반드시 natural BC를 만족하는 해를 구하게 된다. 따라서 이를 강하게 natural BC를 적용하였다고 표현한다. 

하지만 weak formulation에서는 natural BC를 식에 적용하고 그 대신에 solution function space의 제약조건을 약화시켰기 때문에 약하게 적용되었다고 표현한다. 

## Functional Form
Weak formulation은 functional $B,l$을 이용해 다음과 같이 간단하게 나타낼 수 있다.

$$ \text{find } u \in \mathcal{U_{relax}} \quad s.t. \quad \forall w \in \mathcal{W_{relax}}, \quad B(w,u) = l(w) $$

$$ \begin{gathered} \text{Where, } B(w,u) :=  \int_\Omega a\frac{dw}{dx}\frac{du}{dx} + wcu \thinspace dV \\ \quad l(w) := w(L)Q_L + \int_\Omega wf \thinspace dV \end{gathered} $$

> Reference  
> [Note - M. J. Zahr](https://mjzahr.github.io/content/ame40541/spr20/ch03-wres-solo.pdf)