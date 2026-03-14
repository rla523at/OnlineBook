# Upper Bound and Lower Bound
POSET $X$와 $X$의 부분집합 $U$가 있다고 하자.

## Upper Bound
다음을 만족하는 $M \in X$를 $U$의 `upper bound`라고 한다.

$$ M \in X \st \forall u \in U, \quad u \le M $$

## Lower Bound
다음을 만족하는 $m \in X$를 $U$의 `lower bound`라고 한다.

$$ m \in X \st  \forall u \in U, \quad m \le u $$

## 참고1
$U$의 greatest, least element는 $U$의 element이지만 $U$의 upper, lower bound는 $X$의 element이다.

즉, upper, lower bound는 $U$의 element가 아닐 수 있다.

## 참고2

$U = \empty$인 경우, $\forall u \in U$가 항상 거짓임으로 공허하게 참인 명제가 된다.

따라서, 다음이 성립한다.

$$ \forall x \in X, \quad x \text{ is an upper and lower bound of } U $$
