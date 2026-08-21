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

$Ax=b$라는 표기는 compact하지만, 각 equation이 다른 equation에는 없는 새로운 조건을 주는지, 같은 조건을 반복하는지, 또는 서로 모순되는지는 바로 드러나지 않는다. 이러한 관계에 따라 solution의 존재 여부와 자유롭게 선택할 수 있는 variable이 달라진다.

예를 들어 $\alpha,\beta,\lambda\in\mathbb F$에 대해 다음 system을 생각하자.

$$
\begin{aligned}
x_1+x_2&=\alpha,\\
x_1+\lambda x_2&=\beta
\end{aligned}
$$

두 번째 equation에서 첫 번째 equation을 빼면

$$
(\lambda-1)x_2=\beta-\alpha
$$

를 얻는다. 이 변형은 첫 번째 equation을 다시 더하면 원래 두 번째 equation으로 되돌릴 수 있으므로, 변형 전후의 system은 같은 solution set을 갖는다.

- $\lambda\ne1$이면 $x_2=(\beta-\alpha)/(\lambda-1)$로 결정되고, 첫 번째 equation에서 $x_1$도 결정되므로 solution이 유일하다.
- $\lambda=1$이고 $\beta\ne\alpha$이면 $0=\beta-\alpha$라는 모순이 생기므로 solution이 없다.
- $\lambda=1$이고 $\beta=\alpha$이면 두 번째 equation은 $0=0$이 되어 새로운 조건을 주지 않는다. 따라서 $x_2$를 자유롭게 선택하고 $x_1=\alpha-x_2$로 정할 수 있다.

따라서 해의 구조를 알아보려면 equation들을 solution set을 보존하는 되돌릴 수 있는 방식으로 변형하여 중복과 모순을 드러내고, 각 variable을 결정하는 조건을 분리해야 한다. 이를 coefficient와 right-hand side만으로 체계적으로 수행하기 위해 augmented matrix를 도입한다.

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

Row operation으로 같은 solution set을 나타내는 system을 여러 형태로 바꿀 수 있다. 이때 목적은 단순히 $0$을 많이 만드는 것이 아니다. 아래쪽 equation부터 읽으면서 모순이 있는지 확인하고, 모순이 없다면 일부 variable의 값을 선택한 뒤 나머지 variable을 하나씩 결정할 수 있는 형태로 system을 변형하는 것이다.

예를 들어 row operation 결과가 다음 system이라고 하자.

$$
\begin{aligned}
p_1x_1+a_{12}x_2+a_{13}x_3+a_{14}x_4&=c_1,\\
p_2x_3+a_{24}x_4&=c_2,\\
0&=0
\end{aligned}
\qquad
p_1,p_2\ne0
$$

마지막 equation은 어떤 variable에도 새로운 조건을 주지 않는다. 그 위의 equation에서 $x_4$를 선택하면

$$
x_3=\frac{c_2-a_{24}x_4}{p_2}
$$

로 $x_3$를 결정할 수 있다. 이어서 $x_2$를 선택하고 구한 $x_3$를 첫 번째 equation에 대입한 뒤 $x_2,x_4$에 관한 항을 정리하면

$$
\begin{aligned}
x_1
&=
\frac{c_1-a_{12}x_2-a_{13}\left(\dfrac{c_2-a_{24}x_4}{p_2}\right)-a_{14}x_4}{p_1}\\
&=
\frac{c_1}{p_1}
-\frac{a_{13}c_2}{p_1p_2}
-\frac{a_{12}}{p_1}x_2
-\left(
\frac{a_{14}}{p_1}
-\frac{a_{13}a_{24}}{p_1p_2}
\right)x_4
\end{aligned}
$$

로 $x_1$도 결정할 수 있다. 따라서 $x_2,x_4$의 값은 자유롭게 선택하지만, $x_3,x_1$의 값은 아래쪽 nonzero equation부터 위로 올라가며 차례로 결정된다.

