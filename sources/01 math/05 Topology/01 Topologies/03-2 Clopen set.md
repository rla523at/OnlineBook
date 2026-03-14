# Clopen set
## 정의
Topological space $X$가 있다고 하자.

subset $U$가 open set이면서 동시에 closed set일 떄, $U$를 `clopen set`이라고 한다.

### 명제
Topological space $X$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ X \text{ has discrete topology } \iff \text{every subset of }X\text{ are clopen} $$

**Proof**

[$\implies$]  
$X$의 임의의 subset을 $U$라고 하자.

그러면 다음이 성립한다.

$$ U^c \text{ is an subset of } X $$

전제에 의해 다음이 성립한다. 

$$ U,U^c \text{ is an open set} $$

$U^c$가 open set임으로 closed set의 정의에 의해 다음이 성립한다.

$$ U \text{ is a closed set} $$

$X$의 임의의 subset이 clopen set임으로 다음이 성립한다.

$$ \text{every subset of }X\text{ are clopen} \qed $$

[$\impliedby$]  
$X$의 모든 subset이 open set임으로 다음이 성립한다.

$$ X \text{ has discrete topology } \qed $$