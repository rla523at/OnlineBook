# Four Fundamental Subspaces

## 한 줄 요약

Real matrix $A\in\mathbb R^{m\times n}$는 domain $\mathbb R^n$을 row space와 null space로, codomain $\mathbb R^m$을 column space와 left null space로 orthogonally 분해한다.

## 네 공간을 따로 아는 것만으로는 관계가 보이지 않는다

[Column Space, Row Space, and Rank](<../04 Linear Systems and Rank/22 Column Space, Row Space, and Rank.md>)에서는 matrix의 column과 row가 만드는 공간을 정의하고, row reduction의 pivot 개수가 두 공간의 공통 dimension인 rank임을 확인했다. [Kernel](<../02 Linear Maps and Isomorphisms/11 Kernel.md>)에서는 linear map이 zero vector로 보내는 input의 집합을 정의했다.

이 결과로 $A$와 $A^{\mathsf T}$에서 네 subspace를 만들 수 있지만, 각 공간을 목록으로 나열하는 것만으로는 이들이 왜 네 개의 짝을 이루는지 알기 어렵다. [Orthogonal Complement and Orthogonal Projection](<../05 Inner Product and Orthogonality/35 Orthogonal Complement and Orthogonal Projection.md>)을 사용하면 각 kernel이 다른 한 공간에서 보이지 않는 orthogonal direction 전체라는 사실을 밝힐 수 있다.

이 문서에서는 transpose를 사용하는 orthogonality 관계를 설명하기 위해 real matrix를 다룬다. Complex matrix에서는 transpose 대신 conjugate transpose를 사용한다.

## 네 fundamental subspaces

$A\in\mathbb R^{m\times n}$가 정의하는 linear map을

$$
L_A:\mathbb R^n\to\mathbb R^m,
\qquad
x\mapsto Ax
$$

라고 하자. $A$와 $A^{\mathsf T}$에서 다음 네 subspace를 얻는다.

| 공간 | 정의 | Ambient space | 의미 |
| --- | --- | --- | --- |
| Column space | $\mathcal C(A)=\{Ax\mid x\in\mathbb R^n\}$ | $\mathbb R^m$ | $A$가 만들 수 있는 output |
| Left null space | $\mathcal N(A^{\mathsf T})=\{y\in\mathbb R^m\mid A^{\mathsf T}y=0\}$ | $\mathbb R^m$ | 모든 column에 orthogonal한 output direction |
| Row space | $\operatorname{Row}(A)=\mathcal C(A^{\mathsf T})$ | $\mathbb R^n$ | $A$의 row들이 감지하는 input direction |
| Null space | $\mathcal N(A)=\{x\in\mathbb R^n\mid Ax=0\}$ | $\mathbb R^n$ | $A$가 zero로 보내는 input direction |

Left null space는 $A^{\mathsf T}$의 null space를 뜻한다. Column space와 left null space는 codomain $\mathbb R^m$에 있고, row space와 null space는 domain $\mathbb R^n$에 있다. Ambient space가 다른 공간 사이에는 orthogonal complement 관계를 직접 말할 수 없다.

## Null space는 row space의 orthogonal complement다

$A$의 row를 $q_1^{\mathsf T},\ldots,q_m^{\mathsf T}$라고 하면

$$
Ax
=
\begin{bmatrix}
q_1^{\mathsf T}x\\
\vdots\\
q_m^{\mathsf T}x
\end{bmatrix}.
$$

따라서 $Ax=0$이라는 하나의 matrix equation은 $x$가 모든 row와 orthogonal하다는 조건과 같다.

$$
\begin{aligned}
x\in\mathcal N(A)
&\Longleftrightarrow Ax=0\\
&\Longleftrightarrow q_i^{\mathsf T}x=0,\quad i=1,\ldots,m\\
&\Longleftrightarrow x\perp\operatorname{span}\{q_1,\ldots,q_m\}\\
&\Longleftrightarrow x\in\operatorname{Row}(A)^\perp.
\end{aligned}
$$

그러므로

$$
\boxed{\mathcal N(A)=\operatorname{Row}(A)^\perp}
$$

이다. Finite-dimensional space에서는 $(W^\perp)^\perp=W$이므로 반대 관계도 얻는다.

