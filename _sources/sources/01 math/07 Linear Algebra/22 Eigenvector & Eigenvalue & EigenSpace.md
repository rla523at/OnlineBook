# Eigenvector & Eigenvalue
## Motivation
선형변환 $L_A: \R^2 \rightarrow \R^2 \st x \mapsto Ax$ 가 임의의 $m \in \R$ 에 대해서 다음과 같이 정의된다.

$$ A = \frac{1}{1+m^2} 
\begin{bmatrix}
  1-m^2 & 2m    \\
  2m    & m^2-1 \\
\end{bmatrix} $$

이 때, $\R^2$ 의 기저 $\beta$ 를 다음과 같이 정의하자.

$$ \beta = \Set{\beta_1, \beta_2} = \Set{ \begin{bmatrix} 1 \\ m \end{bmatrix},  \begin{bmatrix} m \\ -1 \end{bmatrix} } $$

그러면 다음이 성립한다.

$$[L_A]^\beta_\beta = \begin{bmatrix}
  1 & 0  \\
  0 & -1 \\
\end{bmatrix} $$

위의 예시에서도 알 수 있듯이, 선형변환의 행렬표현은 basis 의 선택에 의존하기 때문에 basis 를 잘 선택하기만 하면 행렬표현이 간단한 대각행렬로 표현이 가능해진다.

그러면 어떤 basis 를 선택해야 선형변환의 행렬표현식이 대각행렬이 되는지 알아보자.

임의의 $n$차원 vector space 를 $V/\F$ 라고 할 때, $T \in \End(V)$ 라고 하자. 이 때, $V/\F$ 의 어떤 basis $\beta$ 에 대해서 $[T]^\beta_\beta$ 가 대각행렬이 되기 위해서는 $[ [T(\beta_1)]_\beta, \cdots, [T(\beta_n)]_\beta]$ 가 대각행렬이 되어야 한다. 즉, $i \in [1,n]$ 일 때,  $T(\beta_i) = \lambda_i\beta_i, \quad \lambda_i \in \F$ 형태여야 된다.

위의 내용을 다시 한번 간단하게 정리하면, $T(\beta_i) = \lambda_i\beta_i, \quad \lambda_i \in \F$ 형태를 만족하는 $\beta_i$들로 이루어진 basis $\beta$가 존재한다고 하면 diagonal matrix로 $T$를 표현할 수 있게 된다.

$$ [T]_\beta^\beta = \begin{bmatrix} \lambda_1 &&0 \\ & \ddots & \\ 0&&\lambda_n \end{bmatrix} $$

### 참고1
예시로 주어진 $L_A$ 는 $y=mx$ 의 대칭점으로 변환시켜주는 선형변환이다.

그래서 $T_x : \R^2 \rightarrow \R^2 \st \begin{bmatrix} x \\ y \end{bmatrix} \mapsto \begin{bmatrix} x \\ -y \end{bmatrix}$ 는 $x$ 축 대칭 변환이고 $R_\theta$ 는 $\theta$ 만큼 회전시켜주는 회전변환 이라고 하면 $L_A = R_\theta T_x R_{-\theta}$ 로 표현된다.

이 의미를 살펴보면, $R_{-\theta}$ 는 basis 를 $\theta$ 만큼 돌림으로써 $y=mx$ 선이 돌아간 basis 의 $x$ 축이 되게 한다. 그다음 $T_x$ 를 적용시킴으로써 $y=mx$ 의 대칭점을 얻은 다음 다시 basis 를 $-\theta$ 만큼 돌려 원래 좌표계에서 대칭점의 좌표를 얻는것이다.

## Definition(Eigenvector & Eigenvalue)
Vector space $V/ \F$와 $T \in \End(V)$가 있을 때, $T$의 `고유벡터(eigenvector)` $v$는 다음을 만족하는 벡터이다.

$$ v \in V - \{ 0_V \} \st T(v)  = \lambda v \quad \lambda \in \F$$

