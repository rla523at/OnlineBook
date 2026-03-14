# Convergence of Sequence
## 정의
$\R$위의 sequence $s$가 있다고 하자.

$s$가 $x \in \R$에 수렴한다는 말은 다음과 같다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies |x - s(n)| < \epsilon $$

### 참고
sequence $s(n)$은 $s_n$으로 표기하기도 한다.


### 명제1
$\R$위의 sequence $s$가 다음과 같이 주어졌다고 하자.

$$ s(n) = \frac{1}{n} $$

이 떄, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty}s(n) = 0 $$

**Proof**

임의의 $\epsilon\in\R^+$가 있다고 하자.

$\epsilon/2 \in \R^+$임으로 Archimedean property에 의해 다음이 성립한다.

$$ \exist N \in \N \st \frac{1}{N} < \frac{\epsilon}{2} $$

위를 만족하는 $N$에 대해 다음이 성립한다.

$$ \forall n > N, \quad |s(n)-0| = \frac{1}{n} < \frac{1}{N} < \frac{\epsilon}{2} < \epsilon $$

임의의 $\epsilon$에 대해서 위가 성립함으로, 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st \forall n > N, \quad |s(n)| < \epsilon $$

따라서 convergece의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty}s(n) = 0 \qed $$

### 명제2
$\R$위의 sequence $s$가 다음과 같이 주어졌다고 하자.

$$ s(n) = 1+(-1)^n $$

이 떄, 다음을 증명하여라.

$$ s(n) \text{ is diverge} $$

**Proof**

$s(n)$의 정의에 의해 다음이 성립한다.

$$ \forall n\in \N, \quad |s(n) - s(n+1)| = 2 $$

따라서, 임의의 $n\in\N, \enspace x\in\R$에 대해서 다음이 성립한다.

$$ \begin{aligned} |s(n) - s(n+1)| &= |s(n) - x + x - s(n+1)| \\&\le |s(n) - x| + |x - s(n+1)| \\&= |s(n) - x| + |s(n+1) -x| \\ 2 &\le |s(n) - x| + |s(n+1) -x| \end{aligned} $$

이 때, 다음을 가정하자.

$$ s(n) \text{ is converge } $$

그러면 다음이 성립한다.

$$ \exist x \in \R \st \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies |s(n) - x| < \epsilon $$

따라서, $\epsilon_1 < 1$에 대해서 다음이 성립한다.

$$ \exist N_1 \st N_1 \le n \implies |s(n) - x| < \epsilon_1 $$

그러면 다음이 성립한다.

$$ N_1 \le n \implies |s(n) - x | + |s(n+1)-x| < 2\epsilon_1 < 2 $$

이는 $s(n)$의 정의에 의해 유도된 성질에 모순됨으로 proof by contradiction에 의해 다음이 성립한다.

$$ s(n) \text{ is diverge} \qed $$

### 명제3
$\R$위의 convergent sequence $s$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty} s(n) \text{ is unique} $$

**Proof**

$\lim_{n\rightarrow\infty} s(n) = x_1,x_2$라고 하자.

임의의 $\epsilon\in\R^+$에 대해 $\epsilon/2 \in \R^+$임으로 convergent sequence의 정의에 의해 다음이 성립한다.

$$ \exist N_1,N_2 \in \N \st \begin{gathered} N_1 \le n \implies |s(n) - x_1| < \frac{\epsilon}{2} \\ N_2 \le n \implies |s(n) - x_2| < \frac{\epsilon}{2} \end{gathered}  $$

그러면 다음이 성립한다.

$$ \max(N_1,N_2) \le n \implies |x_1-x_2| < |x_1 - s(n)| + |x_2 - s(n)| < \epsilon $$

임의의 $\epsilon\in\R^+$에 대해 위가 성립함으로, 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad |x_1-x_2| < \epsilon $$

따라서, $\R$의 성질에 의해 다음이 성립한다.

$$ x_1 = x_2 \qed $$

### 명제4
$\R$위의 sequence $a,b$가 각 각 $\alpha,\beta \in \R$로 수렴한다고 하자.

이 떄, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty} (a_n+b_n) = \alpha + \beta $$

