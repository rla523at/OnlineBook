# Linear Systems and Row Reduction

## 한 줄 요약

Linear system을 augmented matrix로 나타내고 solution set을 보존하는 row operation을 적용하면, pivot variable과 free variable을 구분하여 해의 존재 여부와 전체 구조를 읽을 수 있다.

## Matrix equation만으로는 해의 구조가 바로 보이지 않는다

$A\in\mathbb F^{m\times n}$, $x\in\mathbb F^n$, $b\in\mathbb F^m$에 대한 matrix equation

$$
Ax=b
$$

를 생각하자. $A$의 $i$번째 row를 $r_i^{\mathsf T}$라고 하면 이 식은 다음 $m$개의 scalar equation을 한 번에 쓴 것이다.

$$
r_i^{\mathsf T}x=b_i,
\qquad
i=1,\ldots,m
$$

$Ax=b$라는 표기는 compact하지만 이것만 보고는 해가 존재하는지, 해가 유일한지, 또는 어떤 variable을 자유롭게 선택할 수 있는지 바로 알기 어렵다. 이러한 정보는 equation들을 solution set이 바뀌지 않는 방식으로 단순화할 때 드러난다.

## Augmented matrix

Linear system의 coefficient와 right-hand side를 함께 기록한 matrix

$$
[A\mid b]
=
\left[
\begin{array}{ccc|c}
a_{11} & \cdots & a_{1n} & b_1\\
\vdots & & \vdots & \vdots\\
a_{m1} & \cdots & a_{mn} & b_m
\end{array}
\right]
$$

를 `augmented matrix`라고 한다. Vertical line의 왼쪽은 unknown의 coefficient이고 오른쪽은 equation의 right-hand side다.

한 row는 하나의 scalar equation을 나타낸다. 따라서 equation의 순서를 바꾸거나, 같은 equation의 양변에 nonzero scalar를 곱하거나, 한 equation에 다른 equation의 scalar multiple을 더해도 원래 system과 정확히 같은 solution set을 얻는다.

### Elementary row operations

다음 세 연산을 `elementary row operation`이라고 한다.

1. 두 row를 교환한다.
2. 한 row에 nonzero scalar를 곱한다.
3. 한 row에 다른 row의 scalar multiple을 더한다.

각 연산에는 원래 상태로 되돌리는 elementary row operation이 존재한다. 예를 들어 $R_i\leftarrow R_i+cR_j$는 $R_i\leftarrow R_i-cR_j$로 되돌릴 수 있다. 따라서 row operation 전후의 system은 서로의 모든 equation을 다시 복원할 수 있고, 두 system의 solution set은 같다.

Row operation으로 서로 바꿀 수 있는 matrix를 `row equivalent`하다고 한다. 특히 augmented matrix가 row equivalent하면 대응하는 linear system의 solution set도 같다.

## Echelon form과 pivot

Row operation으로 같은 solution set을 나타내는 matrix를 여러 형태로 만들 수 있다. 이때 목적은 단순히 $0$을 많이 만드는 것이 아니라, 아래쪽 equation부터 variable을 하나씩 결정할 수 있도록 equation을 배치하는 것이다.

예를 들어 row operation 결과가

$$
\left[
\begin{array}{cccc|c}
p_1&a_{12}&a_{13}&a_{14}&c_1\\
0&0&p_2&a_{24}&c_2\\
0&0&0&0&0
\end{array}
\right],
\qquad
p_1,p_2\ne0
$$

이라고 하자. 아래쪽 nonzero row는

$$
p_2x_3+a_{24}x_4=c_2
$$

이므로 $x_4$를 정하면

$$
x_3=\frac{c_2-a_{24}x_4}{p_2}
$$

로 $x_3$를 결정할 수 있다. 그 결과를 첫 번째 row에 대입하면

$$
x_1
=
\frac{c_1-a_{12}x_2-a_{13}x_3-a_{14}x_4}{p_1}
$$

이므로 $x_2,x_4$를 선택한 뒤 $x_3,x_1$을 차례로 결정할 수 있다. 반면 마지막 zero row는 어떤 variable에도 새로운 조건을 주지 않는다.

이처럼 아래에서 위로 풀려면, 아래쪽 equation은 위쪽 equation에서 먼저 다룰 variable을 다시 포함하지 않아야 한다. 따라서 각 nonzero row에서 가장 왼쪽에 있는 nonzero entry의 위치는 아래 row로 갈수록 오른쪽으로 이동하고, 그 entry 아래는 모두 $0$이어야 한다. 새로운 조건을 주지 않는 zero row는 맨 아래에 모은다. 각 nonzero row에서 가장 왼쪽에 있는 nonzero entry를 그 row의 `leading entry`라고 한다.

Matrix가 다음 조건을 만족하면 `row echelon form`, 줄여서 REF라고 한다.

1. Zero row는 모든 nonzero row보다 아래에 있다.
2. 아래쪽 nonzero row의 leading entry는 바로 위쪽 row의 leading entry보다 오른쪽에 있다.
3. 각 leading entry 아래의 entry는 모두 $0$이다.

이 문서에서는 leading entry를 row에서 가장 왼쪽에 있는 nonzero entry로 정했다. 따라서 2번 조건을 아래쪽 nonzero row들에 반복해 적용하면, 각 아래 row에서 이전 leading entry가 놓인 column까지 모두 $0$이다. Zero row는 전체가 $0$이므로 3번 조건도 성립한다. 3번 조건은 row elimination이 pivot 아래를 제거한다는 핵심 동작을 명시적으로 드러내기 위해 따로 적는다.

