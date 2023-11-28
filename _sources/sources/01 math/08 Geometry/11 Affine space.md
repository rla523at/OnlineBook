# Affine space
## 정의
집합 $A$와 vector space $V_A/\F$와 Right group action $+:A\times V_A \rightarrow A$가 있다고 하자.

이 떄, $+$가 다음을 만족한다고 하자.

$$ \forall p \in A, \quad  \exist f_p : V_A \rightarrow A \st v \mapsto p + v \text{ is bijective} $$

`Affine space`란 $(A,V,+)$인 algebraic structure이다.

> Reference  
> [wiki - Affine space](https://en.wikipedia.org/wiki/Affine_space#Definition)  

### 참고1
집합 $A$의 원소를 `점(point)`, $+$를 `translation`이라고 한다.

### 참고2
$f_p$에서 $p$를 `원점(origin)`이라고 한다. 

즉, origin $p$가 주어진 경우 $f_p$는 affine structure를 보존하는	affine space isomorphism이다.

그리고 $f_p$에 의해 두 affine space $(A,V_A)$와 $(V_A,V_A)$는 affine space isomorphic이다. 

origin $p$가 주어졌을 때, $f_p^{-1}$의 image인 $V_A$는 origin $p$를 갖으며, affine space $A$와 구분되어져야 한다. 

하지만 일반적으로 $A$와 동일한 symbol을 사용하고 대신에 origin이 특정된 후의 vector space라고 표기한다.

따라서, point를 vector로 또는 반대로 vector를 point로 볼 수 있다.

> Reference
> [wiki](https://en.wikipedia.org/wiki/Affine_transformation#Structure)

### 참고3
Affine space 정의 자체에는 origin이 주어져 있지 않다.

따라서, 특정한 origin이 정해지기 전까지는 vector는 고정된 origin을 가지고 있지 않으며 동시에 어떤 point와도 1:1 대응될 수 없다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Affine_space)  

### 참고3
임의의 $p \in A$가 주어진 경우, 다음과 같은 vector space를 정의할 수 있다.

$$ V_p := \{(p,v) \enspace | \enspace v \in V  \} $$

### 명제1
Affine space $A$와 $v \in V_A$가 있다고 하자.

이 때, 함수 $f_v$를 다음과 같이 정의하자.

$$f_v : A \rightarrow A \st p \mapsto p + v$$
이 때, $f_v$가 전단사 함수임을 증명하여라.

**Proof**

[injective]  
$p_1,p_2 \in A$라 하자.  

$$ \begin{aligned} & f_v(p_1) = f_v(p_2) \\ \implies& p_1 + v = p_2 + v \\ \implies& p_1 + v + v^{-1} = p_2 + v + v^{-1} \\ \implies& p_1 + 0_V = p_2 + 0_V \\ \implies& p_1 = p_2 \qed\end{aligned} $$

[surjective]  
$+$는 binary operation임으로 다음이 성립한다.

$$ \forall p \in A, \quad  p + (-v) \in A $$

이 떄, $f_v$의 정의에 의해 다음이 성립한다.

$$ f_v(p + (-v)) = p + (-v) + v = p $$

따라서, surjective function의 정의에 의해 다음이 성립한다.

$$ f_v \text{ is surjective } \qed $$

### 명제2
Affine space $A$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \forall p \in A, \quad  f_p(0_V) = p $$

**Proof**

$f_p$의 정의에 의해 다음이 성립한다.

$$ f_p(0_V) = p + 0_V = p \qed $$


### 명제3
Affine space $A$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \forall p_1, p_2 \in A, \quad \exist! v \in V_A \st p_2 = p_1+v$$

**Proof**

$f_{p_1}$가 bijective임으로 다음이 성립한다.

$$ \exist!v \in V_A \st f_{p_1}(v) = p_1 + v = p_2 \qed $$

## Substraction and Weyl's axioms
위의 주어진 $+$의 성질을 활용하여 다음같은 이항연산을 정의할 수 있다.

$$ - := A \times A \rightarrow V \st (p_1,p_2) \mapsto p_2 - p_1 $$


$$ \text{Where, }  p_1 + (p_2 - p_1) = p_2 $$

이 때, $-$는 다음 성질들을 만족한다.


$$ \begin{aligned} 1) \quad & P \in A \enspace \land \enspace v \in V \Rightarrow \exist! p_v \in A \st p_v - P = v \\ 2) \quad & p_1,p_2,p_3 \in A \Rightarrow (p_3 - p_2) + (p_2 - p_1) = p_3 - p_1 \end{aligned} $$

다음 두 명제를 Weyl's axioms라고 하며, 2)을 일반적으로 `평행사변형 법칙(parallelogram rule)`이라고 한다.

Affine space는 집합 A에 벡터 공간 $V / \mathbb F$와 $-$를 주어도 동일하게 정의 할 수 있으며, 이 경우에는 $+$ 연산은 Weyl's axioms을 활용하여 정의할 수 있다.

### 명제1
vector space $V / \mathbb F$가 주어진 affine space $A$가 있을 때, $p_0 \in A$와  $V$의 기저 $\beta$를 결정하면 bijective function $\phi : A \rightarrow \R^n$이 존재함을 증명하여라.

**Proof**

$\phi$를 다음과 같이 정의하자.

$$ \phi : A \rightarrow \R^n \st P \mapsto [P-p_0]_\beta $$

명제2에 의해 $P-p_0$가 유일하게 존재함으로, 함수 $\phi$는 잘 정의되며 injective이다.

또한 +는 연산에 닫혀있음으로, $a \in \R^n$에 대해, $P = p_0 + a_i\beta_i$를 만족하는 $P$가 항상 존재함으로 surjective이다. $\qed$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Affine_space)  
> [what-are-differences-between-affine-space-and-vector-space - mathematics](https://math.stackexchange.com/questions/884666/what-are-differences-between-affine-space-and-vector-space)  
> [an-affine-space-is-nothing-more-than-a-vector-space - mathmatics](https://math.stackexchange.com/questions/3527297/an-affine-space-is-nothing-more-than-a-vector-space-whose-origin-we-try-to-forg)  
> [when-is-a-vector-glued-to-the-origin - mathmatics](https://math.stackexchange.com/questions/2392479/when-is-a-vector-glued-to-the-origin)  
> [must-vectors-in-mathbbrn-have-their-tail-at-origin - mathematics](https://math.stackexchange.com/questions/627616/must-vectors-in-mathbbrn-have-their-tail-at-origin)
> [Euclidean space - wiki](https://en.wikipedia.org/wiki/Euclidean_space) 

## Affine subspace
vector space $V_A / \mathbb F$가 주어진 affine space $A$와 $S \le V$가 있다고 하자.

$p \in A$에 대해서, 집합 $B$를 다음과 같이 정의하자.

$$ B := \{ p + v \enspace | \enspace v \in S \} $$

그러면 $B$는 vector space $S$가 주어진 affine space가 되며, $B$를 $A$의 affine subspace라 한다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Affine_space)  

### 참고
$B$가 $A$의 affine subspace라는 것을 다음과 같이 간단하게 나타낸다.

$$ B \le A $$

## Parallel
vector space $V_A / \mathbb F$가 주어진 affine space $A$가 있다고 하자.

$B,C \le A$일 때 $V_B = V_C$이면, $B$와 $C$가 `평행(parallel)`하다고 한다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Affine_space)  