**Proof**

Triangle inequality에 의해 다음이 성립한다.

$$ |(\alpha+\beta) - (a_n+b_n)| \le |\alpha-a_n| + |\beta-b_n| $$

임의의 $\epsilon \in \R^+$가 있을 때, $\epsilon/2\in\R^+$이고 $a,b$가 convergent sequence임으로 다음이 성립한다.

$$ \exist N_1,N_2 \in \N \st \begin{gathered} N_1 \le n \implies |\alpha - a_n| < \frac{\epsilon}{2} \\ N_2 \le n \implies |\beta - b_n| < \frac{\epsilon}{2} \end{gathered} $$

따라서, 다음이 성립한다.

$$ \max(N_1,N_2) \le n \implies |(\alpha+\beta) - (a_n+b_n)| < \epsilon $$

임의의 $\epsilon\in\R^+$에 대해서 위가 성립함으로 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies |(\alpha+\beta) - (a_n+b_n)| < \epsilon $$

따라서, convergent sequence의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty} (a_n+b_n) = \alpha + \beta \qed $$

### 명제5
$\R$위의 sequence $a,b$가 각 각 $\alpha,\beta \in \R$로 수렴한다고 하자.

이 떄, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty} (a_n-b_n) = \alpha-\beta $$

**Proof**

Triangle inequality에 의해 다음이 성립한다.

$$ |(\alpha-\beta) - (a_n-b_n)| \le |\alpha-a_n| + |\beta-b_n| $$

임의의 $\epsilon \in \R^+$가 있을 때, $\epsilon/2\in\R^+$이고 $a,b$가 convergent sequence임으로 다음이 성립한다.

$$ \exist N_1,N_2 \in \N \st \begin{gathered} N_1 \le n \implies |\alpha - a_n| < \frac{\epsilon}{2} \\ N_2 \le n \implies |\beta - b_n| < \frac{\epsilon}{2} \end{gathered} $$

따라서, 다음이 성립한다.

$$ \max(N_1,N_2) \le n \implies |(\alpha-\beta) - (a_n-b_n)| < \epsilon $$

임의의 $\epsilon\in\R^+$에 대해서 위가 성립함으로 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies |(\alpha-\beta) - (a_n-b_n)| < \epsilon $$

따라서, convergent sequence의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty} (a_n-b_n) = \alpha-\beta \qed $$

### 명제6
$\R$위의 sequence $a$가 $\alpha \in \R$로 수렴한다고 하자.

이 떄, 다음을 증명하여라.

$$ \forall c \in \R, \quad  \lim_{n\rightarrow\infty} (ca_n) = c\alpha $$

**Proof**

절대값에 성질의해 다음이 성립한다.

$$ |c\alpha - ca_n| = |c||\alpha-a_n| $$

임의의 $\epsilon \in \R^+$가 있을 때, $\epsilon/|c|\in\R^+$이고 $a$가 convergent sequence임으로 다음이 성립한다.

$$ \exist N_1 \in \N \st N_1 \le n \implies |\alpha - a_n| < \frac{\epsilon}{|c|} $$

임의의 $\epsilon\in\R^+$에 대해서 위가 성립함으로 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies |c\alpha - ca_n| < \epsilon $$

따라서, convergent sequence의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty} (ca_n) = c\alpha \qed $$

### 명제7
$\R$위의 sequence $a,b$가 각 각 $\alpha,\beta \in \R$로 수렴한다고 하자.

이 떄, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty} a_nb_n = \alpha\beta $$

**Proof**

Triangle inequality에 의해 다음이 성립한다.

$$ \begin{aligned} |(\alpha\beta) - (a_nb_n)| &= |\beta(\alpha-a_n) + a_n(\beta-b_n)| \\&\le |\beta||\alpha-a_n| + |a_n||\beta-b_n| \end{aligned}  $$

이 떄, $a_n$이 convergent sequence임으로 다음이 성립한다.

$$ \exist a_M \in \R^+ \st \forall n \in \N, \quad |a_n| \le a_M $$

따라서, $M = \max(a_M,\beta)$라고 하면  다음이 성립한다.

