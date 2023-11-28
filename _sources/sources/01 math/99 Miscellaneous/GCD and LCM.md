# GCD and LCM
## Greatest Common Divisor
$0$이 아닌 두 정수를 $a,b$라고 하자.

$$\gcd(a,b) := \max(\Set{d \in \N | a = dq_1, b = dq_2, \text{ for some } q_1,q_2 \in \Z})  $$

그리고 임의의 정수 $c$에 대해서 다음이 성립한다.

$$ \gcd(c,0) = |c| $$

### 명제1
임의의 정수를 $a,b,k$라고 할 때, 다음을 증명하여라.

$$ \gcd(a,b) = \gcd(a + kb, b) $$

**Proof**

$a,b$의 common divisior를 $x$라고 하자.

그러면 $a+kb$또한 $x$의 multiple임으로 $x$는 $a+kb,b$의 common divisor이다.

이번에는 $a+kb,b$의 common divisor를 $y$라고 하자.

그러면 $a = a+kb - kb$ 또한 $y$의 multiple임으로 $y$는 $a,b$의 common divisor 이다.

따라서, $a,b$의 common divisor의 집합과 $a+kb,b$의 common divisor의 집합이 같음으로 $\gcd$ 또한 같다. $\qed$

> Reference  
> [namu.wiki](https://namu.wiki/w/%EC%B5%9C%EB%8C%80%EA%B3%B5%EC%95%BD%EC%88%98)

##  Least Common Multiple
$0$이 아닌 두 정수를 $a,b$라고 하자.

$$\lcm(a,b) := \min(\Set{d \in \N | d = q_1a = q_2b, \text{ for some } q_1,q_2 \in \Z})  $$


### 명제1
$0$이 아닌 두 정수를 $a,b$라고 할 떄, 다음을 증명하여라.


$$ \gcd(a,b)\lcm(a,b) = |a||b| $$

**Proof**

$\gcd(a,b) = D, \enspace \lcm(a,b) = M$이라고 하면 다음이 성립한다.

$$ \begin{aligned} & a = Dq_1, \enspace b = Dq_2 \text{ for some } q_1,q_2 \in \Z \\\implies& M = |q_1||q_2|D \\\implies& MD = |q_1||q_2|D^2 = |a||b| \qed \end{aligned} $$

> Reference  
> [namu.wiki](https://namu.wiki/w/%EC%B5%9C%EC%86%8C%EA%B3%B5%EB%B0%B0%EC%88%98)