# Cyclic Group
## Definition
Group $G$와 $G$의 임의의 singletone set $\Set{x}$가 있다고 하자.

이 때, singletone set $\Set{x}$에 의해 생성되는 $\span(\Set{x})$를 `cyclic group`이라고 한다.

### Example
* $(\Z,+) = \braket{1} = \braket{-1}$
* $(\Z/n\Z,+) = \braket{[1]_n}$

### 명제1(Classification)
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

이 때, 어떤 $n \in \N$에 대해서 다음을 증명하여라.

$$ \braket{x} \cong (\Z,+) \lor \braket{x} \cong (\Z/n\Z, +) $$

**Proof**

$\braket{x}$는 다음 두가지 경우로 나눌 수 있다.

1. $\forall i,j \in \Z, \enspace i \neq j \implies x^i \neq x^j$
2. $\exist i,j \in \Z \st i \neq j \land x^i = x^j$

[case1]  
함수 $f$를 다음과 같이 정의하자.

$$f : \braket{x} \rightarrow \Z \st x^i \mapsto i $$

전제의 대우명제에 의해 $x^i=x^j \implies i=j \iff f(x^i) = f(x^j)$임으로 $f$는 well-defined이다.

[$f$ is group isomorphism]  
-[$f$ is a group homorphism]  
$\braket{x}$의 임의의 elements를 $x^i,x^j$라고 하면 다음이 성립한다.

$$ f(x^i*x^j) = f(x^{i+j}) = i+j = f(x^i) + f(x^j) \qed $$

-[$f$ is an injective]  
$\braket{x}$의 임의의 elements를 $x^i,x^j$라고 하면 다음이 성립한다.

$$ f(x^i) = f(x^j) \implies i=j \implies x^i = x^j \qed $$

-[$f$ is a surjective]  
$\Z$의 임의의 element를 $i$라고 하면 다음이 성립한다.

$$ \exist x^i \in \braket{x} \st f(x^i) = i \qed $$

[case2]  
일반성을 잃지 않고 $j < i$라고 하자.

그러면 $i-j \in \N$이고 $x^{i-j} = e_G$이다.

이 때, set $S$를 다음과 같이 정의하자.

$$ S := \Set{n \in \N | x^n = e_G} $$

그러면, $S$는 $\N$의 non empty subset이다.

따라서, well ordering principle에 의해서 다음이 성립한다.

$$ \exist n_0 = \min(S) $$

함수 $f$를 다음과 같이 정의하자.

$$ f : \braket{x} \rightarrow \Z/n_0 \Z \st x^i \mapsto [i]_{n_0} $$

[$f$ is well defined]  
$\braket{x}$의 임의의 elements를 $x^i,x^j$라고 하면 다음이 성립한다.

$$ x^i = x^j \implies i = kn_0 + j \implies [i]_{n_0} = [kn_0 + j]_{n_0} = [j]_{n_0} \implies f(x^i) = f(x^j) \qed $$

[$f$ is a group isomorphism]  
-[$f$ is a group homorphism]  
$\braket{x}$의 임의의 elements를 $x^i,x^j$라고 하면 다음이 성립한다.

$$ f(x^i*x^j) = f(x^{i+j}) = [i+j]_{n_0} = [i]_{n_0} + [j]_{n_0} = f(x^i) + f(x^j) \qed $$

-[$f$ is an injective]  
$\braket{x}$의 임의의 elements를 $x^i,x^j$라고 하면 다음이 성립한다.

$$ f(x^i)=f(x^j) \implies [i]_{n_0} = [j]_{n_0} \implies i = kn_0 + j \implies x^i = x^{kn_0 +j} = x^j \qed $$

-[$f$ is surjective]  
$\Z/n_0\Z$의 임의의 element를 $[i]_{n_0}$라고 하면 다음이 성립한다.

$$ \exist x^i \in \braket{x} \st f(x^i) = [i]_{n_0} \qed $$

#### 참고1
$\braket{x}$를 나누는 조건을 살펴보자.