이 계산이 가능한 이유는 아래쪽 equation에서 왼쪽의 variable들이 이미 사라져 있기 때문이다. 아래쪽 nonzero equation으로 갈수록 nonzero coefficient가 처음 나타나는 variable이 오른쪽으로 이동하면, 오른쪽에 있는 variable의 값을 선택하거나 아래에서 이미 구한 뒤 그 equation에서 가장 왼쪽에 남은 variable을 결정할 수 있다. 새로운 조건을 주지 않는 zero equation은 nonzero equation의 계단 구조를 한눈에 읽을 수 있도록 맨 아래에 모은다.

위 system의 augmented matrix는

$$
\left[
\begin{array}{cccc|c}
p_1&a_{12}&a_{13}&a_{14}&c_1\\
0&0&p_2&a_{24}&c_2\\
0&0&0&0&0
\end{array}
\right]
$$

이다. 각 nonzero row에서 가장 왼쪽에 있는 nonzero entry는 그 row가 어느 column에서 시작하는지를 나타낸다. 이 entry를 그 row의 `leading entry`라고 한다. 위 matrix에서는 $p_1$과 $p_2$가 leading entry이고, 아래 row의 leading entry인 $p_2$가 $p_1$보다 오른쪽에 있다.

이처럼 leading entry가 아래 row로 갈수록 오른쪽으로 이동하여 계단 모양을 이루고 zero row가 맨 아래에 놓인 matrix를 `row echelon form`, 줄여서 REF라고 한다. 정확히는 다음 조건을 만족하는 matrix다.

1. Zero row는 모든 nonzero row보다 아래에 있다.
2. 아래쪽 nonzero row의 leading entry는 바로 위쪽 row의 leading entry보다 오른쪽에 있다.
3. 각 leading entry 아래의 entry는 모두 $0$이다.

Leading entry를 row에서 가장 왼쪽에 있는 nonzero entry로 정했으므로, 2번 조건을 반복해 적용하면 각 아래 row에서 이전 leading entry가 놓인 column까지 모두 $0$이다. 3번 조건은 row elimination이 leading entry 아래를 제거한다는 동작을 명시적으로 드러낸다.

REF의 leading entry가 있는 위치를 `pivot position`이라고 하고, 그 위치의 entry를 `pivot`이라고 한다. Pivot이 놓인 coefficient column에 대응하는 unknown은 `pivot variable`, pivot이 없는 coefficient column에 대응하는 unknown은 `free variable`이다.

위 예에서 $p_1,p_2$는 pivot이고, $x_1,x_3$는 pivot variable이며 $x_2,x_4$는 free variable이다. System에 모순이 없다면 free variable의 값을 선택한 뒤 pivot variable을 아래에서 위로 결정할 수 있다.

REF만으로도 해를 구할 수 있지만, 각 pivot variable이 free variable에 어떻게 의존하는지를 matrix에서 바로 읽으려면 두 가지 작업이 더 필요하다. 먼저 pivot $p_1,p_2$는 nonzero이기만 하면 되므로, 같은 equation을 나타내는 row도 nonzero scalar를 곱한 정도에 따라 서로 다른 pivot을 가질 수 있다. 또한 첫 번째 equation에 아래쪽 pivot variable인 $x_3$가 남아 있으므로, 두 번째 equation에서 구한 $x_3$를 첫 번째 equation에 대입해야 한다.

$p_1,p_2\ne0$이므로 먼저 $R_1,R_2$를 각각 scale하여 pivot을 $1$로 만든 다음, 마지막 row operation으로 두 번째 pivot 위의 entry를 제거할 수 있다.

$$
R_1\leftarrow \frac{1}{p_1}R_1,
\qquad
R_2\leftarrow \frac{1}{p_2}R_2,
\qquad
R_1\leftarrow R_1-\frac{a_{13}}{p_1}R_2
$$

그 결과 matrix는 다음과 같이 변한다.

$$
\left[
\begin{array}{cccc|c}
1&\dfrac{a_{12}}{p_1}&0&\dfrac{a_{14}}{p_1}-\dfrac{a_{13}a_{24}}{p_1p_2}&\dfrac{c_1}{p_1}-\dfrac{a_{13}c_2}{p_1p_2}\\
0&0&1&\dfrac{a_{24}}{p_2}&\dfrac{c_2}{p_2}\\
0&0&0&0&0
\end{array}
\right].
$$

