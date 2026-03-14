# Tangent Vector
## 정의
$\R$의 open set $U$와 $r : U \rightarrow\R^n$이 있다고 하자.

$r$로 표현되는 curve를 $C$라 하고 $a \in U$에서 $r$이 differentiable할 때, $a$에서 $C$의 tangent vector $t$는 다음과 같이 정의한다.

$$ t := r'(a) $$

### 참고
Derivative의 정의에 의해 다음이 성립한다.

$$ r'(a) = \lim_{h\rightarrow0}\frac{r(a+h)-r(a)}{h} $$

이 때, $r'(a)$의 기하학적 의미를 살펴보자.

$A_{\R^n}$이 $\R^n$에 affine structure을 준 affine space라고 하자.

$r(a+h),r(h) \in A_{\R^n}$라 보고, $r(a+h)$점을 $Q$, $r(a)$점을 $P$라고 했을 때 affine space의 성질에 의해 다음이 성립한다.

$$ r(a+h) - r(a) = \overline{PQ} $$

```{figure} _image/0201.png
```

$r'(a)$의 정의를 기하학적으로 표현한 위 그림에 잘 나와있듯이, $h$가 작아지면 작아질수록 $\overline{PQ}$는 $P$에서 $C$의 접선과 같은 방향을 갖는 다는걸 알 수 있다.

따라서, $r'(a)$를 $a$에서 $C$의 tangent vector라고 정의한 것이다.

### 명제1
다음을 증명하여라.

$$ \text{Tangent vector is parametric dependent} $$

**Proof**

$\R$의 open set $U_{1,2}$와 $r_{1,2} : U_{1,2} \rightarrow\R^n \in C^1$이 있을 떄, 동일한 curve $C$가 $r_1,r_2$로 각각 표현된다고 하자.

$C$가 $r_1,r_2$로 표현됨으로 다음이 성립한다.

$$ \begin{gathered} \exist \text{bijective } f:U_1\rightarrow U_2 \st  \forall x \in U_1, \quad  r_1(x) = r_2(f(x)) \end{gathered} $$

따라서, chain rule에 의해 다음이 성립한다.

$$ \diff{r_1}{x} = \diff{r_2}{f}\diff{f}{x} $$

그럼으로 서로 다른 parameter로 표현된 $C$의 tangent vector는 그 두 parameter사이의 변환을 나타내는 $f'$만큼 차이를 갖는다.

$$ r_1' = f'r_2' \qed $$


## Unit Tangent Vector
$\R$의 open set $U$와 $r : U \rightarrow\R^n$이 있다고 하자.

$r$로 표현되는 curve를 $C$라 하고 $a \in U$에서 $r$이 differentiable할 때, $a$에서 $C$의 unit tangent vector $T$는 다음과 같이 정의한다.

$$ T:= \frac{r'(a)}{\norm{r'(a)}} $$

### 명제1
다음을 증명하여라.

$$ \text{Unit tangent vector is parametric independent} $$

**Proof**

$\R$의 open set $U_{1,2}$와 $r_{1,2} : U_{1,2} \rightarrow\R^n \in C^1$이 있을 떄, 동일한 curve $C$가 $r_1,r_2$로 각각 표현된다고 하자.

$C$가 $r_1,r_2$로 표현됨으로 다음이 성립한다.

$$ \begin{gathered} \exist \text{bijective } f:U_1\rightarrow U_2 \st  \forall x \in U_1, \quad  r_1(x) = r_2(f(x)) \end{gathered} $$

따라서, $y = f(x)$로 두면 chain rule에 의해 다음이 성립한다.

$$ \diff{r_1}{x} = \diff{r_2}{y}\diff{y}{x} $$

$0 < df/dx$라고 가정하면 다음이 성립한다.

$$ \norm{r_1'} = y'\norm{r_2'} $$

그럼으로 다음이 성립한다.

$$ T_1(x) = \frac{r'_1}{\norm{r'_1}} = \frac{r_2'y'}{y'\norm{r'_2}} = \frac{r'_2}{\norm{r'_2}} = T_2(y) \qed $$

#### 참고
$0 < df/dx$임을 가정하였다.

이 떄, $df/dx \neq 0$인 조건을 regurlar curve라고 부른다. 

### 명제2
다음을 증명하여라.

$$ \text{Derivative of unit tangent vector is parametric dependent} $$

**Proof**

$\R$의 open set $U_{1,2}$와 $r_{1,2} : U_{1,2} \rightarrow\R^n \in C^1$이 있을 떄, 동일한 curve $C$가 $r_1,r_2$로 각각 표현된다고 하자.

$C$가 $r_1,r_2$로 표현됨으로 다음이 성립한다.

$$ \begin{gathered} \exist \text{bijective } f:U_1\rightarrow U_2 \st  \forall x \in U_1, \quad  r_1(x) = r_2(f(x)) \end{gathered} $$

이 때, 명제1에 의해 $y = f(x)$로 두면 다음이 성립한다.

$$ T_1(x) = T_2(y) $$

따라서, chain rule에 의해 다음이 성립한다.

$$ \diff{T_1}{x} = \diff{T_2}{y}\diff{y}{x} $$

그럼으로 서로 다른 parameter로 표현된 $C$의 derivative of unit tangent vector는 그 두 parameter사이의 변환을 나타내는 $y'$만큼 차이를 갖는다.

$$ T_1' = y'T_2' \qed $$

**Proof2**

$\R$의 open set $U_{1,2}$와 $r_{1,2} : U_{1,2} \rightarrow\R^n \in C^1$이 있을 떄, 동일한 curve $C$가 $r_1,r_2$로 각각 표현된다고 하자.

$C$가 $r_1,r_2$로 표현됨으로 다음이 성립한다.

$$ \begin{gathered} \exist \text{bijective } f:U_1\rightarrow U_2 \st  \forall x \in U_1, \quad  r_1(x) = r_2(f(x)) \end{gathered} $$

$y = f(x)$로 두고 $0 < y'$을 가정하면 chain rule에 의해 다음이 성립한다.

$$ \begin{aligned} r_1(x) &= r_2(y) \\ r'_1 &= r'_2 y' \\ \norm{r'_1} &= \norm{r'_2}y' \\ \diff{}{x}\norm{r'_1} &= \diff{}{x}(\norm{r'_2}) y' + \norm{r'_2}y'' = \diff{}{y}(\norm{r'_2})(y')^2 + \norm{r'_2}f''  \\ r''_1 &= r''_2(y')^2 + r'_2y'' \end{aligned} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} \diff{T_1}{x} &= \frac{r''_1\norm{r'_1} - r'_1 \diff{}{x}\norm{r'_1}}{\norm{r'_1}^2}  \\&= \frac{(r''_2(y')^2 + r'_2y'')(\norm{r'_2}y') - r'_2 y'(\diff{}{y}(\norm{r'_2})(y')^2 + \norm{r'_2}f'')}{\norm{r'_2}^2(y')^2} \\&= \frac{r''_2\norm{r'_2}(y')^3 - r'_2 \diff{}{y}(\norm{r'_2})(y')^3}{\norm{r'_2}^2(y')^2} \\&= \diff{T_2}{y}y' \end{aligned} $$

그럼으로 서로 다른 parameter로 표현된 $C$의 derivative of unit tangent vector는 그 두 parameter사이의 변환을 나타내는 $y'$만큼 차이를 갖는다.

$$ T_1' = y'T_2' \qed $$
