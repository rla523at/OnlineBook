# Partial Derivatives
## 정의
$\R^n$의 open set $U$와 함수 $f: U \rightarrow \R$이 있다고 하자.

$\R^n$의 임의의 basis를 $\beta$라 할 떄, $a \in U$는 다음과 같이 표현된다.

$$ a = a^i\beta_i $$

$a$에서 $f$의 $i$번째 변수의 `편미분(partial derivatives)` $D_i^{f(a)}$은 다음과 같이 정의된다.

$$ \begin{aligned} D_i^{f(a)} &:= \lim_{h \rightarrow 0}\frac{f(a + h \beta_i) -f (a)}{h} \\&= \lim_{h \rightarrow 0} \frac{1}{h}(f(a^1, \cdots, a^i + h, \cdots, a^n) - f(a^1, \cdots, a^n))  \end{aligned} $$

위의 극한값이 존재할 경우 편미분은 그 극한값이 되며, 존재하지 않을 경우 편미분은 존재하지 않게 된다.

> Reference  
> {cite}`hubbard` chap 1.7

### 참고1
편미분의 정의는 basis 선택에 independent하다.

하지만 편미분의 값은 basis 선택에 depend한다. 

왜냐하면 $D_i^{f(a)}$는 $f(a)$에서 $i$번째 basis 방향으로 움직일 때 $f$가 어떻게 변하는지를 나타내는 값이기 때문이다.

### 참고2
가장 널리 쓰이는 표기법은 다음과 같다.

$$ D_i^{f(a)} = \frac{\partial f}{\partial x_i}\bigg|_{x=a} $$

### 참고3
편미분은 다변수 함수를 일변수 함수처럼 보고 미분하는 방식이다.

다시 말해, 나머지 변수는 전부 상수로 간주하고 한 변수에 대해서 미분을 구하는 방식이다.

### 명제1
$\R^n$의 open set $U$와 linear map $f: U \rightarrow \R$이 있다고 하자.

$\R^n$의 임의의 basis를 $\beta$라 하고 $a \in U$에서 $f$의 partial derivatives가 존재할 때, 다음을 증명하여라.

$$ D_i^{f(a)} = f(\beta_i) $$

**Proof**

Partial derivatve의 정의에 의해 다음이 성립한다.

$$ D_i^{f(a)} = \lim_{h \rightarrow 0}\frac{f(a + h \beta_i) -f (a)}{h} $$

$f$가 linear map임으로 다음이 성립한다.

$$ \frac{f(a + h \beta_i) -f (a)}{h} = \frac{f(a) + hf( \beta_i) -f (a)}{h} =  f(\beta_i) $$

따라서, 위의 결과를 종합하면 다음이 성립한다.

$$ D_i^{f(a)} = f(\beta_i) \qed $$
