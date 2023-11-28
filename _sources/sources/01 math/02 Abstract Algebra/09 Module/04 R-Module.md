# R-Module
## 정의
Abelian group $A$와 ring $R$이 있다고 하자.

$R$이 commutative ring일 때, $A$와 $R$ 그리고 action of $R$ on $A$ $\cdot$이 주어진 대수구조를 `R-가군(R-module)`이라고 한다. 

### 명제1
Abelian group $A$와 commutative ring $R$이 있다고 하자.

이 때, $_RA$나 $A_R$이 주어졌을 떄, 다음을 증명하여라.

$$ \exist A_R \text{ or } _RA \st {_RA} = A_R $$

**Proof**

먼저, left action of $R$ on $A$ $\cdot_L$이 주어져있다고 하자.

이 떄, $\cdot_R$을 다음을 만족하게 정의하자.

$$ \begin{gathered} \forall x \in A, \quad \forall r \in R, \quad r \cdot_R x = x \cdot_L r \\ \cdot_R \text{ is right action of } R \text{ on } A \end{gathered}  $$

그러면 $\forall r_1,r_2 \in R, \quad \forall m_1,m_2 \in M$에 대해 다음이 성립한다.

$$ \begin{aligned} (r_1 + r_2) \cdot_L m_1 &= r_3\cdot _Lm_1 \\&=m_1 \cdot_R r_3 \\&= m_1 \cdot_R (r_1 +r_2) \\ r_1 \cdot_L m_1 + r_2 \cdot_L m_1 &= m_1 \cdot_R r_1 + m_1 \cdot_R r_2 \\&= m_1 \cdot_R (r_1 + r_2)\end{aligned} $$

$$ \begin{aligned} r_1 \cdot_L (m_1 + m_2) &= r_1 \cdot_L m_3 \\&= m_3 \cdot_R r_1 \\&= (m_1 + m_2)\cdot_R r_2 \\ r_1 \cdot_L m_1 + r_1 \cdot_L m_2 &= m_1 \cdot_R r_1 + m_2 \cdot_R r_1 \\&= (m_1 + m_2)\cdot_R r_2 \end{aligned} $$

$$ \begin{aligned} (r_1 \cdot r_2)\cdot_Lm_1 &= r_3 \cdot_L m_1 \\&= m_1 \cdot_R r_3 \\&= m_1 \cdot_r (r_1 \cdot r_2) \\ r_1 \cdot_L (r_2 \cdot_L m_1) &= r_1 \cdot_L (m_1 \cdot_R r_2) \\&= (m_1 \cdot_R r_2) \cdot_R r_1 \\&= m_1 \cdot_R (r_2 \cdot r_1) \\&= m_1 \cdot_r (r_1 \cdot r_2) \end{aligned} $$

$$ 1_R \cdot_L m_1 = m_1 \cdot_R 1_R $$


즉, 위와 같이 정의한 right action이 모든 경우에 대해 left action과 동일한 값을 준다는 것을 알 수 있다.

그럼으로 다음이 성립한다.

$$ A_R = _RA $$

똑같이, right action of $R$ on $A$ $\cdot_R$이 주어져 있을 떄 동일한 과정을 거쳐서 다음을 증명할 수 있다.

$$ _RA = A_R \qed $$


> Reference  
> [math.stackexchange1](https://math.stackexchange.com/questions/773402/modules-over-commutative-rings)  
> [math.stackexchange2](https://math.stackexchange.com/questions/2203324/when-ring-is-commutative-prove-that-left-and-right-modules-coincide)

#### 참고1
$R$이 commutative ring이 아닌 경우에 다음을 만족하는 $\cdot_R$이 있다고 가정하자.

$$ \exist \cdot_R : A \times R \rightarrow A \st  \forall x \in A, \quad \forall r \in R, \quad r \cdot_R x = x \cdot_L r $$

$\cdot_R$의 정의에 의해 다음이 성립한다.

$$ \begin{gathered} (r_1\cdot r_2)\cdot_L m_1 = m_1 \cdot_r (r_1\cdot r_2) \\ r_1 \cdot_L (r_2\cdot_Lm_1) = (m_1 \cdot_r r_2) \cdot_r r_1 = m_1 \cdot_r(r_2 \cdot r_1) \end{gathered}  $$

이 떄, $\cdot_L$의 성질에 의해 $(r_1\cdot r_2)\cdot_L m_1 = r_1 \cdot_L (r_2\cdot_Lm_1)$임으로 다음이 성립한다.

$$ m_1 \cdot_r (r_1\cdot r_2) = m_1 \cdot_r(r_2 \cdot r_1) $$

$R$이 commutative ring이 아니라는 전제에 모순임으로 proof by contradiction에 의해 $\cdot_R$은 존재하지 않는다.

#### 참고2
commutative ring이 아닌 경우에 right action의 추가 조건으로 $(xr_1)r_2 = r_1(r_2x)$을 줘도 $(xr_1)r_2r_3 = r_1r_2(r_3x)$를 만족하지 못한다. 즉, commutative ring이 아니면 left module과 동일한 값을 주는 right module을 만들 수 없다.


#### 참고3
한 마디로 순서를 바꿔 적어도 동일한 값을 주는 action을 정의할 수 있음으로 순서에 상관없이 표기가 가능해진다.

예를 들어, Left module이 주어져서 $x(r_1r_2)$을 알고 있다고 하자.

이때 위의 명제에 의해 $x(r_1r_2) = (r_1r_2)x$인 right module을 만들 수 있다.

따라서 동일한 값이기 때문에 $x(r_1r_2)$과 $(r_1r_2)x$를 구분해서 쓸 필요가 없어진다. 