# Partial Derivative
## 정의
$\R^n$의 standard basis인 $e_i$방향의 directional derivative를 다음과 같이 표현한다.

$$ D_{e_i}f(a) = \lim_{h \rightarrow 0} \frac{1}{h}(f(a^1, \cdots, a^i + h, \cdots, a^n) - f(a^1, \cdots, a^n)) = \pdiff{f}{x_i} $$

이를 `편미분(partial derivative)`라고 한다.

### 참고1
편미분은 다변수 함수를 일변수 함수처럼 보고 미분하는 방식이다.

다시 말해, 나머지 변수는 전부 상수로 간주하고 한 변수에 대해서 미분을 구하는 방식이다.

### 명제1
$\R^n$의 open set $U$와 linear map $f: U \rightarrow \R$이 있다고 하자.

$a \in U$에서 $f$의 partial derivatives가 존재할 때, 다음을 증명하여라.

$$ \pdiff{f}{x_i}(a) = f(e_i) $$

**Proof**

Partial derivatve의 정의에 의해 다음이 성립한다.

$$ \pdiff{f}{x_i}(a) = \lim_{h \rightarrow 0}\frac{f(a + h e_i) -f (a)}{h} $$

$f$가 linear map임으로 다음이 성립한다.

$$ \frac{f(a + h e_i) -f (a)}{h} = \frac{f(a) + hf( e_i) -f (a)}{h} =  f(e_i) $$

따라서, 위의 결과를 종합하면 다음이 성립한다.

$$ \pdiff{f}{x_i}(a) = f(e_i) \qed $$

### 명제2
$\R^n$의 open set $U$와 $f: U \rightarrow \R$이 있다고 하자.

$f$의 partial derivatives가 존재할 때, Euclidean graident $\nabla_Ef$를 다음과 같이 정의하자.

$$ \nabla_Ef = \left( \pdiff{f}{x_1}, \cdots, \pdiff{f}{x_n} \right) $$

$a \in U$와 $v\in \R^n \st \norm{v} = 1$가 있을 때, 다음을 증명하여라.

$$ D_vf(a) = \nabla_E f \cdot v $$

**Proof**

Directional derivative의 정의에 의해 다음이 성립한다.

$$ D_vf(a) = \lim_{h\rightarrow 0} \frac{f(a+hv) - f(a)}{h} $$

이 때, $g(h) = f(x(h)), \enspace x(h) = a+hv$로 두면 다음이 성립한다.

$$ D_vf(a) = \lim_{h\rightarrow 0} \frac{g(h) - g(0)}{h} = g'(0) $$

$x(h)$는 differentiable함으로 chain rule에 의해 다음이 성립한다.

$$ \begin{aligned} g'(0) &= \pdiff{f}{x_1}\diff{x_1}{h} + \cdots + \pdiff{f}{x_n}\diff{x_n}{h} \\&=  \nabla_Ef \cdot v \qed \end{aligned} $$