$$ \begin{gathered} \forall i,j \in \Z, \enspace i \neq j \implies x^i \neq x^j \\ \exist i,j \in \Z \st \enspace i \neq j \land x^i = x^j \end{gathered} $$

먼저 1번 조건의 부정이 $\exist i,j \in \Z \st \enspace i \neq j \land x^i = x^j$게 적힌다는 점이 흥미롭다.

그리고 1번 조건을 보면 $i=j$인 경우에 대해서 어떤것도 보장해주지 않는다. 따라서 1번 조건만 보면 $i=j$일 때, $x^i = x^j$일 수도 있고 $x^i \neq x^j$일 수도 있다. 하지만 $*$이 함수이기 때문에 $i=j \implies x^i=x^j$가 보장된다.

만약에 $\braket{x}$를 다음 조건으로 나눠보자.

$$ \begin{gathered} \forall i,j \in \Z, \enspace x^i \neq x^j \implies i \neq j \\ \exist i,j \in \Z \st \enspace x^i \neq x^j \land i = j \end{gathered} $$

1번 조건은 $x^i=x^j$인 경우에 대해서 어떤것도 보장해주지 않는다. 즉, $x^i=x^j$일 떄, $i=j$일수도 있고 $i \neq j$일 수도 있다.

2번 조건은 연산이 함수의 성질을 만족하지 못한다는 말이된다.

결국 이상하다.

#### 참고2
$\exist i,j \in \Z \st \enspace i \neq j \land x^i = x^j$인 경우를 생각해보자.

일반성을 잃지 않고 $i<j \implies i - j \in \N$이라고 두면 $S:=\Set{n\in\N | x^n = e_G}$는 공집합이 아닌 $\N$의 부분집합이다.

따라서, well-ordering principle에 의해 $\min(S) = n_0$라고 하면 $S = \Set{kn_0 | k\in\N}$형태이다.

왜냐하면 $S$의 임의의 element를 $x^m$이라고 하자.

division algorithm에 의해 $m = qn_0 + r$이고 $x^r = x^{m-qn_0} = e_G$이다.

이 떄, 만약 $r \neq 0$이라면 $r \in S$이고 이는 $\min(S) = n_0$라는 사실에 모순이다.

따라서, proof by contradiction에 의해 $r=0$이고 $S = \Set{kn_0 | k\in\N}$ 형태일 수 밖에 없다.

#### 참고3
$\forall i,j \in \Z, \enspace i \neq j \implies x^i \neq x^j$인 경우에 $f : \Z \rightarrow \braket{x}$로 두고 well-defined임을 보여보자.

$$ i= j \implies f(i) = x^i = x^j = f(j) \qed $$

이 때, $x^i = x^j$인것은 주어진 연산 $*$이 well-defined 함수임으로 동일한 연산을 취한 값은 항상 같은 값이여야 하기 때문이다.

$\exist i,j \in \Z \st \enspace i \neq j \land x^i = x^j$인 경우에 $f : \Z/n\Z \rightarrow \braket{x}$로 두고 well-defined임을 보여보자.

$$ [i] = [j] \implies i = kn_0 +j \implies f([i]) = x^i = x^{kn_0 + j} = x^j = f([j]) \qed $$

### 명제2
Group $G$와 $G$의 임의의 cyclic group $\braket{x}$가 있다고 하자.

$\braket{x}$의 subgroup $H$가 있을 때, 다음을 증명하여라.

$$ \exist a \in \Z \st  H = \braket{x^a} $$

**Proof**

Set $S$를 다음과 같이 정의하자.

$$ S := \Set{n \in \N | x^n \in H} $$

그러면 $S$는 $\N$의 non empty subset임으로 well ordering principle에 의해 다음이 성립한다.

$$ \exist n_0 = \min(S) $$

이 때, $H = \braket{x^{n_0}}$임을 보이자.

[$\braket{x^{n_0}} \subseteq H$]  
$n_0 \in S$이고 $H$가 group 임으로 다음이 성립한다.

