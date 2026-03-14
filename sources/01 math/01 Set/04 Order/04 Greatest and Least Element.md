# Greatest and Least Element
Poset $X$와 $X$의 subset $U$가 있다고 하자.

## Greatest Element
$u_G \in U$가 다음을 만족할 때, $u_G$을 $U$의 greatest element라고 한다.

$$ \forall u \in U, \quad u \le u_G $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Greatest_element_and_least_element)

### 명제
Poset $X$와 $X$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Greatest element of } U \text{ is unique.} $$

**Proof**

$u_{G_1},u_{G_2} \in U$가 greateset element라고 하자.

그러면 정의에 의해 다음이 성립한다.

$$ \begin{gathered} u_{G_1} \le u_{G_2} \\ u_{G_2} \le u_{G_1} \end{gathered} $$

따라서, partial order의 성질에 의해 다음이 성립한다.

$$ u_{G_1} = u_{G_2} \qed $$

## Least Element
$u_L \in U$가 다음을 만족할 때, $u_L$을 $U$의 least element라고 한다.

$$ \forall u \in U, \quad u_L \le u $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Greatest_element_and_least_element)

### 명제
Poset $X$와 $X$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ \text{Least element of } U \text{ is unique.} $$

**Proof**

$u_{L_1},u_{L_2} \in U$가 least element라고 하자.

그러면 정의에 의해 다음이 성립한다.

$$ \begin{gathered} u_{L_1} \le u_{L_2} \\ u_{L_2} \le u_{L_1} \end{gathered} $$

따라서, partial order의 성질에 의해 다음이 성립한다.

$$ u_{L_1} = u_{L_2} \qed $$

## 참고1
$U$의 maximal and minimal element는 비교 가능한 원소들 중에서 가장 크거나 작은 element이다.

하지만 $U$의 greatest and least element는 $U$의 모든 원소들과 비교가 가능해야 되며, 그 중에 가장 크거나 작은 element이다.

즉, $U$의 모든 element와 비교가 가능해야 된다는 조건이 필요하다.

## 참고2
Greatest and minimal element의 존재성은 보장되지 않는다.

1. $\N$은 greatest element를 갖지 않는다.
