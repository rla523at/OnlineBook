# Euler's Phi Function
## 정의

$$ \varphi : \Z \rightarrow \Z \st n \mapsto |\Set{k \in [0,n) | \gcd(n,k) = 1}| $$

### 참고
$\varphi(n)$은 $n$보다 작은 relative prime의 개수이다.

### 명제1
Euler's phi function $\varphi$와 prime number $p$가 있다고 하자.

그러면 $1$보다 큰 자연수 $i$에 대해서 다음이 성립한다.

$$ \varphi(p^i) = p^i - p^{i-1} $$

**Proof**

$\varphi$의 정의에 의해 다음이 성립한다.

$$ \varphi(p^i) = |\Set{k \in [0,p^i) | \gcd(p^i,k) = 1}| $$

즉 $p^i$개의 선택지 중에서 relative prime이 아닌 수를 빼야 한다.

이 때, $p^i$와 relative prime이 아닌 수들의 집합을 생각하면 $p$의 배수 형태여야 함으로 다음이 성립한다.

$$ \Set{mp | m \in [0, p^{i-1})} $$

즉, relative prime이 아닌 수들은 총 $p^{i-1}$개가 된다.

따라서 다음이 성립한다.

$$ \varphi(p^i) = p^i - p^{i-1} \qed $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Euler%27s_totient_function#Computing_Euler's_totient_function)

### 명제2
Euler's phi function $\varphi$와 relative prime number $a,b$가 있다고 하자.

그러면 다음이 성립한다.

$$ \varphi(ab) = \varphi(a)\varphi(b) $$

**Proof**

먼저 수를 다음과 같이 나열해보자.

$$ \begin{matrix} 1 & \cdots & i & \cdots & a \\ a+1 & \cdots & a+i & \cdots & 2a \\ && \vdots && \\ (b-1)a + 1 & \cdots & (b-1)a+i & \cdots & ba \end{matrix} $$

이 때, $i \in [1,a]$에 대해서 $\gcd(i,a) = 1$이라고 해보자.

그러면 $\gcd$의 성질에 의해 임의의 정수 $k$에 대해서 $\gcd(i+ka,a) = 1$이 됨으로 $i$번째 column은 전부 $a$와 relative prime이 된다.

이 때, $\varphi(a) = n$이라고 하면 총 $n$개의 열에 있는 숫자들이 전부 $a$와 relative prime이 된다.

다음으로 한 column안에서 $b$와 relative prime인 숫자들을 생각해보자.

$i \in [1,a]$ column에서 임의의 두 숫자를 뽑으면 $i + q_1a, i+q_2a$  로 표현할 수 있다.

그러면 다음이 성립한다.

$$ \begin{aligned} & (i+q_1a) \bmod b - (i+q_2a) \bmod b = ((q_1-q_2)a) \bmod b\\\implies& (i+q_1a) \bmod b - (i+q_2a) \bmod b \neq 0 \\\implies& (i+q_1a) \bmod b \neq (i+q_2a) \bmod b  \end{aligned} $$

즉, 한 column 안에 있는 모든 숫자는 $b$로 나눴을 때, 나머지가 전부 다름으로 다음과 같이 표현할 수 있다.

$$ q_0b, q_1b +1, \cdots, q_{b-1}b + b-1 $$

그리고, $\gcd$의 성질에 의해 $i\in[0,b-1]$에 대해서 다음이 성립한다.

$$ \gcd(q_ib + i,b) = \gcd(i,b) $$

즉, column에 있는 $b$와 relative prime인 수의 개수는 $\varphi(b)$가 된다.

따라서, $ab$와 relative prime인 숫자의 개수는 $a$와 $b$에 동시에 relative prime인 숫자의 개수와 같고 $a$와 relative prime인 column이 $\varphi(a)$개 있고 그 안에서 $b$와 relative prime인 숫자의 개수는 $\varphi(b)$개 있음으로 다음이 성립한다.

$$ \varphi(ab) = \varphi(a)\varphi(b) \qed $$

### 명제3
Euler's phi function $\varphi$와 임의의 자연수 $n$이 있다고 하자.

prime decomposition에 의해 $n=\prod_{i=1}^mp_i^{k_i}$라고 하면 다음이 성립한다.

$$ \varphi(n) = n \prod_{i=1}^m\left(1 - \frac{1}{p_i}\right)  $$

**Proof**

$$ \begin{aligned} \varphi(n) &= \varphi(\prod_{i=1}^mp_i^{k_i}) \\&= \prod_{i=1}^m (p_i^{k_i} - p_i^{k_i-1}) \\&= \prod_{i=1}^m p_i^{k_i} \prod_{i=1}^m \left(1 - \frac{1}{p_i}\right) \\&= n \prod_{i=1}^m\left(1 - \frac{1}{p_i}\right) \qed \end{aligned} $$