$$ x^{n_0} \in H \implies \braket{x^{n_0}} \subseteq H \qed $$

[$H \subseteq \braket{x^{n_0}}$]  
$H$의 임의의 element를 $x^m$이라고 하자.

division algorihtm에 의해 다음이 성립한다.

$$ m = qn_0 + r $$

이 떄, $\braket{x^{n_0}} \subseteq H$임으로 다음이 성립한다.

$$ x^r = x^{m-qn_0} \implies  x^r \in H $$

이 떄, 다음을 가정하자.

$$ r \neq 0 $$ 

그러면, $r \in S$이고 $1\le r \le n_0-1$이다.

하지만 이는 $n_0 = \min(S)$라는 사실에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ r= 0 $$

그러면 다음이 성립한다.

$$ x^m = x^{n_0q} \in \braket{x^{n_0}} \qed $$

#### 참고
$\braket{x} = \Set{e,x,x^2,\cdots}$이기 때문에 $\braket{x}$의 subgroup은 반드시 $x$로만 이루어져 있어야 한다는걸 알 수 있다.

## Order
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

이 때, set $S$를 다음과 같이 정의하자.

$$ S := \Set{n \in \N | x^n = e_G} $$

이 때, $x$의 order $o(x)$는  다음과 같이 정의된 자연수이다.

$$ o(x) = \min(S) $$

### 참고
1. $x^n = e$를 만족하는 $n$이 없는 경우 $o(x) = \infty$이다.
2. $o(x)=n < \infty$인 경우 $\braket{x} \cong \Z/n\Z$이다.
3. $o(x)= \infty$인 경우 $\braket{x} \cong \Z$이다.

### Observation
$\braket{x} = \Set{e,x,x^2,x^3}$이라고 해보자.

$\braket{x}$의 subgroup들은 $\braket{x} = \braket{x^3}, \braket{x^2},\braket{x^4}=\braket{e_G}$이 있다.

이 때, $\braket{x^3} = \braket{x}$를 만족하는데, subgroup이 $\braket{x}$가 된다는 말은 그 subgroup의 generator가 $\braket{x}$의 generator이기도 하다는 말이다.

그러면 subgroup중에 $\braket{x}$가 되는 subgroup은 몇 개일까? 즉, 다시 말해 $\braket{x}$의 generator는 몇개일까?

#### Abstraction
finite cyclic group $\braket{x}=\Set{e, \cdots, x^{n-1}}$와 $\braket{x}$의 subgroup $\braket{x^a}$가 있다고 하자.

이 때, $\braket{x}=\braket{x^a}$면 $|\braket{x}|=|\braket{x^a}|$이다.

따라서 $\braket{x^a} = \Set{e, x^a,\cdots, x^{a(n-1)}}$임으로 $x^{ka} = e, \enspace k \in \N$를 만족하는 가장 작은 $k$는 $n$이다.

즉, $\lcm(n,a) = na$라는 얘기이고 $\gcd(n,a) = 1$이라는 얘기이다.

따라서, $n$과 relative prime인 $a$에 대해서 $\braket{x^a} = \braket{x}$이고 $x^a$는 $\braket{x}$의 generator가 된다.

#### Abstraction2
그러면 $\gcd(n,a) \neq 1$인 $\braket{x}$의 subset $\braket{x^a}$는 어떻게 되는지 생각해보자.

$\gcd(n,a) = k \neq 1$이라고 하면 $\exist q \in \N \st n = kq$이다. 

따라서, $\lcm(n,a) = \frac{n}{\gcd(n,a)} = q$임으로 $\braket{x^a} = \Set{e,x^a,\cdots,x^{(q-1)a}}$가 된다.

즉, $o(x^a) = q = \frac{n}{\gcd(n,a)}$이 된다.


### 명제1
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ o(x) = n \implies |\braket{x}| = n  $$

**Proof**

$$ x^n = e_G \implies \braket{x} = \Set{e,x,x^2,\cdots,x^{n-1}} \implies |\braket{x}| = n \qed $$ 


### 명제2
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

