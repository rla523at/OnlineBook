# Ring
## 정의
집합 $R$과 두 이항연산 $+:R \times R \rightarrow R$, $\cdot : R \times R \rightarrow R$이 있을 때, `환(ring)` $(R,+,\cdot)$이란 다음을 만족하는 대수 구조이다.

$$ \begin{gathered} (R,+) \text{ is an Abelian group} \\ (R,\cdot) \text { is a monoid } \\ \cdot \text { is distributive w.r.t. } + \end{gathered} $$
     
> Reference  
> [wiki](https://en.wikipedia.org/wiki/Ring_(mathematics)#Definition)

### 참고1
$R$의 $\cdot$에 대한 identity를 $1_R$이라고 표현한다.



### 명제1
환 $R$이 있다고 하자.

이 때, 다음을 증명하여라.

$$\forall x \in R, \quad 0_Rx =0_R$$

**Proof**

$$ \begin{gathered} 0_Rx  = (0_R + 0_R)x = 0_Rx + 0_Rx \\0_Rx = 0_Rx + 0_R \end{gathered} \implies \therefore 0_Rx = 0_R \qed $$

### 명제2

환 $(R,+,\cdot)$이 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \forall x,y \in R, \enspace -(xy)=(-x)y$$

**proof**

$$ (x + (-x))y = xy + (-x)y = 0_R \implies -(xy) = (-x)y $$

