# Fundamental Theorem of Calculus
## Fundamental Theorem of Calculus1
$[a,b]\subset\R$에서 continuous한 함수 $f$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \diff{}{x} \int^x_a f(t) \thinspace dt = f(x) $$

**Proof**

함수 $F$를 다음과 같이 정의하자.

$$ F(x):= \int^x_a f(t) \thinspace dt $$

$x,x+h \in (a,b)$라고 하자.

그러면 $F$의 정의에 의해 다음이 성립한다.

$$ F(x+h) - F(x) = \int^{x+h}_x f(t)\thinspace dt $$

$f$는 continuous function임으로 extream value theorem에 의해서 다음이 성립한다.

$$ \exist x_m,x_M \in [\min(x,x+h), \max(x,x+h)] \st f(x_m),f(x_M)\text{ are minimum and maximum } $$

보조명제에 의해 다음이 성립한다.

$$ f(x_m) \le \frac{F(x+h)-F(x)}{h}  \le f(x_M) $$

이 떄, $\lim_{h\rightarrow0}x_m = \lim_{h\rightarrow0}x_M = x$임으로 다음이 성립한다.

$$ f(x) \le \lim_{h\rightarrow0}\frac{F(x+h)-F(x)}{h}  \le f(x) $$

Squeeze theorem에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0}\frac{F(x+h)-F(x)}{h} = f(x) $$

Derivative의 정의를 만족하는 극한값이 $f(x)$로 주어져 있음으로 다음이 성립한다.

$$ \begin{gathered} F \text{ is differentiable on } (a,b) \\ F'(x) = f(x) \end{gathered} \qed $$

> Reference  
> {cite}`stewart` Chapter4.3

### 보조명제
다음을 증명하여라.

$$ f(x_m) \le \frac{F(x+h)-F(x)}{h}  \le f(x_M) $$

**Proof**

$h>0$이라고 하면 다음이 성립한다.

$$ f(x_m)h \le \int^{x+h}_x f(t)\thinspace dt \le f(x_M)h $$

$h$로 나눠주고 이를 $F$에 대해 쓰면 다음과 같다.

$$ f(x_m) \le \frac{F(x+h)-F(x)}{h}  \le f(x_M)  $$

다음으로 $h<0$이라고 하면 다음이 성립한다.

$$ f(x_M)h \le \int^{x+h}_x f(t)\thinspace dt \le f(x_m)h $$

$h$로 나눠주고 이를 $F$에 대해 쓰면 다음과 같다.

$$ f(x_m) \le \frac{F(x+h)-F(x)}{h}  \le f(x_M) \qed  $$

### 따름명제
$[a,b]\subset\R$에서 continuous한 함수 $f$가 있다고 하자.

$[a,b]\subset\R$에서 $f$의 antiderivative를 $F$라고 하자.

$$ F'(x) = f(x) $$

이 떄, $x_1,x_2 \in [a,b]$에 대해서 다음을 증명하여라.

$$ \int^{x_2}_{x_1} f(x) \thinspace dx = F(x_2)- F(x_1) $$

**Proof**

$[a,b]\subset\R$에서 함수 $G$를 다음과 같이 정의하자.

$$ G(x):= \int^x_a f(t) \thinspace dt $$

그러면, Fundamental theorem of calculus에 의해 다음이 성립한다.

$$ \begin{aligned} G'(x) &= f(x) \\&= F'(x) \end{aligned}  $$

보조명제에 의해 다음이 성립한다.

$$ \exist c \in \R \st \forall x\in [a,b], \quad  G(x) = F(x) + c $$

그러면 다음이 성립한다.

$$ \begin{gathered} G(x_2) = \int^{x_2}_a f(t) \thinspace dt = F(x_2)+ c \\ G(x_1) = \int^{x_1}_a f(t) \thinspace dt = F(x_1)+ c \end{gathered} $$

따라서, 다음이 성립한다.

$$ G(x_2) - G(x_1) = \int^{x_2}_{x_1} f(x) \thinspace dx = F(x_2)- F(x_1) \qed $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Fundamental_theorem_of_calculus#Formal_statements)

#### 보조명제
다음을 증명하여라.

$$ \exist c \in \R \st \forall x\in [a,b], \quad  G(x) = F(x) + c $$

**Proof**

Mean value thoerem에 의해 다음이 성립한다.