$$ |(\alpha\beta) - (a_nb_n)| < |\beta||\alpha-a_n| + a_M|\beta-b_n| < M(|\alpha-a_n| + |\beta - b_n|) $$

임의의 $\epsilon \in \R^+$가 있을 때, $\frac{\epsilon}{2M}\in\R^+$이고 $a,b$가 convergent sequence임으로 다음이 성립한다.

$$ \exist N_1,N_2 \in \N \st \begin{gathered} N_1 \le n \implies |\alpha - a_n| < \frac{\epsilon}{2M} \\ N_2 \le n \implies |\beta - b_n| < \frac{\epsilon}{2M} \end{gathered} $$

따라서, 다음이 성립한다.

$$ \max(N_1,N_2) \le n \implies |(\alpha\beta) - (a_nb_n)| < \epsilon $$

임의의 $\epsilon\in\R^+$에 대해서 위가 성립함으로 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies |(\alpha\beta) - (a_nb_n)| < \epsilon $$

따라서, convergent sequence의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty} (a_nb_n) = \alpha\beta \qed $$

### 명제8
$\R$위의 sequence $a$와 $\R-\Set{0}$위의 sequence $b$가 각 각 $\alpha \in \R,\beta \in \R-\Set{0}$로 수렴한다고 하자.

이 떄, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty} \frac{a_n}{b_n} = \frac{\alpha}{\beta} $$

**Proof**

Triangle inequality에 의해 다음이 성립한다.

$$ \begin{aligned} \left|\frac{\alpha}{\beta} - \frac{a_n}{b_n}\right| &= \left|\frac{\alpha b_n -\beta a_n}{\beta b_n}\right| \\&= \left|\frac{\beta (\alpha-a_n) +\alpha(b_n - \beta)}{\beta b_n}\right| \\&\le \left|\frac{1}{b_n}\right||\alpha-a_n| + \left|\frac{\alpha}{\beta b_n}\right||\beta-b_n| \end{aligned}  $$

이 떄, $b_n$이 convergent sequence이고 $|\beta|/2\in\R^+$임으로 다음이 성립한다.

$$ \exist N_1 \in \N \st N_1 \le n \implies |\beta - b_n| \le \frac{|\beta|}{2} $$

절대값의 성질에 의해 다음이 성립한다.

$$ |\beta| - |b_n| \le |\beta-b_n| \le \frac{|\beta|}{2} \implies \frac{|\beta|}{2} \le |b_n| \iff \frac{1}{|b_n|} \le \frac{2}{|\beta|} $$

따라서, $M = \max(2/\beta,2\alpha/\beta^2)$이라고 하면 $N_1 \le n$에 대해 다음이 성립한다.

$$ \left|\frac{\alpha}{\beta} - \frac{a_n}{b_n}\right| \le \left|\frac{2}{\beta}\right||\alpha-a_n| + \left|\frac{2\alpha}{\beta^2}\right||\beta-b_n| \le M(|\alpha-a_n| + |\beta-b_n|) $$

임의의 $\epsilon \in \R^+$가 있을 때, $\frac{\epsilon}{2M}\in\R^+$이고 $a,b$가 convergent sequence임으로 다음이 성립한다.

$$ \exist N_2,N_3 \in \N \st \begin{gathered} N_2 \le n \implies |\alpha - a_n| < \frac{\epsilon}{2M} \\ N_3 \le n \implies |\beta - b_n| < \frac{\epsilon}{2M} \end{gathered} $$

따라서, 다음이 성립한다.

$$ \max(N_1,N_2,N_3) \le n \implies \left|\frac{\alpha}{\beta} - \frac{a_n}{b_n}\right| < \epsilon $$

임의의 $\epsilon\in\R^+$에 대해서 위가 성립함으로 다음이 성립한다.

$$ \forall \epsilon \in \R^+, \quad \exist N \in \N \st N \le n \implies \left|\frac{\alpha}{\beta} - \frac{a_n}{b_n}\right| < \epsilon $$

따라서, convergent sequence의 정의에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty}\frac{a_n}{b_n} = \frac{\alpha}{\beta}\qed $$

