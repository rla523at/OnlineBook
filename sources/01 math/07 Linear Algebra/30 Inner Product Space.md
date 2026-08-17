# Inner Product Space

## 한 줄 요약

Vector space에 inner product를 추가하면 vector의 length와 angle을 정의할 수 있고, 이를 바탕으로 orthogonality, projection과 distance 같은 Euclidean geometry의 개념을 일반적인 vector space에서도 다룰 수 있다.

## Motivation

$\F\in\{\R,\C\}$인 vector space $V/\F$를 생각하자. Vector space의 공리는 vector addition과 scalar multiplication을 설명한다. 따라서 vector를 어떻게 linear combination할지는 알 수 있지만, vector의 크기나 두 vector 사이의 관계를 나타내는 scalar는 정해져 있지 않다. 여기서는 vector space에 Euclidean-type geometry를 추가하려면 어떤 규칙이 필요한지 생각해 본다.

먼저 우리가 원하는 length를 $L$이라고 하자. Length는 적어도 zero vector를 다른 vector와 구별하고 scalar multiplication과 다음과 같이 호환되어야 한다.

$$
L(x)\ge0,
\qquad
L(x)=0\iff x=0_V,
\qquad
L(cx)=\lvert c\rvert L(x).
$$

그러나 각 vector의 length만 정해서는 length와 vector addition 사이의 관계가 드러나지 않는다. 예를 들어 $x\ne0_V$일 때 $x$와 $-x$는 같은 length를 갖지만 다음 두 합의 length는 서로 다르다.

$$
L(x+x)=2L(x),
\qquad
L(x+(-x))=0.
$$

따라서 $L(x+y)$가 $L(x)$와 $L(y)$에 어떻게 연결되는지 설명하려면 두 vector가 합 안에서 만드는 interaction에 관한 정보가 추가로 필요하다. Euclidean-type geometry에서는 이 정보를 vector의 pair에 대응하는 하나의 scalar로 나타내고, 그 scalar가 linear combination과 호환되도록 하고자 한다. 이를 다음 함수로 나타내자.

$$
B:V\times V\rightarrow\F.
$$

$B(x,y)$는 $x$와 $y$가 함께 나타날 때의 interaction을 표현하기 위한 값이다. Vector space의 핵심 연산은 linear combination이므로, 한 vector가 합으로 분해되면 interaction도 같은 방식으로 분해되는 것이 자연스럽다. 이 문서에서는 첫 번째 argument를 기준으로 다음 linearity를 요구한다.

$$
B(x_1+x_2,y)
=
B(x_1,y)+B(x_2,y),
\qquad
B(cx,y)=cB(x,y).
$$

이제 두 argument의 순서를 바꾸었을 때의 관계를 정해야 한다. Real vector space에서는 두 vector의 interaction이 순서에 의존하지 않도록 symmetry

$$
B(x,y)=B(y,x)
$$

를 요구할 수 있다. 첫 번째 argument의 linearity와 symmetry를 결합하면 두 번째 argument에 대한 linearity도 따라오므로 $B$는 bilinear해진다.

Complex vector space에서는 같은 symmetry를 그대로 사용할 수 없다. 첫 번째 argument의 linearity와 symmetry를 함께 요구하면 $B$가 두 argument 모두에 대해 complex-linear해진다. 이때 diagonal value $B(x,x)$를 squared length로 사용하려고 하면

$$
B(ix,ix)=i^2B(x,x)=-B(x,x)
$$

가 되어 nonnegative squared length로 사용할 수 없다. 또한 scalar multiplication에 대해 필요한 scaling은 $c^2$이 아니라 $\lvert c\rvert^2=c\overline c$이다. 따라서 complex case에서는 symmetry를 다음 conjugate symmetry로 바꾼다.

$$
B(x,y)=\overline{B(y,x)}.
$$

Real vector space에서는 complex conjugation이 값을 바꾸지 않으므로 이 조건은 ordinary symmetry와 같다. Complex case에서는 첫 번째 argument의 linearity와 conjugate symmetry로부터 두 번째 argument의 conjugate linearity가 따라온다.

$$
\begin{aligned}
B(x,cy)
&=\overline{B(cy,x)}\\
&=\overline{cB(y,x)}\\
&=\overline c\,B(x,y).
\end{aligned}
$$

따라서 두 argument를 동시에 $c$배하면

$$
B(cx,cx)
=c\overline c\,B(x,x)
=\lvert c\rvert^2B(x,x)
$$

가 되어 squared length에 필요한 scaling을 얻는다. 또한 conjugate symmetry에 $y=x$를 대입하면

$$
B(x,x)=\overline{B(x,x)}
$$

이므로 diagonal value는 real number다.

이제 $B(x,x)$를 squared length로 사용하려면 이 값이 nonnegative여야 하고, nonzero vector의 squared length가 $0$이 되어서는 안 된다. 따라서 다음 positive definiteness를 요구한다.

$$
B(x,x)\in\R_{\ge0},
\qquad
B(x,x)=0\iff x=0_V.
$$

그러면

$$
L(x):=\sqrt{B(x,x)}
$$

로 정의한 함수는 앞에서 요구한 length의 positivity와 scaling을 만족한다.

마지막으로 $B(x,y)$가 처음에 필요했던 interaction을 실제로 나타내는지 확인해 보자. Linearity와 conjugate symmetry를 사용하면 다음을 얻는다.

$$
\begin{aligned}
L(x+y)^2
&=B(x+y,x+y)\\
&=B(x,x)+B(x,y)+B(y,x)+B(y,y)\\
&=L(x)^2+L(y)^2+2\operatorname{Re}B(x,y).
\end{aligned}
$$

따라서 diagonal value $B(x,x)$는 vector 하나의 squared length를 나타내고, off-diagonal value $B(x,y)$는 개별 length만으로는 알 수 없었던 두 vector의 interaction을 나타낸다.

이처럼 linear combination과 호환되고, argument의 교환에 대해 conjugate symmetry를 가지며, diagonal이 positive-definite squared length가 되는 pairwise function을 inner product라고 한다. 이 조건들은 vector space에서 자동으로 따라오는 것이 아니라 Euclidean-type geometry를 선택하는 추가 규칙이다. Inner product가 주어지면 length뿐 아니라 distance, angle, orthogonality와 projection을 차례로 정의할 수 있다. 다음 절에서 inner product의 조건을 정확히 정의한다.

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