첫 번째와 두 번째 nonzero row는 각각 다음 equation을 나타낸다.

$$
\begin{aligned}
x_1
+\frac{a_{12}}{p_1}x_2
+\left(
\frac{a_{14}}{p_1}
-\frac{a_{13}a_{24}}{p_1p_2}
\right)x_4
&=
\frac{c_1}{p_1}
-\frac{a_{13}c_2}{p_1p_2},\\
x_3+\frac{a_{24}}{p_2}x_4
&=
\frac{c_2}{p_2}.
\end{aligned}
$$

첫 번째 equation은 앞에서 정리한 $x_1$ 식의 free variable 항을 왼쪽으로 옮긴 식이고, 두 번째 equation은 앞에서 구한 $x_3$ 식의 $x_4$ 항을 왼쪽으로 옮긴 식이다. 따라서 matrix의 각 coefficient와 right-hand side를 앞의 back substitution 결과에 항별로 대응시킬 수 있다.

앞의 두 row operations는 pivot을 $1$로 만들어 row의 임의적인 nonzero scalar 배수를 표준화한다. 마지막 row operation은 두 번째 pivot 위의 entry를 $0$으로 만들어 첫 번째 equation에서 $x_3$를 제거한다. Pivot 아래의 entry는 REF에서 이미 $0$이므로, 이제 각 pivot은 자신의 column에서 유일한 nonzero entry다. 따라서 각 nonzero equation에는 자신의 pivot variable과 free variable만 남고, pivot variable을 아래에서부터 대입하지 않아도 각각의 식을 바로 읽을 수 있다.

이처럼 REF가 다음 조건도 만족하면 `reduced row echelon form`, 줄여서 RREF라고 한다.

1. 모든 pivot은 $1$이다.
2. 각 pivot은 자신의 column에서 유일한 nonzero entry다.

첫 번째 조건은 pivot의 크기에 남아 있는 scalar 배수의 차이를 제거하고, 두 번째 조건은 pivot 위의 entry까지 제거하여 back substitution을 없앤다. 따라서 pivot position과 해의 존재 여부만 판별할 때는 REF로 충분하지만, 해를 free variable로 나타낸 식을 matrix에서 직접 읽을 때는 RREF가 편리하다.

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

$R_2$와 $R_3$를 교환한 뒤 $R_3\leftarrow R_3+2R_2$를 적용하면 다음 REF를 얻는다.

$$
\left[
\begin{array}{cccc|c}
1&2&1&0&3\\
0&0&1&-1&2\\
0&0&0&0&0
\end{array}
\right].
$$

이 상태에서도 두 번째 row에서 $x_3$를 구하여 첫 번째 row에 대입하면 해를 구할 수 있다. 그러나 세 번째 coefficient column의 pivot 위에 $1$이 남아 있어, 첫 번째 row만으로는 $x_1$이 free variable에 어떻게 의존하는지 바로 읽을 수 없다. $R_1\leftarrow R_1-R_2$를 적용하여 이 entry를 제거하면 다음 RREF를 얻는다.

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

을 `homogeneous linear system`이라고 한다. $x=0$을 대입하면

$$
A0=0
$$

이므로 homogeneous system은 항상 trivial solution $x=0$을 갖는다. Trivial solution 이외의 solution이 언제 생기는지 알아보기 위해 free variable이 없는 경우와 있는 경우를 비교해 보자.

### Free variable이 없는 경우

다음 RREF가 나타내는 homogeneous system을 생각하자.

$$
\left[
\begin{array}{cc|c}
1&0&0\\
0&1&0
\end{array}
\right]
$$

두 coefficient column에 모두 pivot이 있으므로 $x_1,x_2$는 모두 pivot variable이고 free variable은 없다. 두 row는

$$
x_1=0,
\qquad
x_2=0
$$

을 나타내므로 solution은 $x=0$뿐이다.

### Free variable이 있는 경우

