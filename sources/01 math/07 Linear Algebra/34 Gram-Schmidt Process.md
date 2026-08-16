# Gram-Schmidt Process

## 한 줄 요약

Gram-Schmidt process는 linearly independent한 ordered subset을 같은 subspace를 span하는 orthogonal subset으로 바꾼다.

## Motivation

[Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)에서 본 것처럼 arbitrary basis만으로도 coordinate를 표현할 수 있지만, basis vector가 서로 orthogonal하지 않으면 projection coefficient와 Gram matrix가 복잡해진다. 따라서 주어진 basis가 span하는 공간을 바꾸지 않으면서 orthogonal basis를 구성하는 절차가 필요하다. 이 절차가 Gram-Schmidt process다.

## 정리: orthogonal basis의 존재

$n$차원 inner product space $V / \F$가 있다고 하자.

$V$ 의 basis 이면서 동시에 orthogonal subset 인 집합을 orthogonal basis 라고 할 때, 다음을 증명하여라. 

$$ V \text{ has orthogonal basis}$$

**Proof**

$V$ 의 임의의 basis 를 $\beta$ 라고 하자.

이 때, $\beta'$ 을 다음과 같이 정의하자.

$$ \beta' = 
\begin{dcases}
\beta'_1 &= \beta_1 \\ 
\beta'_i &= \beta_i - \sum_{j=1}^{i-1} \text{proj}_{\beta'_j}(\beta_i), & (2 \le i)
\end{dcases} $$

보조명제에 의해 $\beta'$ 은 orthogonal subset 이다.

명제1에 의해 $\beta'$은 linearly independent하고 cardinality가 $n$이므로 $\beta'$은 basis다.

따라서 $\beta'$ 은 orthogonal basis 이다. $\qed$

### 따름명제

$$ V \text{ has orthonormal basis} $$

**Proof**

명제 3에 의해 $V$ 가 orthogonal basis 를 갖음으로, orthonormal basis 를 normalize 하면 orthonormal basis 가 된다. $\qed$

### 보조명제
$\beta'$ 이 orthogonal subset 임을 증명하여라.

**Proof**

[$\beta' \subset V - \Set{0_{\F}}$]

임의의 $1 \le i$ 에 대해서 $\beta'_i = 0_V$ 라고 하자.

그러면 

$$ 0_V = \beta_i - \sum_{j=1}^{i-1} \text{proj}_{\beta'_j}(\beta_i) = \beta_i - \sum_{j=1}^{i-1} \frac{B(\beta_i,\beta'_j)}{B(\beta'_j, \beta'_j)} \beta'_j $$

이 때, $\beta'_j$ 는 $\beta_1,\cdots,\beta_j$ 의 선형결합으로 표현됨으로 $\beta_i$ 는 $\beta_1,\cdots,\beta_{i-1}$ 의 선형결합으로 표현된다.

이는 $\beta$ 가 basis 라는, 정확히는 linear independent set 이라는 사실에 모순이 발생한다.

따라서 proof by contradiction 에 의해 임의의 $1 \le i$ 에 대해서 $\beta'_i \neq 0_V$ 이다. $\qed$

[orthogonal property]

수학적 귀납법을 이용해서 증명한다.

$\Set{\beta'_1}$ 은 자명하게 orthogonal subset 이다.

이 때, 임의의 $2 \le i$ 에 대해서 $\Set{\beta'_1,\cdots,\beta'_{i-1}}$ 이 orthogonal subset 이라고 가정하자.

이 때, $\Set{\beta'_1,\cdots,\beta'_{i}}$ 이 orthogonal subset 임을 증명하자.

임의의 $k < i$ 에 대해서 다음이 성립한다.

$$ \begin{aligned}
B(\beta'_i,\beta'_k) &= B(\beta_i - \sum_{j=1}^{i-1} P_{\beta'_j}{\beta_i}, \beta'_k) \\
&= B(\beta_i, \beta'_k) - \sum_{j=1}^{i-1}B(P_{\beta'_j}{\beta_i}, \beta'_k) 
\end{aligned}  $$


$P_{\beta'_j}{\beta_i}$ 는 $\beta'_j$ 와 평행한 vector 이고 $\Set{\beta'_1,\cdots,\beta'_{i-1}}$ 이 orthogonal subset 임으로 다음이 성립한다.

$$ j \neq k \implies  B(P_{\beta'_j}{\beta_i}, \beta'_k) = 0 $$

이를 위에 식에 대입해서 정리하면

$$ \begin{aligned}
B(\beta'_i,\beta'_k) &= B(\beta_i, \beta'_k) - \sum_{j=1}^{i-1}B(P_{\beta'_j}{\beta_i}, \beta'_k) \\
&= B(\beta_i, \beta'_k) - B(P_{\beta'_k}{\beta_i}, \beta'_k) \\
&= B(\beta_i, \beta'_k) - \frac{B(\beta_i,\beta'_k)}{B(\beta'_k, \beta'_k)}B( \beta'_k, \beta'_k) \\
&= 0_{\F} \qed
\end{aligned}  $$

### 참고1
Subset $S\subset V$가 주어졌을 때 위와 같이 $S'$을 정의하는 방식을 `Gram-Schmidt process`라고 한다.

Gram-Schmidt process는 주어진 ordered subset으로부터 orthogonal property를 갖는 ordered subset을 만드는 구체적인 방법이다.

입력 subset이 linearly independent하면 과정에서 zero vector가 만들어지지 않으므로 결과는 orthogonal subset이 된다.

### 참고2
Orthogonal Basis 는 inner product 의 선택에 따라 달라진다.