$$
\boxed{\operatorname{Row}(A)=\mathcal N(A)^\perp}
$$

Row space는 $A$가 input에서 실제로 측정하는 direction이고, null space는 그 모든 측정에서 사라지는 direction이다.

## Left null space는 column space의 orthogonal complement다

$A$의 column을 $a_1,\ldots,a_n\in\mathbb R^m$라고 하자. $y\in\mathbb R^m$에 대해

$$
A^{\mathsf T}y
=
\begin{bmatrix}
a_1^{\mathsf T}y\\
\vdots\\
a_n^{\mathsf T}y
\end{bmatrix}.
$$

따라서

$$
\begin{aligned}
y\in\mathcal N(A^{\mathsf T})
&\Longleftrightarrow A^{\mathsf T}y=0\\
&\Longleftrightarrow a_j^{\mathsf T}y=0,\quad j=1,\ldots,n\\
&\Longleftrightarrow y\perp\operatorname{span}\{a_1,\ldots,a_n\}\\
&\Longleftrightarrow y\in\mathcal C(A)^\perp.
\end{aligned}
$$

그러므로

$$
\boxed{\mathcal N(A^{\mathsf T})=\mathcal C(A)^\perp},
\qquad
\boxed{\mathcal C(A)=\mathcal N(A^{\mathsf T})^\perp}
$$

이다. Column space는 가능한 output direction이고, left null space는 가능한 모든 output에 orthogonal하여 $A^{\mathsf T}$를 적용했을 때 사라지는 direction이다.

## Rank가 네 공간의 dimension을 결정한다

$r=\operatorname{rank}(A)$라고 하자. Column space와 row space의 dimension은 rank의 정의와 row rank equals column rank에 의해 모두 $r$이다.

$$
\dim\mathcal C(A)=r,
\qquad
\dim\operatorname{Row}(A)=r
$$

Orthogonal complement의 dimension 관계를 적용하면 나머지 두 dimension도 결정된다.

$$
\dim\mathcal N(A)=n-r,
\qquad
\dim\mathcal N(A^{\mathsf T})=m-r
$$

이를 표로 정리하면 다음과 같다.

| Ambient space | 공간 | Dimension | Orthogonal complement |
| --- | --- | ---: | --- |
| $\mathbb R^n$ | $\operatorname{Row}(A)$ | $r$ | $\mathcal N(A)$ |
| $\mathbb R^n$ | $\mathcal N(A)$ | $n-r$ | $\operatorname{Row}(A)$ |
| $\mathbb R^m$ | $\mathcal C(A)$ | $r$ | $\mathcal N(A^{\mathsf T})$ |
| $\mathbb R^m$ | $\mathcal N(A^{\mathsf T})$ | $m-r$ | $\mathcal C(A)$ |

Rank-nullity theorem은 domain의 decomposition에 해당하는 $r+(n-r)=n$을 준다. $A^{\mathsf T}$에 같은 theorem을 적용하면 codomain의 decomposition에 해당하는 $r+(m-r)=m$을 얻는다.

## Domain과 codomain의 orthogonal decomposition

Orthogonal decomposition theorem에 의해 domain과 codomain은 각각 다음 direct sum으로 분해된다.

$$
\boxed{
\mathbb R^n
=
\operatorname{Row}(A)
\oplus
\mathcal N(A)
}
$$

$$
\boxed{
\mathbb R^m
=
\mathcal C(A)
\oplus
\mathcal N(A^{\mathsf T})
}
$$

따라서 모든 input $x\in\mathbb R^n$은 unique하게

$$
x=x_{\mathrm{row}}+x_{\mathrm{null}},
\qquad
x_{\mathrm{row}}\in\operatorname{Row}(A),
\quad
x_{\mathrm{null}}\in\mathcal N(A)
$$

로 분해된다. 이때

$$
Ax
=
A x_{\mathrm{row}}+A x_{\mathrm{null}}
=
A x_{\mathrm{row}}
$$

이므로 output에는 row-space component만 나타난다. Null-space component는 $A$를 적용하면 완전히 사라진다.

마찬가지로 모든 $b\in\mathbb R^m$은 unique하게