이 때, 스칼라 값 $\lambda$를 $v$의 `고유값(eigenvalue)`라 한다.

### 참고
#### 1
Eigenvector $v$는 $T$의 의존해서 결정되는 vector이다. 따라서, $v$가 eigenvector라고 하면, 어떤 linear map의 eigenvector인지 명확해야 한다.

또한 eigenvalue $\lambda$는 $v$에 의존해서 결정되는 scalar이다. 따라서, $\lambda$가 eigenvalue라고 하면 어떤 eigenvector의 eigenvalue인지 명확해야 한다.

#### 2
$v$가 $T$의 eigenvector이고 $\lambda$가 $v$의 eigenvalue라고 하자.

$\F$의 임의의 element를 $a$라고 할 때, $T$는 linear map임으로 $T(av) = \lambda av$가 성립한다.

따라서, $av$도 eigenvector임으로 $V$의 subspace $S := \Set{ av | a \in \F}$는 eigenvector들로 이루어진 subspace이다.

#### 3
vector space $V/ \F$와 $T \in \End(V)$가 있고, $T$의 eigenvector $v$와 $v$의 eigenvalue $\lambda$가 있을 때, $T$의 정의역을 $S := \{ cv | c \in \F\} < V$로 `restriction`시킨 restriction map을 생각해보자.

$$ T|_S : S \rightarrow S \quad s.t. \quad cv \mapsto \lambda cv$$

이 경우에 $T|_S$는 상수 곱처럼 동작한다.

#### 4
$v \in \ker(T)$라고 하면 $T(v) = 0_V = 0_\F v$이다.

따라서, $v$는 $T$의 eigenvector이고 $v$의 eigenvalue는 $0_\F$이다.

#### 5
Eigenvector의 정의에 의해 다음이 성립한다.

$$ (T - \lambda id)(v) = 0_V $$

즉, 새로운 linear map $T - \lambda id$에 대해 다음이 성립한다.

$$ v \in \ker(T-\lambda id) $$

### 명제1
vector space $V/ \F$와 $T \in \End(V)$가 있을 때, 다음을 증명하여라.

$$ \lambda \in \mathbb{F} \text{ is eigenvalue of } T \Leftrightarrow \det(T - \lambda id) = 0 $$

**Proof**

$\beta$를 $V$의 기저라고 하자.

[$\Rightarrow$]  

$$ \begin{aligned} 
& \exist v \in V - \{ 0_V \} \quad s.t. \quad (T - \lambda id)(v) = 0_V \\ 
\Rightarrow \enspace & [(T - \lambda id)]^\beta_\beta [v]_\beta = 0 \\ 
\Rightarrow \enspace & \det( [(T - \lambda id)]^\beta_\beta ) = 0 \quad (\because [v]_\beta \neq 0) \\ 
\Rightarrow \enspace & \det(T-\lambda id) = 0 \quad {_\blacksquare}  
\end{aligned} $$

[$\Leftarrow$]  

$$ \begin{aligned} 
& \det(T-\lambda id) = 0 \\ 
\Rightarrow \enspace &  \det([(T - \lambda id)]^\beta_\beta) = 0 \\ 
\Rightarrow \enspace & \exist v \in V - \{ 0_V \} \quad s.t. \quad [(T - \lambda id)]^\beta_\beta [v]_\beta = 0 \\
\Rightarrow \enspace & [T]^\beta_\beta [v]_\beta = \lambda [v]_\beta \\
\Rightarrow \enspace & T(v) = \lambda v \quad {_\blacksquare}  
\end{aligned} $$

> Q. $\det(A)=0 \Rightarrow \exist x \neq 0 \quad s.t. \quad Ax = 0$임을 증명하여라.
> https://math.stackexchange.com/questions/2905852/there-exists-x-neq-0-such-that-ax-0-iff-det-a-0-or-det-a-neq

### 명제2
$n$차원 vector space $V/ \F$와 $T \in \End(V)$가 있다.

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
$n$차원 vector space $V/ \F$와 $T \in \End(V)$가 있다.

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