# Completeness
## 정의
POSET $X$가 있다고 하자.

$X$가 다음을 만족할 떄, $X$를 `complete` 하다고 한다.

$$ \text{Every nonempty bounded above subset of } X \text{ has supremum} $$


> Reference  
> [wiki](https://en.wikipedia.org/wiki/Completeness_(order_theory))


### 명제1
Complete POSET $X$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Every nonempty bounded below subset of } X \text{ has infimum } $$

**Proof**

$X$의 임의의 bounded below subset을 $U$라고 하자.

이 떄, Set $Y$를 다음과 같이 정의하자.

$$ Y := \Set{x \in X | x \text{ is a lower bound of } U} $$

$U$가 bounded below임으로 다음이 성립한다.

$$ Y \text{ is nonempty} $$

$Y$의 정의에 의해 $Y$는 bounded above set이고, 따라서 $X$의 completeness에 의해 다음이 성립한다.

$$ \exist \sup(Y) $$

$\forall u \in U$는 $Y$의 upper bound임으로 supremum의 정의에 의해 다음이 성립한다.

$$ \begin{aligned} & \forall u \in U, \quad \sup(Y) \le u \\ \implies& \sup(Y)\text{ is lower bound of } U \\ \implies& \sup(Y) \in Y \\\implies& \sup(Y) \text{ is an greatest element of } Y \\\implies& \sup(Y) = \inf(X) \end{aligned} $$

따라서, 다음이 성립한다.

$$ \text{Every nonempty bounded below subset of } X \text{ has infimum } $$

> Reference  
> {cite}`abbott` Exercise 1.3.3

#### 참고
따라서, Supremum 대신 infimum을 갖는다고 정의해도 동일하다.
