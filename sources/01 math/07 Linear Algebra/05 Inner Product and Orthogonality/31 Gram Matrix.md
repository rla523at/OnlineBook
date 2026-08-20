# Gram Matrix

## 한 줄 요약

Gram matrix는 선택한 basis에서 inner product를 나타내는 행렬이며, 두 vector의 coordinates로부터 inner product를 계산할 수 있게 한다.

## Motivation

[Inner Product Space](<30 Inner Product Space.md>)에서 정의한 inner product $B$는 basis의 선택과 관계없이 두 vector에 대해 정의된다. 하지만 실제 계산에서는 두 vector를 선택한 basis에 대한 coordinate vector로 나타내므로, 이러한 coordinates로부터 $B(x,y)$를 어떻게 계산할 수 있는지 살펴볼 필요가 있다. 선택한 basis를 $\beta=(\beta_1,\ldots,\beta_n)$라고 하고

$$
x=\sum_{i=1}^n a_i\beta_i,
\qquad
y=\sum_{j=1}^n b_j\beta_j
$$

라고 하자. Inner product의 linearity와 conjugate linearity를 사용하면 다음을 얻는다.

$$
B(x,y)
=
\sum_{i=1}^n\sum_{j=1}^n
a_i\overline{b_j}B(\beta_i,\beta_j).
$$

따라서 coordinate의 같은 위치끼리 곱한 $\sum_{i=1}^n a_i\overline{b_i}$만으로는 일반적으로 $B(x,y)$를 계산할 수 없다. 각 coordinate가 나타내는 basis vector들이 inner product 아래에서 어떻게 관계하는지, 즉 $B(\beta_i,\beta_j)$도 알아야 한다.

위 전개식은 필요한 정보와 그 배열 방법을 동시에 보여준다. $x$의 $i$번째 coordinate와 $y$의 $j$번째 coordinate 사이에 곱해지는 값 $B(\beta_i,\beta_j)$를 matrix의 $(i,j)$ entry에 놓으면 전체 double sum을 하나의 matrix product로 쓸 수 있다. 따라서 basis가 정해져 있으면 이 entries만으로 임의의 두 vector 사이의 inner product를 계산할 수 있다. 반대로 $x=\beta_i$, $y=\beta_j$를 대입하면 matrix의 $(i,j)$ entry인 $B(\beta_i,\beta_j)$를 그대로 얻는다. 즉 이 matrix는 선택한 basis에서 inner product에 관한 정보를 빠짐없이 담고 있다. 이렇게 얻는 matrix가 Gram matrix다.

## 정의와 계산

Inner product space $(V,B)$의 basis를 $\beta=(\beta_1,\ldots,\beta_n)$라고 하고 다음과 같이 정의하자.

$$
G_{ij}:=B(\beta_i,\beta_j)
$$

$G$를 inner product $B$의 basis $\beta$에 대한 `Gram matrix`라고 한다. Diagonal entry는 각 basis vector의 squared length를 기록하고, off-diagonal entry는 서로 다른 basis vector 사이의 interaction을 기록한다.

$a=[x]_\beta$와 $b=[y]_\beta$라고 하면 이 문서의 convention에서는 다음이 성립한다.

$$
B(x,y)
=
a^{\mathsf T}G\overline b.
$$

$\F=\R$이면 다음과 같이 단순해진다.

$$
B(x,y)
=
[x]_\beta^{\mathsf T}G[y]_\beta.
$$

Conjugate symmetry가 Gram matrix에 주는 성질부터 확인하자. 여기서 $G^{\mathsf *}:=\overline G^{\mathsf T}$는 conjugate transpose다. 임의의 $i,j$에 대해 다음이 성립한다.

$$
\begin{aligned}
(G^{\mathsf *})_{ij}
&=\overline{G_{ji}} \\
&=\overline{B(\beta_j,\beta_i)} \\
&=B(\beta_i,\beta_j) \\
&=G_{ij}.
\end{aligned}
$$

세 번째 등식은 inner product의 conjugate symmetry에서 나온다. $G^{\mathsf *}$와 $G$의 corresponding entry가 모두 같으므로 $G^{\mathsf *}=G$이다.

다음으로 nonzero coordinate column $a\in\F^n$을 잡고 $x=\sum_{i=1}^n a_i\beta_i$라고 하자. $\beta$가 basis이므로 $a\neq0$이면 $x\neq0_V$이다. 앞에서 얻은 coordinate 공식과 inner product의 positive definiteness에 의해 다음이 성립한다.

$$
0
<
B(x,x)
=
a^{\mathsf T}G\overline a.
$$

Matrix의 positive definiteness는 보통 $z^{\mathsf *}Gz$를 사용하여 표기하며, 여기서 $z^{\mathsf *}:=\overline z^{\mathsf T}$이다. $z:=\overline a$라고 두면 $z^{\mathsf *}=a^{\mathsf T}$이므로 다음을 얻는다.

$$
z^{\mathsf *}Gz
=
a^{\mathsf T}G\overline a
=
B(x,x)
>
0.
$$

Map $a\mapsto\overline a$는 nonzero coordinate column 전체에서 일대일 대응이므로, 이 부등식은 모든 nonzero $z\in\F^n$에 대해 성립한다. 따라서 Gram matrix는 다음 두 성질을 갖는다.

$$
G^{\mathsf *}=G,
\qquad
z^{\mathsf *}Gz>0
\quad
(z\in\F^n,\ z\neq0).
$$

첫 번째 성질을 만족하는 matrix를 Hermitian, 두 번째 성질을 만족하는 matrix를 positive definite라고 한다. $\F=\R$이면 complex conjugation이 사라지므로 각각 $G^{\mathsf T}=G$와 $z^{\mathsf T}Gz>0$이 된다.

Inner product와 basis 중 하나라도 바뀌면 Gram matrix도 일반적으로 바뀐다. 일반적인 basis에서는 $G$가 identity matrix일 필요가 없으므로, real inner product space에서 coordinate column끼리 계산한 $[x]_\beta^{\mathsf T}[y]_\beta$는 일반적으로 원래 inner product와 같지 않다. Complex inner product space에서는 coordinate의 complex conjugation도 함께 고려해야 한다.

### Euclidean space에서의 예

$\R^n$에 standard inner product를 주고 basis vector를 column으로 갖는 matrix를

$$
P=
\begin{bmatrix}
\beta_1&\cdots&\beta_n
\end{bmatrix}
$$

라고 하자. 그러면 $[x]_\beta=a$와 $[y]_\beta=b$에 대해 $x=Pa$, $y=Pb$이므로

$$
x^{\mathsf T}y
=
a^{\mathsf T}P^{\mathsf T}Pb.
$$

따라서 이 basis의 Gram matrix는 다음과 같다.

$$
G=P^{\mathsf T}P.
$$

예를 들어 $\beta_1=(2,0)$, $\beta_2=(0,1)$이면

$$
G=
\begin{bmatrix}
4&0\\
0&1
\end{bmatrix}.
$$

이 경우 $a=(a_1,a_2)^{\mathsf T}$가 나타내는 vector의 squared length는 $a_1^2+a_2^2$가 아니라 $4a_1^2+a_2^2=a^{\mathsf T}Ga$다. Basis와 coordinate가 변하는 일반적인 관계는 [Change of Basis and Coordinate Matrix](<../03 Matrix Representation and Eigenstructure/23 Change of Basis and Coordinate Matrix.md>)에서 설명한다. Inner product를 보존하는 map의 matrix condition이 Gram matrix에 따라 어떻게 달라지는지는 [Orthogonal Map](<37 Orthogonal Map.md>)에서 다룬다.
