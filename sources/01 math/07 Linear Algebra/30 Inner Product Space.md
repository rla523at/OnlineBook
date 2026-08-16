# Inner Product Space

## 한 줄 요약

Vector space에 inner product를 추가하면 vector의 length와 angle을 정의할 수 있고, 이를 바탕으로 orthogonality, projection과 distance 같은 Euclidean geometry의 개념을 일반적인 vector space에서도 다룰 수 있다.

## Motivation

Vector space의 공리는 vector addition과 scalar multiplication을 설명한다. 따라서 linear combination, basis와 subspace는 정의할 수 있지만 다음 질문에는 답하지 못한다.

- Vector의 length는 얼마인가?
- 두 vector가 이루는 angle은 얼마인가?
- 두 vector가 orthogonal한가?
- 한 vector와 subspace 사이에서 가장 가까운 point는 무엇인가?

이 질문들은 vector space 자체가 아니라 그 위에 추가로 선택한 기하 구조에 의해 결정된다. 여기서는 length, angle과 nearest point를 다루는 Euclidean-type geometry를 목표로 하고, 이러한 질문에 답하려면 어떤 규칙이 추가로 필요한지 살펴본다.

한 vector와 subspace 사이에서 가장 가까운 point를 찾으려면 먼저 두 후보 중 어느 쪽이 더 가까운지 비교할 수 있어야 한다. 그러려면 두 point $x,y\in V$가 얼마나 떨어져 있는지를 nonnegative real number로 나타내는 distance $d(x,y)$가 필요하다.

여기서 원하는 Euclidean-type geometry에서는 두 point를 같은 vector $a$만큼 평행이동해도 두 point 사이의 distance가 변하지 않아야 한다.

$$
d(x+a,y+a)=d(x,y).
$$

여기서 $a=-x$로 두면 다음을 얻는다.

$$
d(x,y)=d(0,y-x).
$$

따라서 모든 pair의 distance를 따로 정할 필요는 없다. 원점에서 displacement vector $v$까지의 distance를 그 vector의 length

$$
L(v):=d(0,v)
$$

로 정하면 모든 distance가

$$
d(x,y)=L(y-x)
$$

로 결정된다. 결국 distance를 정하는 문제는 각 displacement vector의 length를 정하는 문제로 바뀐다.

Length를 그대로 사용해도 되지만, vector addition과 scalar multiplication이 length에 미치는 영향을 식으로 전개할 때는 length의 제곱을 사용하는 편이 더 편리하다. 이 squared length를 다음 함수로 나타내자.

$$
Q:V\rightarrow\R_{\ge0},
\qquad
Q(x):=L(x)^2.
$$

Length가 vector space의 scalar multiplication과 호환되려면 $L(cx)=\lvert c\rvert L(x)$여야 한다. 따라서 squared length는 $Q(x)=0$과 $x=0_V$가 동치이고 다음 관계를 만족해야 한다.

$$
Q(cx)=\lvert c\rvert^2Q(x).
$$

이제 두 vector $x,y$를 더한 vector의 squared length $Q(x+y)$를 생각하자. $Q(x)$와 $Q(y)$만으로는 $Q(x+y)$를 결정할 수 없다. 두 vector가 서로 어떤 방향 관계에 있는지에 따라 합의 length가 달라지기 때문이다. 먼저 개별 squared length의 합을 기준으로 실제 합의 squared length가 얼마나 달라지는지를 다음 discrepancy로 나타내자. 여기서는 real vector space를 생각한다.

$$
\Delta_Q(x,y)
:=
Q(x+y)-Q(x)-Q(y).
$$

이 식은 새로운 성질을 가정한 것이 아니다. 실제 값 $Q(x+y)$에서 이미 알고 있는 두 값 $Q(x)$와 $Q(y)$를 뺀 나머지에 이름을 붙인 것이다.

익숙한 평면에서 $Q(x)=Q(y)=1$인 두 vector를 비교해 보자.

- 같은 방향으로 겹쳐서 $y=x$이면 $Q(x+y)=Q(2x)=4$이므로 $\Delta_Q(x,y)=2$이다.
- 두 vector가 직각이면 Pythagorean theorem에 의해 $Q(x+y)=2$이므로 $\Delta_Q(x,y)=0$이다.
- 반대 방향으로 겹쳐서 $y=-x$이면 $Q(x+y)=Q(0)=0$이므로 $\Delta_Q(x,y)=-2$이다.

