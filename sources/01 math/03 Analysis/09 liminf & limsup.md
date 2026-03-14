# Limit Inferior & Superior
## 정의
$\R$위의 bounded sequence $s$가 있다고 하자.

이 때, $\R$위의 sequence $a,b$을 다음과 같이 정의하자.

$$ \begin{gathered} a \st a_k := \inf(\Set{s_n | k \le n}) \\ b \st b_k := \sup(\Set{s_n | k \le n}) \end{gathered} $$

$s$의 `limit inferior`과 `limit superior`은 다음과 같이 정의한다.

$$ \begin{gathered} \liminf_{n\rightarrow\infty} s_n := \lim_{n\rightarrow\infty} a_n \\ \limsup_{\ninf} s_n := \lim_{n\rightarrow\infty} b_n  \end{gathered} $$

### 참고1
$s$가 bounded sequence이기 때문에 $\R$의 completeness에 의해 $a_k,b_k$가 모두 잘 정의된다.

### 참고2
정의에 의해 $a_n$은 monotone increasing bounded above sequence이고 $b_n$은 monotone decreasing bounded below sequence이다.

따라서, MCT에 의해 다음이 성립한다.

$$ \lim_{n\rightarrow\infty} a_n = \sup(a_n), \enspace \lim_{n\rightarrow\infty} b_n = \inf(b_n) $$

이런 이유로 다음과 같은 표기를 사용하기도 한다.

$$ \begin{gathered} \liminf_{\ninf} s_n = \sup_{1\le n}\inf_{n\le m} s_m \\ \limsup_{\ninf} s_n = \inf_{1\le n}\sup_{n\le m} s_m \end{gathered} $$

### 참고3
$s_n$이 convergence sequence가 아니더라도, limit inferior과 limit superior은 존재한다.

### 참고4
limit inferior은 개념적으로 inferior들 중 큰 값을 찾아가는 과정이라고 볼 수 있고, limit superior은 개념적으로 superior들 중 작은 값을 찾아가는 과정이라고 볼 수 있다.

### 명제1
$\R$위의 bounded sequence $s$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \liminf_{\ninf}s_n \le \limsup_{\ninf}s_n $$

**Proof**

sequence $a,b$을 다음과 같이 정의하자.

$$ \begin{gathered} a_k := \inf(\Set{s_n | k \le n}) \\ b_k := \sup(\Set{s_n | k \le n}) \end{gathered} $$

그러면 정의에 의해 다음이 성립한다.

$$\forall n\in\N, \enspace a_n \le b_n $$

따라서, convergent sequence의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{\ninf}a_n \le \lim_{\ninf} b_n \\\iff& \liminf_{\ninf}s_n \le \limsup_{\ninf}s_n \qed \end{aligned} $$

### 명제2
$\R$위의 bounded sequence $s$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \limsup_{\ninf}s_n = \beta \iff \forall\epsilon\in\R^+, \enspace \begin{gathered} \exist N\in\N \st N \le n \implies s_n < \beta + \epsilon \\ \forall n \in \N, \enspace \exist N\in\N \st n \le N \land \beta-\epsilon < s_N  \end{gathered} $$

**Proof**

$\R$위의 bounded sequence $b$를 다음과 같이 정의하자.

$$ b_n := \sup(\Set{s_k|n\le k}) $$

[$\implies$]  
-[1]  
limit supremum의 정의에 의해 다음이 성립한다.

$$ \lim_{\ninf}b_n = \beta $$

$b_n$은 monotone decreasing bounded sequence임으로, 다음이 성립한다.

$$ \lim_{\ninf}b_n = \inf(b) = \beta $$

따라서, infimum과 monotone decreasing에 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist N \in \N \st \beta \le b_N < \beta+\epsilon \\ \implies& \forall\epsilon\in\R^+, \enspace \exist N \in \N \st N \le n \implies \beta \le b_n < \beta+\epsilon  \end{aligned} $$

이 때, $b_n$의 정의에 의해 $s_n \le b_n$임으로 다음이 성립한다.

$$ \forall\epsilon\in\R^+, \enspace \exist N \in \N \st N \le n \implies s_n < \beta+\epsilon \qed $$

-[2]  
$b_n$은 monotone decreasing sequence임으로 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+,\enspace \forall n\in\N,\enspace \beta-\epsilon < \beta \le b_n \\\iff& \forall\epsilon\in\R^+,\enspace\forall n \in \N, \enspace \beta-\epsilon < \sup(\Set{s_k|n\le k}) \\\iff& \forall\epsilon\in\R^+,\enspace\forall n \in \N, \enspace \beta-\epsilon\text{ is not an upper bound of } \Set{s_k|n\le k} \\\iff& \forall\epsilon\in\R^+,\enspace\forall n \in \N,\enspace \exist N \in \N \st \enspace n \le N \land \beta-\epsilon < s_N \qed \end{aligned}  $$

