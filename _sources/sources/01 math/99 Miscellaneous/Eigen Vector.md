# Eigenvector
vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있을 때, $T$의 `고유벡터(eigenvector)` $v$는 다음을 만족하는 벡터이다.
$$ v \in V - \{ 0_V \} \quad \land \quad T(v)  = \lambda v \quad \lambda \in \mathbb F$$

이 때, 스칼라 값 $\lambda$를 `고유값(eigenvalue)`라 한다.

### 참고1
vector space $V/ \mathbb F$와 $T \in L(V;V)$가 있고, $T$의 고유벡터와 고유값 $v, \lambda$가 있을 때, $T$의 정의역을 $S := \{ cv | c \in \mathbb F\} < V$로 `restriction`시킨 restriction map을 생각해보자.
$$ T|_S : S \rightarrow S \quad s.t. \quad cv \mapsto \lambda cv$$

이 경우에 $T|_S$는 상수 곱처럼 동작한다.

### 참고2
고유벡터의 정의에 의해 다음이 성립한다.
$$ (T - \lambda id)(v) = 0_V $$

즉, 새로운 linear map $T - \lambda id$에 대해 다음이 성립한다.
$$ v \in \ker(T-\lambda id) $$

### 명제1
vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있을 때, 다음을 증명하여라.
$$ \lambda \in \mathbb{F} \text{ is eigenvalue of } T \Leftrightarrow \det(T - \lambda id) = 0 $$

**Proof**

$\beta$를 $V$의 기저라고 하자.

[$\Rightarrow$]  
$$ \begin{aligned} & \exist v \in V - \{ 0_V \} \quad s.t. \quad (T - \lambda id)(v) = 0_V \\ \Rightarrow \enspace & \frak m^\beta_\beta(T - \lambda id) \frak m_\beta(v) = 0 \\ \Rightarrow \enspace & \det( m^\beta_\beta(T - \lambda id) ) = 0 \quad (\because \frak m_\beta(v) \neq 0) \\ \Rightarrow \enspace & \det(T-\lambda id) = 0 \quad {_\blacksquare}  \end{aligned} $$

[$\Leftarrow$]  
$$ \begin{aligned} & \det(T-\lambda id) = 0 \\ \Rightarrow \enspace &  \det(\frak m^\beta_\beta(T - \lambda id)) = 0 \\ \Rightarrow \enspace & \exist v \in V - \{ 0_V \} \quad s.t. \quad \frak m^\beta_\beta(T - \lambda id) \frak m_\beta(v) = 0 \\ \Rightarrow \enspace & \frak m^\beta_\beta(T) \frak m_\beta(v) = \lambda \frak m_\beta(v) \\ \Rightarrow \enspace & T(v) = \lambda v \quad {_\blacksquare}  \end{aligned}  $$

> Q. $\det(A)=0 \Rightarrow \exist x \neq 0 \quad s.t. \quad Ax = 0$임을 증명하여라.

### 명제2
$n$차원 vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있다.

$\beta_{\lambda^i}$가 eigenvalue $\lambda^i$와 관련된 eigenvectors의 집합일 떄, $i \neq j$이면 $\lambda^i \neq \lambda^j$라 하자.

이 때, 다음을 증명하여라.
$$ \beta_{\lambda^i} \cap \beta_{\lambda^j} = \empty $$

**Proof**

$\beta_{\lambda^i} \cap \beta_{\lambda^j} \neq \empty$라 가정하자.

$\beta \in \beta_{\lambda^i} \cap \beta_{\lambda^j}$라 하면 다음이 성립한다.
$$ \begin{aligned} & T(\beta) = \lambda^i\beta = \lambda^j\beta \\ \Rightarrow \enspace & (\lambda^i - \lambda^j)\beta = 0_V \end{aligned}  $$

$\lambda^i \neq \lambda^j$임으로 $\beta = 0_V$여야한다. 하지만, eigenvector의 정의와 모순이 발생한다.

따라서 가정에 의해 모순이 발생했음으로, $\beta_{\lambda^i} \cap \beta_{\lambda^j} = \empty$이다. $\quad {_\blacksquare}$

### 명제3
$n$차원 vector space $V/ \mathbb F$와 $T \in \text{End}(V)$가 있다.

$\beta_{\lambda^i}$가 eigenvalue $\lambda^i$와 관련된 eigenvectors의 집합일 떄, $i \neq j$이면 $\lambda^i \neq \lambda^j$라 하자.

이 때, 다음을 증명하여라.
$$ \bigcup_{i=1}^k \beta_{\lambda^i} \text{ is an linearly independent set.} $$

**Proof**

다음과 같이 가정하자.
$$ \beta = \bigcup_{i=1}^k \beta_{\lambda^i} $$

$$ \text{Where, } \beta_{\lambda^i} = \{ \beta_{i,1}, \cdots, \beta_{i,m_i} \} $$

$\beta$가 선형종속이라고 가정하고, 일반성을 잃지 않고 $\beta$의 subset중 가장 cardinallity가 작은 부분집합이 다음과 같다고 하자.
$$ \{\beta_{1,1}, \cdots, \beta_{l,m_l} \} $$

그러면, 다음이 성립한다.
$$  \begin{aligned} & \beta_{l,m_l} = a^{1,1}\beta_{1,1} + \cdots + a^{l,m_l-1}\beta_{l,m_l-1} \\ \Rightarrow \enspace & T(\beta_{l,m_l}) = T(a^{1,1}\beta_{1,1} + \cdots + a^{l,m_l-1}\beta_{l,m_l-1}) \\ \Rightarrow \enspace & \lambda^l \beta_{l,m_l} = \lambda^1 a^{1,1} \beta_1 + \cdots + \lambda^{l}a^{l, m_l - 1}\beta_{l, m_l - 1} \end{aligned} $$

동시에 다음도 성립한다.
$$ \begin{aligned} & \beta_{l,m_l} = a^{1,1}\beta_{1,1} + \cdots + a^{l,m_l-1}\beta_{l,m_l-1} \\ \Rightarrow \enspace &  \lambda^l\beta_{l,m_l} = \lambda^l a^{1,1}\beta_{1,1} + \cdots + \lambda^l a^{l,m_l-1}\beta_{l,m_l-1} \end{aligned} $$

두 식을 빼면 다음과 같다.
$$ 0_V = a^{1,1}(\lambda^l - \lambda^1)\beta_{1,1} + \cdots + a^{l-1,m_{l-1}}(\lambda^l - \lambda^{l-1})\beta_{l-1,m_{l-1}} $$

$\{\beta_{1,1}, \cdots, \beta_{l,m_l} \}$ 가장 cardinallity가 작은 linearly dependent set이였음으로, $\{\beta_{1,1}, \cdots, \beta_{l-1,m_{l-1}} \}$은 linear independent set이다.

따라서, 서로 다른 고유값을 가지고 있음으로 $a_1 = \cdots = a_{m-1} = 0$이되고 $\beta_{l,m_l} = 0_V$가 된다. 이 때, 고유값은 $0_V$가 아님으로 모순이 발생한다.

고유벡터의 집합이 선형종속이라 가정하였을 때 모순이 발생함으로, 고유벡터의 집합은 선형독립이다. $\quad {_\blacksquare}$