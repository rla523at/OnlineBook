# Subspace
## 정의
Vector space $V/\F$와 $V$의 subset $S$가 있다고 하자.

$S$가 $V$의 연산들에 의해 vector space가 될 떄, $S$를 $V$의 `subspace`라고 한다.

### 명제1
Vector space $V/\F$와 $V$의 subset $S$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \begin{gathered} S \text{ is closed under operations of } V \\ S \neq \empty \end{gathered} \iff S \text{ is a subspace of } V  $$

**Proof**

[Abeilian Group]  
-[identity]  
$S$의 임의의 element를 $x$라고 하자.

$S$가 scalar multiplication에 닫혀 있음으로 다음이 성립한다.

$$ 0x = 0_V \in S $$ 

-[inverse]  
$S$의 임의의 element를 $x$라고 하자.

$S$가 scalar multiplication에 닫혀 있음으로 다음이 성립한다.

$$ (-1)x = x^{-1} \in S $$

-[commutative]  
$S$의 element는 $V$의 element임으로 commutative 하다. $\qed$

[module]  
$S$의 element는 $V$의 element이고 scalar multiplication에 닫혀 있음으로 $\F-$module의 정의을 만족한다. $\qed$

