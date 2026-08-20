# Column Space, Row Space, and Rank

## 한 줄 요약

Matrix의 column space는 $Ax$로 만들 수 있는 output의 집합이고, row space는 equation의 coefficient direction이 만드는 공간이며, 두 공간의 dimension은 pivot의 개수와 같은 rank다.

## 하나의 right-hand side를 푸는 것만으로는 부족하다

[Linear Systems and Row Reduction](<21 Linear Systems and Row Reduction.md>)에서는 주어진 $b$에 대해 $Ax=b$를 row reduction하여 solution을 구했다. 그러나 $b$가 바뀔 때마다 augmented matrix를 새로 계산하는 것만으로는 $A$가 어떤 output을 만들 수 있는지 한눈에 알기 어렵다.

$A\in\mathbb F^{m\times n}$의 column을 $a_1,\ldots,a_n\in\mathbb F^m$라고 쓰면

$$
A=
\begin{bmatrix}
a_1&\cdots&a_n
\end{bmatrix}
$$

이고, $x=(x_1,\ldots,x_n)^{\mathsf T}$에 대해 matrix-vector product는 다음 linear combination이다.

$$
Ax
=
x_1a_1+\cdots+x_na_n
$$

따라서 $A$로 만들 수 있는 모든 output을 알려면 $A$의 column들이 만드는 모든 linear combination을 조사해야 한다.

## Column space

$A$의 `column space`를 다음과 같이 정의한다.

$$
\mathcal C(A)
:=
\operatorname{span}\{a_1,\ldots,a_n\}
=
\{Ax\mid x\in\mathbb F^n\}
\subseteq
\mathbb F^m
$$

두 번째 등식은 정의와 별개의 우연한 성질이 아니다. $Ax$가 column들의 linear combination이고, 반대로 column들의 coefficient를 모으면 vector $x$가 되기 때문에 두 집합이 정확히 같다.

Matrix가 정의하는 linear map

$$
L_A:\mathbb F^n\to\mathbb F^m,
\qquad
x\mapsto Ax
$$

를 생각하면 column space는 [Image](<../02 Linear Maps and Isomorphisms/12 Image.md>)의 matrix version이다.

$$
\mathcal C(A)=\operatorname{img}(L_A)
$$

따라서 linear system의 solvability는 다음처럼 공간의 membership 문제로 바뀐다.

### 정리1: Solvability와 column space

$$
Ax=b\text{가 solution을 갖는다}
\quad\Longleftrightarrow\quad
b\in\mathcal C(A)
$$

**Proof**

$Ax=b$인 $x$가 존재하면 column space의 정의에 의해 $b\in\mathcal C(A)$다. 반대로 $b\in\mathcal C(A)$이면 어떤 coefficient $x_1,\ldots,x_n$에 대해 $b=x_1a_1+\cdots+x_na_n=Ax$이므로 $x=(x_1,\ldots,x_n)^{\mathsf T}$는 solution이다. $\qed$

## Pivot column이 필요한 이유

모든 column을 사용하면 column space를 생성할 수 있지만, linearly dependent한 column은 새로운 direction을 추가하지 않는다. 따라서 column space의 basis를 구하려면 같은 공간을 span하면서 linearly independent한 column들만 선택해야 한다.

$A$를 row reduction하여 RREF $R$을 얻고 pivot position이 나타나는 column index를

$$
j_1,\ldots,j_r
$$

라고 하자. 여기서 `pivot column of $A$`는 $R$의 pivot과 같은 index에 있는 원래 matrix $A$의 column

$$
a_{j_1},\ldots,a_{j_r}
$$

를 뜻한다. Column space의 basis를 구할 때는 $R$의 column이 아니라 원래 $A$의 column을 선택해야 한다.

### 정리2: Pivot columns form a basis of the column space

$A$의 pivot column들은 $\mathcal C(A)$의 basis다.

**Proof**

Row reduction은 elementary matrix들의 곱인 invertible matrix $E$를 왼쪽에 곱하는 것과 같으므로

$$
R=EA
$$

로 쓸 수 있다. $R$의 $j$번째 column을 $r_j$라고 하면 $r_j=Ea_j$다. 임의의 coefficient $c_1,\ldots,c_n$에 대해

$$
\sum_{j=1}^n c_jr_j
=
E\left(\sum_{j=1}^n c_ja_j\right)
$$

이고 $E$가 invertible이므로

$$
\sum_{j=1}^n c_jr_j=0
\quad\Longleftrightarrow\quad
\sum_{j=1}^n c_ja_j=0.
$$

즉, $A$와 $R$의 column들 사이에는 같은 coefficient를 갖는 linear dependence relation이 성립한다.

