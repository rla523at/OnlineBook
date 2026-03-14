# Subgroup
## 정의
Group $G$와 $G$의 부분집합 $H$가 있다고 하자.

$(H,*_G)$가 군이 되면 $H$를 $G$의 `부분군(subgroup)`이라고 하고 $H\le G$로 표기한다.  

### 예시
$$(\mathbb{Q},+) \le (\R,+) \\ (\mathbb{Q}-\{0\},\times) \le (\R-\{0\},\times)$$

### 명제1
Group $G$와 $G$의 subgroup $H$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ e_H = e_G $$

**Proof**

$H \subseteq G$임으로 다음이 성립한다.

$$ e_H, e_G \in G $$

$\forall x \in H$에 대해서, $x \in G$임으로 다음이 성립한다.

$$ x+e_H = x + e_G $$

이 떄, group의 cancellation law에 의해 다음이 성립한다.

$$ e_H = e_G \qed $$

### 명제2
Group $G$와 $G$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ U\le G \iff \begin{gathered} \forall a,b\in U, \quad a*_Gb\in U \\ \forall x \in U, \quad  x^{-1} \in U  \end{gathered} $$

**proof**

[$\implies$]  
전제에 의해 $(H,*_G)$는 group이기 때문에, group의 성질에 의해 다음이 성립한다.

$$ \begin{gathered} H \text{ is closed under }*_G \\ \forall x \in H, \quad \exist x^{-1} \in H \qed \end{gathered}  $$

[$\impliedby$]  
-[closed]  
첫번째 전제에 의해 연산에 닫혀있다. $\qed$

-[identity]  
inverse element가 존재하고 연산에 닫혀있음으로 다음이 성립한다.

$$ e_G \in H \qed $$

-[inverse element]  
두번째 전제에 의해 inverse element가 존재한다. $\qed$

### 명제3
Group $G$와 $G$의 두 subgroup $H_{1,2}$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ H_1\cap H_2 \text{ is a subgroup of } G $$

**Proof**

$H_1\cap H_2$의 임의의 두 element를 $a,b$라고 하면 다음이 성립한다.

$$ a,b \in H_{1,2} \implies a*b \in H_{1,2} \implies a*b \in H_1 \cap H_2 $$

그리고 다음도 성립한다.

$$ a^{-1} \in H_{1,2} \implies a^{-1} \in H_1 \cap H_2 $$

따라서, 명제2에 의해 다음이 성립한다.

$$ H_1\cap H_2 \text{ is a subgroup of } G \qed $$