### 명제9(Monotone Convergence Theorem)
$\R$위의 sequence $a_n$과 $A = \img(a_n)$이 있다고 하자.

$a_n$이 monotone increasing이고 bounded above일 때, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty}a_n = \sup(A) $$

**Proof**

$a_n$이 bounded above sequence임으로 $\R$의 completness에 의해 다음이 성립한다.

$$ \exist\sup(A) $$

$\R$의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist a_N \in A \st \sup(A)-\epsilon < a_N < \sup(A) \\\implies& \forall\epsilon\in\R^+, \enspace \exist N\in\N \st |\sup(A)-a_N|<\epsilon \end{aligned} $$

이 때, $a_n$이 monotone increasing sequence이고 $\sup(A)$가 $a_n$의 upper bound이기 때문에 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist N\in\N \st N\le n \implies |\sup(A)-a_n|<\epsilon \\\implies& \lim_{n\rightarrow\infty}a_n = \sup(A) \qed \end{aligned} $$

### 명제10(Monotone Convergence Theorem)
$\R$위의 sequence $a_n$과 $A = \img(a_n)$이 있다고 하자.

$a_n$이 monotone decreasing이고 bounded below일 때, 다음을 증명하여라.

$$ \lim_{n\rightarrow\infty}a_n = \inf(A) $$

**Proof**

$a_n$이 bounded below sequence임으로 $\R$의 completness에 의해 다음이 성립한다.

$$ \exist\inf(A) $$

$\R$의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist a_N \in A \st \inf(A) \le a_N < \inf(A) + \epsilon \\\implies& \forall\epsilon\in\R^+, \enspace \exist N\in\N \st |\inf(A)-a_N|<\epsilon \end{aligned} $$

이 때, $a_n$이 monotone decreasing sequence이고 $\inf(A)$가 $a_n$의 lower bound이기 때문에 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist N\in\N \st N\le n \implies |\inf(A)-a_n|<\epsilon \\\implies& \lim_{n\rightarrow\infty}a_n = \inf(A) \qed \end{aligned} $$

#### 참고
극한값과 supremum, infimum은 원래 관계가 없는데, MCT를 통해  특별한 경우에는 이 둘이 같음 보여준다.


### 명제11
$\R$위의 convergent sequence $a$가 다음을 만족한다고 하자.

$$ \exist N_1 \in \N \st N_1 \le n \implies 0 \le a_n $$

이 떄, 다음을 증명하여라.

$$ 0 \le \lim_{\ninf} a_n $$

**Proof**

다음을 가정하자.

$$ \lim_{\ninf} a_n < 0 $$

$\lim_{\ninf} a_n = \alpha$라고 할 때, convergence의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist N\in\N \st N\le n \implies |\alpha - a_n| < \epsilon \\\implies& \exist N_2 \in \N \st N_2 \le n \implies |\alpha - a_n| < -\frac{\alpha}{2} \\\implies& \max(N_1,N_2) \le n \implies |\alpha - a_n| < -\frac{\alpha}{2} \end{aligned}  $$

이 떄, 가정과 전제에 의해 $\max(N_1,N_2) \le n$일 때, $\alpha - a_n < 0$임으로 다음이 성립한다.

$$ \max(N_1,N_2) \le n \implies 0 \le a_n < \frac{\alpha}{2}$$

이는 $\alpha <0$이라는 가정에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ 0 \le \lim_{\ninf} a_n \qed $$

### 명제11
$\R$위의 convergent sequence $a,b$가 다음을 만족한다고 하자.

$$ \exist N \in \N \st N\le n \implies a_n \le b_n $$

이 떄, 다음을 증명하여라.

$$ \lim_{\ninf} a_n \le \lim_{\ninf} b_n $$

**Proof**

convergent sequence의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall n\in\N, \enspace 0 \le b_n - a_n \\\implies& 0 \le \lim_{\ninf} (b_n - a_n) \\\implies& 0 \le \lim_{\ninf} b_n - \lim_{\ninf} a_n \\\implies& \lim_{\ninf} a_n \le \lim_{\ninf} b_n \qed  \end{aligned} $$