$$ \exist c \in \R \st \forall x\in (a,b), \quad  G(x) = F(x) + c $$

다음으로, $x=a,b$에서도 위 식이 성립하는지 확인해보자.

$F$는 정의에 의해 $[a,b]$에서 미분가능함으로 다음이 성립한다.

$$ F \text{ is continuous on } [a,b] $$

보조명제에 의해 다음이 성립한다.

$$ G \text{ is continuous on } [a,b] $$

$F,G$ 모두 $[a,b]$에서 연속임으로 다음이 성립한다.

$$\begin{gathered} \lim_{x\rightarrow a^+} G(x) = \lim_{x\rightarrow a^+} F(x) + c \implies G(a) = F(a) + c \\ \lim_{x\rightarrow b^-} G(x) = \lim_{x\rightarrow b^-} F(x) + c \implies G(b) = F(b) + c \end{gathered} $$

따라서, 다음이 성립한다.

$$ \exist c \in \R \st \forall x\in [a,b], \quad  G(x) = F(x) + c \qed $$

##### 보조명제
다음을 증명하여라.

$$ G \text{ is continuous on } [a,b] $$


**Proof**

Fundamental theorem of Calculus1에 의해 $G$가 (a,b)에서 differentiable함으로 다음이 성립한다.

$$ G(x) \text{ is continuous on } (a,b) $$

이제 $x=a,x=b$에서 연속인지 살펴보자.

정적분에 성질에 의해 다음이 성립한다.(proof)

$$ 0 \le \lim_{h\rightarrow0} \int^{x+h}_x f(t)\thinspace dt \le 0 $$

Squeeze theorem에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0} \int^{x+h}_x f(t)\thinspace dt = 0 $$

따라서, 다음이 성립한다.

$$ \begin{gathered} \lim_{x\rightarrow a^+} G(x) = \lim_{h\rightarrow 0^+} G(a+h) = G(a) + \lim_{h\rightarrow 0^+} \int^{a+h}_a f(t)\thinspace dt = G(a) \\ \lim_{x\rightarrow b^-} G(x) = \lim_{h\rightarrow 0^-} G(b+h) = G(b) + \lim_{h\rightarrow 0^-} \int^{a+h}_a f(t)\thinspace dt = G(b) \end{gathered}  $$

위의 결과를 종합하면 다음이 성립한다.

$$ G \text{ is continuous on } [a,b] $$


## Fundamental Theorem of Calculus2
$[a,b]\subset\R$에서 continuous하고 $(a,b)$에서 differentiable한 함수 $f$가 있다고 하자.

$x_1,x_2 \in [a,b]$일 떄, 다음을 증명하여라.

$$ \int^{x_2}_{x_1} f'(x) \thinspace dx = f(x_2) - f(x_1) $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Fundamental_theorem_of_calculus#Formal_statements)


---

### 보조명제2
다음을 증명하여라.

$$ F \text{ is continuous on } [a,b] $$

**Proof**

$F$가 (a,b)에서 differentiable함으로 다음이 성립한다.

$$ F(x) \text{ is continuous on } (a,b) $$

이제 $x=a,x=b$에서 연속인지 살펴보자.

$f(x_m)h \le \int^{x+h}_x f(t)\thinspace dt \le f(x_M)h$임으로 다음이 성립한다.

$$ 0 \le \lim_{h\rightarrow0} \int^{x+h}_x f(t)\thinspace dt \le 0 $$

Squeeze theorem에 의해 다음이 성립한다.

$$ \lim_{h\rightarrow0} \int^{x+h}_x f(t)\thinspace dt = 0 $$

따라서, 다음이 성립한다.

$$ \begin{gathered} \lim_{x\rightarrow a^+} F(x) = \lim_{h\rightarrow 0^+} F(a+h) = F(a) + \lim_{h\rightarrow 0^+} \int^{a+h}_a f(t)\thinspace dt = F(a) \\ \lim_{x\rightarrow b^-} F(x) = \lim_{h\rightarrow 0^-} F(b+h) = F(b) + \lim_{h\rightarrow 0^-} \int^{a+h}_a f(t)\thinspace dt = F(b) \end{gathered}  $$

위의 결과를 종합하면 다음이 성립한다.

$$ F \text{ is continuous on } [a,b] \qed $$