이번에는 다음 RREF를 생각하자.

$$
\left[
\begin{array}{cc|c}
1&2&0
\end{array}
\right]
$$

두 번째 coefficient column에는 pivot이 없으므로 $x_2$는 free variable이다. $x_2=t$라고 두면

$$
x_1=-2t,
\qquad
x
=
t
\begin{bmatrix}
-2\\1
\end{bmatrix}
$$

이다. $t=0$이면 trivial solution을 얻지만, $t=1$로 선택하면

$$
x=
\begin{bmatrix}
-2\\1
\end{bmatrix}
\ne0
$$

인 nontrivial solution을 얻는다.

Homogeneous system을 row reduction해도 right-hand side의 zero vector는 그대로 유지된다. Free variable이 없으면 모든 variable이 pivot variable이므로 RREF의 각 nonzero row에서 $x_i=0$을 바로 읽을 수 있고, trivial solution만 존재한다. 반대로 free variable이 있으면 그중 하나를 nonzero로 선택하고 pivot variable을 결정할 수 있다. 이때 선택한 free coordinate가 nonzero이므로 얻은 solution도 nontrivial이다.

따라서 homogeneous system에서는 free variable이 없는 것과 trivial solution만 존재하는 것이 동치이고, free variable이 있는 것과 nontrivial solution이 존재하는 것이 동치다.

Matrix가 정의하는 linear map $L_A:x\mapsto Ax$를 생각하면 homogeneous system의 solution set은 [Kernel](<../02 Linear Maps and Isomorphisms/11 Kernel.md>)이다.

$$
\ker(L_A)
=
\{x\in\mathbb F^n\mid Ax=0\}
$$

일반 system $Ax=b$가 solution을 갖는다고 하고, 그 solution set을

$$
S_b
=
\{x\in\mathbb F^n\mid Ax=b\}
$$

라고 하자. Particular solution 하나 $x_p\in S_b$를 고정한다. 먼저 임의의 $x\in S_b$를 선택하고 두 solution의 차이를

$$
z:=x-x_p
$$

라고 두면

$$
Az
=
A(x-x_p)
=
Ax-Ax_p
=
b-b
=0
$$

이므로 $z\in\ker(L_A)$다. 또한 $z=x-x_p$를 다시 정리하면

$$
x=x_p+z
$$

이다. Kernel의 각 원소를 $x_p$에 더해 얻는 집합을

$$
x_p+\ker(L_A)
:=
\{x_p+z\mid z\in\ker(L_A)\}
$$

라고 쓴다. 따라서 $x\in x_p+\ker(L_A)$다. 여기서 $z$는 미리 찾아야 하는 값이 아니라 선택한 $x$와 $x_p$의 차이로 정해진다. 그러므로 임의의 $x\in S_b$마다 이러한 $z$가 반드시 존재하고

$$
S_b\subseteq x_p+\ker(L_A)
$$

이다.

반대로 임의의 $z\in\ker(L_A)$를 선택하면

$$
A(x_p+z)
=
Ax_p+Az
=
b+0
=b
$$

이므로 $x_p+z\in S_b$다. 따라서

$$
x_p+\ker(L_A)\subseteq S_b
$$

이고, 두 포함 관계를 합치면 전체 solution set은

$$
S_b
=
x_p+\ker(L_A)
=
\{x_p+z\mid z\in\ker(L_A)\}
$$

이다. 특히 solution이 유일한 것과 $\ker(L_A)=\{0\}$인 것은 동치다.

## 관련 문서

- [Kernel](<../02 Linear Maps and Isomorphisms/11 Kernel.md>)
- [Image](<../02 Linear Maps and Isomorphisms/12 Image.md>)
- [Matrix Representation](<../03 Matrix Representation and Eigenstructure/20 Matrix Representation.md>)
- [Column Space, Row Space, and Rank](<22 Column Space, Row Space, and Rank.md>)
- [Eigenvector & Eigenvalue & EigenSpace](<../03 Matrix Representation and Eigenstructure/24 Eigenvector & Eigenvalue & EigenSpace.md>)
