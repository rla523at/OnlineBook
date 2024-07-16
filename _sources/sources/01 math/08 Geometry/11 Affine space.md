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
Affine space $A$와 $v \in V_A$가 있다고 하자.

그리고 함수 $F_v:A \rightarrow A$를 다음과 같이 정의하자.

$$F_v : A \rightarrow A \st p \mapsto p + v$$

이 때, $F_v$가 bijectivce임을 증명하여라.

**Proof**

[well defined]  
$p_1,p_2 \in A$라고 하자.

$+$는 group action이기 때문에, well defined 되어 있음으로 다음이 성립한다.

$$ \begin{aligned} 
        & p_1 = p_2 \\ 
\implies& p_1 + v = p_2 + v \qed \\ 
\end{aligned} $$

[injective]  
$p_1,p_2 \in A$라 하자.  

$$ \begin{aligned} & F_v(p_1) = F_v(p_2) \\ \implies& p_1 + v = p_2 + v \\ \implies& p_1 + v + v^{-1} = p_2 + v + v^{-1} \\ \implies& p_1 + 0_V = p_2 + 0_V \\ \implies& p_1 = p_2 \qed\end{aligned} $$

[surjective]   
$A$의 임의의 element를 $p$라고 할 떄, $q$를 다음과 같이 정의하자.

$$ q:= p+(-v) $$

이 때, $+$는 binary operation임으로 다음이 성립한다.

$$ q \in A $$

따라서, $F_v$의 정의에 의해 다음이 성립한다.

$$ F_v(q) = q + v = p+(-v)+v = p+0_{V_A} = p $$

즉, 다음이 성립한다.

$$ \forall p \in A, \quad \exist q \in A \st F_v(q) = p \qed $$

> Reference  
> [math.stackexchange - prove-that-an-action-is-well-defined](https://math.stackexchange.com/questions/2541410/prove-that-an-action-is-well-defined)  

#### 참고
$F_v$가 bijective이기 때문에 다음이 성립한다.

$$ \forall p \in A, \quad \exist! q \in A \st F_v(q) = p \qed $$

이를, Existence of one-to-one translations라고 표현한다.

> Reference  
> [wikipedia - Affine_space#Definition](https://en.wikipedia.org/wiki/Affine_space#Definition)

### 명제2
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

#### 참고2
substraction의 정의에 따라 $p,q \in A$, $v \in V_A$에 대해서 다음 표현식들이 성립한다.

$$ p + (q-p) = q $$

$$ (p + (q-p)) - p = q-p $$

$$ (p+v) - p = v $$

#### 따름정리
Affine space $A$가 있다고 하자.

임의의 $p \in A$에 대해서, 다음이 성립함을 증명하여라.

$$ p+v = p+w \iff v = w $$

**Proof**

[$\implies$]  
$p+v=p+w=q$라고 하자.

그러면, 명제1에 의해 $p+v = q$,  $p+w =q$를 만족하는 $v,w$는 유일하게 존재한다.

즉, $v=w$일 수 밖에 없다. $\qed$

[$\impliedby$]  
Affine space의 정의에 의해 $F_p : V_A \rightarrow A \st v \mapsto p + v$는 well-defined 함수이다.

따라서, 다음이 성립한다.

$$v=w \implies F_p(v) = F_p(w) \implies p+v = p+w \qed $$

### 명제2
Affine space $A$가 있다고 하자.

임의의 $p,q,r \in A$에 대해서 다음이 성립함을 증명하여라.

$$ (r - q) + (q - p) = (r-p) $$

**Proof**

substraction 표현의 정의에 따라 다음이 두가지가 성립한다.

$$ p + (r-q) + (q-p) = p + (q-p) + (r-q) = q+(r-q) = r $$

$$ p +(r-p) = r $$

그리고 명제1의 따름정리에 의해 다음이 성립한다.

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

## Coordinate System of Affine Space
affine space $A$가 있다고 하자.

$A$의 임의의 점을 $p_0$라고 하고 $V_A$의 임의의 기저를 $\beta$라고 할 때, $(p_0,\beta)$를 $A$의 coordinate system이라고 한다.

## Coordinate of Affine Space
affine space $A$와 coordinate system $(p_0, \beta)$가 있다고 하자.

그러면 $p\in A$에 대해서, $p$의 coordinate는 다음과 같이 정의된다.

$$ [p]_{(p_0,\beta)} := [p-p_0]_\beta $$

### 명제1
affine space $A$와 coordinate system $(p_0,\beta)$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \phi : A \rightarrow \R^n \st p \mapsto [p]_{(p_0,\beta)} \text{ is a bijective} $$

**Proof**

[well defined]    
$p_1, p_2 \in A$에 대해서 $p_1=p_2$ 라고 하자.

$$ \begin{aligned} 
        & p_1 = p_2 \\ 
\implies& [p_1]_{p_0,\beta} = [p_1-p_0]_\beta = [p_2-p_0]_\beta = [p_2]_{p_0,\beta} \qed \\ 
\end{aligned} $$

[injective]  

명제2에 의해 $P-p_0$가 유일하게 존재함으로, 함수 $\phi$는 잘 정의되며 injective이다.

또한 +는 연산에 닫혀있음으로, $a \in \R^n$에 대해, $P = p_0 + a_i\beta_i$를 만족하는 $P$가 항상 존재함으로 surjective이다. $\qed$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Affine_space)  
> [what-are-differences-between-affine-space-and-vector-space - mathematics](https://math.stackexchange.com/questions/884666/what-are-differences-between-affine-space-and-vector-space)  
> [an-affine-space-is-nothing-more-than-a-vector-space - mathmatics](https://math.stackexchange.com/questions/3527297/an-affine-space-is-nothing-more-than-a-vector-space-whose-origin-we-try-to-forg)  
> [when-is-a-vector-glued-to-the-origin - mathmatics](https://math.stackexchange.com/questions/2392479/when-is-a-vector-glued-to-the-origin)  
> [must-vectors-in-mathbbrn-have-their-tail-at-origin - mathematics](https://math.stackexchange.com/questions/627616/must-vectors-in-mathbbrn-have-their-tail-at-origin)
> [Euclidean space - wiki](https://en.wikipedia.org/wiki/Euclidean_space) 

## Change of Coordinate of Affine Space

affine space $A$와 두개의 coordinate system $(p_1, \beta), (p_2, \gamma)$가 있다고 하자.

임의의 $p \in A$에 대해서 두개의 coordinate system에서 coordinate 사이에 관계는 다음과 같다.

$$ [p-p_2]_\gamma = [p-p_1 + p_1-p_2]_\gamma = [p-p_1+t]_\gamma$$