# Change of Basis and Coordinate Matrix

## 한 줄 요약

Coordinate change matrix $P_{\beta\leftarrow\gamma}$는 같은 vector의 $\gamma$-coordinate를 $\beta$-coordinate로 바꾸며, 같은 linear map의 두 square matrix representation을 similarity relation으로 연결한다.

## Why the Direction Must Be Explicit

$n$-dimensional vector space $V/\F$의 두 ordered bases를

$$
\beta=(\beta_1,\ldots,\beta_n),
\qquad
\gamma=(\gamma_1,\ldots,\gamma_n)
$$

이라고 하자. 같은 vector $v\in V$라도

$$
v
=
\sum_{i=1}^n a_i\beta_i
=
\sum_{j=1}^n b_j\gamma_j
$$

이므로 coordinate columns $[v]_\beta$와 $[v]_\gamma$는 일반적으로 다르다.

각 $\gamma_j$를 $\beta$로 표현하면

$$
\gamma_j
=
\sum_{i=1}^n P_{ij}\beta_i.
$$

이를 $v=\sum_jb_j\gamma_j$에 대입하면

$$
\begin{aligned}
v
&=
\sum_{j=1}^n b_j
\sum_{i=1}^nP_{ij}\beta_i\\
&=
\sum_{i=1}^n
\left(
\sum_{j=1}^nP_{ij}b_j
\right)\beta_i.
\end{aligned}
$$

따라서

$$
[v]_\beta
=
P[v]_\gamma.
$$

같은 matrix가 $\gamma$ basis vector들을 $\beta$ basis로 표현하는 coefficient를 기록하면서, coordinate column은 반대쪽인 $\gamma$-coordinate에서 $\beta$-coordinate로 보낸다. “$\beta$에서 $\gamma$로 기저를 바꾼다”라는 말만으로는 이 두 관점의 방향을 혼동하기 쉽다. 이 문서에서는 coordinate의 입력과 출력을 화살표로 직접 표시한다.

## Coordinate Change Matrix

### Definition

$\gamma$-coordinate를 $\beta$-coordinate로 바꾸는 `coordinate change matrix`를

$$
P_{\beta\leftarrow\gamma}
:=
[id_V]_\gamma^\beta
=
\begin{bmatrix}
[\gamma_1]_\beta
&
\cdots
&
[\gamma_n]_\beta
\end{bmatrix}
$$

로 정의한다. 이 matrix를 두 bases 사이의 `change-of-basis matrix`라고도 한다.

Matrix representation의 fundamental coordinate equation에 identity map을 적용하면 모든 $v\in V$에 대해

$$
\boxed{
[v]_\beta
=
P_{\beta\leftarrow\gamma}[v]_\gamma
}
$$

를 얻는다. Basis vector들을 형식적으로 나란히 놓으면 같은 coefficient relation을

$$
\begin{bmatrix}
\gamma_1&\cdots&\gamma_n
\end{bmatrix}
=
\begin{bmatrix}
\beta_1&\cdots&\beta_n
\end{bmatrix}
P_{\beta\leftarrow\gamma}
$$

로 쓸 수 있다.

## Example

$\R^2$에서

$$
\beta=(e_1,e_2),
\qquad
\gamma=(e_1+e_2,e_2)
$$

라고 하자. $\gamma$ basis vector들의 $\beta$-coordinate를 column으로 모으면

$$
P_{\beta\leftarrow\gamma}
=
\begin{bmatrix}
1&0\\
1&1
\end{bmatrix}.
$$

Vector $v=e_1+2e_2$는

$$
[v]_\beta
=
\begin{bmatrix}
1\\
2
\end{bmatrix},
\qquad
[v]_\gamma
=
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

이고 실제로

$$
\begin{bmatrix}
1&0\\
1&1
\end{bmatrix}
\begin{bmatrix}
1\\
1
\end{bmatrix}
=
\begin{bmatrix}
1\\
2
\end{bmatrix}.
$$

따라서 subscript의 화살표는 basis vector가 아니라 coordinate column이 이동하는 방향을 나타낸다.

## Inverse and Composition

### 정리1

$$
P_{\gamma\leftarrow\beta}
=
P_{\beta\leftarrow\gamma}^{-1}.
$$

**Proof**

모든 $v\in V$에 대해

$$
[v]_\beta
=
P_{\beta\leftarrow\gamma}[v]_\gamma,
\qquad
[v]_\gamma
=
P_{\gamma\leftarrow\beta}[v]_\beta.
$$

두 식을 합성하면

$$
[v]_\beta
=
P_{\beta\leftarrow\gamma}
P_{\gamma\leftarrow\beta}
[v]_\beta.
$$

