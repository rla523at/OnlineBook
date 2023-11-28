# FE Formulation
## Finite Elements
$\Omega$의 subset collection $\Omega_E$를 다음과 같이 정의하자.

$$ \Omega_E := \Set{\Omega_i\subseteq\Omega | i \in [1,N_E]} $$

$\Omega_E$는 $\Omega$를 근사하기 위한 collection으로 `유한요소집합(finite element set)`이라고 한다.

$$ \Omega \approx \bigcup \Omega_E $$

이 때, $\Omega_i \in \Omega_E$를 `요소(element)`라고 부르며 element는 다음 성질을 만족한다.

$$ \begin{gathered} \forall \Omega_i \in \Omega_E, \quad \Omega_i \neq \empty \\ \forall \Omega_j,\Omega_k \in \Omega_E, \quad \Omega_j \neq \Omega_k \implies \Omega_j \cap \Omega_k = \empty \end{gathered} $$

### 참고
$\Omega = \bigcup\Omega_E$를 만족하면, $\Omega_E$는 $\Omega$의 `분할(partition)`이 된다.

## BVP on FEs

$\Omega$를 $\Omega_E$로 근사했을 떄, 다음과 같다고 하자.

```{figure} _image/0301.png
```

$\Set{x^i}$를 $\Omega_i$의 절점들의 set이라고 하자.

```{figure} _image/0302.png
```

$f^{int}_{i,j}$는 $i$ element가 $j$ element에 가하는 내력(internal force)일 때, $x^i_{1,2}$에 가해지는 force $f^i_{1,2}$를 다음과 같이 정의하자.

$$ f^i_1 = \begin{cases} 0 & i=1 \\ f^{int}_{i,i-1}(x^i_1) & i=2,\cdots,N_E \end{cases}, \enspace f^i_2 = \begin{cases} f^{int}_{i,i+1}(x^i_2) & i=1,\cdots,N_E-1 \\ f^{ext} & i=N_E \end{cases} $$

이 떄, $x^1_1$은 기존문제에서 essential BC가 주어져있는 절점이기 때문에 별도의 natural BC를 줄 필요가 없어 $f^1_1=0$으로 정의한다.

기존의 문제를 Element 단위로 나눠 free body diagram을 그리면 다음과 같다.

```{figure} _image/0303.png
```

Function space $\mathcal{U^i}$를 다음과 같이 정의하자.

$$ \mathcal{U^i} := \Set{u \in C^2(\Omega_i) | u \text{ satisfies BCs on } \partial\Omega_i} $$

이 떄, $\Omega_i$에서 BVP는 다음과 같다.

$$ \text{find } u \in \mathcal{U^i} \st \pdiff{}{x}\left(EA\pdiff{u}{x}\right) + p = 0 $$

Natural BC는 다음과 같다.

$$ x= x^i_j, \enspace EA\pdiff{u}{x} = f^i_j, \enspace j=1,2 $$

먄약 $i=1$이면 essential BC가 추가된다.

$$ x = x^1_1, \enspace u=0 $$

이로써 기존의 문제가 서로 다른 BC를 갖는 $N_E$개의 BVP로 나뉘게 된다.

### Internal force
$f^{int}_{i,j}$는 $i$ element가 $j$ element에 가하는 내력이다.

$$ f^{int}_{i,j} : \partial\Omega_i \cap \partial\Omega_j \rightarrow \R $$

$f^{int}_{i,j}$는 Newton's 3rd law of motion에 의해 다음을 만족한다.

$$ \forall x \in \partial\Omega_i \cap \partial\Omega_j, \quad f^{int}_{i,j}(x) + f^{int}_{j,i}(x) = 0 $$


## Weak Formulation on FEs
Function space $\mathcal{U^i_{relax}}, \mathcal{W^i_{relax}}$를 다음과 같이 정의하자.

$$ \begin{gathered} \mathcal{U^i_{relax}} := \Set{ u \in C^1(\Omega_i) | u \text{ satisfies essential BCs}} \\ \mathcal{W^i_{relax}} := \Set{ w \in C^\infty(\Omega_i) | w = 0 \text{ on } \partial\Omega_E} \end{gathered} $$

Functional $B^i,l^i$를 다음과 같이 정의하자.

$$ \begin{gathered} B^i(w,u) = \int_{\Omega_i} \pdiff{w}{x}EA\pdiff{u}{x} \thinspace dV \\ l^i(w) := \int_{\Omega_i} pw \thinspace dV + f^i_1w(x^i_1) + f^i_2w(x^i_2) \end{gathered} $$

이 떄, $\Omega_i$에서 weak formulation은 다음과 같다.

$$ \text{find } u \in \mathcal{U^i_{relax}} \st \forall w \in \mathcal{W^i_{relax}}, \quad B^i(w,u) = l^i(w) $$

