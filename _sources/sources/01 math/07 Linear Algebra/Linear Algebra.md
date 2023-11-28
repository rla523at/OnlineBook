# Linear Algebra


## sum
유한차원 벡터공간 $V / \mathbb F$와 $U,W \le V$가 있을 때, $U$와 $W$의 `합(sum)` $U+W$는 다음과 같이 정의된다.
$$ U+W := \{ u + w \enspace | \enspace u \in U, w \in W \} $$

### 명제1
유한차원 벡터공간 $V / \mathbb F$와 $U,W \le V$가 있을 때, $U+W \le V$임을 증명하여라.

**Proof**

[기본 연산 법칙]  
벡터 공간의 부분집합과 연산을 그대로 사용하기 때문에 교환법칙 분배법칙등이 $F-$가군의 성질들이 전부 성립한다. 

[연산에 닫힘]  
$T_1, T_2 \in L(V,W)$, $a \in \Bbb F$라 하자.

$v_1,v_2 \in V$, $b \in \mathbb{F}$가 있을 때 다음이 성립한다.
$$ \begin{aligned} (aT_1 + T_2)(bv_1 + v_2) &= aT_1(bv_1 + v_2) + T_2(bv_1 + v_2) \\ &= abT_1(v_1) + bT_2(v_1) + aT_1(v_2) + T_2(v_2) \\ &= b(aT_1+T_2)(v_1) + (aT_1+T_2)(v_2) \end{aligned}  $$

따라서, $(aT_1 + T_2)$도 $V$에서 $V$로 가는 선형변환임으로 연산에 닫혀있다. $\quad {_\blacksquare}$

[$+$연산 항등원의 존재성]  
영함수가 다음과 같이 정의되어 있다고 하자.
$$ T_0 : V \rightarrow W \quad s.t. \quad v \mapsto 0_W $$



### 명제2
유한차원 벡터공간 $V / \mathbb F$와 $U,W \le V$가 있을 때, $U \le U+W$와 $W \le U+W$임을 증명하여라.

## direct sum
유한차원 벡터공간 $V / \mathbb F$와 $U,W \le V$가 있을 때, $U \cap W = \{ 0_V \}$면 $U$와 $W$의 `direct sum` $U \oplus W$는 다음과 같이 정의된다.
$$ U \oplus W := \{ u + w \enspace | \enspace u \in U, w \in W \} $$

### 참고
direct sum은 여러항으로 다음과 같이 확장이 가능하다.

$U_i \le V \enspace i = 1, \cdots, k$가 있을 때, $i = 1, \cdots, k$에 대해 $U_i \cap \bigcup_{\begin{subarray}{l} j = 1 \\ j \neq i \end{subarray}}^k U_j = \{ 0_V \}$면 여러항의 direct sum은 다음과 같이 정의 된다.

$$ \bigoplus_{i=1}^k U_i = \{ u_1 + \cdots + u_k  \enspace | \enspace u_i \in U_i \enspace i = 1, \cdots, k \} $$

### 명제1
유한차원 벡터공간 $V / \mathbb F$와 $W_1, \cdots, W_n \le V$가 있을 때, 다음을 증명하여라.
$$V = W_1 \oplus \cdots \oplus W_n \Rightarrow v \in V, \quad \exist! w_i \in W_i \quad s.t. \quad  v = w_1 + \cdots + w_n$$

**Proof**

$V = W_1 \oplus \cdots \oplus W_n$임으로 $\exist w_i \in W_i \quad s.t \quad v = w_1 + \cdots + w_n$는 자명하다. 

$\exist q_i \in W_i \quad s.t \quad v = q_1 + \cdots + q_n$라 하자. 두 표현식을 빼면 다음과 같다.
$$ 0_V = (w_1 - q_1) + \cdots + (w_n - q_n) $$

$W_i \le V$임으로 $(w_i-q_i) \in W_i$면 $(w_i-q_i)^{-1} \in W_i$이다. 

또한 $W_i \cap W_j = \{ 0_V \}$임으로 $(w_i-q_i)^{-1} \notin W_j$이다. 

즉, $(w_2 - q_2) + \cdots + (w_n - q_n)$은 $(w_1 - q_1)$의 역원이 될 수 없고 식을 만족하기 위해서는 $(w_1 - q_1) = 0_V$이어야 한다. 

마찬가지로 $w_i - q_i = 0_V$임으로 $w_i = q_i$이다. 즉, $v = w_1 + \cdots + w_n$를 만족하는 $w_i \in W_i$는 유일하다.










## 차원
벡터공간 $V/ \mathbb F$의 `차원(dimension)`은 기저 `집합의 크기(cardinality)`이다.

만약 $\dim(V) < \infty$이면 $V$를 유한 차원 벡터 공간이라고 하고 아니면 $V$를 무한 차원 벡터 공간이라고 한다.

### 명제1
유한차원 벡터공간 $V/ \mathbb F$와 subspace $S \le V$가 있을 때, 다음을 증명하여라.
$$ \dim(S) = \dim(V) \Rightarrow S = V $$