[$\impliedby$]  
$\R^+$의 임의의 element를 $\epsilon$이라고 하자.

$\epsilon/2 \in \R^+$임으로 1번 조건에 의해 다음이 성립한다.

$$ \begin{aligned} & \exist N_1\in\N \st N_1\le n \implies s_n < \beta + \frac{\epsilon}{2} \\\iff& \beta + \frac{\epsilon}{2} \text{ is an upper bound of } \Set{s_k|N_1\le k} \\\iff& b_{N_1} \le \beta + \frac{\epsilon}{2} \end{aligned}  $$

이 때, 정의에 의해 $b$는 monotone decreasing sequence임으로 다음이 성립한다.

$$ N_1 \le n \implies b_n \le \beta + \frac{\epsilon}{2} \implies b_n < \beta+\epsilon $$

그리고 2번 조건에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall n \ge N_1, \enspace \exist N_2 \in \N \st n \le N_2 \land \beta - \epsilon < s_{N_2} \\\implies& \forall n\ge N_1,\enspace \exist s_* \in \Set{s_k |n \le k} \st \beta-\epsilon < s_* \\\iff& \forall n\ge N_1, \enspace \text{any upper bound of } \Set{s_k |n \le k} \text{ should be greater than } \beta-\epsilon \\\iff& \forall n\ge N_1, \enspace \beta-\epsilon < b_n \end{aligned}  $$

따라서, 다음이 성립한다.

$$ \begin{aligned} & \beta-\epsilon<b_n<\beta+\epsilon \\\iff& |\beta-b_n| < \epsilon \end{aligned} $$

위의 결과들이 임의의 $\epsilon\in\R^+$에 대해 성립함으로, 다음이 성립한다.

$$ \begin{aligned} &\forall\epsilon\in\R^+, \enspace \exist N \in \N \st N_1\le n \implies |\beta-b_n| < \epsilon \\\iff& \lim_{\ninf} b_n = \beta \\\iff& \limsup_{\ninf}s_n = \beta \qed \end{aligned} $$

#### 참고
$\limsup s_n = \beta$라는 것은 $\beta+\epsilon$이 $b$의 lower bound가 아니라는 사실과 $\beta-\epsilon$이 $\Set{s_i|n\le i}$의 upper bound가 아니라는 사실을 알려준다.

반대로 $\beta+\epsilon$이 $b$의 lower bound가 아니라는 사실과 $\beta-\epsilon$이 $\Set{s_i|n\le i}$의 upper bound가 아니라는 사실은 $\beta-\epsilon < b_n < \beta + \epsilon$이라는 사실을 알려준다.

### 명제3

$\R$위의 bounded sequence $s$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \liminf_{\ninf} s_n = \beta \iff \forall\epsilon\in\R^+, \enspace \begin{gathered} \exist N \in \N \st N \le n \implies \beta-\epsilon < s_n \\ \forall n\in\N, \enspace \exist N \in \N, \st n\le N \land s_N < \beta+\epsilon \end{gathered} $$

**Proof**

$\R$위의 sequence $a$를 다음과 같이 정의하자.

$$ a_n := \inf(\Set{s_k|n\le k}) $$

[$\implies$]  
-[1]  
limit infimum의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \sup(a) = \beta \\\iff& \forall\epsilon\in\R^+, \enspace\exist N\in\N \st \beta-\epsilon < a_N \le \beta  \end{aligned}  $$

$a$는 정의에 의해 monotone increasing sequence임으로 다음이 성립한다.

$$ \forall\epsilon\in\R^+, \enspace\exist N\in\N \st N\le n \implies  \beta-\epsilon < a_n $$

이 떄, $a_n$의 정의에 의해 $\forall n\in\N, \enspace a_n \le s_n$임으로 다음이 성립한다.

$$ \forall\epsilon\in\R^+, \enspace\exist N\in\N \st N\le n \implies  \beta-\epsilon < s_n \qed $$

-[2]  
$a$는 monotone increasing sequece임으로 다음이 성립한다.

$$ \begin{aligned} & \forall \epsilon \in \R^+, \enspace \forall n \in \N, \enspace a_n \le \beta < \beta + \epsilon \\\iff& \forall \epsilon \in \R^+, \enspace \forall n \in \N, \enspace \inf(\Set{s_k|n\le k}) < \beta+\epsilon \\\iff& \forall \epsilon \in \R^+, \enspace \forall n \in \N, \enspace \beta+\epsilon \text{ is not an lower bound of } \Set{s_k|n\le k} \\\iff& \forall \epsilon \in \R^+, \enspace \forall n \in \N, \enspace \exist N\in\N \st n \le N \land s_N < \beta + \epsilon \qed  \end{aligned} $$

