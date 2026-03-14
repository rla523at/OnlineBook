# Derivatives
## 정의
open set $U \subset \R$과 일변수 함수 $f : U \rightarrow \R^m$이 있다고 하자.

$a \in U$에서 $f$의 `미분계수(derivative)`은 다음과 같이 정의된다.

$$ f'(a) := \lim_{h \rightarrow 0} \frac{1}{h}(f(a + h) - f(a)) $$

> Reference  
> {cite}`hubbard` chap 1.7  
> {cite}`stewart` chap 13.2  

### 참고1
$f$가 $a$에서 미분계수가 존재하는 경우 $f$가 $a$에서 `미분가능(differentiable)`하다고 한다.

이 때, 분자가 0으로 가기 때문에 극한값이 존재하기 위해서는 분모도 0으로 가야 한다.

$$ \lim_{h \rightarrow 0} f(a + h) = f(a) $$

이는 $\lim_{t\rightarrow x}f(t) = f(x)$와 동치임으로 $f$가 continuous해야 한다는 조건이 생긴다.

### 참고2
$f'$은 $U$에서 $a$에 한없이 가까이갈 때, $U$에서의 변화량과 $f$에서의 변화량의 비율의 극한이다.

$$ \frac{\text{change in }f}{\text{change in }U} = \frac{f(a+h)-f(a)}{(a+h)-a}  $$

### 참고3
$U$가 open set이기 때문에 다음이 성립한다.
$$ \exist\epsilon \in \R^+ \st B(a,\epsilon) \subseteq U$$

따라서 $h$가 위의 $\epsilon$보다 작아진면 다음이 성립한다.

$$ a+h \in U $$

### 명제1(Chain Rule)
$\R$의 open set $U,V$와 함수 $f : U \rightarrow V, g : V \rightarrow \R^m$이 있다고 하자.

$f$가 $a \in U$에서 differentiable하고 $g$가 $f(a) \in V$에서 differentiable할 때, 다음을 증명하여라.

$$ \begin{gathered} g \circ f \text{ is diffrentiable at } a \\ (g \circ f)'(a) =(g' \circ f)(a)f'(a) \end{gathered} $$

**Proof**

함수 $r : \R \rightarrow \R, s : \R \rightarrow \R^m$를 다음과 같이 정의하자.

$$ \begin{gathered} r(h) = f(a+h) - f(a) - f'(a)h \\ s(t) = g(f(a)+t) - g(f(a)) - g'(f(a))t \end{gathered} $$

$\Delta f(h) = r(h) + f'(a)h$라고 하면 $r$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} (g\circ f)(a+h) &= g(f(a+h)) \\&= g(f(a) + r(h) + f'(a)h) \\&= g(f(a) + \Delta f) \end{aligned} $$

$s$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} g(f(a) + \Delta f) = g(f(a)) + g'(f(a))\Delta f + s(\Delta f)  \end{aligned} $$

이를 정리하면 다음이 성립한다.

$$ \frac{(g\circ f)(a+h) - (g\circ f)(a)}{h}  = (g'\circ f)(a)\frac{\Delta f}{h} + \frac{s(\Delta f)}{h} $$

이 때, 보조명제1.1,1.2에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0}\frac{\Delta f}{h} = f'(a), \enspace \lim_{h\rightarrow0}\frac{s(\Delta f)}{h} = 0_m $$

따라서, 다음이 성립한다.

$$ \lim_{h\rightarrow0}\frac{(g\circ f)(a+h) - (g\circ f)(a)}{h}  = (g'\circ f)(a)f'(a) $$

미분의 정의에 의한 극한값이 존재함으로 다음이 성립한다.

$$ \begin{gathered} g \circ f \text{ is diffrentiable at } a \\ (g \circ f)'(a) =(g'\circ f)(a)f'(a) \end{gathered} \qed $$

#### 보조명제1.1
다음을 증명하여라.

$$ \lim_{h\rightarrow0}\frac{\Delta f}{h} = f'(a) $$

**Proof**

$\Delta f$를 풀어 쓰면 다음과 같다.

$$ \Delta f = f(a+h) - f(a) $$

$f$가 $a$에서 미분 가능함으로 다음이 성립한다.

$$ \lim_{h\rightarrow0}\frac{\Delta f}{h} = f'(a) \qed $$

#### 보조명제1.2
다음을 증명하여라.

$$ \lim_{h\rightarrow0}\frac{s(\Delta f)}{h} = 0_m $$

**Proof**

다음과 같이 수식을 변경하자.

$$ \begin{aligned} \frac{s(\Delta f)}{h} &= \frac{s(\Delta f)}{\Delta f}\frac{\Delta f}{h} \end{aligned}  $$

$s$의 정의에 의해 다음이 성립한다.

$$ \frac{s(\Delta f)}{\Delta f} = \frac{g(f(a)+\Delta f) - g(f(a))}{\Delta f}  - g'(f(a)) $$

이 때, $\Delta f$의 정의에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0} \Delta f = \lim_{h\rightarrow0} (f(a+h) - f(a)) = 0_m$$

따라서 다음이 성립한다.

$$ \begin{aligned} \lim_{h\rightarrow0}\frac{s(\Delta f)}{\Delta f} &= \lim_{h\rightarrow0}\left(\frac{g(f(a)+\Delta f) - g(f(a))}{\Delta f}  - g'(f(a))\right) \\&= \lim_{\Delta f \rightarrow0} \frac{g(f(a)+\Delta f) - g(f(a))}{\Delta f} - g'(f(a)) \\&= 0_m \end{aligned} $$

그럼으로, 다음이 성립한다.

$$ \lim_{h\rightarrow0}\frac{s(\Delta f)}{h} = 0f'(a) = 0_m \qed $$

### 명제2
미분 가능한 두 함수 $f,g$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \lim_{h \rightarrow 0} \frac{f(g(x+h))-f(g(x))}{h} =  \lim_{h \rightarrow 0} \frac{f(g(x) + hg'(x))-f(g(x))}{h}$$

**Proof**

$hg'(x) = \Delta g$라 두면 다음이 성립한다.

$$ \begin{aligned} f(g(x) + hg'(x)) &= f(g(x) + \Delta g) \\&= f(g(x)) + \Delta gf'(g(x)) + O(\Delta g^2) \end{aligned} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} \lim_{h \rightarrow 0} \frac{f(g(x) + hg'(x)) - f(g(x))}{h} &= \lim_{h \rightarrow 0} \frac{\Delta gf'(g(x)) + O(\Delta g^2)}{h} \\&= \lim_{h \rightarrow 0} f'(g(x))(g'(x)) + h(g'(x))^2 \\&= f'(g(x))g'(x) \end{aligned} $$

명제1에 의해 다음이 성립한다.

$$ \lim_{h \rightarrow 0} \frac{f(g(x+h))-f(g(x))}{h} = f'(g(x))g'(x) $$

그럼으로 다음이 성립한다.

$$ \lim_{h \rightarrow 0} \frac{f(g(x+h))-f(g(x))}{h} =  \lim_{h \rightarrow 0} \frac{f(g(x) + hg'(x))-f(g(x))}{h} \qed $$


>참고  
{cite}`stewart` chapter 2.5  
[blog-Paul's online note](https://tutorial.math.lamar.edu/Classes/CalcI/DerivativeProofs.aspx)  