모든 column $[v]_\beta\in\F^n$에 대해 성립하므로

$$
P_{\beta\leftarrow\gamma}
P_{\gamma\leftarrow\beta}
=
I_n.
$$

반대 순서도 같은 방식으로 identity이므로 두 matrix는 서로 inverse다. $\qed$

세 번째 basis $\alpha$가 있으면 coordinate change를 두 단계로 합성할 수 있다.

$$
\boxed{
P_{\alpha\leftarrow\gamma}
=
P_{\alpha\leftarrow\beta}
P_{\beta\leftarrow\gamma}
}
$$

오른쪽 matrix가 먼저 작용하므로 $\gamma$-coordinate를 $\beta$-coordinate로 바꾼 뒤 $\alpha$-coordinate로 바꾼다.

## Every Invertible Matrix Is a Coordinate Change Matrix

### 정리2

모든 invertible matrix $A\in\F^{n\times n}$는 $\F^n$의 두 bases 사이의 coordinate change matrix다.

**Proof**

$\epsilon$을 $\F^n$의 standard basis라고 하고 $A$의 columns를

$$
\gamma=(a_1,\ldots,a_n)
$$

이라고 하자. $A$가 invertible이므로 columns는 linearly independent한 $n$개의 vector이고 $\gamma$는 basis다. Definition에 의해

$$
P_{\epsilon\leftarrow\gamma}
=
\begin{bmatrix}
[a_1]_\epsilon&\cdots&[a_n]_\epsilon
\end{bmatrix}
=
A.
\qed
$$

## The Same Linear Map in Two Bases

$T\in\operatorname{End}(V)$의 두 matrix representations를

$$
A_\beta:=[T]_\beta^\beta,
\qquad
A_\gamma:=[T]_\gamma^\gamma
$$

라고 하고

$$
P:=P_{\beta\leftarrow\gamma}
$$

라고 하자. 같은 input $v$에 대해 $\beta$-coordinate로 먼저 바꾸어 $T$를 적용하면

$$
[T(v)]_\beta
=
A_\beta P[v]_\gamma.
$$

반대로 $\gamma$-coordinate에서 $T$를 적용한 뒤 $\beta$-coordinate로 바꾸면

$$
[T(v)]_\beta
=
P A_\gamma[v]_\gamma.
$$

모든 $[v]_\gamma$에 대해 두 결과가 같으므로

$$
A_\beta P=PA_\gamma.
$$

$P$는 invertible이므로

$$
\boxed{
A_\gamma
=
P^{-1}A_\beta P
}
$$

를 얻는다. Basis가 바뀌면 matrix entry는 달라지지만 underlying linear map $T$는 같다.

## Similar Matrices

Square matrices $A,B\in\F^{n\times n}$에 대해 어떤 invertible matrix $P$가 존재하여

$$
B=P^{-1}AP
$$

이면 $A$와 $B$가 `similar`하다고 하고

$$
A\sim B
$$

라고 쓴다.

같은 endomorphism을 서로 다른 bases에서 나타낸 두 matrix는 similar하다. 반대로 $B=P^{-1}AP$이면 정리2에 의해 $P$를 coordinate change matrix로 해석할 수 있으므로 $A$와 $B$를 같은 linear map의 두 basis representations로 볼 수 있다.

## Similarity Invariants

Similar matrices $B=P^{-1}AP$는 다음 값을 공유한다.

$$
\det B
=
\det(P^{-1})\det(A)\det(P)
=
\det A,
$$

$$
\operatorname{tr}B
=
\operatorname{tr}(P^{-1}AP)
=
\operatorname{tr}(APP^{-1})
=
\operatorname{tr}A.
$$

Characteristic polynomial을

$$
\chi_A(t):=\det(tI-A)
$$

로 정의하면

$$
\begin{aligned}
\chi_B(t)
&=
\det(tI-P^{-1}AP)\\
&=
\det\left(P^{-1}(tI-A)P\right)\\
&=
\chi_A(t).
\end{aligned}
$$

따라서 finite-dimensional endomorphism $T$에 대해 임의의 basis $\beta$를 선택하여

$$
\det T:=\det([T]_\beta^\beta),
$$

$$
\operatorname{tr}T:=\operatorname{tr}([T]_\beta^\beta),
$$

$$
\chi_T(t):=\det\left(tI-[T]_\beta^\beta\right)
$$

로 정의해도 값은 basis 선택에 의존하지 않는다.

## 관련 문서

- [Coordinate](<06 Coordinate.md>)
- [Matrix Representation](<20 Matrix Representation.md>)
- [Eigenvector & Eigenvalue & EigenSpace](<24 Eigenvector & Eigenvalue & EigenSpace.md>)