[$\impliedby$]  
$\R$의 임의의 element를 $\epsilon$이라고 하자.

$\epsilon/2 \in \R$임으로 전제1에 의해 다음이 성립한다.

$$ \begin{aligned} &  \exist N\in\N \st N \le n \implies \beta-\frac{\epsilon}{2} < s_n \\\iff& \beta-\frac{\epsilon}{2} \text{ is an lower bound of } \Set{s_k|N\le k} \\\iff& \beta-\frac{\epsilon}{2} \le a_N \\\iff& \beta-\epsilon < a_N \end{aligned}  $$

$a$는 monotone increasing sequence임으로 다음이 성립한다.

$$ \exist N \in \N \st  N \le n \implies \beta-\epsilon < a_n $$

그리고 전제2에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall n \ge N,\enspace \exist N_2\in\N \st s_{N_2} < \beta + \epsilon \\\iff& \forall n\ge N, \enspace \exist s_* \in \Set{s_k|n\le k} \st s_* < \beta+\epsilon \\\iff& \forall n\ge N, \enspace \text{every lower bound of } \Set{s_k|n\le k} \text{should be less than } \beta+\epsilon \\\iff& \forall n \ge N, \enspace a_n < \beta+\epsilon \end{aligned} $$

따라서, 다음이 성립한다.

$$ \begin{aligned} &\beta-\epsilon<a_n<\beta+\epsilon \\\iff& |\beta-a_n| < \epsilon \end{aligned}  $$

임의의 $\epsilon$에 대해서 위의 결과들이 성립함으로, 다음이 성립한다.

$$ \begin{aligned} & \forall\epsilon\in\R^+, \enspace \exist N\in\N \st N\le n \implies |\beta-a_n|<\epsilon \\\iff& \lim_{\ninf}a_n = \beta \\\iff& \liminf_{\ninf}s_n = \beta \qed \end{aligned} $$

### 명제4
$\R$위의 bounded sequence $s$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \liminf_{\ninf}s_n = \limsup_{\ninf}s_n = \beta \iff \lim_{\ninf}s_n = \beta $$

**Proof**

[$\implies$]  
$\R$위의 sequence $a,b$를 다음과 같이 정의하자.

$$ a_n := \inf(\Set{s_k|n\le k}), \enspace b_n := \sup(\Set{s_k|n\le k}) $$

그러면 $a,b$의 정의에 의해 다음이 성립한다.

$$ \forall n\in\N, \enspace a_n \le s_n \le b_n $$

전제에 의해 $\lim_{\ninf} a,b = \beta$임으로 sandwitch theorem에 의해 다음이 성립한다.

$$ \lim_{\ninf}s_n = \beta \qed $$

[$\impliedby$]  
전제에 의해 다음이 성립한다.

$$ \forall\epsilon\in\R^+, \enspace \exist N\in\N \st N \le n \implies \beta-\epsilon < s_n < \beta+\epsilon $$

명제2,3에 의해 다음이 성립한다.

$$ \liminf_{\ninf}s_n = \limsup_{\ninf}s_n = \beta \qed $$

### 명제5
$\R$위의 bounded sequence $s$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \liminf_{\ninf}s_n = -\limsup_{\ninf}(-s_n) $$

**Proof**

$\R$위의 sequence $a,b$를 다음과 같이 정의하자.

$$ \begin{gathered} a_n := \inf(\Set{s_k|n\le k}) \\ b_n := \sup(\Set{-s_k|n\le k}) \end{gathered} $$


임의의 $n \in \N$이 있다고 하자.

$\Set{s_k|n\le k}$의 lower bound들의 set을 $S$라고 한다면 $-S$는 $\Set{-s_k|n\le k}$의 upper bound가 된다.

따라서, $\inf(S)$는 $\sup(-S)$는 다음을 만족한다.

$$ \begin{aligned}& \inf(S) = -\sup(-S) \\\implies& a_n = -b_n \end{aligned}  $$

따라서, limit infimum과 limit supremum의 정의에 의해 다음이 성립한다.

$$ \liminf_{\ninf}s_n = -\limsup_{\ninf}(-s_n) \qed $$



### 명제6
$\R$위의 bounded sequence $s,t$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \liminf_{\ninf} s_n + \liminf_{\ninf} t_n \le \liminf_{\ninf} (s_n+t_n) $$

**Proof**

$\R$위의 sequence $a,b,c$를 다음과 같이 정의하자.

