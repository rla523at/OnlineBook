# Left Module
## 정의
Abelian group $A$와 ring $R$이 있다고 하자.

함수 $\cdot : R \times A \rightarrow A$이 다음을 만족시킨다고 하자.

$$ \begin{array}{ll} \forall s \in A & 1_R \cdot s = s \\  \forall r_1,r_2 \in R & \forall s \in A, \quad (r_1 \cdot_R r_2)\cdot s = r_1\cdot (r_2 \cdot s) \\ \forall r_1,r_2 \in R, \quad \forall s \in A &  (r_1 +_R r_2)\cdot s = r_1\cdot s + r_2\cdot s \\ \forall r \in R, \quad \forall s_1,s_2 \in A &  r\cdot (s_1 + s_2) = r\cdot s_1 + r \cdot s_2  \end{array} $$

이 떄, `왼쪽 가군(left module)` $_RA$는 $(A,R,\cdot)$이 주어진 algebraic structure이다.

### 참고
1,2번 성질에 의해 group action임을 요구한다.

3,4번 성질은 module action이 각각의 + 연산과 distributive함을 요구한다.

### 명제1
Left module $_RA$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ \forall x \in A, \quad  0_Rx =0_A $$

**Proof**

다음이 성립한다.

$$ 0_Rx = (0_R + 0_R)x = 0_Rx + 0_Rx \implies 0_Rx = 0_A \qed $$

### 명제2
Left module $_RA$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \forall x \in A, \quad -x = (-1_R)x $$

**Proof**

Left action의 성질에 의해 다음이 성립한다. 

$$ \forall x \in A, \quad \begin{gathered} -x + x = (-1_R + 1_R)x = 0_Rx \\ x + (-x) = (1_R + -1_R)x = 0_Rx  \end{gathered}  $$

명제1에의해 다음이 성립한다.

$$ \forall x \in A, \quad \begin{gathered} (-1_R)x + x = 0_M \\ x + (-1_R)x = 0_M  \end{gathered}  $$

따라서, inverse element의 정에의해 다음이 성립한다.

$$ \forall x \in A, \quad -x = (-1_R)x \qed $$