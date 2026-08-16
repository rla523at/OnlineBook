# Gram Matrix

## 한 줄 요약

Gram matrix는 선택한 basis vector 사이의 inner product를 기록하여, 임의의 basis에서 vector의 coordinate로 inner product를 계산하게 한다.

## Motivation

[Inner Product Space](<30 Inner Product Space.md>)에서 정의한 inner product는 basis와 무관한 함수지만 실제 계산에서는 vector를 선택한 basis의 coordinate로 나타낸다. Orthonormal basis에서는 coordinate끼리 바로 inner product를 계산할 수 있지만, 일반적인 basis에서는 basis vector의 length와 서로 이루는 관계까지 반영해야 한다. 이 정보를 하나의 matrix에 모은 것이 Gram matrix다.

## 정의와 계산

$V$의 basis를 $\beta=(\beta_1,\ldots,\beta_n)$라고 하고 다음과 같이 정의하자.

$$
G_{ij}:=B(\beta_i,\beta_j)
$$

$G$를 inner product $B$의 basis $\beta$에 대한 `Gram matrix`라고 한다. Gram matrix는 basis vector 사이의 모든 inner product를 기록하여 coordinate에 필요한 geometry를 보존한다.

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

Conjugate symmetry와 positive definiteness에 의해 Gram matrix는 다음을 만족한다.

$$
G^{\mathsf *}=G,
\qquad
z^{\mathsf *}Gz>0
\quad
(z\neq0).
$$

여기서 $G^{\mathsf *}:=\overline G^{\mathsf T}$는 conjugate transpose다. 첫 번째 성질을 Hermitian, 두 번째 성질을 positive definite라고 한다. Inner product와 basis 중 하나라도 바뀌면 Gram matrix도 일반적으로 바뀐다. 또한 다음 두 조건은 동치다.

$$
\beta\text{ is an orthonormal basis}
\qquad\Longleftrightarrow\qquad
G=I.
$$

따라서 real inner product space의 arbitrary basis에서는 coordinate column끼리 계산한 $[x]_\beta^{\mathsf T}[y]_\beta$가 일반적으로 원래 inner product와 같지 않다. Complex inner product space에서는 coordinate의 complex conjugation도 함께 고려해야 한다.

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

이 경우 $a=(a_1,a_2)^{\mathsf T}$가 나타내는 vector의 squared length는 $a_1^2+a_2^2$가 아니라 $4a_1^2+a_2^2=a^{\mathsf T}Ga$다. Basis와 coordinate가 변하는 일반적인 관계는 [Change of Basis and Coordinate Matrix](<21 Change of Basis and Coordinate Matrix.md>)에서 설명한다. Inner product를 보존하는 map의 matrix condition이 Gram matrix에 따라 어떻게 달라지는지는 [Orthogonal Map](<36 Orthogonal Map.md>)에서 다룬다.
