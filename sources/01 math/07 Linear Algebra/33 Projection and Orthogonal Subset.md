# Projection and Orthogonal Subset

## 한 줄 요약

Projection은 vector에서 특정 방향의 component를 추출하며, 선택한 방향들이 pairwise orthogonal하면 여러 방향의 component를 서로 간섭 없이 독립적으로 계산할 수 있다.

## Projection

### Motivation

[Norm, Distance and Angle](<32 Norm Distance and Angle.md>)에서 두 vector $u,w$의 inner product가 $B(u,w)=0_\F$이면 두 vector가 orthogonal하다고 정의했다. Inner product space $V/\F$, vector $x\in V$와 nonzero vector $v\in V-\{0_V\}$가 있다고 하자. 이제 이 관계를 이용해 $x$에서 $v$와 같은 방향으로 놓인 부분만 분리하기 위해 $x$를

$$
x=p+r,
\qquad
p\in\operatorname{span}\{v\},
\qquad
r\perp v
$$

와 같이 나누고자 한다. 여기서 $p$는 $v$ 방향의 component이고, $r$은 그 component를 제거하고 남은 vector다. $p$는 $v$와 parallel하므로 어떤 $\alpha\in\F$에 대해 $p=\alpha v$로 쓸 수 있다. 나머지 $r=x-\alpha v$가 $v$와 orthogonal해야 한다는 조건을 적용하면

$$
\begin{aligned}
0_\F
&=B(x-\alpha v,v)\\
&=B(x,v)-\alpha B(v,v)
\end{aligned}
$$

를 얻는다. $v\ne0_V$이면 positive definiteness에 의해 $B(v,v)\ne0_\F$이므로 $\alpha$는 다음과 같이 결정된다.

$$
\alpha
=
\frac{B(x,v)}{B(v,v)}.
$$

따라서 $\alpha$와 $p=\alpha v$는 orthogonality 조건에 의해 유일하게 결정된다. 이처럼 component를 제거한 나머지가 해당 direction과 orthogonal하도록 만드는 연산이 projection을 정의하게 한다.

### Definition

Inner product space $V/\F$에서 nonzero vector $v\in V-\{0_V\}$가 정하는 direction으로의 `projection`을 다음 함수로 정의한다.

$$
\operatorname{proj}_v
:
V\rightarrow\operatorname{span}\{v\},
\qquad
\operatorname{proj}_v(x)
:=
\frac{B(x,v)}{B(v,v)}v.
$$

Vector $\operatorname{proj}_v(x)$는 $x$의 $v$ 방향 component다. Motivation의 orthogonality 조건에서 이 공식을 얻었으므로

$$
\operatorname{proj}_v(x)\in\operatorname{span}\{v\},
\qquad
x-\operatorname{proj}_v(x)\perp v
$$

가 성립한다. 이 정의는 한 방향의 component를 추출한다. 여러 vector가 span하는 subspace 방향의 component를 one-dimensional projection들의 합으로 구할 수 있는지는 선택한 방향들 사이의 관계에 따라 달라진다.

## Orthogonal Property

### Motivation

한 방향이 아니라 여러 방향이 span하는 subspace의 component를 구하고 싶다고 하자. $S=\{s_1,\ldots,s_k\}$가 finite-dimensional subspace $W$의 basis이고

$$
W=\operatorname{span}(S)
$$

라고 하자. $x$의 $W$ 방향 component를

$$
p
=
\sum_{i=1}^k c_i s_i
$$

라고 쓰면, 제거하고 남은 $x-p$는 $W$의 모든 방향과 orthogonal해야 한다. $S$가 $W$를 span하므로 각 $s_j$에 대해 이 조건을 적용하면

$$
B(x-p,s_j)=0_\F
\qquad\Longleftrightarrow\qquad
\sum_{i=1}^k c_iB(s_i,s_j)=B(x,s_j).
$$

일반적인 basis에서는 $i\ne j$인 항 $B(s_i,s_j)$도 남는다. 이 항들은 서로 다른 basis direction 사이의 interaction을 나타내는 cross term이므로, 하나의 식에 여러 coefficient가 함께 나타나고 $c_1,\ldots,c_k$를 coupled system으로 풀어야 한다. 따라서 여러 direction의 coefficient를 독립적으로 구하려면 서로 다른 basis vector 사이의 모든 cross term을 없애는 조건이 필요하다.

### Definition

Inner product space $V/\F$의 subset $S=\{s_1,\ldots,s_k\}$가 다음 조건을 만족하면 $S$가 `orthogonal property`를 갖는다고 한다.

$$
i\ne j
\implies
B(s_i,s_j)=0_\F.
$$

즉, 서로 다른 모든 pair가 orthogonal하다는 뜻이다.

### Projection onto Span

$S=\{s_1,\ldots,s_k\}\subset V-\{0_V\}$가 orthogonal property를 갖고 $W=\operatorname{span}(S)$라고 하자. Motivation에서 얻은 coefficient system에서는 $i\ne j$인 모든 cross term이 $0_\F$이므로 $j$번째 식에 $c_j$만 남는다.

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

### Motivation

Orthogonal property만으로는 zero vector를 배제할 수 없다. Zero vector는 모든 vector와 orthogonal하지만 독립된 direction을 나타내지 못하고, $B(0_V,0_V)=0_\F$이므로 projection 공식의 denominator에도 사용할 수 없다. 따라서 실제 direction들을 모은 집합으로 사용하려면 모든 vector가 nonzero라는 조건이 필요하다.

또한 orthogonal한 vector들의 length는 서로 다를 수 있다. 각 vector를 length $1$로 normalize하면 direction과 orthogonality는 유지하면서 projection coefficient에서 denominator를 제거할 수 있다. 이러한 집합을 구분하기 위해 orthogonal subset과 orthonormal subset을 정의한다.

### Definition

Zero vector를 제외한 subset

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
