# Bijective Function
## 정의
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$f$가 injective function이면서 동시에 surjective function이면 `전단사 함수(bijective function)` 혹은 `일대일 대응(one-to-one correspondence)`라고 한다.

### 명제1
집합 $X,Y,Z$와 함수 $f : X \rightarrow Y, g : Y \rightarrow Z$가 있을 때 다음을 증명하여라.

$$ f,g \text{ is bijective} \implies g \circ f : X \rightarrow Z \text{ is bijective} $$

**proof**

$f,g$는 injective function임으로, 다음이 성립한다.

$$ g \circ f \text{ is an injective function} $$

동시에 $f,g$는 surjective function임으로, 다음이 성립한다.

$$ g \circ f \text{ is an surjective function} $$

따라서 bijective function의 정의에 의해, 다음이 성립한다.

$$g \circ f \text{ is a bijective function} \qed$$

### 명제2
집합 $X,Y$와 함수 $f : X \rightarrow Y$가 있다고 하자.

$S \subseteq X$가 있을 때, 다음을 증명하여라.

$$ f \text{ is bijective} \implies f|_{S \times f(S)} \text{ is bijective} $$

**Proof**

Injective function의 성질에 의해 다음이 성립한다.

$$f|_{S \times f(S)} \text{ is an injective function} $$

그리고 codomain이 $f(S)$로 restriction 됐음으로 다음이 성립한다.

$$f|_{S \times f(S)} \text{ is a surjective function} $$

따라서 bijective function의 정의에 의해, 다음이 성립한다.

$$f|_{S \times f(S)} \text{ is a bijective function} \qed$$