$o(x) = n < \infty$이고 $\Z$의 임의의 element를 $a$라고 할 때, 다음을 증명하여라.

$$ o(x^a) = \frac{n}{\gcd(n,a)} $$

**Proof1**

$o(x) = n$임으로, $o(x^a) = \lcm(a,n) / |a|$가 될 것이다.

이 때, $\lcm$의 성질에 의해 다음이 성립한다.

$$ \frac{\lcm(a,n)}{|a|} = \frac{n}{\gcd(a,n)} \qed $$

**Proof2**

집합 $S$를 다음과 같이 정의하자.

$$ S := \Set{m \in \N | (x^a)^m = e_G} $$

$S$의 임의의 element $m$에 대해서 $o(x) = n$이기 때문에 division algorithm에 의해 어떤 $q \in \Z$에 대해서,  $am = qn$이다.

이 때, $a' = a / \gcd(n,a), n' = n / \gcd(n,a)$이라고 하면 $a'$과 $n'$은 relative prime임으로 다음이 성립한다.

$$ am = qn \implies a'm = qn' \implies m = q'n' $$

이 때, $m,n \in \N$임으로 $q' \in \N$이고 따라서 다음이 성립한다.

$$ n' \le m \iff \frac{n}{\gcd(n,a)} \le m $$

그리고 $a' \in \Z$임으로 다음이 성립한다.

