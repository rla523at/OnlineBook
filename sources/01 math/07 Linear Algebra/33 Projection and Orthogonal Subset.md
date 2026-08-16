# Projection and Orthogonal Subset

## 한 줄 요약

Projection은 vector에서 특정 방향의 component를 추출하며, 선택한 방향들이 pairwise orthogonal하면 여러 방향의 component를 서로 간섭 없이 독립적으로 계산할 수 있다.

## Motivation

[Inner Product Space](<30 Inner Product Space.md>)에서 얻은 orthogonality를 이용해 한 방향의 component를 구하는 projection을 여러 방향이 span하는 subspace로 확장하면 coefficient들이 서로 영향을 줄 수 있다. Orthogonal property는 이 cross term을 없애며, orthogonal subset은 이러한 독립적인 방향들을 모아 놓은 집합이다.

## Projection

Inner product space $V/\F$, nonzero vector $v\in V-\{0_V\}$와 $w\in V$가 있다고 하자. $w$에서 $v$ 방향의 component만 추출하는 것을 $v$ 방향으로의 `projection`이라고 한다.

$w$를 $v$ 방향으로 projection하여 얻은, $v$와 parallel한 vector를 $w^\parallel$이라고 하자. 그러면 제거하고 남은 $w-w^\parallel$에는 $v$와 orthogonal한 component만 있어야 한다.

$$
w-w^\parallel\perp v
\qquad\Longleftrightarrow\qquad
B(w-w^\parallel,v)=0_\F.
$$

$w^\parallel$은 $v$와 parallel하므로 어떤 $\alpha\in\F$에 대해 $w^\parallel=\alpha v$로 표현할 수 있다. 따라서 다음이 성립한다.

$$ \begin{aligned}
B(w-w^\parallel, v) &= B(w-\alpha v, v)  \\
&= B(w,v) - \alpha B(v,v) \\
\end{aligned}  $$

이를 $B(w-w^\parallel,v)=0_\F$에 대입하고 $\alpha$에 대해 정리하면 다음과 같다.

$$ \alpha = \frac{B(w,v)}{B(v,v)} $$

따라서 $w$를 $v$ 방향으로 projection한 vector를 다음과 같이 정의한다.

$$
\operatorname{proj}_v(w)
:=
w^\parallel
=
\frac{B(w,v)}{B(v,v)}v.
$$

이 공식은 한 방향의 component를 추출한다. 여러 vector가 span하는 subspace 방향의 component를 구할 때 이 one-dimensional projection들을 단순히 더할 수 있는지는 선택한 방향들 사이의 관계에 따라 달라진다.

## Orthogonal Property

여러 방향의 projection을 독립적으로 계산하려면 각 방향이 서로 섞이지 않아야 한다. Inner product space $V/\F$의 subset $S=\{s_1,\ldots,s_k\}$가 다음을 만족하면 $S$가 `orthogonal property`를 갖는다고 한다.

$$ i \neq j \implies B(s_i, s_j) = 0 $$

즉, 서로 다른 모든 pair가 orthogonal하다는 뜻이다.

이 조건이 projection 계산을 단순하게 만드는 이유를 살펴보자. $S=\{s_1,\ldots,s_k\}\subset V-\{0_V\}$이고 $W=\operatorname{span}(S)$라고 하자. $x$의 $W$ 방향 component를

$$
p=\sum_{i=1}^k c_i s_i
$$

라고 쓰면, 제거하고 남은 $x-p$는 $W$의 모든 방향과 orthogonal해야 한다. $S$가 $W$를 span하므로 각 $s_j$에 대해 다음을 요구하면 된다.

$$
B(x-p,s_j)=0
\qquad\Longleftrightarrow\qquad
\sum_{i=1}^k c_iB(s_i,s_j)=B(x,s_j).
$$

$S$가 orthogonal property를 가지면 $i\ne j$인 모든 cross term이 $0$이므로 $j$번째 식에는 $c_j$만 남는다.

$$
c_j
=
\frac{B(x,s_j)}{B(s_j,s_j)}.
$$

따라서 각 coefficient를 다른 방향과 독립적으로 구할 수 있고, $W$ 방향 component는 one-dimensional projection들의 합이 된다.

