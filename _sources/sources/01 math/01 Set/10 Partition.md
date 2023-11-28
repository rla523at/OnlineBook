# Partition
## Motivation
somethings를 여러 category로 `분할(partition)`한다고 해보자.

이 때, category들이 어떤 성질들을 만족해야 partition이라는 직관에 부합하는지 하나씩 생각해보자.

먼저, 빈 category는 없어야 한다. 왜냐하면 그 어떤 something도 포함되어 있지 않은 빈 카테고리는 partition을 하기 위해서 존재할 필요가 없기 때문이다.

다음으로는 서로 다른 두 category는 공통의 something을 갖지 않아야 한다. 왜냐하면 여러 category에 동시에 포함되는 something이 있다는 것은 partition이라는 직관에 맞지 않기 때문이다.

그리고 모든 category 안에 있는 something을 모았을 때, 전체 somethings가 되어야 한다. 그렇지 않으면 그 어떤 category에도 포함되지 않는 something이 있다는 말인데 이 또한 partition이라는 직관에 맞지 않기 때문이다.

이제, 위의 생각들을 집합의 언어로 다시 표현해보자.

## Definition
집합 $S$와 임의의 $S$의 subset family $H$가 있다고 하자.

이 때, $H$가 다음을 만족하는 경우 $H$를 $S$의 partition이라고 한다.

$$ \begin{gathered} \empty \notin H \\ \forall H_i,H_j \in H, \enspace H_i \neq H_j \implies H_i \cap H_j = \empty \\ \bigcup H = S \end{gathered} $$

### 예시
다음과 같이 집합을 정의하자.  
$$[k] := \{ x \in \Z | x = 3a + k, \quad a,k \in \Z \}$$

정의에 의해 $\cdots [-3] = [0] = [3] \cdots, \quad \cdots [-2] = [1] = [4] \cdots, \quad \cdots [-1] = [2] = [5] \cdots$ 이다.

이 때, $[0],[1],[2]$는 각각 공집합이 아니며 교집합은 공집합이고 합집합은 $\Z$가 된다.

따라서 $F := \{ [x] | x \in \Z \} = \{ [0], [1], [2] \}$는 분할이다.