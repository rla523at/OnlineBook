# Image
## Definition
Group $G_1,G_2$와 함수 $f : G_1\rightarrow G_2$가 있다고 하자.

이 때, $\img(f)$는 다음과 같이 정의된 집합이다.

$$ \img(f) := \Set{ f(a) \in G_2 | a \in G_1}$$

### 명제1
Group $G_1,G_2$와 group homeomorphism $f : G_1 \rightarrow G_2$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \img(f) \text{ is an subgroup of } G_2 $$

**Proof**

[closed]  
$\img(f)$의 임의의 elements를 $f(a),f(b)$라고 하면 다음이 성립한다.

$$ \begin{gathered} f(a) + f(b) = f(a+b) \\ a+b \in G_1 \end{gathered} \implies f(a+b) \in \img(f) \qed $$

[inverse]  
$\img(f)$의 임의의 elements를 $f(a)$라고 하면 다음이 성립한다.

$$ \begin{gathered} f(a)^{-1} = f(a^{-1}) \\ a^{-1} \in G_1 \end{gathered} \implies f(a)^{-1} \in \img(f) \qed $$