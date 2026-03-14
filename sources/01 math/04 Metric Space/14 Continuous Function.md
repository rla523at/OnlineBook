# Continuous Function
## 정의
Metric space $M_1,M_2$가 있다고 하자.

$X$가 $M_1$의 open set일 때, 함수 $f : X \rightarrow M_2$가  `연속 함수(continuous function)`이라는 말은 다음과 동치이다.

$$ \forall x \in X, \quad f \text{ is continuous at } x $$

> Reference  
> {cite}`hubbard` Chapter 1.5

### 명제1
Metric space $M_1,M_2$가 있다고 하자.

함수 $f : M_1 \rightarrow M_2$가  있을 때, 다음을 증명하여라

$$ f \text{ is a continuous function } \iff \text{preimage of every open subset is open} $$

**Proof**

[$\implies$]  
$M_2$의 임의의 open set을 $V$라 하면, open set의 성질에 의해 다음이 성립한다.

$$ \forall x \in \preimg(V), \quad \exist r \in \R^+ \st B_{M_2}(f(x),r) \subseteq V $$

그러면 전제에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall x \in \preimg(V), \quad \exist\delta \in \R^+ \st B_{M_1}(x,\delta) \subseteq \preimg(B_{M_2}(y,r)) \subseteq \preimg(V) \\ \implies& \preimg(V) \text{ is an open set of } M_1 \qed  \end{aligned}  $$

[$\impliedby$]  
$M_1$의 임의의 원소를 $x$라고 하자.

그러면 open ball의 성질에 의해 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad B_{M_2}(f(x),\epsilon) \text{ is an open set of } M_2 $$

이 때, 전제에 의해 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \preimg(B_{M_2}(f(x),\epsilon)) \text{ is an open set of } M_1 $$

따라서, open set의 성질에 의해 다음이 성립한다.

$$ \forall p \in \preimg(B_{M_2}(f(x), \epsilon)), \quad \exist\delta \in \R^+ \st B_{M_1}(p,\delta) \subseteq \preimg(B_{M_2}(f(x), \epsilon)) $$

이 때, $x \in \preimg(B_{M_2}(f(x),\epsilon))$임으로 다음이 성립한다.

$$ \begin{aligned} & \forall \epsilon \in \R^+, \quad \exist\delta \in \R^+ \st  B_{M_1}(x,\delta) \subseteq \preimg(B_{M_2}(f(x),\epsilon)) \\ \iff& f \text{ is continuous at } x \end{aligned} $$

$M_1$의 임의의 원소에서 $f$가 연속임으로 다음이 성립한다.

$$ f \text{ is continous function } \qed $$

> Reference  
> {cite}`LeeTM` p.399

#### 참고
continuos function $f$가 있다고 하자.

이 때, open set의 image는 일반적으로 open set이 아니다.

예를 들어, $y =  x^2$을 생각해보자. 

open set$(-1,1)$의 image는$[0,1)$로 open set이 아니다.

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/1074769/image-of-open-set-is-not-open)  

### 명제2
함수 $f$가 다음과 같이 주어졌다고 하자.

$$ f: \R^2 \rightarrow  \R \st x \mapsto x_1+x_2 $$

이 떄, 다음을 증명하여라.

$$ f \text{ is continuous} $$

**Proof**

$\R^2$의 임의의 element를 $x$라 하자.

$\norm{x-t} < \delta$를 만족하는 $t \in \R^2$에 대해서 다음이 성립한다.

$$ \begin{aligned} & \sqrt{(x_1-t_1)^2+(x_2-t_2)^2} < \delta \\\implies& |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta \end{aligned}  $$

따라서, 다음이 성립한다.

$$ \begin{aligned} |f(x) - f(t)| &= |(x_1 + x_2)-(t_1+t_2)| \\&= |(x_1-t_1) + (x_2-t_2)| \\&< |x_1-t_1| + |x_2-t_2| \\&< \sqrt{2}\delta \end{aligned}  $$

이 떄, $\delta$를 다음과 같이 정의하자

$$ \delta := \frac{\epsilon}{\sqrt{2}} $$

그러면 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \norm{x-t} < \delta \implies |f(x) - f(t)| < \epsilon $$

따라서, $f$는 임의의 점 $x$에서 continuous함으로 다음이 성립한다.

$$ f \text{ is continuous} \qed $$

### 명제3
함수 $f$가 다음과 같이 주어졌다고 하자.

$$ f: \R^2 \rightarrow  \R \st x \mapsto x_1-x_2 $$

이 떄, 다음을 증명하여라.

$$ f \text{ is continuous} $$

**Proof**

$\R^2$의 임의의 element를 $x$라 하자.

$\norm{x-t} < \delta$를 만족하는 $t \in \R^2$에 대해서 다음이 성립한다.

