# Definite Integral
## 정의
$[a,b] \in \R$에서 정의된 함수 $f$가 있다고 하자.

$[a,b]$를 균일한 $n$개의 subinterval $s_i \enspace i=1,\cdots,n$로 나누자.

$$ s_i := \Set{ x\in\R | a \le x \le a+(i-1)\Delta x} $$

$$ \Delta x := \frac{b-a}{n} $$

각각의 $s_i$에서 임의로 뽑은 sample point를 $x^*_i$라고 할 때, a에서 b까지 $f$의 definite integral은 다음과 같이 정의한다.

$$ \int^b_a f(x) \thinspace dx := \lim_{n\rightarrow\infty}\sum_{i=1}^n f(x^*_i) \Delta x $$

### 참고

선택가능한 모든 sample point의 조합의 collection을 $X^*$라고 하자.

$$ X^* := \Set{ x^* \in \R^n | x^*_i \in s_i} $$

다음을 만족할 때, $f$가 $[a,b]$에서 integrable하다고 한다.

$$ \exist! c \in \R \st \forall x^*\in X^*, \quad \lim_{n\rightarrow\infty}\sum_{i=1}^n f(x^*_i) \Delta x = c $$

이를 간단하게 다음과 같이 표현한다.

$$ \int^b_a f(x) \thinspace dx = c $$

