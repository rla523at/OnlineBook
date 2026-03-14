# Parametric Curve
## 정의
$\R$의 open set $U$가 있다고 하자.

Vector valued function $r : U \rightarrow\R^n$이 있을 때, 다음과 같이 정의된 집합 $C$를 `parametric curve`라고 한다.

$$ C:= \img(r) $$

### Remark
5. $C$가 유한하게 꼬여있으면 정의역과 공역을 적절하게 restriction 시켜서 bijective function들을 만들 수 있다.
   1. $r|_{U_1\times r(U_1)}$과 $r|_{U_2\times r(U_2)}$의 역함수는 연속인가?
   
   
> Reference  
> [컴팩트 거리공간에서 연속인 전단사 함수의 역함수는 연속이다](https://freshrimpsushi.github.io/posts/proof-of-that-inverse-function-of-continuous-bijection-in-compact-metric-space-is-continuous/)  

  

## Parametric Equations & Parameter
$r$은 다음과 같다.

$$ r = (r^1, \cdots, r^n) $$

$r$의 componenet functions는 다음과 같다.

$$ r^i : U \rightarrow \R, \enspace i=1,\cdots,n $$

이 떄, $r^1,\cdots,r^n$를 `parametric equations` of $C$, $x \in U$를 `parameter`라고 한다.