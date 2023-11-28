# Function

## 정의
집합 $A,B$가 있다고 하자.

다음과 같이 정의된 관계 $f$를 `함수(function)`라고 한다.

$$ f := \Set{(a,b) \in A \times B | a \in A \implies \exist ! b \in B} $$

이를 풀어서 설명하면 "$A$의 모든 원소에 대해 $B$에 속하는 원소가 유일하게 존재하고 그 순서쌍들을 원소로 갖는 $A \times B$의 부분집합"이다.

### 참고1
$f$는 관계이며, $(x,y) \in f$에 대해 $x \sim_f y$라고 표현할 수 있다.

### 참고2
$f : A \rightarrow B$로 표현할 수 있다.

### 참고3
$(a,b) \in f$에서 $b$는 $a$에 의존하여 $f$에 의해 유일하게 결정된다.

따라서 $b$를 $f(a)$로 표기하기도 한다.


### 명제1

함수 $f$에 대해 다음을 증명하여라.

$$ f(\emptyset) = \empty $$

**proof**


$$f(\emptyset) = \Set{f(x) | x \in \empty} = \empty \qed$$




## 정의역과 공역
집합 $A,B$와 함수 $f : A \rightarrow B$가 있다고 하자.

이 때, 집합 $A$를 `정의역(domain)`이라고 하고 집합 $B$를 `공역(codomain)`이라고 한다.


## Restirction
집합 $A,B$와 함수 $f : A \rightarrow B$가 있다고 하자.

$U \subseteq A$가 있을 때, 함수 $f$의 $U$로의 `정의역 제한(domain restiriction)` $f|_U$는 다음과 같이 정의된 관계이다.

$$ f|_U := \Set{(x,y) \in f | x \in U} $$

$V \subseteq B$가 있을 때, 함수 $f$의 $S$와 $V$로의 `정의역과 공역 제한(domain and codomain restriction)` $f|_{U \times V}$는 다음과 같이 정의된 관계다.

$$ f|_{U \times V} = \Set{(x,y) \in f | x \in U  \enspace\land\enspace y \in V} $$



## 공집합 함수

집합 $A,B$와 함수 $f : A \rightarrow B$가 있을 때, $A = \empty$인 경우를 생각해보자.

$A = \empty$면 $A \times B = \empty$임으로 $A \times B$의 유일한 부분집합은 $\empty$임으로 $f$는 $\empty$일 수 밖에 없다. 

이 때, $A = \empty$ 임으로 $\forall a \in A$가 거짓이고, 가정이 거짓인 명제(공허하게 참인 명제)는 항상 참임으로 $f = \empty$는 함수의 정의를 만족한다.

아무런 대응 관계를 갖지 않지만 함수의 공리를 만족하는 함수를 `공집함 함수(empty function)`라고 한다.

> Reference  
> [Wiki - 공허하게 참인 명제](https://ko.wikipedia.org/wiki/%EA%B3%B5%EC%A7%91%ED%95%A9)  
> [블로그 - 가정이 거짓이면 항상 명제가 참인 이유(집합론)](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=infinity7076&logNo=221328533139)  
> [블로그 - 가정이 거짓이면 항상 명제가 참인 이유(논리학)](https://hoohaha.tistory.com/71)

## 함수의 합성
집합 $A,B,C$와 함수 $f : A \rightarrow B, g : B \rightarrow C$가 있을 때, 다음과 같이 정의된 집합 $g \circ f$를 함수의 `합성(composition)`이라고 한다.

$$ {g \circ f} := \Set{(x,z) \in A \times C | x \in A, \quad z = g(f(x))} \subseteq A \times C $$

즉, $(g \circ f)(x) = g(f(x))$이다.