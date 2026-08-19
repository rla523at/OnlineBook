# Orthogonal Complement and Orthogonal Projection

## 한 줄 요약

Orthogonal complement $W^\perp$는 $W$의 모든 vector와 orthogonal한 vector들의 subspace다. 이를 사용하면 finite-dimensional inner product space의 모든 vector $v$를 $P_W(v)\in W$와 $v-P_W(v)\in W^\perp$로 유일하게 분해할 수 있다.

## Orthogonal Complement

### Motivation

[Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)에서는 subspace $W$의 서로 orthogonal한 basis $S=\{s_1, \ldots, s_k\}$를 사용하여 vector $x$를 $W$ 위로 projection한 vector $p=\sum_{i=1}^k\operatorname{proj}_{s_i}(x)$와 나머지 $r=x-p$로 나누었다. 이때 $r$은 각 $s_i$와 orthogonal하다. 또한 $S$가 $W$를 span하므로 $r$은 $W$에 속하는 모든 vector와 orthogonal하다.

이처럼 $W$에 속하는 모든 vector와 orthogonal한 vector들을 하나의 집합으로 모은 것이 orthogonal complement다. Orthogonal complement는 projection의 residual이 놓이는 공간을 basis와 무관하게 설명한다.

### Definition

Inner product space $V/\F$와 subset $S\subseteq V$가 있다고 하자. $S$의 `orthogonal complement`를 다음과 같이 정의한다.

$$
S^\perp
:=
\{v\in V\mid B(v,s)=0_\F\text{ for every }s\in S\}.
$$

Conjugate symmetry에 의해 $B(v,s)=0_\F$와 $B(s,v)=0_\F$는 동치이므로 argument의 순서는 orthogonality 자체를 바꾸지 않는다.

Orthogonality는 linear combination과 호환되므로 $S$의 개별 vector들과 orthogonal한 것은 $\operatorname{span}(S)$의 모든 vector와 orthogonal한 것과 같다.

$$
S^\perp
=
\operatorname{span}(S)^\perp.
$$

실제로 $v\in S^\perp$이고 $w=\sum_i a_is_i\in\operatorname{span}(S)$이면

$$
B(v,w)
=
\sum_i\overline{a_i}B(v,s_i)
=
0_\F
$$

이다.

### Basic Properties

#### 정리1 (Orthogonal complement is a subspace)

Inner product space $V/\F$의 모든 subset $S\subseteq V$에 대해 $S^\perp$는 $V$의 subspace다.

**Proof**

$B(0_V,s)=0_\F$이므로 $0_V\in S^\perp$이다. $v_1,v_2\in S^\perp$와 $a_1,a_2\in\F$를 잡으면 모든 $s\in S$에 대해

$$
\begin{aligned}
B(a_1v_1+a_2v_2,s)
&=
a_1B(v_1,s)+a_2B(v_2,s)\\
&=
0_\F.
\end{aligned}
$$

따라서 $a_1v_1+a_2v_2\in S^\perp$이므로 $S^\perp\le V$이다. $\qed$

#### 정리2 (Basic relations)

Subset $S,T\subseteq V$에 대해 다음이 성립한다.

$$
\{0_V\}^\perp=V,
\qquad
V^\perp=\{0_V\},
$$

$$
S\subseteq T
\implies
T^\perp\subseteq S^\perp.
$$

**Proof**

모든 $v\in V$에 대해 $B(v,0_V)=0_\F$이므로 $\{0_V\}^\perp=V$다. 한편 $v\in V^\perp$이면 특히 $B(v,v)=0_\F$이고, positive definiteness에 의해 $v=0_V$다. 따라서 $V^\perp=\{0_V\}$다.

마지막으로 $S\subseteq T$일 때 $T$의 모든 vector와 orthogonal한 vector는 $S$의 모든 vector와도 orthogonal하므로 $T^\perp\subseteq S^\perp$다. $\qed$

## Orthogonal Projection onto a Subspace

### Motivation

[Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)에서는 orthogonal basis가 주어진 경우 각 basis vector 위로의 projection을 더하는 공식을 구했다. 또한 finite-dimensional subspace $W\le V$에는 [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)를 적용하여 orthonormal basis를 선택할 수 있다. 이 basis를

$$
\beta=(\beta_1,\ldots,\beta_k)
$$

라고 하면 orthonormality에 의해 $B(\beta_i,\beta_j)=\delta_{ij}$이므로 각 projection coefficient는 $B(v,\beta_i)$이고 cross term은 모두 사라진다. 이제 이 합을 $W$ 위로의 projection을 나타내는 하나의 map으로 정의하고, 그 정의가 선택한 orthonormal basis에 의존하지 않음을 보인다.

### Definition