### Dimensional reduction
Finite function space $\mathcal{U^i_f},\mathcal{W^i_f}, \R^k_s$를 다음과 같이 정의하자.

$$ \begin{gathered} \mathcal{U^i_f} := \span(\Set{u_1,\cdots,u_k}) \subset C^1(\Omega_i) \\ \mathcal{W^i_f} := \span(\Set{w_1,\cdots,w_k}) \subset C^\infty(\Omega_i) \\ \R^k_s := \Set{a \in \R^k | a^ju_j \text{ satisfy essential BCs}} \end{gathered} $$

정의에 의해 $B^i$는 bilinear map임으로 $\Omega_i$에서 dimensional reduced weak formulation은 다음과 같다.

$$ \text{find } a \in \R^n_s \st B^i(w_j,u_m)a^m = l^i(w_j) \enspace j=1,\cdots,k $$

### Bodunov-Galerkin method
$x^i_{1,2}$를 approximate point로 갖는 shape function을 $n^i_{1,2}$라 하자.

$u_j = n^i_j$로 두고, Bodunov-Galerkin method를 사용하면 다음과 같다.

$$ \text{find } a \in \R^n_s \st B^i(n^i_j,n^i_m)u^i_m = l^i(n^i_j) \enspace j=1,2 $$

이 때, $u^i_m = u(x^i_m)$이다.

이를 matrix form으로 나타내면 다음과 같다.

$$ \begin{bmatrix} B^i(n^i_1,n^i_1) & B^i(n^i_1,n^i_2) \\ B^i(n^i_2,n^i_1) & B^i(n^i_2,n^i_2) \end{bmatrix} \begin{bmatrix} u^i_1 \\ u^i_2 \end{bmatrix} = \begin{bmatrix} l^i(n^i_1) \\ l^i(n^i_2) \end{bmatrix} $$

$$ K^iu^i = f^i $$

이 떄, shape function의 성질에 의해 다음이 성립한다.

$$ f^i = \begin{bmatrix} \int_{\Omega_i} Pn^i_1\thinspace dV + f^i_1 \\ \int_{\Omega_i} Pn^i_2\thinspace dV + f^i_2 \end{bmatrix} $$

#### 참고
$B^i$의 정의에 의해 $K^i$는 symmetric matrix이다.

## Assemblying
각각의 element에서 weak formulation을 구하게 되면 총 $N_E$개의 matrix equation을 얻을 수 있다.

이를 전부 더해주면 $u^i_1 = u^{i-1}_2, \enspace i=2,\cdots,N_E$이고 $u^i_2 = u^{i+1}_1, \enspace i=1,\cdots,N_E-1$임으로 다음과 같이 정리된다.

$$ \begin{bmatrix} K^1_{11} & K^1_{12} && \\ K^1_{21} & K^1_{22} + K^2_{11} & K^2_{12} &&0 \\  & K^2_{21} & K^2_{22} + K^3_{11}  \\ &&& \ddots \\ &0&&& K^{N_E-1}_{22} + K^{N_E}_{11} & K^{NE}_{12} \\ &&&& K^{N_E}_{21} & K^{N_E}_{22} \end{bmatrix} \begin{bmatrix} u^1_1 \\ u^1_2 = u^2_1 \\ \vdots \\ u^{N_E-1}_2 = u^{N_E}_1 \\ u^{N_E}_2 \end{bmatrix} = \begin{bmatrix} f^1_1 \\ f^1_2 + f^2_1 \\ \vdots \\ f^{NE-1}_2 + f^{N_E}_1 \\ f^{N_E}_2 \end{bmatrix} $$

$$ K d = f $$

이 떄, $f^{int}$의 성질에 의해 미지수이던 내력들이 전부 상쇄되어 다음이 성립한다.

$$ f = \begin{bmatrix} \int_{\Omega_1}Pn^1_1 \thinspace dV \\ \int_{\Omega_1}Pn^1_2 \thinspace dV + \int_{\Omega_2}Pn^2_1 \thinspace dV \\ \vdots \\ \int_{\Omega_1}Pn^{N_E-1}_2 \thinspace dV + \int_{\Omega_2}Pn^{N_E}_1 \thinspace dV \\ f^{ext}  \end{bmatrix} $$

### Considering essential BC
$K$의 첫번째 열을 제거한 행렬을 $K'$ 첫번째 행을 제거한 $d$를 $d_E$라고 하자.

Essential BC에 의해서 결정된 $d_1 = u^1_1 = 0$을 고려하면 다음과 같다.

$$ K'd_E = f - d_1K_{*1} $$

다음으로 첫번째 행을 제거한 $K'$를 $K_E$ 첫번째 행을 제거한 $f-d_1K_{*1}$를 $f_E$라고 하자.

무의미한 첫번째 식을 제거한 최종형태는 다음과 같다. 

$$ K_Ed_E = f_E $$