# Relation
## 정의
집합 $A,B$가 있다고 하자.

`이항관계(binary relation)` $R$은 $A \times B$의 subset이다.

이 때, $(a,b) \in R$이면 $a \sim_R b$라고 표현한다.

### 예시1

두 집합 $A = \Set{1,2}, B = \Set{4,5}$ 이 주어졌을 때

곱집합 $A \times B = \Set{\Set{1,4} , \Set{1,5}, \Set{2,4}, \Set{2,5}}$의 부분집합 $R = \Set{\Set{1,4} , \Set{1,5}}$을 선택하면

$1 \sim_R 4, 1 \sim_R 5$라고 표현할 수 있다.

### 예시2

집합 $S$와 관계 $R \subseteq S \times S$를 다음과 같이 정의하자.

$$ \begin{gathered} S:= \Set{x | x \text{ is a student in SNU}} \\ R:= \Set{(x,y) \in S \times S | x \text{ thinks } y \text{ is a friend}} \end{gathered} $$

이때, $a,b,c \in S$에 대해 다음과 같은 재미있는 질문들을 할 수 있다.

1.  자기 자신은 자기를 친구로 생각할 것인가? $a \sim_R a$ ?
2.  $a$가 $b$를 친구로 생각하면 $b$는 $a$를 친구로 생각하는가? $a \sim_R b \implies b \sim_R a$ ?
3.  $a$가 $b$를 친구로 생각하고 $b$가 $c$를 친구로 생각하면 $a$는 $c$를 친구로 생각하는가? $a \sim_R b \land b \sim_R c \implies a \sim_R c$ ?