$$ \begin{gathered} a_n := \inf(\Set{s_k|n\le k}) \\ b_n := \inf(\Set{t_k|n\le k}) \\ c_n := \inf(\Set{s_k+t_k|n\le k}) \end{gathered}  $$

$a,b$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned}& \forall n\in\N, \enspace a_n \le s_n \land b_n \le t_n \\\implies& \forall n\in\N, \enspace a_n + b_n \le s_n + t_n \\\implies& \forall n\in\N, \enspace \inf(\Set{a_k|n \le k}) + \inf(\Set{a_k|n \le k}) \le c_n \end{aligned}   $$

정의에 의해 $a,b$는 monotone inreasing sequence임으로 다음이 성립한다.

$$ \forall n\in\N, \enspace a_n + b_n \le c_n $$

따라서, convergent sequence의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{\ninf} a_n +  \lim_{\ninf} b_n \le \lim_{\ninf} c_n \\\iff& \liminf_{\ninf} s_n + \liminf_{\ninf} t_n \le \liminf_{\ninf} (s_n+t_n) \qed \end{aligned} $$

> Reference  
> [blog](http://mathonline.wikidot.com/properties-of-the-limit-superior-inferior-of-a-sequence-of-r)

### 명제7
$\R$위의 bounded sequence $s,t$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \limsup_{\ninf} (s_n+t_n) \le \limsup_{\ninf} s_n + \limsup_{\ninf} t_n $$

**Proof**

$\R$위의 sequence $a,b,c$를 다음과 같이 정의하자.

$$ \begin{gathered} a_n := \sup(\Set{s_k|n\le k}) \\ b_n := \sup(\Set{t_k|n\le k}) \\ c_n := \sup(\Set{s_k+t_k|n\le k}) \end{gathered}  $$

$a,b$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned}& \forall n\in\N, \enspace s_n \le a_n \land t_n \le b_n \\\implies& \forall n\in\N, \enspace s_n + t_n \le a_n + b_n \\\implies& \forall n\in\N, \enspace c_n \le \sup(\Set{a_k|n \le k}) + \sup(\Set{a_k|n \le k}) \end{aligned}   $$

정의에 의해 $a,b$는 monotone decreasing sequence임으로 다음이 성립한다.

$$ \forall n\in\N, \enspace c_n \le a_n + b_n $$

따라서, convergent sequence의 성질에 의해 다음이 성립한다.

$$ \begin{aligned} & \lim_{\ninf} c_n \le \lim_{\ninf} a_n + \lim_{\ninf} b_n \\\iff& \limsup_{\ninf} (s_n+t_n) \le \limsup_{\ninf} s_n + \limsup_{\ninf} t_n \qed \end{aligned} $$

### 명제8
$\R$위의 bounded sequence $s,t$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \liminf_{\ninf} (s_n+t_n) \le \liminf_{\ninf} s_n + \limsup_{\ninf} t_n $$

**Proof**

명제6에 의해 다음이 성립한다.

$$ \begin{aligned} \liminf_{\ninf}(s_n) &= \liminf_{\ninf}(s_n + t_n - t_n) \\&\ge \liminf_{\ninf}(s_n+t_n) + \liminf_{\ninf} (-t_n) \end{aligned} $$

따라서, 명제5에 의해 다음이 성립한다.

$$ \begin{aligned} \liminf_{\ninf}(s_n+t_n) - \limsup_{n\infty} t_n &\le \liminf_{\ninf}(s_n) \\ \liminf_{\ninf}(s_n+t_n) &\le \liminf_{\ninf}(s_n) + \limsup_{n\infty} t_n \qed \end{aligned} $$

> Reference  
> [note](https://www.isibang.ac.in/~creraja/Ana1_11/mex.pdf)

### 명제9
$\R$위의 bounded sequence $s,t$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \liminf_{\ninf} s_n + \limsup_{\ninf} t_n \le \limsup_{\ninf} (s_n+t_n) $$

**Proof**

명제8에 의해 다음이 성립한다.

$$ \begin{aligned} \limsup_{\ninf}(t_n) &= \limsup_{\ninf}(t_n + s_n - s_n) \\&\le \limsup_{\ninf}(t_n+s_n) + \limsup_{\ninf} (-s_n) \end{aligned} $$

따라서, 명제5에 의해 다음이 성립한다.

$$ \begin{aligned} \limsup_{n\infty} t_n &\le \liminf_{\ninf}(s_n+t_n) - \liminf_{\ninf}(s_n) \\ \liminf_{\ninf}(s_n) + \limsup_{n\infty} t_n &\le \liminf_{\ninf}(s_n+t_n) \qed \end{aligned} $$