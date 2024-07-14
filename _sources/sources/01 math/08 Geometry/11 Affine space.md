# Affine space
## 정의
집합 $A$와 vector space $V_A/\F$와 right group action $+:A\times V_A \rightarrow A$가 있다고 하자.

임의의 $p\in A$에 의존하여 정의되는 함수 $F_p$가 다음을 만족하는 경우 $(A,V_A,+)$를 `affine space`라고 한다.

$$ F_p : V_A \rightarrow A \st v \mapsto p + v \text{ is bijective} $$

> Reference  
> [wiki - Affine space](https://en.wikipedia.org/wiki/Affine_space#Definition)  

### 참고1
집합 $A$의 원소를 `점(point)`, $V_A$의 원소를 `vector` 혹은  `translation`이라고 하며 $F_p$에서 $p$를 `원점(origin)`이라고 한다. 

### 참고2
$+$가 right group action이기 때문에 right indenity와 associativity가 성립한다.

**right identity**

$$ \forall p \in A, \quad  p + 0_{V_A} = p $$

**Associativity**

$$ \forall v ,w \in V_A, \quad \forall a \in A, \quad (a+v) +w = a + (v+w) $$

### 참고3
origin $p$가 주어진 경우, $F_p$는 bijective 이기 때문에, $A$와 $V_A$는 isomorphic하다.

따라서, point를 vector로 vector를 point로 볼 수 있게 된다.

$$ A \xtofrom[F_p]{F_p^{-1}} V_A$$

참고로, Affine space 정의 자체에는 origin이 주어져 있지 않다.

따라서, 특정한 origin이 정해지기 전까지는 vector는 고정된 origin을 가지고 있지 않으며 동시에 어떤 point와도 1:1 대응될 수 없다.

> Reference
> [wikipedia - Affine_transformation#Structure](https://en.wikipedia.org/wiki/Affine_transformation#Structure)

### 명제1
Affine space $A$가 있다고 하자.

임의의 $p,q \in A$에 대해서, 다음이 성립함을 증명하여라.

$$ \exist! v \in V_A \st p+v = q $$

**Proof**

$F_p:V_A \rightarrow P$를 다음과 같이 정의하자.

$$ F_p : V_A \rightarrow A \st v \mapsto p + v $$

그러면 Affine space의 정의에 의해 $F_p$는 bijective function임으로 다음이 성립한다.

$$ \exist! v \in V_A \st F_p(v) = p+v = q \qed $$

#### 참고1
$p,q \in A$면 $F_p(v) = q$인 $v$가 $p,q$의 선택에 의존하여 유일하게 존재함으로 $v$를 다음과 같이 표현할 수 있다.

$$ v \equiv q-p $$

따라서, 다음과 같이 표현이 성립한다.

$$ p + (q-p) = q $$

이를 substraction이라고 한다.

또한, $q-p$를 $\overrightarrow{pq}$로 표현하기도 한다.

> Reference  
> [wikipedia - Affine_space#Subtraction_and_Weyl's_axioms](https://en.wikipedia.org/wiki/Affine_space#Subtraction_and_Weyl's_axioms)  

### 명제2
Affine space $A$가 있다고 하자.

임의의 $p \in A$에 대해서, 다음이 성립함을 증명하여라.

$$ p+v = p+w \implies v = w $$

**Proof**

$F_p:V_A \rightarrow P$를 다음과 같이 정의하자.

$$ F_p : V_A \rightarrow A \st v \mapsto p + v $$

그러면 Affine space의 정의에 의해 $F_p$는 injective function임으로 임의의 $v,w \in V_A$ 다음이 성립한다.

$$ F_p(v) = F_p(w) \implies v = w $$

이를 풀어 쓰면 다음과 같다.

$$ p+v = p+w \implies v=w \qed $$

### 명제3
Affine space $A$가 있다고 하자.

임의의 $p \in A$와 $v\in V_A$에 대해서 다음이 성립함을 증명하여라.

$$ (p+v) - p = v $$

**Proof**

$p+v \in A$임으로, 명제1에 의해 다음이 성립한다.

$$ \exist! w \in V_A \st p +w = p+v $$

이를 substraction으로 표현하면 다음과 같다.

$$ w = (p+v) - p $$

그리고 명제 2에 의해 다음이 성립한다.

$$ w= v $$

위의 두 표현식을 합치면 다음과 같다.

$$ (p+v) - p = v \qed $$

#### 따름정리
임의의 $p \in A$와 $v \in V_A$가 있을 때, 다음이 성립함을 증명하여라.

$$ \exist! q \in A \st q-p = v $$

**Proof**

$q = p+v$로 두면 명제3에 의해 자명하다.

### 명제4
Affine space $A$가 있다고 하자.

임의의 $p,q,r \in A$에 대해서 다음이 성립함을 증명하여라.

$$ (r - q) + (q - p) = (r-p) $$

**Proof**

substraction 표현의 정의에 따라 다음이 두가지가 성립한다.

$$ p + (r-q) + (q-p) = p + (q-p) + (r-q) = q+(r-q) = r $$

$$ p +(r-p) = r $$

그리고 명제2에 의해 다음이 성립한다.

$$ p+ (r-q)+(q-p) = p + (r-p) \implies (r - q) + (q - p) = (r-p) \qed $$

#### 따름정리
Affine space $A$가 있다고 하자.

이 때, 임의의 $a,b,c,d \in A$에서 다음이 성립함을 증명하여라.

$$ b-a = d-c \iff c-a = d-b $$

**Proof**

명제 4에 의해 다음이 성립한다.

$$ d-a = (d-b) + (b-a) = (d-c) + (c-a) $$

따라서, 다음이 성립한다.

$$ b-a = d-c \iff c-a = d-b \qed $$

##### 참고
이를 `평행사변형 특성(parallelogram property)`이라고 하며, Affine space는 parallelogram property를 만족한다.

> Reference  
> [wikipedia - Affine_space#Subtraction_and_Weyl's_axioms](https://en.wikipedia.org/wiki/Affine_space#Subtraction_and_Weyl's_axioms)  
> [wikipedia - Equipollence_(geometry)#Parallelogram_property](https://en.wikipedia.org/wiki/Equipollence_(geometry)#Parallelogram_property)  


## Affine subspace
Affine space $A$와 $V_B \le V_A$가 있다고 하자.

$p \in A$에 대해서, 집합 $B$를 다음과 같이 정의하자.

$$ B := \{ p + v \enspace | \enspace v \in V_B \} $$

그러면 $B$는 vector space $V_B$가 주어진 affine space가 되며, $B$를 $A$의 affine subspace라 한다.

> Reference   
> [wikipedia - Affine_space#Affine_subspaces_and_parallelism](https://en.wikipedia.org/wiki/Affine_space#Affine_subspaces_and_parallelism)  

### 참고1
Affine space $A$와 $B\le A$가 있다고 하자.

이 때, $V_B$를 `direction`이라고 한다.

> Reference  
> [wikipedia - Affine_space#Affine_subspaces_and_parallelism](https://en.wikipedia.org/wiki/Affine_space#Affine_subspaces_and_parallelism)  

### 참고2
affine space $A$와 subspace $B,C \le A$가 있다고 하자.

이 때, $V_B = V_C$이면, $B$와 $C$가 `평행(parallel)`하다고 한다.

따라서, 모든 translation $A \rightarrow A : a \mapsto a+v$는 모든 affine subspace를 parallel 한 subspace로 mapping 한다.

예를 들어, $B=\{ p+u | u\in V_B \}$를 translation 하면 $B' = \set{ (p+v)+u | u \in V_B}$이 되고 둘은 paralle하다.

> Reference  
> [wikipedia - Affine_space#Affine_subspaces_and_parallelism](https://en.wikipedia.org/wiki/Affine_space#Affine_subspaces_and_parallelism)  


---

### 명제1
Affine space $A$와 $v \in V_A$가 있다고 하자.

이 때, 함수 $f_v:A \rightarrow A$를 다음과 같이 정의하자.

$$f_v : A \rightarrow A \st p \mapsto p + v$$

이 때, $f_v$가 bijectivce임을 증명하여라.

**Proof**

[injective]  
$p_1,p_2 \in A$라 하자.  

$$ \begin{aligned} & f_v(p_1) = f_v(p_2) \\ \implies& p_1 + v = p_2 + v \\ \implies& p_1 + v + v^{-1} = p_2 + v + v^{-1} \\ \implies& p_1 + 0_V = p_2 + 0_V \\ \implies& p_1 = p_2 \qed\end{aligned} $$

[surjective]  
$A$의 임의의 element를 $p$라고 할 떄, $q$를 다음과 같이 정의하자.

$$ q:= p+(-v) $$

이 때, $+$는 binary operation임으로 다음이 성립한다.

$$ q \in A $$

따라서, $f_v$의 정의에 의해 다음이 성립한다.

$$ f_v(q) = q + v = p+(-v)+v = p+0_{V_A} = p $$

즉, 다음이 성립한다.

$$ \forall p \in A, \quad \exist q \in A \st f_v(q) = p \qed $$

#### 참고
$f_v$가 bijective이기 때문에 다음이 성립한다.

$$ \forall p \in A, \quad \exist! q \in A \st f_v(q) = p \qed $$

이를, Existence of one-to-one translations라고 표현한다.

> Reference  
> [wikipedia - Affine_space#Definition](https://en.wikipedia.org/wiki/Affine_space#Definition)




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




## Coordinate System of Affine Space
affine space $A$가 있다고 하자.

$A$의 임의의 점을 $p_0$라고 하고 $V_A$의 임의의 기저를 $\beta$라고 할 때, $(p_0,\beta)$를 $A$의 coordinate system이라고 한다.

## Coordinate of Affine Space
affine space $A$와 coordinate system $(p_0, \beta)$가 있다고 하자.

그러면 $p\in A$에 대해서, $p$의 coordinate는 다음과 같이 정의된다.

$$ [p-p_0]_\beta $$

## Change of Coordinate of Affine Space

affine space $A$와 두개의 coordinate system $(p_1, \beta), (p_2, \gamma)$가 있다고 하자.

임의의 $p \in A$에 대해서 두개의 coordinate system에서 coordinate 사이에 관계는 다음과 같다.

$$ [p-p_2]_\gamma = [p-p_1 + p_1-p_2]_\gamma = [p-p_1+t]_\gamma$$