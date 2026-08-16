# Orthogonal Complement

## 한 줄 요약

Orthogonal complement는 주어진 subspace의 모든 방향에 수직인 vector들을 모아, 전체 공간을 subspace component와 수직 component로 분해한다.

## Motivation

[Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)에서 한 vector 방향의 projection을 subspace 전체로 확장하면, 원래 vector를 subspace 안의 component와 그 subspace의 모든 방향에 orthogonal한 component로 나누고 싶어진다. 이때 두 번째 component가 놓이는 공간을 orthogonal complement가 설명한다.

## 정의

$n$차원 inner product space $V/\F$와 subset $S\subseteq V$가 있다고 하자. $S$의 `orthogonal complement` $S^\perp$를 $S$의 모든 vector와 orthogonal한 vector의 집합으로 정의한다.

$$ S^\perp := \{ v \in V \enspace | \enspace \forall s \in S, \quad  B(v,s) = 0_\F \}$$

## 명제1
$n$차원 inner product space $V/\F$가 있다고 하자.

$W \le V$가 있을 때, 다음을 증명하여라.

$$ W^\perp \le V $$

**Proof**

[기본 연산 법칙]  
$w \in W^\perp$면, $w \in V$이기 때문에 교환법칙 분배법칙등 $F-$가군의 성질들이 전부 성립한다. 

[연산에 닫힘]  
$w_1,w_2 \in W^\perp$, $a \in \Bbb F$이고 $s \in W$라 하면 다음이 성립한다.

$$ B(aw_1 + w_2, s) = aB(w_1,s) + B(w_2,s) = 0_\F  $$

따라서, $aw_1 + w_2 \in W^\perp$임으로 연산에 닫혀있다. $\qed$

[$+$연산 항등원의 존재성]  
$s \in W$라 하면 다음이 성립한다.

$$ B(0_V,s) =  0_\F $$

따라서 $0_V \in W^\perp$이다.

[$+$연산 역원의 존재성]  
상수곱이 정의되어 있음으로 환의 명제2에 의해 역원이 존재한다.

## 명제2
$n$차원 inner product space $V/\F$ 와 $W \le V$ 가 있을 떄, 다음을 증명하여라.

$$ W \cap W^{\perp} = \set{0_V} $$

**Proof**

$w \in W \cap W^{\perp}$ 라고 하면 다음이 성립한다.

$$ B(w,w) = 0_V \implies w = 0_V \qed $$

## 명제3
$n$차원 inner product space $V / \F$와 $k \le n$ 차원 subspace $W \le V$ 가 있다고 하자.

$W$ 의 orthogonal basis 를 $\beta$ 라고 할 때, 임의의 $v \in V$ 에 대해 다음을 증명하여라.

$$ v - \sum_{i=1}^k P_{\beta_i}(v) \in W^{\perp} $$

**Proof**

임의의 $\beta_i$ 에 대해서 다음이 성립한다.

$$ \begin{aligned}
B(v - \sum_{j=1}^k P_{\beta_j}(v), \beta_i) &= B(v, \beta_i) - \sum_{j=1}^k \frac{B(v,\beta_j)}{B(\beta_j, \beta_j)} B(\beta_j, \beta_i) \\
&= B(v, \beta_i) - \frac{B(v,\beta_i)}{B(\beta_i, \beta_i)} B(\beta_i, \beta_i) \\
&= 0_\F 
\end{aligned} $$

따라서, $\forall w \in W$ 에 대해서 다음이 성립한다.

$$ \begin{aligned}
B(v - \sum_{j=1}^k P_{\beta_j}(v),w) &= a_j B(v - \sum_{j=1}^k P_{\beta_j}(v), \beta_j) \\
&= 0_\F \qed
\end{aligned} $$

## 명제4
$n$차원 inner product space $V / \F$와 $k \le n$ 차원 subspace $W \le V$ 가 있다고 하자.

$W$ 의 orthogonal basis 를 $\beta, \gamma$ 라고 할 때, 임의의 $v \in V$ 에 대해 다음을 증명하여라.

$$ \sum_{i=1}^k P_{\beta_i}(v) = \sum_{i=1}^k P_{\gamma_i}(v) $$

**Proof**

$\sum_{i=1}^k P_{\gamma_i}(v), \sum_{i=1}^k P_{\beta_i}(v) \in W$ 임으로 다음이 성립한다.

$$ \sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) \in W $$

그리고 명제3에 의해 $v - \sum_{i=1}^k P_{\beta_i}(v),v - \sum_{i=1}^k P_{\gamma_i}(v) \in W^\perp$ 임으로 다음이 성립한다.

$$ \sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) = \left( v - \sum_{i=1}^k P_{\gamma_i}(v) \right) - \left( v - \sum_{i=1}^k P_{\beta_i}(v) \right) \in W^\perp $$

그러면 $\sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) \in W \cap W^\perp$ 임으로 명제2에 의해 다음이 성립한다.

$$ \sum_{i=1}^k P_{\beta_i}(v) - \sum_{i=1}^k P_{\gamma_i}(v) = 0_\F \qed $$

### 참고
$v \in V$ 에 대해서 $W$ 로의 projection $P_W(v)$ 를 다음과 같이 정의한다.

$$ P_W(v) = \sum_{i=1}^k P_{\beta_i}(v), \quad \beta \text{ is any orthogonal basis of } W $$

## 명제5
$n$차원 inner product space $V / \F$와 $k \le n$ 차원 subspace $W \le V$ 가 있을 때, 다음을 증명하여라.

$$ \forall w \in W, \quad \norm{v-P_W(v)} \le \norm{v-w} $$

**Proof**

$v-w = (v-P_W(v)) - (P_W(v)-w)$ 이고 $v-P_W(v) \in W^\perp$, $P_W(v)-w \in W$ 임으로 다음이 성립한다.

$$ (v-P_W(v)) \perp (P_W(v)-w) $$

따라서, Pythagorean theorem 에 의해 다음이 성립한다.

$$ \norm{v-w}^2 = \norm{v-P_W(v)}^2 + \norm{P_W(v)-w}^2 $$

inner product 의 성질에 의해 $0_F \le \norm{P_W(v)-w}^2$ 이고 $0_F \le \norm{\cdot}$ 임으로 다음이 성립한다.

$$ \norm{v-P_W(v)}^2 \le \norm{v-w}^2 \implies \norm{v-P_W(v)} \le \norm{v-w} \qed $$

### 참고
$P_W(v)$ 는 $W$ 의 모든 vector 중 가장 $v$ 에 가깝다. 따라서 $P_W(v)$ 를 `closest vector` 라고 한다.

## 명제6
$n$차원 inner product space $V/\F$가 있다고 하자.

$W \le V$가 있을 때, 다음을 증명하여라.

$$ V = W \oplus W^\perp $$

**Proof**

[$V = W + W^\perp$]  
임의의 $v\in V$ 에 대해 $P_W(v) \in W$ 이고 $v-P_W(v) \in W^\perp$ 임으로 $V = W + W^\perp$이다. $\qed$

[$W \cap W^\perp = \{ 0_V \}$]  
$w \in W \cap W^\perp$라 하자.

$w \in W$이면서 $w \in W^\perp$임으로, $W^\perp$의 정의에 의해 다음이 성립한다.

$$ B(w,w) = 0_\F $$

내적의 정의에 의해 $w = 0_V$이다. $\qed$

### 참고1
$v \in V$면 $v = w + w^\perp, \enspace w \in W, w^\perp \in W^\perp$이 성립하고 direct sum의 성질에 의해 이러한 표현법이 유일하다.