REF의 leading entry가 있는 위치를 `pivot position`이라고 하고, 그 위치의 entry를 `pivot`이라고 한다. Pivot이 놓인 coefficient column에 대응하는 unknown은 `pivot variable`, pivot이 없는 coefficient column에 대응하는 unknown은 `free variable`이다.

REF가 다음 조건도 만족하면 `reduced row echelon form`, 줄여서 RREF라고 한다.

1. 모든 pivot은 $1$이다.
2. 각 pivot은 자신의 column에서 유일한 nonzero entry다.

REF는 back substitution에 필요한 구조까지만 요구하므로 pivot을 $1$로 만들거나 pivot 위를 제거할 필요가 없다. RREF에서는 이 작업까지 수행하여 각 pivot variable이 free variable에 어떻게 의존하는지가 matrix에 직접 나타난다.

## 예제: Pivot variable과 free variable

다음 system을 생각하자.

$$
Ax=b,
\qquad
A=
\begin{bmatrix}
1&2&1&0\\
2&4&0&2\\
1&2&2&-1
\end{bmatrix},
\qquad
b=
\begin{bmatrix}
3\\2\\5
\end{bmatrix}.
$$

Augmented matrix에서 다음 row operations를 수행한다.

$$
\left[
\begin{array}{cccc|c}
1&2&1&0&3\\
2&4&0&2&2\\
1&2&2&-1&5
\end{array}
\right]
\xrightarrow{\substack{R_2\leftarrow R_2-2R_1\\R_3\leftarrow R_3-R_1}}
\left[
\begin{array}{cccc|c}
1&2&1&0&3\\
0&0&-2&2&-4\\
0&0&1&-1&2
\end{array}
\right].
$$

$R_2$와 $R_3$를 교환한 뒤 $R_3\leftarrow R_3+2R_2$, $R_1\leftarrow R_1-R_2$를 적용하면 RREF를 얻는다.

$$
\left[
\begin{array}{cccc|c}
1&2&0&1&1\\
0&0&1&-1&2\\
0&0&0&0&0
\end{array}
\right].
$$

첫 번째와 세 번째 coefficient column에 pivot이 있으므로 $x_1,x_3$는 pivot variable이고 $x_2,x_4$는 free variable이다. $x_2=s$, $x_4=t$라고 두면

$$
x_1=1-2s-t,
\qquad
x_3=2+t
$$

이므로 모든 solution은 다음과 같다.

$$
x
=
\begin{bmatrix}
1\\0\\2\\0
\end{bmatrix}
+
s
\begin{bmatrix}
-2\\1\\0\\0
\end{bmatrix}
+
t
\begin{bmatrix}
-1\\0\\1\\1
\end{bmatrix},
\qquad
s,t\in\mathbb F.
$$

Free variable마다 solution이 움직일 수 있는 independent direction이 하나씩 나타난다.

## 해의 존재 조건

Row reduction 결과에

$$
\left[
\begin{array}{ccc|c}
0&\cdots&0&c
\end{array}
\right],
\qquad
c\ne0
$$

인 row가 나타나면 대응하는 equation은 $0=c$가 된다. 이는 어떤 $x$도 만족할 수 없으므로 system은 solution을 갖지 않는다. 이러한 row가 없으면 free variable을 임의로 선택하고 pivot variable을 결정할 수 있으므로 적어도 하나의 solution이 존재한다.

따라서 다음 두 조건은 동치다.

$$
Ax=b\text{가 solution을 갖는다}
\quad\Longleftrightarrow\quad
[A\mid b]\text{의 echelon form에 }[0\ \cdots\ 0\mid c],\ c\ne0\text{인 row가 없다}.
$$

## Homogeneous system과 kernel

Right-hand side가 zero vector인 system

$$
Ax=0
$$

을 `homogeneous linear system`이라고 한다. Homogeneous system은 항상 trivial solution $x=0$을 갖는다. Free variable이 존재하면 이를 nonzero로 선택하여 nontrivial solution도 만들 수 있다.

Matrix가 정의하는 linear map $L_A:x\mapsto Ax$를 생각하면 homogeneous system의 solution set은 [Kernel](<11 Kernel.md>)이다.

$$
\ker(L_A)
=
\{x\in\mathbb F^n\mid Ax=0\}
$$

일반 system $Ax=b$의 particular solution 하나를 $x_p$라고 하자. 다른 solution $x$에 대해서

$$
A(x-x_p)=Ax-Ax_p=b-b=0
$$

이므로 $x-x_p\in\ker(L_A)$다. 반대로 $z\in\ker(L_A)$이면 $A(x_p+z)=b$다. 따라서 solution이 존재할 때 전체 solution set은

$$
x_p+\ker(L_A)
=
\{x_p+z\mid z\in\ker(L_A)\}
$$

이다. 특히 solution이 유일한 것과 $ker(L_A)=\{0\}$인 것은 동치다.

## 다음 문서에서 필요한 질문

Row reduction은 주어진 $b$에 대해 system이 consistent한지 판정한다. 그러나 어떤 $b$들이 처음부터 $Ax$ 형태로 만들어질 수 있는지, pivot column이 왜 그 집합의 basis를 주는지, row와 column에서 얻은 pivot 수가 왜 같은지는 아직 설명하지 않았다. 이러한 공간적 의미는 [Column Space, Row Space, and Rank](<22 Column Space, Row Space, and Rank.md>)에서 다룬다.

## 관련 문서

- [Kernel](<11 Kernel.md>)
- [Image](<12 Image.md>)
- [Matrix Representation](<20 Matrix Representation.md>)
- [Column Space, Row Space, and Rank](<22 Column Space, Row Space, and Rank.md>)
- [Eigenvector & Eigenvalue & EigenSpace](<24 Eigenvector & Eigenvalue & EigenSpace.md>)