$R$의 pivot column들은 서로 다른 pivot row에 leading $1$을 가지므로 linearly independent하다. Pivot row가 위에서부터 $1,\ldots,r$이고 $R$의 $k$번째 pivot column을 $r_{j_k}$라고 하면, zero row를 포함한 $m$개 entry 중 $k$번째 entry만 $1$인 vector다. 따라서 임의의 nonpivot column

$$
r_j=
\begin{bmatrix}
d_1\\
\vdots\\
d_r\\
0\\
\vdots\\
0
\end{bmatrix}
$$

은 다음처럼 pivot column들의 linear combination으로 표현된다.

$$
r_j
=
d_1r_{j_1}+\cdots+d_rr_{j_r}
$$

같은 dependence relation이 $A$에서도 성립하므로 $a_{j_1},\ldots,a_{j_r}$는 linearly independent하고 $A$의 모든 column을 span한다. 따라서 이들은 $\mathcal C(A)$의 basis다. $\qed$

### 예제

앞 문서에서 사용한 matrix

$$
A=
\begin{bmatrix}
1&2&1&0\\
2&4&0&2\\
1&2&2&-1
\end{bmatrix}
$$

의 RREF는

$$
R=
\begin{bmatrix}
1&2&0&1\\
0&0&1&-1\\
0&0&0&0
\end{bmatrix}
$$

이다. Pivot은 첫 번째와 세 번째 column에 있으므로 원래 matrix의 column

$$
a_1=
\begin{bmatrix}
1\\2\\1
\end{bmatrix},
\qquad
a_3=
\begin{bmatrix}
1\\0\\2
\end{bmatrix}
$$

가 $\mathcal C(A)$의 basis다. 실제로 RREF의 nonpivot column 관계와 같은 coefficient를 사용하면

$$
a_2=2a_1,
\qquad
a_4=a_1-a_3
$$

를 확인할 수 있다. 따라서

$$
\mathcal C(A)=\operatorname{span}\{a_1,a_3\}.
$$

앞 문서의 right-hand side는

$$
b=
\begin{bmatrix}
3\\2\\5
\end{bmatrix}
=
a_1+2a_3
$$

이므로 $b\in\mathcal C(A)$이고 $Ax=b$는 solution을 갖는다.

## Row space

$A$의 $i$번째 row를 $q_i^{\mathsf T}$라고 하자. 이 row를 transpose한 $q_i\in\mathbb F^n$을 사용하여 $A$의 `row space`를 다음과 같이 정의한다.

$$
\operatorname{Row}(A)
:=
\operatorname{span}\{q_1,\ldots,q_m\}
\subseteq
\mathbb F^n
$$

$A^{\mathsf T}$의 column들이 $A$의 row들을 transpose한 vector이므로

$$
\operatorname{Row}(A)=\mathcal C(A^{\mathsf T})
$$

로 볼 수 있다.

### Row operation은 row space를 보존한다

Elementary row operation으로 얻은 새 row는 원래 row들의 linear combination이므로 새 matrix의 row space는 원래 row space에 포함된다. 각 elementary row operation은 역연산을 가지므로 원래 row들도 새 row들의 linear combination으로 복원된다. 따라서 row equivalent한 matrix $A$와 $R$에 대해

$$
\operatorname{Row}(A)=\operatorname{Row}(R)
$$

이 성립한다.

반면 row operation은 column space를 일반적으로 보존하지 않는다. $R=EA$이면

$$
\mathcal C(R)=E\mathcal C(A)
$$

이지, 일반적으로 $\mathcal C(R)=\mathcal C(A)$는 아니다. 예를 들어

$$
A=
\begin{bmatrix}
1\\0
\end{bmatrix}
\quad\longrightarrow\quad
R=
\begin{bmatrix}
0\\1
\end{bmatrix}
$$

처럼 두 row를 교환하면 두 matrix의 column space는 각각 서로 다른 coordinate axis다. 이것이 column-space basis를 구할 때 RREF의 pivot column이 아니라 원래 matrix의 pivot column을 선택해야 하는 이유다.

### 정리3: Nonzero rows of an echelon form

$A$의 echelon form에서 nonzero row들은 $\operatorname{Row}(A)$의 basis다.

**Proof**

Row operation이 row space를 보존하므로 echelon form의 row들은 $\operatorname{Row}(A)$를 span한다. 각 nonzero row의 leading entry는 아래 row의 leading entry보다 왼쪽에 있다. Nonzero rows의 linear combination이 zero라고 가정하고 가장 위쪽 row의 leading position부터 차례로 보면 해당 position을 상쇄할 수 있는 아래 row가 없으므로 그 row의 coefficient는 $0$이어야 한다. 같은 논리를 아래 row에 반복하면 모든 coefficient가 $0$이다. 따라서 nonzero rows는 linearly independent하며 row space의 basis다. $\qed$