$$
p
=
\sum_{j=1}^k\operatorname{proj}_{s_j}(x).
$$

반면 $S$가 $W$의 basis이지만 orthogonal하지 않으면 $B(s_i,s_j)$인 cross term들이 남으므로 one-dimensional projection들을 단순히 더할 수 없다. 그렇다고 $W$ 방향의 projection이 존재하지 않는 것은 아니다. 위의 coupled equation을 [Gram Matrix](<31 Gram Matrix.md>)를 이용해 풀거나, [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)로 같은 $W$를 span하는 orthogonal basis를 만든 뒤 방향별 projection을 더할 수 있다. 따라서 orthogonal property는 projection의 존재 조건이 아니라 projection coefficient들을 서로 간섭 없이 독립적으로 계산하게 하는 조건이다.

## Orthogonal Subset

Zero vector는 모든 vector와 orthogonal하지만 독립된 방향을 나타내지 못한다. 따라서 zero vector를 제외한 subset

$$
S=\{s_1,\ldots,s_k\}\subset V-\{0_V\}
$$

이 orthogonal property를 가지면 `orthogonal subset`이라고 한다.

Orthogonal subset $S$의 모든 vector가 다음 조건까지 만족하면 `orthonormal subset`이라고 한다.

$$ \norm{s_i} = 1 $$

즉, orthonormal subset은 각 방향을 유지하면서 모든 vector의 length를 $1$로 normalize한 orthogonal subset이다.

### 명제1
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 orthogonal subset일 때 다음을 증명하여라.

$$ S \text { is linearly independent} $$

**Proof**

어떤 $a_i \in \F, \quad i \in \Set{1,\cdots,k}$ 들에 대해서 $a_is_i = 0_V$ 라고하자.

임의의 $j \in \Set{1,\cdots,k}$ 을 고정하면 내적에 성질에 의해 다음이 성립한다.

$$ B(a_is_i, s_j) = a_j B(s_j,s_j) = 0_\F \quad (\text{no sum over } j) $$

$s_j \neq 0_V$ 임으로 inner product 의 정의에 의해 $0 < B(s_j,s_j)$ 임으로 $a_j = 0$ 다.

즉, 모든 $a_i = 0_\F$ 임으로 linearly independent하다. $\qed$

#### 참고
$S$ 가 linearly independent set 임으로 $\span(S)$ 의 basis 는 $S$ 이다. 

### 명제2
$n$차원 inner product space $V/\F$가 있다고 하자.

$S = \{ s_1, \cdots, s_k \} \subset V - \{ 0_V\}$가 orthogonal subset일 때 다음을 증명하여라.

$$ y \in \span(S) \implies y = \sum_{i=1}^k \text{proj}_{s_i}(y) $$

**Proof** 

$y \in \span(S)$ 임으로 다음과 같이 표현할 수 있다.

$$ y = a_is_i, \quad a_i \in \F $$

임의의 $j \in \Set{1,\cdots,k}$ 을 고정하면 orthogonal subset 의 성질에 의해 다음이 성립한다.

$$ \begin{aligned}  
B(y, s_j) &= B(a_is_i, s_j) \\
&= a_j B(s_j, s_j) \quad (\text{no sum over } j) 
\end{aligned} $$

위를 $a_j$ 에 대해서 정리하면 

$$ a_j = \frac{B(y,s_j)}{B(s_j, s_j)}  \quad (\text{no sum over } j) $$

이를 이용해 $y$ 를 표현하면

$$ y = a_is_i = \frac{B(y,s_i)}{B(s_i, s_i)} s_i = \sum_{i=1}^k \text{proj}_{s_i}(y) \qed $$

#### 참고1
$S$ 을 span 한 공간에 있는 vector 를 $x$ 라고 하면 $x$ 는 $S$ 의 각 원소에 projection 시킨 vector 들로 표현된다.

#### 참고2
$S$ 을 span 한 공간에 basis 는 $S$ 이고 임의의 $x \in \span(S)$ 의 coordinate 를 `Fourier coefficient`라고 하며 다음과 같다. 

$$ x = \frac{B(x,s_i)}{B(s_i, s_i)} s_i $$

만약 $S$ 가 orthonormal subset 인 경우 Fourier coefficient 는 $B(y,s_i)$ 로 간단해 진다.
