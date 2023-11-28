# Chain
## 정의
Poset $S$가 있다고 하자.

$S$의 subset $C$가 totally ordered set일 떄, $C$를 `사슬(chain)`이라 한다.

### 참고

$S$안에서는 모든 원소가 비교가능할 필요는 없지만 적어도 $C$안에서는 모든 원소가 비교가능해야 한다.

### 초른의 보조정리(Axiom)
Poset $S$의 모든 chain $C$가 upper bound를 가지면 $S$는 극대원소를 갖는다.

#### 참고1
임의의 chian을 $C$, $C$의 upper bound를 $M_C$라 하자. 

그러면 $M_C$는 $C$의 maximal element가 된다.

하지만 $M_C$가 $S$의 maximal element가 될거라는 보장은 없다. 왜냐하면 $M_C$가 $C$의 element랑만 비교가능하리라는 보장이 없기 때문이다. 

즉, $M_C$가 포함된 다른 모든 chain에서도 $M_C$가 maximal element여야 $M_C$가 $S$의 maximal element라고 할 수 있다.

#### 참고2
임의의 chian을 $C$, $C$의 upper bound를 $M_C$라 하자. 

$S$의 maximal element를 찾는 과정은 다음과 같을 것이다.

1. $M=M_C$ 로 둔다.
2. $M$를 포함하는 모든 chain을 모은다.
3. 모든 chain의 upper bound와 $M$를 비교한다.
4. 만약 $M$이 모든 chain의 upper bound이면 $M$는 $S$의 maximal element이다.
5. 만약 $M$이 아닌 upper bound를 갖는 chain이 있는 경우, $M$을 그 upper bound로 두고 다시 2번으로 돌아간다.

모든 chain이 uuper bound를 갖음으로 위와 같은 과정을 통해 maximal element를 찾을 수 있을 것이다.

하지만 chain이 무한하게 있을 수 있기 때문에 이를 증명할 수 없고 따라서 공리로 채택한다.

> Reference  
[blog 2!=2](https://chocobear.tistory.com/69)