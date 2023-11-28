# Kernel
## Definition
Group $G_1,G_2$와 함수 $f : G_1\rightarrow G_2$가 있다고 하자.

이 때, $\ker(f)$는 다음과 같이 정의된 집합이다.

$$ \ker(f) := \Set{a \in G_1 | f(a) = e_{G_2}}$$

### 명제1
Group $G_1,G_2$와 group homeomorphism $f : G_1 \rightarrow G_2$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \ker(f) \text{ is an subgroup of } G_1 $$

**Proof**

[closed]  
$\ker(f)$의 임의의 elements를 $a,b$라고 하면 다음이 성립한다.

$$ f(a+b) = f(a) + f(b) = e_{G_2} \implies f(a+b) \in \ker(f) \qed $$

[inverse]  
$\ker(f)$의 임의의 elements를 $a$라고 하면 다음이 성립한다.

$$ f(a^{-1}) = f(a)^{-1} = e_{G_2} \implies f(a^{-1}) \in \ker(f) \qed $$

### 명제2
Group $G_1,G_2$와 group homomorphism $f : G_1 \rightarrow G_2$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \ker(f) = \Set{e_{G_1}} \iff f \text{ is an injective} $$

**Proof**

[$\implies$]  
$G_1$의 임의의 elements를 $a,b$라고 하면 다음이 성립한다.

$$ f(a) = f(b) \implies f(a * b^{-1}) = e_{G_2} \implies a*b^{-1} = e_{G_1} \implies a=b \qed $$

[$\impliedby$]  
$f(e_{G_1}) = e_{G_2}$이고 $f$가 injective임으로 자명하다. $\qed$