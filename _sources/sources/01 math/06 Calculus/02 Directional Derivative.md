# Directional Derivative
## Motivation
$\R^n$의 open set을 $U$라고 하고 다변수 함수 $f : U \rightarrow \R^m$이 있다고 하자.

$a \in U$에서 일변수 함수의 미분계수의 정의를 그대로 다변수 함수의 미분계수로 확장해보자.

$$ \lim_{h\rightarrow 0}\frac{f(a+h) - f(a)}{h} $$

위의 식을 살펴보면 $a$는 vector이고 $h$는 scalar인데 정의되지 않는 $+$연산을 하고 있다.

그리고 derivative는 $a$에 한없이 가까워질 때, $U$의 변화량과 $f$의 변화량의 비율의 극한이라는 의미를 갖는데, 일변수 함수의 경우 $a$에 가까워 지는것이 한가지 방향으로만 가능하기 때문에 $h\rightarrow 0$으로 그 의미가 충분히 표현된다. 

하지만 다변수 함수의 경우 $a$에 가까워 지는 방향이 무한하기때문에 $h\rightarrow 0$만으로 그 의미가 충분히 표현되지 못한다.

```{figure} _image/0201.png
```

따라서, 위의 두 문제를 해결하기 위해 특정 방향 $v \in \R^n \st \norm{v} = 1$을 정해서 $v$ 방향으로 $a$에서 derivative를 생각하게 되면 다음과 같이 정의하는 것이 자연스럽다.

$$ \lim_{h\rightarrow 0} \frac{f(a + h v) - f (a)}{h} $$

## 정의
$\R^n$의 open subset $U$와 함수 $f:U \rightarrow \R^m$이 있다고 하자.

$a \in U$에서 $v\in \R^n \st \norm{v}=1$ 방향으로의 $f$의 `방향 미분계수(directional derivative)`는 다음과 같다.

$$ D_vf(a) = \lim_{h \rightarrow 0}\frac{ f(a + h v) - f (a)}{h} $$

### 참고1
$D_vf(a)$는 $\R^m$의 element, 즉 vector이다.

$$ D_vf(a) = \begin{bmatrix} D_vf_1(a) \\ \vdots \\ D_vf_m(a) \end{bmatrix} $$

### 참고2(Partial Derivative)
$\R^n$의 standard basis인 $e_i$방향의 directional derivative를 다음과 같이 표현한다.

$$ D_{e_i}f(a) = \lim_{h \rightarrow 0} \frac{1}{h}(f(a^1, \cdots, a^i + h, \cdots, a^n) - f(a^1, \cdots, a^n)) = \pdiff{f}{x_i}(a) $$

이를 `편미분(partial derivative)`라고 한다.

당연하게, $\pdiff{f}{x_i}(a) \in \R^m$도 vector이다.

$$ \pdiff{f}{x_i}(a) = \begin{bmatrix} \pdiff{f_1}{x_i}(a) \\ \vdots \\ \pdiff{f_m}{x_i}(a) \end{bmatrix} $$

### 참고3
편미분은 다변수 함수를 일변수 함수처럼 보고 미분하는 방식이다.

다시 말해, 나머지 변수는 전부 상수로 간주하고 한 변수에 대해서 미분을 구하는 방식이다.

### 명제1
$\R^n$의 open set $U$와 linear map $f: U \rightarrow \R^m$이 있다고 하자.

$a \in U$에서 $f$의 partial derivatives가 존재할 때, 다음을 증명하여라.

$$ \pdiff{f}{x_i}(a) = f(e_i) $$

**Proof**

Partial derivatve의 정의에 의해 다음이 성립한다.

$$ \pdiff{f}{x_i}(a) = \lim_{h \rightarrow 0}\frac{f(a + h e_i) -f (a)}{h} $$

$f$가 linear map임으로 다음이 성립한다.

$$ \frac{f(a + h e_i) -f (a)}{h} = \frac{f(a) + hf( e_i) -f (a)}{h} =  f(e_i) $$

따라서, 위의 결과를 종합하면 다음이 성립한다.

$$ \pdiff{f}{x_i}(a) = f(e_i) \qed $$