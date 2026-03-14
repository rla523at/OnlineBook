# Inverse Function Theorem
## Statement
함수 $f: \R^n \rightarrow \R^n$가 있다고 하자.

$a \in \R^n$을 포함하는 $\R^n$의 open set $U$에서 $f$가 continuously differentiable할 때, 다음을 증명하여라.

$$ \det(J_f(a)) \neq 0 \implies \exist  V,W \subseteq \R^n \st \begin{gathered} a \in V \subseteq U \\ f(a) \in W \subseteq \R^n \\ f|_{V \times W} \text{ is bijective} \\ \forall y \in W, (f|_{V \times W})^{-1} \text{ is differentiable} \end{gathered} $$

**Proof**

$\R^n$의 임의의 basis를 $\beta,\gamma$라할 때, 행렬 $L$을 다음과 같이 정의하자.

$$ L := J_f(a) = \mathfrak{m}_{\beta}^{\gamma}(D^{f(a)}) $$

그러면 전제에 의해 다음이 성립한다.

$$ \exist L^{-1} $$

이 떄, 다음과 같은 composite function을 생각해보자.

$$ L^{-1}\circ f : \R^n \rightarrow \R^n $$

Chain rule에 의해 다음이 성립한다.

$$ D^{(L^{-1}\circ f)(a)} = D^{L^{-1}(f(a))}\circ D^{f(a)}  $$

Jacobian의 정의에 의해 다음이 성립한다.

$$ J_{L^{-1}\circ f}(a) = J_{L^{-1}}(f(a))J_{f}(a) $$

$L^{-1}$은 linear map임으로 total derivative의 성질에 의해 다음이 성립한다.

$$ J_{L^{-1}}(f(a))J_{f}(a) = \mathfrak{m}_\beta^\gamma(L^{-1})L = I_n $$

---

$$ \lim_{h \rightarrow 0_n} \frac{1}{|h|}(g(a + h) - g(a) - D^{g(a)}(h)) = 0_n $$

$$g := L^{-1}\circ f$$

$$ g(a+h) \neq g(a) $$


### 보조정리1
$\R^n$의 open rectangle $A$와 미분가능한 함수 $f : A \rightarrow \R^n$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ \exist M \in \R^+ \st  \forall x \in A, \quad \norm{\pdiff{f_i}{x_j}} \le M \implies \forall y,z \in A \quad  \norm{f(y) - f(z)} \le n^2M\norm{y-z}  $$

**Proof**

$\forall i\in[1,n]$에서 다음이 성립한다.

$$ f_i(y) - f_i(z) = \sum_{j=1}^n (f_i(y_1,\cdots,y_j,z_{j+1},\cdots,z_n) - f_i(y_1,\cdots,y_{j-1},z_{j},\cdots,z_n)) $$

이 때, mean value theorem에 의해 $\forall j \in [1,n]$에서 다음이 성립한다.

$$ \exist c \in (\min(y_j,z_j),\max(y_j,z_j)) \st f_i(y_1,\cdots,y_j,z_{j+1},\cdots,z_n) - f_i(y_1,\cdots,y_{j-1},z_{j},\cdots,z_n) = \pdiff{f}{x_j}(y_1,\cdots,y_{j-1},c,z_{j+1},\cdots,z_{n})(y_j - z_j) $$

따라서, $c_j := (y_1,\cdots,y_{j-1},c,z_{j+1},\cdots,z_{n})$라 하면, 다음이 성립한다.

$$ f_i(y) - f_i(z) = \sum_{j=1}^n \pdiff{f}{x_j}(c_j)(y_j-z_j) $$

그러면 다음이 성립한다.

$$ \begin{aligned} \norm{f(y) - f(z)} &\le \sum_{i=1}^n \norm{f_i(y) - f_i(z)} \\&= \sum_{i=1}^n \norm{\sum_{j=1}^n \pdiff{f}{x_j}(c_j)(y_j-z_j)} \\&= \sum_{i=1}^n\sum_{j=1}^n \norm{ \pdiff{f}{x_j}(c_j)}|y_j-z_j| \\&\le \sum_{i=1}^n\sum_{j=1}^nM\norm{y-z} \\&= n^2M\norm{y-z} \qed  \end{aligned} $$


> Reference  
> [Note] (UCSD) Inverse Function Theorem Proof  
> [Book] (Lee) Introduction to smooth manifolds p.657
> [note1](https://math.jhu.edu/~jmb/note/invfnthm.pdf)  