세 경우 모두 개별 squared length는 $1$로 같지만 $\Delta_Q(x,y)$는 서로 다르다. 따라서 이 차이는 개별 squared length만으로는 알 수 없는 두 vector의 방향 관계를 담는다.

지금 필요한 것은 $Q$와 무관한 새로운 pairwise function이 아니라, $Q$가 담고 있는 squared-length 정보를 보존하면서 두 vector 사이의 관계까지 표현하는 함수다. 이 함수를 $B(x,y)$라고 하자.

$Q$는 vector 하나를 입력받아 그 squared length를 알려 주고, $B$는 vector 두 개를 입력받아 그 관계를 알려 준다. $B$가 $Q$의 two-vector 확장이라면 $B$에 같은 vector $x$를 두 번 넣었을 때 기존 squared length를 다시 얻을 수 있어야 한다. 즉 다음 조건으로 $B$가 원래의 $Q$를 그대로 보존하도록 한다.

$$
B(x,x)=Q(x).
$$

앞서 정의한 $\Delta_Q$가 이 역할을 그대로 할 수 있는지 확인해 보자. 같은 vector를 두 번 입력하면

$$
\Delta_Q(x,x)
=
Q(2x)-2Q(x)
=
4Q(x)-2Q(x)
=
2Q(x).
$$

$\Delta_Q$를 그대로 $B$로 사용하면 $B(x,x)=2Q(x)$가 되어 처음 정한 squared length $Q(x)$를 직접 복원하지 못한다. 이것이 수학적으로 틀린 것은 아니지만, 원래 geometry의 squared length를 두 배로 바꾸게 된다. 따라서 $B(x,x)=Q(x)$가 되도록 $\Delta_Q$를 절반으로 normalization한다.

$$
B(x,y)
:=
\frac12\Delta_Q(x,y)
=
\frac12\left(Q(x+y)-Q(x)-Q(y)\right),
\qquad
B(x,x)=Q(x).
$$

Factor $\frac12$은 이처럼 pairwise relation $B$가 기존 squared length $Q$를 그대로 확장하도록 맞춘 결과다. 또한 정의를 정리하면

$$
Q(x+y)
=
Q(x)+Q(y)+2B(x,y)
$$

를 얻는다.

다만 임의의 squared-length function $Q$에서 만든 $B$가 언제나 vector 연산과 잘 맞는 것은 아니다. 기하 계산에 쓰려면 다음과 같이 vector addition과 scalar multiplication을 보존하고, 두 argument의 순서를 바꾸어도 값이 같아야 한다.

$$
B(x_1+x_2,y)=B(x_1,y)+B(x_2,y),
\qquad
B(cx,y)=cB(x,y),
\qquad
B(x,y)=B(y,x).
$$

이 조건과 squared length의 positivity가 함께 성립할 때 $B$는 real vector space의 내적(inner product)이 된다. $B(x,x)$는 squared length를, $B(x,y)$는 두 vector 사이의 interaction을 나타내므로 $B$에서 length, angle, orthogonality와 projection을 차례로 정의할 수 있다. Complex vector space에서는 한 argument에 대해 linear하고 다른 argument에 대해 conjugate linear한 sesquilinearity와 conjugate symmetry를 사용해 같은 역할을 유지한다. 다음 절에서 이 조건들을 정확히 정의한다.

이 문서에서는 inner product의 정의에서 출발해 다음 관계를 차례로 설명한다.

$$
\text{inner product}
\Longrightarrow
\text{norm, distance, angle}
\Longrightarrow
\text{orthogonality and projection}.
$$

Inner product $B$ 자체는 basis와 무관하다. 그러나 vector $x,y$를 선택한 basis의 coordinate로 나타내어 $B(x,y)$를 계산하려면 basis vector 사이의 inner product를 기록한 Gram matrix가 필요하며, [Gram Matrix](<31 Gram Matrix.md>)에서 그 계산법을 설명한다. [Norm, Distance and Angle](<32 Norm Distance and Angle.md>)은 inner product에서 기본적인 geometry가 나오는 과정을 다루고, [Projection and Orthogonal Subset](<33 Projection and Orthogonal Subset.md>)은 한 방향의 component와 여러 방향을 독립적으로 계산하는 조건을 설명한다. [Gram-Schmidt Process](<34 Gram-Schmidt Process.md>)는 arbitrary basis를 같은 subspace를 span하는 orthogonal basis로 바꾸며, [Orthogonal Complement](<35 Orthogonal Complement.md>)는 전체 공간을 subspace 방향과 그에 수직인 방향으로 분해한다. [Schur's Theorem](<38 Schur's Theorem.md>)과 [Riesz Representation Theorem](<42 Riesz Representation Theorem.md>)은 이러한 구조가 linear map의 matrix representation과 linear functional의 표현을 어떻게 단순하게 만드는지 보여준다.

