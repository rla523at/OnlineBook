# Trace
$A \in M_{nn}(\R)$이라 하자.

$\tr(A)$는 다음과 같이 정의된다.
$$ \tr(A) := A_{ij}\delta_{ij} = A_{ii} $$


### 명제1
$A,B \in M_{nn}(\R)$이라 하자.

이 떄, 다음을 증명하여라.
$$ \tr(AB) = \sum_{i=1}^n \sum_{j=1}^n A_{ij}B_{ji} $$

**Proof**

직접 계산하면 동일함을 알 수 있다. $\qed$

### 명제2
$M,N \in M_{nn}(\mathbb F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.
$$ \text{tr}(M) = \text{tr}(N) $$

**Proof**

$$ \mathrm{tr}(M) = \mathrm{tr}(B^{-1}NB) = \mathrm{tr}(BB^{-1}N) = \mathrm{tr}(N) \quad (\because \mathrm{tr}(M_1M_2) = \mathrm{tr}(M_2M_1)) \quad {_\blacksquare}  $$

### 명제3
$M,N \in M_{nn}(\mathbb F)$가 있을 때, $M \sim N$라 하자.

이 때, 다음을 증명하여라.
$$ \text{tr}(M^2) = \text{tr}(N^2) $$

**Proof**

$$ \mathrm{tr}(M^2) = \mathrm{tr}(B^{-1}NBB^{-1}NB) = \mathrm{tr}(B^{-1}N^2B) = \mathrm{tr}(N^2)  \quad {_\blacksquare}  $$

### 명제4
$A \in M_{nn}(\R)$이라 하자.

$A$가 symmetric metrix일 때, 다음을 증명하여라.
$$ A : A = \tr(A^2) $$

**Proof**

$$ \begin{aligned} A : A &= A_{ij}A_{ij} \\&= A_{ij}A_{ji} \\&= \tr(A^2) \qed \end{aligned} $$

#### 따름명제
$A \in M_{nn}(\R)$가 symmetric metrix라 하자.

$A \sim B$일 때, 다음을 증명하여라.
$$ A : A = B : B $$

**Proof**

$$ \begin{aligned} A : A &= \tr(A^2) \\ &= \tr(B^2) \\ &= B : B\qed \end{aligned} $$

### 명제5
$A,B \in M_{nn}(\R)$이라 하자.

이 떄, 다음을 증명하여라.
$$ \tr(AB) = \tr(BA) $$

**Proof**

$$ \begin{aligned} \tr(AB) &= \sum_{i=1}^n \sum_{j=1}^n A_{ij}B_{ji} \\ &= \sum_{j=1}^n \sum_{i=1}^n B_{ji}A_{ij} \\&= \tr(BA) \qed \end{aligned} $$



> Referece  
> [Wiki - Trace Notes1](https://en.wikipedia.org/wiki/Trace_(linear_algebra))