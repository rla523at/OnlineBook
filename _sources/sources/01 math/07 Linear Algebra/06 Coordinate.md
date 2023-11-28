# Coordinate
## 정의
$n$ dimensional vector space $V/\F$와 basis $\beta$가 있다고 하자.

$V$의 임의의 element를 $v$라고 할 때, $v = a^i\beta_i$인 경우 $(a^1,\cdots,a^n) \in \F^n$을 $v$의 `coordinate`라고 한다.

### 명제1
$n$ dimensional vector space $V/\F$와 basis $\beta$가 있다고 하자.

$V$의 임의의 element를 $v$라고 할 때, 다음을 증명하여라.

$$ \text{coordinate of } v \text{ is unqiue} $$

**Proof**

$v$가 다음을 만족한다고 하자.

$$ v =a^i\beta_i = b^i\beta_i $$

그러면 $\beta$가 basis임으로 다음이 성립한다.

$$ \begin{aligned} & (a^i-b^i)\beta_i = 0_V \\\implies& a^i-b^i = 0_\F, \enspace i=1,\cdots,n \\ \implies& a^i = b^i \end{aligned}  $$

따라서 $v$는 유일한 coordinate를 갖는다. $\qed$

### 명제2
$n$ dimensional vector space $V/\F$와 basis $\beta$가 있다고 하자.

함수 $\phi$를 다음과 같이 정의하자.

$$ \phi := V \rightarrow \F^n \st v \mapsto (a^1,\cdots,a^n) $$

이 떄, 다음을 증명하여라.

$$ \phi \text{ is bijective} $$

**Proof**

[injective]  
$V$의 임의의 두 element를 $v_1,v_2$라고 하자.

그러면 다음이 성립한다.

$$ \phi(v_1)=\phi(v_2) \implies v_1 = a^i\beta_i = v_2 \qed $$

[surjective]  
$\F^n$의 임의의 element를 $(a^1,\cdots,a^n)$이라고 하자.

$V=\span(\beta)$임으로 basis의 모든 linear combination을 포함한다. 

따라서 다음이 성립한다.

$$ \exist v \in V \st \phi(v) = (a^1,\cdots,a^n) \qed $$