**Proof**

$S$의 기저를 $\beta$라 하자. $\text{span}(\beta) = S$이다.

$S \le V$임으로 $\beta \subset V$이고 따라서 $\beta$는 $V$의 기저이다.  $\text{span}(\beta) = V$이다.

## 행렬과 군
행렬 $A \in F^{m \times n}$, $B \in F^{n \times r}$이 있을 때, 행렬 방정식 $AX=B$를 풀어보자.

만약에 어떤 행렬 집합 $G$에 군 구조가 있고 $A \in G$면 군 구조의 역원 개념을 이용해 $X = A^{-1}B$이다.

군 구조를 갖는 $G$를 찾기 위해 다음의 대수적 구조를 생각해보자.

$$ G := ( \{ M \in F^{m \times n} \} , \times ) $$

$A,B \in G$에 대해, $B = A^{-1}$이려면 $AB = BA$여야 함으로 $m=n$이어야 한다.

따라서 $G$를 다음과 같이 축소시켜 보자.

$$ G := ( \{ M \in F^{n \times n} \} , \times ) $$

이제, $\delta_{ij} = \sum_{k=1}^n a_{ik}b_{kj}$를 만족해야 한다.

$b_{kj} := M_{jk} = M_{kj}^T$로 가정하면 `라플라스 전개(Laplace expansion)`에 의해 다음이 성립한다. 
$$\sum_{k=1}^n a_{ik}b_{kj} = \sum_{k=1}^n a_{ik}M_{jk} = \begin{cases} \det(A) & i=j \\ 0 & i \neq j \end{cases}$$

따라서 $b_{kj} = \frac{1}{\det(A)} M_{kj}^T$이면 $\sum_{k=1}^n a_{ik}b_{kj} = \delta_{ij}$이다.

이 때, `고전적 수반 행렬(adjugate matrix)` $\text{adj}(A) \in F^{n \times n}$은 $\text{adj}(A)_{ij} = M_{ij}^T$로 정의된다.

고전적 수반 행렬을 이용하면 역행렬은 다음과 같이 표현된다.

$$ A^{-1} = \frac{1}{\det(A)} \text{adj}(A) $$

따라서 $A^{-1}$가 정의되기 위해서는 $\det(A) \neq 0$이어야 한다. 

결론적으로 군 구조가 되기 위해서는 다음과 같아야 한다.

$$ G := ( \{ M \in F^{n \times n} | \det(M) \neq 0 \} , \times ) $$

### 명제1
$A \in F^{n \times n}$이 있을 때, 다음이 전부 동치임을 증명하여라.

1. $A$는 역행렬이 존재한다.
2. $AX = 0$의 해는 오직 자명해인 $X=0$만 가능하다.
3. $A$는 행동치(row equivalent)하다.
4. $AX=B$는 언제나 해를 갖는다.
5. $\det(A) \neq 0$
6. $\text{rank}(A)=n$이다.
7. $A$의 모든 행 벡터는 선형 독립이다.
8. $A$의 모든 열 벡터는 선형 독립이다.

## 기본행연산
다음의 연산들을 `기본행연산(elementary row operation)`이라 한다.

1. 행에 0이 아닌 상수를 곱한다.
2. 두 행을 바꾼다.
3. 한 행에 상수를 곱한뒤 다른 행에 더해준다.

기본행연산의 목표는 $A|I$형태에서 기본행연산만을 수행해서 $I|A^{-1}$을 만들어 $A^{-1}$을 찾아내는 것이다.







## 특성다항식(행렬)
$A \in \mathbb F^{n \times n}$가 있을 때, $\det(A - tI)$를 $A$의 특성다항식이라하고 $\varphi_A(t)$로 표기한다.

### 명제1
$A \in \mathbb F^{n \times n}$가 있을 때 다음을 증명하여라
$$\varphi_A(t) = (-1)^n(t^n + a_1t^{n-1} + \cdots + a_{n-1}t + a_n) \quad a_n =\begin{cases} -\mathrm{tr} (A) & i = 1  \\ (-1)^i\det(A) & i = 2 \cdots n \end{cases}$$

**Proof**

수학적 귀납법

어떤 선형변환이 주어졌을 때, 이를 간단하게 나타내는 방법



## 대칭행렬
symmetric matrix일 때, eigenvector는 서로 수직한다.

> 참고  
> [book] (Lai et al) Introduction to Continuum Mechanics Chapter 2.23

## Definite Matrix
$\mathbf M \in Mat_{nn}(\R)$라 할 때, 다음을 만족하는 행렬을 `positive-definite`라고 한다.
$$ \mathbf x \in \R^n - \{ 0 \} \Rightarrow \mathbf x^T \mathbf M \mathbf x > 0 $$

### 명제1
$\mathbf M \in Mat_{nn}(\R)$라 할 때, 다음을 증명하여라.
$$ \mathbf M \text{ is positive definite} \Leftrightarrow \text{all eigenvalues are positive} $$

> 참고  
> [Definite Matrix - Wiki](https://en.wikipedia.org/wiki/Definite_matrix)