$$ \begin{aligned} & \sqrt{(x_1-t_1)^2+(x_2-t_2)^2} < \delta \\\implies& |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta \end{aligned}  $$

따라서, 다음이 성립한다.

$$ \begin{aligned} |f(x) - f(t)| &= |(x_1 - x_2)-(t_1-t_2)| \\&= |(x_1-t_1) - (x_2-t_2)| \\&< |x_1-t_1| + |x_2-t_2| \\&< \sqrt{2}\delta \end{aligned}  $$

이 떄, $\delta$를 다음과 같이 정의하자

$$ \delta := \frac{\epsilon}{\sqrt{2}} $$

그러면 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \norm{x-t} < \delta \implies |f(x) - f(t)| < \epsilon $$

따라서, $f$는 임의의 점 $x$에서 continuous함으로 다음이 성립한다.

$$ f \text{ is continuous} \qed $$

### 명제4
함수 $f$가 다음과 같이 주어졌다고 하자.

$$ f: \R^2 \rightarrow  \R \st x \mapsto x_1x_2 $$

이 떄, 다음을 증명하여라.

$$ f \text{ is continuous} $$

**Proof**

$\R^2$의 임의의 element를 $x$라 하자.

$f$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} |f(x)-f(t)| &= |x_1x_2-t_1t_2| \\&= |x_2(x_1-t_1)+t_1(x_2-t_2)| \\&\le |x_2||x_1-t_1|+|t_1||x_2-t_2| \end{aligned} $$

이 때, $\norm{x-t} < \delta$를 만족하는 $t \in \R^2$에 대해서 보조명제4.1,2에 의해 다음이 성립한다.

$$ \begin{gathered} |t_1| < |x_1| +\delta \\ |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta \end{gathered}  $$

$M = \max(|x_2|,|x_1|+\delta)$라 하면 다음이 성립한다.

$$ |f(x)-f(t)|< \sqrt{2}M\delta $$

이 떄, $\delta$를 다음과 같이 정의하자.

$$ \delta := \frac{\epsilon}{\sqrt{2}M} $$

그러면 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \norm{x-t} < \delta \implies |f(x) - f(t)| < \epsilon $$

$f$는 임의의 점 $x$에서 continuous함으로 다음이 성립한다.

$$ f \text{ is continuous} \qed $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/2136639/show-that-the-function-fx-y-xy-is-continuous)

#### 보조명제4.1
다음을 증명하여라.

$$ \norm{x-t} < \delta \implies |t_1| < |x_1| +\delta $$

**Proof**

$$ \begin{aligned} & (x_1-t_1)^2 < (x_1-t_1)^2 + (x_2-t_2)^2 < \delta^2 \\ \implies& |x_1-t_1| < \delta \\ \implies& |t_1| < |x_1| + \delta \qed  \end{aligned}  $$

#### 보조명제4.2
다음을 증명하여라.

$$ \norm{x-t} < \delta \implies |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta $$

**Proof**

$$ \begin{aligned} & \sqrt{(x_1-t_1)^2+(x_2-t_2)^2} < \delta \\ \implies& \frac{1}{\sqrt{2}} (|x_1-t_1|+|x_2-t_2|) < \delta \\\implies& |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta \qed  \end{aligned}  $$

### 명제5
$\R^2_*$를 다음과 같이 정의하자.

$$ \R^2_* := \Set{x \in \R^2 | x_2 \neq 0} $$

함수 $f$가 다음과 같이 주어졌다고 하자.

$$ f: \R^2_* \rightarrow  \R \st x \mapsto x_1/x_2 $$

이 떄, 다음을 증명하여라.

$$ f \text{ is continuous} $$

**Proof**

$\R^2_*$의 임의의 element를 $x$라 하자.

$f$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} |f(x)-f(t)| &= \left|\frac{x_1}{x_2}-\frac{t_1}{t_2}\right| \\&= \left|\frac{x_1t_2-t_1x_2}{x_2t_2}\right| \\&= \left|\frac{x_2(x_1-t_1)-x_1(x_2-t_2)}{x_2t_2}\right| \\&\le \frac{1}{|t_2|}|x_1-t_1| + \frac{|x_1|}{|x_2t_2|}|x_2-t_2| \end{aligned} $$

$\norm{x-t} < \delta$를 만족하는 $t \in \R^2_*$에 대해서 보조명제 5.1,5.2에 의해서 다음이 성립한다.

$$ \begin{gathered} \frac{1}{|t_2|} < \frac{1}{|x_2| - \delta} \\ |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta \end{gathered} $$

따라서, $M = \max(\frac{1}{|x_2|-\delta},\frac{|x_1|}{(|x_2|-\delta)|x_2|})$이라 하면 다음이 성립한다.

