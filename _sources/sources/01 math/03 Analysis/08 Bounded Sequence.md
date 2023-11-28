# Bounded Sequence
## 정의
$\R$위의 sequence $s$가 있다고 하자.

$s$가 $x \in \R$이 bounded 되어 있다는 말은 다음과 같다.

$$ \exist M \in \R \st  \forall n \in \N , \quad  |s(n)| \le M $$

### 참고
$s(n)=n$이 있다고 해보자.

기존의 정의대로라면 어떤 $M$을 결정하더라도 archimedean property에 의해 더 큰 $n$을 찾을 수 있기 때문에 $s$는 bounded sequence가 아니다.

하지만 bounded sequence의 정의가 다음과 같다고 해보자.

$$ \forall n \in \N, \quad \exist M \in \R \st |s(n)| \le M $$

그러면 $s(n)=n$일 떄, $M = s(n)$두면 정의에 의해 $s(n)$은 bounded sequece가 된다.

새로운 정의는 $s(n)$이 결정되고 난 다음에 $M$을 결정하면 되기 때문에 기존정의와 다르게 bounded sequence를 만들어 낼 수 있다.

두 정의는 선언의 순서만 다를 뿐이지만 그 의미에서는 큰 차이가 난다.

### 명제
$\R$위의 sequence $s$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ s\text{ is converge } \implies s \text{ is bounded} $$

**Proof**

$s$가 $x$에 converge한다고 하면 다음이 성립한다.

$$ \exist N_1 \in \N \st N_1 \le n \implies |s(n) - x| < 1 $$

따라서, 다음이 성립한다.

$$ N_1 \le n \implies -1 + x \le s(n) \le 1 + x $$

이 떄, $M$을 다음과 같이 정의하자.

$$ M = \max\Set{1+x, s(1), \cdots, s(N_1-1)} $$

그러면 다음이 성립한다.

$$ \forall n \in \N , \quad  |s(n)| \le M $$

그럼으로 bounded의 정의에 의해 다음이 성립한다.

$$ s \text{ is bounded} \qed $$