예제의 RREF를 사용하면

$$
\operatorname{Row}(A)
=
\operatorname{span}
\left\{
\begin{bmatrix}1\\2\\0\\1\end{bmatrix},
\begin{bmatrix}0\\0\\1\\-1\end{bmatrix}
\right\}.
$$

## Rank

[Image](<../02 Linear Maps and Isomorphisms/12 Image.md>)에서는 linear map의 rank를 image의 dimension으로 정의했다. Matrix $A$가 정의하는 $L_A:x\mapsto Ax$에 이를 적용하면

$$
\operatorname{rank}(A)
:=
\dim\mathcal C(A)
$$

이다.

### 정리4: Row rank equals column rank

$A$의 pivot 개수를 $r$이라고 하자. 정리2에 의해 $A$의 $r$개 pivot column은 column space의 basis이므로

$$
\dim\mathcal C(A)=r.
$$

Echelon form에는 pivot마다 nonzero row가 정확히 하나씩 있고, 정리3에 의해 이 $r$개 row는 row space의 basis이므로

$$
\dim\operatorname{Row}(A)=r.
$$

따라서

$$
\boxed{
\operatorname{rank}(A)
=
\dim\mathcal C(A)
=
\dim\operatorname{Row}(A)
=
\text{number of pivots}
}
$$

이다. Column space의 dimension을 column rank, row space의 dimension을 row rank라고 부르기도 하지만 두 값이 항상 같으므로 보통 둘 다 rank라고 한다.

## Rank가 solution에 주는 정보

$A\in\mathbb F^{m\times n}$이고 $r=\operatorname{rank}(A)$라고 하자.

- $r=n$이면 모든 coefficient column에 pivot이 있으므로 free variable이 없다. 이는 $\ker(L_A)=\{0\}$인 것과 동치이며, $Ax=b$는 solution을 가질 때 유일하다.
- $r=m$이면 모든 row에 pivot이 있으므로 $\mathcal C(A)=\mathbb F^m$이다. 따라서 모든 $b\in\mathbb F^m$에 대해 $Ax=b$가 적어도 하나의 solution을 갖는다.
- [Rank-nullity theorem](<../02 Linear Maps and Isomorphisms/12 Image.md>)에 의해 $\dim\ker(L_A)=n-r$이므로 free variable의 개수와 kernel의 dimension이 일치한다.

특히 square matrix $A\in\mathbb F^{n\times n}$에 대해서는 다음 조건들이 동치다.

$$
\begin{aligned}
A\text{ is invertible}
&\Longleftrightarrow \operatorname{rank}(A)=n\\
&\Longleftrightarrow \mathcal C(A)=\mathbb F^n\\
&\Longleftrightarrow \ker(L_A)=\{0\}\\
&\Longleftrightarrow Ax=b\text{ has a unique solution for every }b\in\mathbb F^n.
\end{aligned}
$$

이 동치들은 [Change of Basis and Coordinate Matrix](<../03 Matrix Representation and Eigenstructure/23 Change of Basis and Coordinate Matrix.md>)에서 invertible matrix의 column들이 basis를 이룬다는 사실과 [Eigenvector & Eigenvalue & EigenSpace](<../03 Matrix Representation and Eigenstructure/24 Eigenvector & Eigenvalue & EigenSpace.md>)에서 homogeneous system이 nonzero solution을 갖는 조건을 해석하는 데 사용된다.

## 아직 결합하지 않은 네 공간

이제 $\mathcal C(A)$, $\operatorname{Row}(A)=\mathcal C(A^{\mathsf T})$, $\ker(L_A)$, $\ker(L_{A^{\mathsf T}})$라는 네 subspace를 정의할 수 있다. 그러나 어떤 공간들이 서로 orthogonal complement인지 설명하려면 inner product와 orthogonal complement가 필요하다. 이 관계는 해당 개념을 학습한 뒤 [Four Fundamental Subspaces](<../06 Matrix Subspaces and Approximation/36 Four Fundamental Subspaces.md>)에서 다룬다.

## 관련 문서

- [Span](<../01 Vector Space Structure/03 Span.md>)
- [Kernel](<../02 Linear Maps and Isomorphisms/11 Kernel.md>)
- [Image](<../02 Linear Maps and Isomorphisms/12 Image.md>)
- [Matrix Representation](<../03 Matrix Representation and Eigenstructure/20 Matrix Representation.md>)
- [Linear Systems and Row Reduction](<21 Linear Systems and Row Reduction.md>)
- [Four Fundamental Subspaces](<../06 Matrix Subspaces and Approximation/36 Four Fundamental Subspaces.md>)
