# Symmetric Group
## Definition
finite set $X$가 있다고 하자. 

set $S_X$를 다음과 같이 정의하자.

$$ S_X := \Set{f : X \rightarrow X | f \text{ is bijective}} $$

이 때, $(S_X,\circ)$를 $X$위의 `대칭군(symmetric group)`이라고 한다.

### 명제1
Set$X$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ (S_X,\circ) \text{ is a group} $$

**Proof**

[closed]  
$S_X$의 임의의 elements를 $f,g$라고 하면 다음이 성립한다.

$$ f\circ g : f \rightarrow f \in S_X  \qed $$

[identity]  
$S_X$의 임의의 elements를 $f$라고 하면 $id_X \in S_X$임으로 다음이 성립한다.

$$ f \circ id_X = id_X \circ f = f $$

따라서, $id_X$는 $(S_X,\circ)$의 identity element이다. $\qed$

[inverse]  
$S_X$의 임의의 elements를 $f$라고 하면 $f^{-1} \in S_X$임으로 다음이 성립한다.

$$ f \circ f^{-1} = f^{-1} \circ f = id_X $$

따라서, $(S_X,\circ)$의 모든 element는 inverse element를 갖는다.$\qed$

### 명제2
Group $G$와 $G$-set $X$가 있다고 하자.

$G$의 임의의 element를 $g$라고 할 때, 함수 $\lambda_g$를 다음과 같이 정의하자.

$$ \lambda_g : X \rightarrow X \st x \mapsto g \cdot x $$

이 때, 다음을 증명하여라.

$$ \lambda_g \in S_X $$

**Proof**

Group의 element가 고정된 group action은 bijective function임으로 $\lambda_g \in S_X$이다. $\qed$

#### 참고
Symmetric group에는 Group의 element가 고정된 group action들이 포함되어 있다.

### 명제3(Cayley Theorem)
Group $G$가 있다고 하자.

$G$의 임의의 element를 $g$라고 할 때, 함수 $\lambda_g$를 다음과 같이 정의하자.

$$ \lambda_g : G \rightarrow G \st h \mapsto gh $$

이 떄, 다음을 증명하여라.

$$ \lambda : G \rightarrow S_G \st g \mapsto \lambda_g \text{ is a group monomorphism} $$

**Proof**

[homomorphism]  
$G$의 임의의 elements를 $g_1,g_2$라고 하면 보조명제에의해 다음이 성립한다.

$$ \lambda(g_1g_2) = \lambda_{g_1g_2} = \lambda_{g_1} \circ \lambda_{g_2} = \lambda(g_1) \circ \lambda(g_2) \qed $$

[injective]  
-[proof1]  
$$ \begin{aligned} & \lambda_g = id_G \\\implies& \forall h \in G, \enspace \lambda_g(h) = id_G(h) \\\implies& \forall h \in G, gh = h \\\implies& g = e_G \end{aligned}  $$

따라서, 다음이 성립한다.

$$ \ker(\lambda) = \Set{e_G} \implies \lambda \text{ is injective } \qed $$

-[proof2]  
$G$의 임의의 elements를 $g_1,g_2$라고 하면 다음이 성립한다.

$$ \lambda_{g_1} = \lambda_{g_2} \implies \forall h \in G, \enspace g_1h = g_2h \implies g_1 = g_2 \qed $$

#### 보조명제
다음을 증명하여라.

$$ \lambda_{g_1g_2} = \lambda_{g_1} \circ \lambda_{g_2} $$

**Proof**

$G$의 임의의 element를 $h$라고 하면 다음이 성립한다.

$$ \begin{aligned} \lambda_{g_1g_2}(h) &= g_1g_2h \\&= g_1 \cdot \lambda_{g_2}(h) \\&= \lambda_{g_1}( \lambda_{g_2}(h)) \\&= (\lambda_{g_1} \circ \lambda_{g_2})(h) \qed \end{aligned} $$

#### 참고

$\img(\lambda) =\lambda(G)$는 $S_G$의 subgroup임으로 다음이 성립한다.

$$ \lambda' : G \rightarrow \img(\lambda) \text{ is a group isomorphism} $$

즉, 모든 group은 symmetric group의 subgroup과 isomorphic하다.