$$
b=b_{\mathrm{col}}+b_{\mathrm{left}},
\qquad
b_{\mathrm{col}}\in\mathcal C(A),
\quad
b_{\mathrm{left}}\in\mathcal N(A^{\mathsf T})
$$

로 분해된다. $Ax$는 항상 column space에 있으므로 $Ax=b$가 exact solution을 갖는 것은 $b_{\mathrm{left}}=0$인 경우뿐이다.

## 예제

다음 matrix를 생각하자.

$$
A=
\begin{bmatrix}
1&2&3\\
2&4&6
\end{bmatrix}.
$$

두 번째 row는 첫 번째 row의 $2$배이고 모든 column은 $(1,2)^{\mathsf T}$의 scalar multiple이므로 $\operatorname{rank}(A)=1$이다. 네 fundamental subspaces의 basis는 다음과 같다.

$$
\mathcal C(A)
=
\operatorname{span}
\left\{
\begin{bmatrix}1\\2\end{bmatrix}
\right\}
\subseteq\mathbb R^2,
$$

$$
\mathcal N(A^{\mathsf T})
=
\operatorname{span}
\left\{
\begin{bmatrix}-2\\1\end{bmatrix}
\right\}
\subseteq\mathbb R^2,
$$

$$
\operatorname{Row}(A)
=
\operatorname{span}
\left\{
\begin{bmatrix}1\\2\\3\end{bmatrix}
\right\}
\subseteq\mathbb R^3,
$$

$$
\mathcal N(A)
=
\operatorname{span}
\left\{
\begin{bmatrix}-2\\1\\0\end{bmatrix},
\begin{bmatrix}-3\\0\\1\end{bmatrix}
\right\}
\subseteq\mathbb R^3.
$$

실제로

$$
\begin{bmatrix}1&2\end{bmatrix}
\begin{bmatrix}-2\\1\end{bmatrix}
=0
$$

이고

$$
\begin{bmatrix}1&2&3\end{bmatrix}
\begin{bmatrix}-2\\1\\0\end{bmatrix}
=0,
\qquad
\begin{bmatrix}1&2&3\end{bmatrix}
\begin{bmatrix}-3\\0\\1\end{bmatrix}
=0.
$$

따라서 codomain에서는 $1+1=2$, domain에서는 $1+2=3$으로 각 orthogonal pair의 dimension 합이 ambient-space dimension과 일치한다.

## Least squares와의 연결

$b=b_{\mathrm{col}}+b_{\mathrm{left}}$에서 $b_{\mathrm{col}}$은 $b$를 $\mathcal C(A)$ 위로 orthogonally projection한 vector다. $Ax^\star=b_{\mathrm{col}}$인 $x^\star$를 선택하면 residual을 $r^\star:=Ax^\star-b$로 정의했을 때

$$
r^\star
=
-b_{\mathrm{left}}
\in
\mathcal N(A^{\mathsf T})
$$

이므로

$$
A^{\mathsf T}r^\star=0.
$$

이 식이 [Least Squares Problem](<38 Least Squares Problem.md>)의 normal equation이 나타나는 공간적 이유다.

## SVD와의 연결

[Singular Value Decomposition](<../08 Matrix Decompositions/41 Singular Value Decomposition.md>)은 네 fundamental subspaces의 orthonormal basis를 동시에 구성한다. Nonzero singular value에 대응하는 right singular vector들은 row space를, zero singular value에 대응하는 right singular vector들은 null space를 span한다. 같은 방식으로 left singular vector들은 column space와 left null space를 나눈다.

## 관련 문서

- [Kernel](<../02 Linear Maps and Isomorphisms/11 Kernel.md>)
- [Image](<../02 Linear Maps and Isomorphisms/12 Image.md>)
- [Column Space, Row Space, and Rank](<../04 Linear Systems and Rank/22 Column Space, Row Space, and Rank.md>)
- [Orthogonal Complement and Orthogonal Projection](<../05 Inner Product and Orthogonality/35 Orthogonal Complement and Orthogonal Projection.md>)
- [Least Squares Problem](<38 Least Squares Problem.md>)
- [Singular Value Decomposition](<../08 Matrix Decompositions/41 Singular Value Decomposition.md>)
