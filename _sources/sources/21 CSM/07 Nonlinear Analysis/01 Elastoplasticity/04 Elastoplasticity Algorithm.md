# Assumption
1. Small deformation
2. Equilibrium state 
3. ${}^{n}d, {}^{n}\sigma, {}^{n}\alpha, {}^{n}\epsilon_e^p, {}^{n}(\frac{\partial\sigma}{\partial\epsilon})$을 알고 있다.
4. Incremental force method를 적용하여, $n$ step까지 수렴했다.

# Algorithm for Elastoplasticy
$n+1$ step의 displacement based FE formulation은 다음과 같다.
$$ \int_\Omega B^T\sigma(d) \thinspace dV = f^{n+1} $$

$R(d)$를 다음과 같이 정의한다.
$$R(d) := \int_\Omega B^T\sigma(d) \thinspace dV - f^{n+1}$$

$R(d) = 0$을 풀기 위해 Newton-Raphson method와 재료에 맞는 stress calculation algorithm을 사용한다.
1. $k=0$으로 두고 초기값들을 설정한다.
   $$ \begin{aligned} d^k &= {}^{n}d \\ \sigma^k &= {}^{n}\sigma \\ \Delta d^k &= 0 \\ (\tfrac{\partial\sigma}{\partial\epsilon})^k &= {}^{n}(\tfrac{\partial\sigma}{\partial\epsilon}) \\ R(d^k) &= \int_\Omega B^T\sigma^k \thinspace dV - f^{n+1} \end{aligned} $$
2. $R(d)$를 $d = d^k$에서 선형근사한다. 뒤 해를 $d^{k + 1}$로 두고 행렬식을 풀어 $\Delta d$를 구한다.   
   $$ R(d) \approx R_L(d) $$

   $$ \text{Where, } R_L(d) := J_{R^k}(d - d^k) + R(d^k) $$
3. $R_L(d) = 0$을 만족시키는 $\Delta d$를 행렬식을 풀어  구한다.
	$$ \begin{aligned} & R_L(d) = 0 \\\implies& J_{R^k}(d - d^k) = -R(d^k) \\\implies& J_{R^k} \Delta d = -R(d^k) \end{aligned}  $$
4. $\Delta d^{k+1}$를 업데이트 한다.
   $$ \Delta d^{k+1} = \Delta d^k + \Delta d  $$
5. $\Delta \epsilon$을 계산한다.
   $$ \Delta \epsilon = B \Delta d^{k+1} $$
6. Stress calculation algorithm을 통해 $\sigma^{k + 1}, \alpha^{k + 1}, (\epsilon_e^p)^{k + 1},(\frac{\partial\sigma}{\partial\epsilon})^{k + 1}$를 계산한다.
   $$ \{ \sigma^{k + 1}, \alpha^{k + 1}, (\epsilon_e^p)^{k + 1}, (\tfrac{\partial\sigma}{\partial\epsilon})^{k + 1} \} = f({}^{n}\sigma, {}^{n}\alpha, {}^{n}\epsilon_e^p, \Delta\epsilon) $$
7. $R(d^{k + 1})$을 계산한다.
   $$ R(d^{k+1}) = \int_\Omega B^T\sigma^{k + 1} \thinspace dV - f^{n+1} $$
8. $R(d^{k + 1})$이 convergence criterion을 만족하는지 확인한다.
   $$ |R(d_{k+1})| \le \epsilon \enspace \land \enspace N \le k+1    $$
9.  Convergence criterion을 만족하면 변수들을 업데이트하고 알고리즘을 끝낸다.
   $$ \begin{aligned} {}^{n+1}d &= {}^{n}d + \Delta d^{k+1} \\ {}^{n+1}\sigma &= \sigma^{k+1} \\ {}^{n+1}\alpha &= \alpha^{k+1} \\ {}^{n+1}(\epsilon_e^p) &= (\epsilon_e^p)^{k+1} \\ {}^{n+1}(\tfrac{\partial\sigma}{\partial\epsilon}) &= (\tfrac{\partial\sigma}{\partial\epsilon})^{k+1} \end{aligned}  $$
10. Convergence criterion을 만족하지 않는다면 $k = k + 1$로 두고 2번으로 돌아간다.

## 명제1
다음을 증명하여라.
$$ J_{R^k} = \int_{\Omega} B^T \frac{\partial\sigma}{\partial\epsilon}(d^k)B \thinspace dV $$

**Proof**

$$ J_{R^k} = \frac{\partial R}{\partial d}(d^k) = \int_{\Omega} B^T \frac{\partial\sigma}{\partial\epsilon}(d^k)\frac{\partial\epsilon}{\partial d} \thinspace dV = \int_{\Omega} B^T \frac{\partial\sigma}{\partial\epsilon}(d^k)B \thinspace dV$$