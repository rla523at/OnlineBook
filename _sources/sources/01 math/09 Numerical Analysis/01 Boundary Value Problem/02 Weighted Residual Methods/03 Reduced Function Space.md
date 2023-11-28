# Reduced Function Space

## Petrov-Galerkin method
유한차원 함수공간 $\mathcal W_f, \mathcal U_f$의 기저함수 $\{ w_1, \cdots, w_n \}, \{ u_1, \cdots, u_n \}$를 독립적으로 정의하는 방법을 `Petrov-Galerkin method`라고 한다.

결론적으로 Petrov-Galerkin method는 독립적으로 정의된 $\mathcal W_h$공간에서 weighted residual method를 만족하는 $u$를 $\mathcal U_h$ 공간에서 찾는 방법이다.

## Bubnov-Galerkin method

유한차원 함수공간 $\mathcal W_f, \mathcal U_f$의 기저함수 $\{ w_1, \cdots, w_n \}, \{ u_1, \cdots, u_n \}$를 동일하게 정의하는 방법을 `Bubnov-Galerkin method`라고 한다.  

$$ w_i = u_i $$

### 참고
Bubnov-Galerkin method를 dimensional reduction된 WRF에 적용하면 다음과 같다.

$$ \int_\Omega u_ir \thinspace dV = 0, \enspace i=1,\cdots,n  $$

따라서, 이렇게 찾은 solution $u$는 $r$을 solution function space에 orthogonal하게 만드는 solution이다.

즉, solution function space로 projection 된 $r$이 0이 되게 하는 값이다.

## Least square method

유한차원 함수공간 $\mathcal W_f, \mathcal U_f$의 기저함수 $\{ w_1, \cdots, w_n \}, \{ u_1, \cdots, u_k \}$를 독립적으로 정의하되 $\mathcal W_h$의 기저함수로 다음과 같이 정의된 값을 쓰는 방법을 `least square method`라고 한다.  

$$ w_i = \pdiff{r}{x^i} $$

### 참고
Least square method를 dimensional reduction된 WRF에 적용하면 다음과 같다.

$$ \begin{aligned} & \int_{\Omega_i} \pdiff{r}{x^i}r \thinspace dV = 0 , \enspace i=1,\cdots,n  \\ \iff \enspace & \pdiff{}{x^i}\int_{\Omega_i} r^2 \thinspace dV = 0 , \enspace i=1,\cdots,n \end{aligned} $$  

즉, $r$ 제곱의 합이 최소가 되게 하는 $x$을 찾는 것임으로 least square method라고 한다.

## Point collocation method
유한차원 함수공간 $\mathcal W_f, \mathcal U_f$의 기저함수 $\{ w_1, \cdots, w_n \}, \{ u_1, \cdots, u_k \}$를 독립적으로 정의하되 $\mathcal W_h$의 기저함수로 Dirac-delta 함수를 사용하는 방법을 `point collocation method`라고 한다.  

$$ w_i = \delta(x - x_i) $$

### 참고
Point collocation method를 dimensional reduction된 WRF에 적용하면 다음과 같다.

$$ \mathcal r(x_i) = 0, \quad i = 1, \cdots, n $$

즉, collocation node로 불리는 $x_i$점에서 residual을 0으로 만드는 solution을 찾는 방법이다. 

이렇게 찾은 solution은 collocation node에서 strong formulation을 만족한다.

## Subdomain collocation method
유한차원 함수공간 $\mathcal W_f, \mathcal U_f$의 기저함수 $\{ w_1, \cdots, w_n \}, \{ u_1, \cdots, u_k \}$를 독립적으로 정의하되 $\mathcal W_h$의 기저함수로 계단 함수를 사용하는 방법을 `subdomain collocation method`라고 한다.  

$$ w_i = \begin{cases} 1 & \text{if } x_i \in \Omega_i \\ 0 & \text{else} \end{cases}$$

### 참고
Subdomain collocation method를 dimensional reduction된 WRF에 적용하면 다음과 같다.

$$ \int_{\Omega_i} r \thinspace dV = 0, \quad i = 1, \cdots, n $$

즉, $\Omega_i$에서 residaul의 평균을 0으로 만드는 solution이다.