## Inner Product

$\F\in\{\R,\C\}$인 $n$차원 vector space $V/\F$가 있다고 하자. 이 문서에서는 첫 번째 인자에 대해 linear인 convention을 사용한다. 함수

$$
B:V\times V\rightarrow\F
$$

가 다음 조건을 만족하면 $B$를 $V$의 inner product라고 한다.

$x,y,z \in V$이고 $c \in \F$일 때,

1. $$ B(x+y, z) = B(x,z) + B(y,z) $$
2. $$ B(cx,y) = cB(x,y) $$
3. $$ B(x,y) = \overline{B(y,x)} $$
4. $$ B(x,x)\in\R_{\ge 0}, \qquad B(x,x)=0_\F \iff x=0_V $$

첫 번째와 두 번째 조건은 inner product가 vector space의 linear structure와 호환되게 한다. 세 번째 조건인 conjugate symmetry는 두 인자의 순서를 바꾸었을 때의 관계를 정하며, 네 번째 조건인 positive definiteness는 $B(x,x)$가 length의 제곱으로 사용될 수 있게 한다.

### 예시: 같은 vector space의 서로 다른 inner product

$x=(x_1,x_2)$와 $y=(y_1,y_2)$가 $\R^2$의 vector라고 하자. 다음 두 함수를 정의한다.

$$
B_1(x,y)=x_1y_1+x_2y_2,
\qquad
B_2(x,y)=4x_1y_1+x_2y_2.
$$

두 함수는 모두 각 인자의 addition과 real scalar multiplication을 보존하고 symmetric하다. 또한 다음이 성립한다.

$$
B_1(x,x)=x_1^2+x_2^2,
\qquad
B_2(x,x)=4x_1^2+x_2^2.
$$

두 값은 모두 nonnegative이고 $x=0$일 때만 $0$이므로 $B_1$과 $B_2$는 inner product의 네 조건을 만족한다. 따라서 같은 vector space $\R^2$에도 서로 다른 inner product를 줄 수 있다.

$e_1=(1,0)$에 대해서는 $B_1(e_1,e_1)=1$이고 $B_2(e_1,e_1)=4$다. 이후 정의할 norm을 사용하면 $e_1$의 length는 각각 $1$과 $2$가 된다. 즉, vector space가 같더라도 inner product의 선택에 따라 geometry가 달라진다.

### 두 번째 인자에 대한 성질

첫 번째, 두 번째 조건에 의해 $B$는 첫 번째 인자에 대해 linear하다. 세 번째 조건까지 사용하면 두 번째 인자에 대해서는 다음과 같이 conjugate linear하다.

$$ B(x,cy + z) = \bar c B(x,y) + B(x,z) $$

$\F=\R$이면 complex conjugation이 값을 바꾸지 않으므로 $B$는 두 번째 인자에 대해서도 linear하고 symmetric하다. 따라서 real inner product는 symmetric bilinear form이다.

$$ B(x,cy + z) = c B(x,y) + B(x,z) $$

### 표기

Inner product에는 다음 표기를 많이 사용한다.

$$ B(x,y) \equiv \lang x,y \rang $$

## Inner Product Space

Vector space $V/\F$에 inner product $B$가 함께 주어진 공간 $(V,B)$를 `내적 공간(inner product space)`이라고 한다. 같은 vector space에도 서로 다른 inner product를 줄 수 있으므로 $V$만 적는 대신, inner product의 선택이 중요할 때는 $(V,B)$처럼 함께 표시한다.

(gram-matrix)=

### 명제: 모든 vector와 orthogonal인 vector

$n$차원 vector space $V/\F$와 내적 $B$가 있다고 하자.

$v \in V$가 있을 때, 다음을 증명하여라.

$$
\left(\forall w\in V,\quad B(v,w)=0_\F\right)
\iff
v=0_V
$$

**Proof**

$v=0_V$이면 inner product의 linearity에 의해 모든 $w\in V$에 대해 $B(v,w)=0_\F$이다.

반대로 모든 $w\in V$에 대해 $B(v,w)=0_\F$라고 하자. $w=v$로 두면 다음이 성립한다.

$$ B(v,v) = 0_\F $$

Positive definiteness에 의해 $v=0_V$이다. $\qed$
