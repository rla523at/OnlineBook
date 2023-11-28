# Partial Derivatives of Vector-valued Function
## 정의
$\R^n$의 open set $U$가 있다고 하자.

$f : U \rightarrow \R^m$는 다음과 같이 표현된다.

$$ (f^1,\cdots,f^m) $$

$f$의 component functions는 다음과 같다.

$$ f^i:U \rightarrow \R, \enspace i=1,\cdots,m $$

$\R^n$의 basis를 $\beta$라 할 떄, $a \in U$는 다음과 같이 표현된다.

$$ a = a^i\beta_i $$

$a \in U$에서 $f$의 $i$번째 변수의 편미분 $D_i^{f(a)} \in \R^m$은 다음과 같이 정의된다.

$$ \begin{aligned} D_i^{f(a)} &:= \lim_{h \rightarrow 0}\frac{1}{h}(f(a + h \beta_i) -f (a)) \\&=\lim_{h \rightarrow 0} \frac{1}{h}(f(a^1, \cdots, a^i + h, \cdots, a^n) - f(a^1, \cdots, a^n)) \\&= \left( \lim_{h \rightarrow 0} \frac{f^1(a^1, \cdots, a^i + h, \cdots, a^n) - f^1(a^1, \cdots, a^n)}{h}, \cdots, \lim_{h \rightarrow 0} \frac{f^m(a^1, \cdots, a^i + h, \cdots, a^n) - f^m(a^1, \cdots, a^n)}{h}\right) \\&= \left( D^{f^{1}(a)}_i, \cdots, D^{f^{m}(a)}_i\right) \end{aligned} $$ 

> Reference  
> {cite}`hubbard` chap 1.7

### 참고1
$D_i^{f(a)}$는 $f(a)$에서 $i$번째 basis 방향으로 움직일 때 $f$가 어떻게 변하는지를 나타내는 값이다.