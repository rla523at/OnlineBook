# Sumpremum and Infimum
POSET $X$와 $X$의 subset $U$가 있다고 하자.

## Supremum
$U$가 bounded from above일 때, $U$의 upper bound의 집합을 $S$라고 하자.

이 떄, $S$의 least element를 $U$의 `least upper bound, supremum`이라고 한다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Infimum_and_supremum)

### 명제1
POSET $X$와 $X$의 bounded from above subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \sup(U) \in U \implies \sup(U) \text{ is an greatest elemt of } U $$

**Proof**

supremum과 greatest element의 정의에 의해 자명하다. $\qed$

### 명제2
POSET $X$와 $X$의 bounded from above subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ u_M \text{ is greatest element of } U \implies u_M = \sup(U) $$

**Proof**

다음을 가정하자.

$$ u_M \neq \sup(U) $$

그러면 다음이 성립한다.

$$ \exist M \in X \st \begin{gathered} M < u_M \\ \forall u \in U, \quad u \le M \end{gathered}  $$

Greatest element의 정의에 의해 $u_M \in U$임으로 다음이 성립한다.

$$ u_M \le M \land M < u_M $$

이는 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ u_M = \sup(U) \qed $$


## Infimum
$U$가 bounded from below일 때, $U$의 least bound의 집합을 $S$라고 하자.

이 떄, $S$의 maximal element를 $U$의 `greatest lower bound, infimum`이라고 한다.

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Infimum_and_supremum)

### 명제1
POSET $X$와 $X$의 bounded below subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \inf(U) \in U \implies \inf(U) \text{ is an least elemt of } U $$

**Proof**

infimum과 least element의 정의에 의해 자명하다. $\qed$

### 명제2
POSET $X$와 $X$의 bounded from below subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ u_m \text{ is least element of } U \implies u_m = \inf(U) $$

**Proof**

다음을 가정하자.

$$ u_m \neq \inf(U) $$

그러면 다음이 성립한다.

$$ \exist m \in X \st \begin{gathered} u_m < m \\ \forall u \in U, \quad m \le u \end{gathered}  $$

Greatest element의 정의에 의해 $u_M \in U$임으로 다음이 성립한다.

$$ u_m < m \land m \le u_m $$

이는 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ u_M = \inf(U) \qed $$