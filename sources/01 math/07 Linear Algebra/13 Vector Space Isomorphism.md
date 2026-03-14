# Vector Space Isomorphism
## 정의
vetor space $V,W/\F$과 $\Phi \in L(V;W)$가 있다고 하자.

$\Phi^{-1}:W \rightarrow V \in L(W;V)$면 $\Phi$를 `벡터 공간 동형 사상(vector space isomorphism)`이라고 한다.

### 명제1  
유한 차원 벡터 공간 $V,W/\F$와 $\Phi \in L(V,W)$가 있을 때 다음을 증명하여라.  

$$\Phi \text{ is bijective} \iff \Phi \text{ is a vectorspace isomorphism }$$

**Proof**  

[$\implies$]  
$W$의 임의의 element를 $w_1,w_2$라고 하자.

그러면 전제에 의해 다음이 성립한다.

$$ \exist! v_1,v_2 \in V \st \Phi^{-1}(w_1)=v_1, \Phi^{-1}(w_2)=v_2 $$

이 때, $\Phi$는 linear map임으로 다음이 성립한다.

$$\begin{aligned} \Phi(av_1 + v_2) &= a\Phi(v_1) + \Phi(v_2) \\&=  aw_1 + w_2 \end{aligned} $$

따라서, 다음이 성립한다.

$$ \Phi^{-1}(aw_1+w_2) = av_1 +v_2 = a\Phi^{-1}(w_1) + \Phi^{-1}(w_2) $$

$\Phi^{-1}\in(L(W;V)$임으로 다음이 성립한다.

$$\Phi \text{ is a vectorspace isomorphism} \qed $$

[$\impliedby$]  
정의에 의해 자명하다. $\qed$

### 명제2
$n$차원 vector space $V,W/\F$와 vector space isomorphism $f$가 있다고 하자.

$V$의 기저 $\beta = \{ \beta_1, \cdots, \beta_n \}$가 있을 때, 다음을 증명하여라.

$$ f(\beta) :=  \{ \Phi(\beta_1),\cdots,\Phi(\beta_n) \} \text { is a basis of } W$$

**Proof**  

$f$가 injective임으로 다음이 성립한다.

$$ \ker(T) = \Set{0_V} $$

따라서, $\img(f)$의 성질에 의해 $f(\beta)$는 $\img(f)$의 basis이다.

이 떄, $f$가 surjective임으로 다음이 성립한다.

$$ W = \img(f) $$

따라서, 다음이 성립한다.

$$ f(\beta) \text { is a basis of } W \qed $$

## Vectorspace Isomorphic
vector space $V,W/ \F$와 vector space isomorphism $\Phi:V \rightarrow W$가 있다고 하자.

이 떄, $V$와 $W$를 `벡터 공간 동형(vector space isomorphic)`이라고 하고 $V \overset{\Phi \;}{\cong} W$ 또는 $V \cong W$라고 표기한다.

### 명제1
유한 차원 벡터 공간 $V,W/F$가 있을 때, 다음을 증명하여라.

$$V \cong W \iff \dim(V)=\dim(W)$$

**Proof**  

[$\implies$]  
전제에 의해 bijective linear map $T$가 존재한다.

$T$가 bijective임으로 다음이 성립한다.

$$ \begin{gathered} \ker(T) = \Set{0_V} \\ \dim(V)=\dim(W) \end{gathered}  \qed $$

[$\impliedby$]  
$T \in L(V;W)$가 $\nullity(T) = 0$이면 전제에 의해 $T$는 bijective이고 따라서 $T$는 vector space isomorphism이고 $V,W$는 vector space isomorphic하다.

이 때, 중요한점은 $\nullity(T) = 0$를 만족하는 $T \in L(V;W)$가 존재하는지이다.

[one of example]  
$\beta = \Set{\beta_1,\cdots,\beta_n}, \gamma= \Set{\gamma_1,\cdots,\gamma_n}$를 $V,W$의 basis라고 하자.

이 떄, 함수 $T$를 다음과 같이 정의하자.
$$T:V \rightarrow W \quad s.t \quad a^i\beta_i \mapsto a^i\gamma_i$$  

-[$T \in L(V,W)$]  
$v_1 = a^i\beta_i, v_2 = b^i\beta_i \in V, c \in F$에 대해,

$$ \begin{aligned} T(cv_1+v_2) &= (ca^i + b^i)\gamma_i \\ &= ca^i\gamma_i + b^i\gamma_i \\ &= cT(v_1) + T(v_2) \qed \end{aligned} $$

-[$\nullity(T) = 0$]  
$\ker(T) = \{ 0_V \}$임으로 $\nullity(T) = 0$이다. $\qed$

### Remark
차원을 통해 Vector space를 분류할 수 있다. 즉, 차원이 같으면 벡터 공간 동형이고 차원이 다르면 벡터 공간 동형이 아닌 경우로 분류가 된다.

벡터 공간 동형은 집합론적 관점에서 동치 관계를 만족한다. 따라서 분할을 갖게 되며 차원이 같은 경우에는 동일한 그룹으로 볼 수 있다는것을 의미한다.

이 때, $\F^n = \F\times \cdots \times \F$은 $\dim(\F^n)=n$임으로 임의의 $n$ 차원 벡터 공간 $V$와 $V \cong \F^n$이다. 이를통해 매우 추상적인 벡터 공간 $V$와 실질적인 $\F^n$을 동일한 그룹으로 볼 수 있다. 그리고 이 관계를 통해 추상적인 $V$를 편하게 서술할 수 있는 관점을 제공 받는다.