$$ |f(x)-f(t)| < \sqrt{2}M\delta $$

이 떄, $\delta$를 다음과 같이 정의하자.

$$ \delta := \frac{\epsilon}{\sqrt{2}M} $$

그러면 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \norm{x-t} < \delta \implies |f(x) - f(t)| < \epsilon $$

$f$는 임의의 점 $x$에서 continuous함으로 다음이 성립한다.

$$ f \text{ is continuous} \qed $$

#### 보조명제5.1
다음을 증명하여라.

$$ \norm{x-t} < \delta \implies \frac{1}{|t_2|} < \frac{1}{|x_2| - \delta} $$

**Proof**

$$ \begin{aligned} & |x_2-t_2| < \delta \\ \implies& |x_2| - \delta < |t_2| \\\implies& \frac{1}{|t_2|} < \frac{1}{|x_2| - \delta} \qed  \end{aligned}  $$

#### 보조명제5.2
다음을 증명하여라.

$$ \norm{x-t} < \delta \implies |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta $$

**Proof**

$$ \begin{aligned} & \sqrt{(x_1-t_1)^2+(x_2-t_2)^2} < \delta \\ \implies& \frac{1}{\sqrt{2}} (|x_1-t_1|+|x_2-t_2|) < \delta \\\implies& |x_1-t_1| + |x_2-t_2| < \sqrt{2}\delta \qed  \end{aligned}  $$

### 명제6
Metric space $M$과 함수 $f,g : M \rightarrow \R$이 있다고 하자.

$f,g$가 continuous function일 떄, 다음을 증명하여라.

$$ f+g \text{ is continuous} $$

**Proof**

$M$의 임의의 element를 $x$라하고 $\R^+$의 임의의 element를 $\epsilon$이라고 하자.

$f,g$가 연속이고 $\epsilon/2 \in \R^+$임으로 $t \in M$에 대해서 다음이 성립한다.

$$ \begin{gathered} \exist \delta_1 \in \R^+ \st d(x,t) < \delta_1 \implies |f(x)-f(t)| < \frac{\epsilon}{2} \\ \exist \delta_2 \in \R^+ \st d(x,t) < \delta_2 \implies |g(x)-g(t)| < \frac{\epsilon}{2} \end{gathered} $$

$h = f+g$이라 할 때, Triangle inequality에 의해 다음이 성립한다.

$$ |h(x) - h(t)| = |f(x)+g(x)-(f(t)+g(t))| \le |f(x)-f(t)|+ |g(x)-g(t)| $$

그러면 $\delta_m = \min(\delta_1,\delta_2)$이라 할 때, 다음이 성립한다.

$$ d(x,t) < \delta_m \implies |h(x)-h(t)| < \epsilon $$

임의의 $x$에서 $h$가 continuous함으로 다음이 성립한다.

$$ f+g \text{ is continuous} \qed $$


### 명제7
Metric space $M$과 함수 $f,g : M \rightarrow \R$이 있다고 하자.

$f,g$가 continuous function일 떄, 다음을 증명하여라.

$$ fg \text{ is continuous} $$

**Proof**

$h=fg$라고 할 떄, $h$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} |h(x)-h(t)| &= |f(x)g(x)-f(t)g(t)| \\&= |g(x)(f(x)-f(t))+f(t)(g(x)-g(t))| \\&\le |g(x)||f(x)-f(t)|+|f(t)||g(x)-g(t)| \end{aligned} $$

이 떄, $f$가 continuous function임으로 다음이 성립한다.

$$ \exist \delta_1 \in \R^+ \st d(x,t) < \delta_1 \implies |f(x)-f(t)| < 1 \\ \implies f(t) < 1 + |f(x)| $$

$M = \max(|g(x)|,1+|f(x)|)$일 떄, 다음이 성립한다.

$$ |h(x)-h(t)| < M(|f(x)-f(t)| + |g(x)-g(t)|) $$

$\R^+$의 임의의 element를 $\epsilon$이라고 하자.

$\epsilon/2M \in \R^+$이고, $f,g$가 continuous function임으로 다음이 성립한다.

$$ \begin{gathered} \exist \delta_2 \in \R^+ \st d(x,t) < \delta_2 \implies |f(x)-f(t)| < \frac{\epsilon}{2M} \\ \exist \delta_3 \in \R^+ \st d(x,t) < \delta_3 \implies |g(x)-g(t)| < \frac{\epsilon}{2M} \end{gathered}  $$

따라서, $\delta_m = \min(\delta_1,\delta_2,\delta_3)$일 떄, 다음이 성립한다.

$$ d(x,t) < \delta_m \implies |f(x)g(x)-f(t)g(t)| < \epsilon $$

임의의 $x$에서 $h$가 continuous함으로 다음이 성립한다.

$$ fg \text{ is continuous} \qed $$