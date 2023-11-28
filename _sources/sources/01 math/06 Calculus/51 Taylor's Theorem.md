# Taylor's Theorem
## $f: \R \rightarrow \R$

### 명제1
함수 $f: \R \rightarrow \R$가 있다고 하자.

$f$가 $a \in \R$에서 $k$번 미분가능할 떄, 다음을 증명하여라.
$$ \exist h_k(x) \quad s.t. \quad f(x) = f(a) + \sum_{i=1}^k \frac{f^{(k)}(a)}{k!} + h_k(x)(x-a)^k \enspace \land \enspace \lim_{x \rightarrow a}h_k(x) = 0 $$

**Proof**

> Reference  
> [Wiki - Taylor's Theorem](https://en.wikipedia.org/wiki/Taylor%27s_theorem)

#### 참고1
아래와 같이 표현할 수 있다.
$$ f = P_k(x) + R_k(x) $$

$$ \begin{aligned} \text {Where, } P_k(x) &:= f(a) + \sum_{i=1}^k \frac{f^{(k)}(a)}{k!}(x-a)^k \\ R_k(x) & := h_k(x)(x-a)^k \end{aligned} $$

이 때, $P_k$를 $k$-th order `Taylor polynomial`이라고 하고 $R_k(x)$를 `remainder term`이라고 한다.

### 명제2
함수 $f: \R \rightarrow \R$가 있다고 하자.

$f$가 $a \in \R$에서 $k+1$번 미분가능하고 $f^{(k+1)}$가 $[a,x]$에서 continuous할 때, 다음을 증명하여라.

$$ f = f(a) + \sum_{i=1}^k \frac{f^{(k)}(a)}{k!} + \int^x_a \frac{f^{(k+1)}(t)}{k!}(x-t)^k \thinspace dt$$

**Proof**

> Reference  
> [Wiki - Taylor's Theorem](https://en.wikipedia.org/wiki/Taylor%27s_theorem)