$W$의 orthonormal basis $\beta$를 사용해 `orthogonal projection onto $W$`를 다음과 같이 정의한다.

$$
P_W:V\rightarrow W,
\qquad
P_W(v)
:=
\sum_{i=1}^k B(v,\beta_i)\beta_i.
$$

정의에서 바로 $P_W(v)\in W$다. 또한 각 $j\in\{1,\ldots,k\}$에 대해

$$
\begin{aligned}
B(v-P_W(v),\beta_j)
&=
B(v,\beta_j)
-
\sum_{i=1}^k
B(v,\beta_i)B(\beta_i,\beta_j)\\
&=
B(v,\beta_j)-B(v,\beta_j)\\
&=
0_\F.
\end{aligned}
$$

따라서

$$
v-P_W(v)\in W^\perp.
$$

이 성질과 다음 decomposition theorem은 $P_W(v)$가 선택한 orthonormal basis에 의존하지 않음을 보장한다.

### 정리3 (Orthogonal decomposition)

Finite-dimensional inner product space $V/\F$와 subspace $W\le V$가 있다고 하자. 모든 $v\in V$는 다음과 같이 유일하게 표현된다.

$$
v=w+u,
\qquad
w\in W,
\qquad
u\in W^\perp.
$$

특히

$$
V=W\oplus W^\perp.
$$

**Proof**

$w:=P_W(v)$, $u:=v-P_W(v)$로 두면 앞의 계산에 의해 $w\in W$, $u\in W^\perp$이고 $v=w+u$이므로 existence가 성립한다.

이제

$$
v=w_1+u_1=w_2+u_2
$$

인 두 표현이 있다고 하자. 그러면

$$
w_1-w_2=u_2-u_1
$$

이고 왼쪽은 $W$, 오른쪽은 $W^\perp$에 속한다. $x\in W\cap W^\perp$이면 $B(x,x)=0_\F$이므로 $x=0_V$다. 따라서 $w_1=w_2$, $u_1=u_2$이고 표현은 유일하다. $\qed$

Orthogonal decomposition에서 $W$에 속하는 항은 반드시 $P_W(v)$여야 한다. 따라서 서로 다른 orthonormal basis로 projection 공식을 계산해도 같은 vector를 얻는다.

### 따름정리1 (Double orthogonal complement)

Finite-dimensional inner product space $V/\F$와 subspace $W\le V$에 대해 다음이 성립한다.

$$
(W^\perp)^\perp=W,
\qquad
\dim V=\dim W+\dim W^\perp.
$$

**Proof**

모든 $w\in W$는 $W^\perp$의 모든 vector와 orthogonal하므로 $W\subseteq(W^\perp)^\perp$다.

반대로 $x\in(W^\perp)^\perp$를 orthogonal decomposition으로 $x=w+u$, $w\in W$, $u\in W^\perp$라고 쓰자. $x\perp u$이고 $w\perp u$이므로

$$
0_\F
=
B(x,u)
=
B(w+u,u)
=
B(u,u).
$$

따라서 $u=0_V$이고 $x=w\in W$다. 그러므로 $(W^\perp)^\perp=W$다. Dimension 식은 direct sum의 dimension formula에서 따라온다. $\qed$

## Closest Vector Property

### 정리4 (Projection minimizes distance)

Finite-dimensional inner product space $V/\F$, subspace $W\le V$, vector $v\in V$가 있다고 하자. 모든 $w\in W$에 대해

$$
\lVert v-P_W(v)\rVert
\le
\lVert v-w\rVert
$$

이고 equality는 $w=P_W(v)$일 때 그리고 그때에만 성립한다.

**Proof**

다음 decomposition을 생각하자.

$$
v-w
=
\bigl(v-P_W(v)\bigr)
+
\bigl(P_W(v)-w\bigr).
$$

첫 번째 항은 $W^\perp$, 두 번째 항은 $W$에 속하므로 두 항은 orthogonal하다. Pythagorean theorem에 의해

$$
\lVert v-w\rVert^2
=
\lVert v-P_W(v)\rVert^2
+
\lVert P_W(v)-w\rVert^2.
$$

두 번째 항이 nonnegative이므로 원하는 부등식을 얻는다. Equality는 $\lVert P_W(v)-w\rVert=0$, 즉 $w=P_W(v)$일 때 그리고 그때에만 성립한다. $\qed$

따라서 $P_W(v)$는 $W$의 vector들 중 $v$와 가장 가까운 유일한 vector다. 이 성질이 [Least Squares Problem](<37 Least Squares Problem.md>)에서 exact solution이 없을 때 best approximation을 선택하는 근거가 된다.

## 관련 문서

- [Norm, Distance and Angle](<32 Norm Distance and Angle.md>)
- [Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)
- [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)
- [Least Squares Problem](<37 Least Squares Problem.md>)
