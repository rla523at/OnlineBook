# Rational Number
## 정의
다음과 같이 정의된 집합 $\Q$을 `유리수(rational number)`라고 한다.

$$ \Q := \Set{\frac{n}{m} | n \in \Z, \enspace m \in \Z-\Set{0}} $$

### 명제1
다음을 증명하여라.

$$ \Q \text{ has an archimedean property} $$

**Proof**

$x \in \Q$가 있다고 하자.

만약 $x \in \Q^-_0$면 모든 자연수가 $x$보다 큼으로 다음이 자명하게 성립한다.

$$ \forall x \in \Q^-_0, \quad \exist n \in \N \st x < n $$


다음으로 $x\in \Q^+$인 경우를 보자.

$\Q^+$의 정의에 의해  다음이 성립한다.

$$ \forall x \in \Q^+, \quad \exist n,m \in \N \st x = \frac{n}{m} $$

따라서, 다음이 성립한다.

$$ mx = n $$

$\forall n \in \N, \quad x \le nx$임으로 다음이 성립한다.

$$ x \le mx \implies x \le n $$

따라서, 다음이 성립한다.

$$ \forall x \in \Q^+, \quad x < n+1 $$

$n+1 \in \N$임으로 다음이 성립한다.

$$ \forall x \in \Q^+, \quad \exist n \in \N \st x < n $$

$\Q = \Q^-_0 \cup \Q^+$임으로 위의 결과를 종합하면 다음이 성립한다.

$$ \forall x \in \Q \implies \exist n \in \N \st x < n $$

#### 따름명제1.1
다음을 증명하여라.

$$ \forall q \in \Q, \quad \exist n \in \N \st -n < q < n $$

**Proof**

명제1에 의해 다음이 성립한다.

$$ \forall q \in \Q, \quad \exist n \in \N \st |q| < n $$

따라서, 다음이 성립한다.

$$ \forall q \in \Q, \quad \exist n \in \N \st -n < q < n \qed $$

### 명제2
$\Z$의 bounded above subset과 bounded below subset $U^q_a, U^q_b$를 다음과 같이 정의하자.

$$ \forall q \in \Q, \quad  \begin{gathered} U^q_b := \Set{ z \in \Z | q \le z} \\ U^q_a := \Set{ z \in \Z | z \le q} \end{gathered}  $$

이 떄, 다음을 증명하여라.

$$ \forall q \in \Q, \quad  \begin{gathered} U^q_b \text{ has an least element} \\ U^q_a \text{ has an greatest element} \end{gathered}  $$

**Proof**

따름명제1.1에 의해 다음이 성립한다.

$$ \forall q \in \Q, \quad \exist n \in \N \st -n < q < n \implies \begin{gathered} -n \text{ is an lower bound of } U_b^q \\ -n \in U_a^q \\ n \text{ is an upper bound of } U_a^q \\ n \in U_b^q \end{gathered}  $$

따라서, 다음이 성립한다.

$$ \forall q \in \Q, \quad  \begin{gathered} U_b^q \text{ is an nonempty bounded below subset of } \Z \\ U_a^q \text{ is an nonempty bounded above subset of } \Z  \end{gathered}  $$

따라서, $\Z$의 성질에 의해 다음이 성립한다.

$$ \forall q \in \Q, \quad  \begin{gathered} U^q_b \text{ has an least element} \\ U^q_a \text{ has an greatest element} \end{gathered} \qed $$

### 명제3
다음을 증명하여라.

$$ \forall q \in \Q, \quad \exist! z \in \Z \st  z \le q < z+1 $$

**Proof**

$U_q$를 다음과 같이 정의하자.

$$ \forall q \in \Q , \quad  U_q := \Set{ z \in \Z | z \le q } $$

명제4에 의해 다음이 성립한다.

$$\forall q \in \Q, \quad \exist! u^q_M \in \Z \st \text{  greatest element of } U_q $$

$u^q_M$은 $U_q$의 greatest element임으로 다음이 성립한다.

$$ \begin{gathered} u^q_M \in U_q \iff u^q_M \le q \\ u^q_M +1 \notin U_q \iff q < u^q_M +1 \end{gathered} $$

이를 종합하면 다음이 성립한다.

$$\forall x \in \R, \quad \exist z \in \Z \st z \le x < z + 1 \qed $$

#### 따름명제3.1
다음을 증명하여라.

$$ \forall q \in \Q, \quad \exist! z \in \Z \st q < z \le q+1 $$

**Proof**

명제3에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall q \in \Q, \quad \exist! z \in \Z \st \begin{gathered} z \le q < z+1 \\ z +1 \le q+1 < z+2 \end{gathered} \\ \implies& \forall q \in \Q, \quad \exist! z \in \Z \st q < z+1 \le q+1 \qed \end{aligned}   $$


### 명제4(division theorem)
다음을 증명하여라

$$ \forall a \in \Z, \enspace \forall b \in \N, \quad \exist! q,r \in \Z \st \begin{gathered} a = qb + r \\ 0 \le r < b \end{gathered}  $$

**Proof**

Set $U$를 다음과 같이 정의하자.

$$ U:= \Set{ x \in \Z | x \le \frac{a}{b}} $$

명제3에 의해 다음이 성립한다.

$$ \exist!z\in Z \st z \le \frac{a}{b} < z+1 $$

위를 만족하는 $z$를 $q$라고 하고 $r$을 다음과 같이 정의하자.

$$ r:= a - bq $$

그러면 $a,b,q \in \Z$임으로 다음이 성립한다.

$$ \exist!r \in \Z $$

그리고 $q$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & 0 \le \frac{a}{b} -q < 1 \\ \iff& 0 \le a-bq < b \\\iff& 0 \le r < b \end{aligned}  $$

따라서, 다음이 성립한다. 

$$ \forall a \in \Z, \enspace \forall b \in \N, \quad \exist! q,r \in \Z \st \begin{gathered} a = qb + r \\ 0 \le r < b \end{gathered} \qed  $$

### 명제5
$\Q$의 subset $U$를 다음과 같이 정의하자.

$$ U := \Set{q \in \Q | 0 \le q^2 \le 2} $$

다음을 증명하여라.

$$ U \text{ doesn't have greatest element } $$

**Proof**

$u_M$ 을 다음과 같이 정의하자.

$$ \forall u \in U, \quad u_M = u + \frac{2 - u^2}{2 + u} $$

$u \in \Q$이고 $\Q$는 filed임으로 다음이 성립한다.

$$ u_M \in \Q $$

$u_M$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} 2-u_M^2 &= 2 - \left( u^2 + \left(\frac{2 - u^2}{2 + u}\right)^2 + 2u\frac{2 - u^2}{2 + u}\right) \\&= \frac{1}{(2+u)^2} \left( 2(2+u)^2 - ((2+u)^2u^2 + (2-u^2)^2 + 2u(2-u^2)(2+u))\right) \\&= \frac{2(2-u^2)}{(2+u)^2} \end{aligned} $$

$0 \le 2-u^2$임으로 다음이 성립한다.

$$ 0 \le 2 - u_M^2 \iff 0 \le u_M^2 \le 2 \iff u_M \in U  $$

즉, $U$의 어떤 element를 뽑더라도 $U$에 속하는 더 큰 원소를 찾아 낼 수 있음으로 다음이 성립한다.

$$ U \text{ doesn't have greatest element } \qed $$