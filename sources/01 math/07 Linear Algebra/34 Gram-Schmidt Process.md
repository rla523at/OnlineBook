# Gram-Schmidt Process

## 한 줄 요약

Gram-Schmidt process는 linearly independent한 ordered subset을 같은 subspace를 span하는 orthogonal subset으로 바꾼다.

## Motivation

[Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)에서 본 것처럼 arbitrary basis만으로도 vector의 coordinate를 표현할 수 있다. 그러나 basis vector가 서로 orthogonal하지 않으면 Gram matrix에 off-diagonal entry가 남고, subspace 방향의 projection coefficient를 구하는 방정식들이 서로 얽힌다. 같은 subspace를 orthogonal basis로 표현할 수 있다면 각 방향의 component를 one-dimensional projection으로 독립적으로 계산할 수 있다. 따라서 주어진 basis가 span하는 공간을 바꾸지 않으면서 basis vector들을 orthogonal하게 만드는 절차를 생각해 보자.

Linearly independent한 ordered subset을

$$
\beta=(\beta_1,\ldots,\beta_n)
$$

이라고 하자. 이 subset으로부터 orthogonal한 ordered subset

$$
\beta'=(\beta'_1,\ldots,\beta'_n)
$$

을 만들되, 각 단계에서 다음 관계를 유지하고자 한다.

$$
\operatorname{span}\{\beta'_1,\ldots,\beta'_i\}
=
\operatorname{span}\{\beta_1,\ldots,\beta_i\}.
$$

이 조건을 유지하면 마지막에 $\beta'$이 $\beta$와 같은 subspace를 span한다. 첫 번째 vector와 비교할 이전 방향은 없으므로 다음과 같이 둔다.

$$
\beta'_1:=\beta_1.
$$

두 번째 vector $\beta_2$를 $\beta'_1$과 orthogonal하게 만들려면 $\beta_2$에서 $\beta'_1$ 방향의 component를 제거하면 된다.

$$
\beta'_2
:=
\beta_2-\operatorname{proj}_{\beta'_1}(\beta_2).
$$

Projection의 정의에 의해 $\beta'_2\perp\beta'_1$이다. 또한 제거한 vector는 $\beta'_1$의 scalar multiple이므로 $\beta_2$를 $\beta'_1$과 $\beta'_2$의 linear combination으로 다시 표현할 수 있다. 따라서 이 변환은 처음 두 vector가 span하는 subspace를 바꾸지 않는다.

이제 $\beta'_1,\ldots,\beta'_{i-1}$이 서로 orthogonal하도록 만들어졌다고 하자. Orthogonal한 여러 방향이 span하는 subspace 방향의 component는 각 방향으로의 projection을 더한 vector이므로, $\beta_i$에서 그 합을 제거한다.

$$
\beta'_i
:=
\beta_i
-
\sum_{j=1}^{i-1}
\operatorname{proj}_{\beta'_j}(\beta_i).
$$

서로 다른 $\beta'_j$ 사이의 inner product는 $0$이므로, 임의의 $k<i$에 대해 위 합에서 $\beta'_k$와의 inner product에 영향을 주는 항은 $k$번째 projection뿐이다. 그 항이 $\beta_i$의 $\beta'_k$ 방향 component를 정확히 제거하므로 다음이 성립한다.

$$
B(\beta'_i,\beta'_k)=0.
$$

또한 제거한 모든 projection은 이전 vector들이 span하는 subspace에 속한다. 따라서 $\beta_i$를 추가하기 전후의 span은 같으며, 만약 $\beta'_i=0_V$라면 $\beta_i$가 이전 vector들의 linear combination이라는 뜻이 되어 $\beta$의 linear independence에 모순이다. 그러므로 이 과정을 반복하면 zero vector를 만들지 않으면서 원래 subset과 같은 subspace를 span하는 orthogonal subset을 얻는다.

이처럼 각 vector에서 이전에 만든 orthogonal 방향의 component를 차례로 제거하는 절차를 `Gram-Schmidt process`라고 한다. 결과의 각 vector를 normalize하면 같은 subspace의 orthonormal basis도 얻을 수 있다. 다음 정리에서는 이 구성이 실제로 orthogonal basis를 만든다는 것을 증명한다.

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