$$  x^{a \frac{n}{\gcd(n,a)}} = x^{n \frac{a}{\gcd(n,a)}} = e^{a'} = e \implies \frac{n}{\gcd(n,a)} \in S  $$

따라서, 다음이 성립한다.

$$ o(x^a) = \frac{n}{\gcd(n,a)} \qed $$

#### 따름명제1
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ o(x) = n \text{ is a prime number} \implies \braket{x} \text{ has no proper subgroup except } \Set{e_G} $$

**Proof**

$\braket{x}$의 subgroup을 $H$라고 하면, 다음이 성립한다.

$$ \exist a \in \Z \st H = \braket{x^a} $$

이 때, 명제2에 의해 다음이 성립한다.

$$ |H| = o(x^a) = \frac{n}{\gcd(n,a)} = 1 \lor n $$

$|H| = n$인 경우 $\braket{x^a}$는 proper subgroup이 아니며 $|H| = 1$인 경우 $H = \Set{e_G}$이다. $\qed$

#### 따름명제2
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

$o(x) = n < \infty$이고 $\Z$의 임의의 element를 $a$라고 할 때, 다음을 증명하여라.

$$ n,a \text{ are relative prime} \implies \braket{x} = \braket{x^a} $$

**Proof**

전제에 의해 다음이 성립한다.

$$ o(x^a) = |\braket{x^a}| = n $$

이 떄, $\braket{x^a}$는 $\braket{x}$의 subgroup이고 $|\braket{x^a}| = |\braket{x}|$임으로 다음이 성립한다.

$$ \braket{x} = \braket{x^a} \qed $$

##### 참고1
$\braket{x} = \braket{x^a}$라는 말은 $x^a$ 또한 $\braket{x}$의 generator라는 말이다.

따라서, $[0,n)$ 구간에서 relative prime의 수가 $\braket{x}$의 generator의 수가 된다.

그리고 Euler's phi 함수 $\varphi$가 다음과 같이 정의된다.

$$ \varphi : \Z \rightarrow \Z \st n \mapsto |\Set{k \in [0,n) | \gcd(n,k) = 1}| $$

즉, Euler's phi의 값이 $\braket{x}$의 generator 수이다.

그럼으로, $\Z/n\Z$의 generator의 수는 $\phi(n)$이다. 참고로 $\Z/\Z = \Set{\Z}$로 element가 $\Z$하나인 group으로 $\Z$가 identity element 역할을 하는 generator가 1개인 cyclic group이다.

만약 $\braket{x}$가 finite cyclic group이 아니라면 generator의 수는 어떻게 알 수 있을까?

##### 참고2
relative prime의 수가 generator의 수가 되어야 하는 이유를 살펴보자.

Cyclic group $\braket{x}$가 있고 $o(x) = n < \infty$이라고 하자.

그리고 $\braket{x}$의 임의의 subgroup을 $\braket{x^a}$라고 했을 때, 

$\gcd(n,a)= \frac{na}{\lcm(n,a)}$임으로 $n,a$가 relative prime이라는 말은 $\lcm(n,a)=na$라는 말과 같다.

##### 예제1
$G = \Z / 6\Z$로 두고 $\braket{\bar{2}}$를 생각해보자.

$\braket{2\bar{2}} = \braket{\bar{4}}$이고 $o(\bar{2}) = 3$임으로 $n=3$ $a=2$인 경우이다.

이 때, $\braket{\bar{2}} = \braket{\bar{4}}$이다.

##### 예제2
$G = \Z / 5\Z$로 두고 $\braket{\bar{1}}$를 생각해보자.

$\braket{3\bar{1}} = \braket{\bar{3}}$이고 $o(\bar{1}) = 5$임으로 $n=5$ $a=3$인 경우이다.

이 때, $\braket{\bar{1}} = \braket{\bar{3}}$이다.

### 명제3
Group $G$와 $G$의 cyclic group $\braket{x}$가 있다고 하자.

$o(x) = n < \infty$ 일 때, 다음을 증명하여라.

$$ \forall d |n, \enspace \exist! H \le \braket{x} \st |H| = d $$ 

**Proof**

[existence]  
$d|n$임으로 $n/d \in \Z$이다.

$H = \braket{x^{n/d}}$라고 두면 다음이 성립한다.

$$ \begin{gathered} H \le \braket{x} \\ |H| = |x^{n/d}| = o(x^{n/d}) = \frac{n}{\gcd(n,n/d)} = d \end{gathered} \qed  $$

[uniquness]  
$H'$가 $H' \le \braket{x}$이고 $|H'| = d$라고 하자.

$H' \le \braket{x}$임으로 다음이 성립한다.

$$ \exist m \in \Z \st H' = \braket{x^m} $$

그리고 $|H'| = d$임으로 다음이 성립한다.

$$ |H'| = o(x^m) = \frac{n}{\gcd(n,m)} = d \implies \frac{n}{d} = \gcd(n,m) $$

-[$\braket{x^m} \subseteq \braket{x^{n/d}}$]  
$\gcd(n,m) = n/d$임으로 다음이 성립한다.

$$ \exist m' \in \Z \st m = m'\gcd(n,m) = m'\frac{n}{d} $$

따라서, 다음이 성립한다.

$$ x^m = x^{m' \frac{n}{d}} \in \braket{x^{n/d}} \qed $$

-[$\braket{x^{n/d}} \subseteq \braket{x^n}$]  
$\gcd(n,m) = n/d$임으로 베주의 항등식에 의해 다음이 성립한다.

$$ \exist s,t \in \Z \st \frac{n}{d} = ns + mt $$

따라서, 다음이 성립한다.

$$ x^{n/d} = x^{ns + mt} = x^{mt} \in \braket{x^m} \qed $$

-[결론]  
따라서, $\braket{x^m} = \braket{x^{n/d}}$임으로 다음이 성립한다.

$$ H' = \braket{x^m} = \braket{x^{n/d}} = H \qed $$

### 명제4
Group $G$와 $G$의 두 cyclic group $\braket{x},\braket{y}$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \braket{x}\times\braket{y} \text{ is a cyclic group} \iff \gcd(o(x),o(y)) = 1 $$

**Proof**

$|\braket{x}\times\braket{y}| = o(x)o(y)$임으로, $\braket{x}\times\braket{y}$이 cyclic group이라는 말은 다음과 동치이다.

$$ \begin{aligned} \iff& \lcm(o(x),o(y)) = o(x)o(y) \\\iff& \gcd(o(x),o(y)) = 1 \qed \end{aligned} $$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/5969/product-of-two-cyclic-groups-is-cyclic-iff-their-orders-are-co-prime)  
