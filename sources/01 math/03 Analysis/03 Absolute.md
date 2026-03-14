# Absolute
## 정의
Abelian group $A$가 있다고 하자.

$A$가 totally ordered set일 떄, 절대값 함수 $abs$는 다음과 같이 정의된 함수이다.

$$ abs: A \rightarrow A \st x \mapsto \begin{cases} x & \text{if } 0_A \le x \\ -x & \text{else} \end{cases} $$

### 참고
$abs(a) = |a|$로 표기한다.

### 명제1
다음을 증명하여라.

$$ \forall x \in A, \enspace \forall r \in A^+, \quad |x| \le r \iff -r \le x \le r $$

**Proof**

[$x \le 0$]  
$x \le 0$ 임으로 다음이 성립한다.

$$ |x| \le r \iff -x \le r \iff -r \le x $$

[$0 \le x$]  
$0 \le x$ 임으로 다음이 성립한다.

$$ |x| \le r \iff x \le r $$

[결론]  
따라서 다음이 성립한다.
$$ \forall x \in A, \enspace \forall r \in A^+, \quad |x| \le r \iff -r \le |x| \le r \qed $$

