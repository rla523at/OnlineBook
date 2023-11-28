# Image
## 정의
Vector space $V,W / \mathbb F$와 $T \in L(V,W)$가 있다고 하자.

이 떄, 다음과 같이 정의된 집합을 $T$의 image이라고 한다.

$$ \text{img}(T) := \{ T(v) \in W \enspace | \enspace v \in V \} $$

$\text{img}(T)$는 $T(V)$로 쓰기도 한다.

### 명제1
Vector space $V,W/\F$과 $T \in L(V,W)$가 있을 때, 다음을 증명하여라.

$$ \img(T) \text{ is a subspace of } W $$

**Proof**

$\img(T)$는 $W$의 subset임으로 연산에 닫혀있음을 보이면 충분하다.

$\img(T)$의 임의의 elements를 $w_1,w_2$라고 하면 다음이 성립한다.

$$ \exist v_1,v_2 \in V \st T(v_i) = w_i   $$

이 떄, $\F$의 임의의 element를 $a$라고 하면 다음이 성립한다.

$$ w_1 + aw_2 = T(v_1) + aT(v_2) = T(v_1 + av_2) \in \img(T) \qed $$

### 명제2(Basis of image)
$n$차원 vector space $V/\F$, vector space $W/\F$와 $T \in L(V; W)$가 있다고 하자.

그리고 $V$의 기저를 $\beta = \Set{\beta_1,\cdots,\beta_n}$라 하고, $\ker(T)$의 기저를 $\gamma = \Set{\gamma_1,\cdots,\gamma_k}$라고 하자.

그러면 Steinitz exchange lemma에 의해서 다음이 성립한다.

$$ \exist \beta' \subseteq \beta \st |\beta'| = n-k \land \span(\gamma\cup\beta') = V$$

이 때, 다음을 증명하여라.

$$ f(\beta') :=  \{ T(\beta'_1),\cdots,T(\beta'_{n-k}) \} \text { is a basis of } \img(T)$$

**Proof**

[linearly independent]  
어떤 $a^1,\cdots,a^{n-k}\in\F$에 대해서 $a^1f(\beta'_1)+\cdots+a^{n-k}f(\beta'_{n-k}) = 0_W$라고 하면 다음이 성립한다.

$$ \begin{aligned} &  a^if(\beta'_{i}) = 0_W \\\implies& f(a^i\beta'_{i}) = 0_W \\\implies& a^i\beta'_{i} \in \ker(T) = \span(\gamma) \\\implies& a^i\beta'_{i} =b^j\gamma_j \\\implies& a^1\beta'_1+\cdots+a^{n-k}\beta'_{n-k} -b^1\gamma_1-\cdots-b^k\gamma_k = 0_V \end{aligned} $$

이 때, $\beta' \cup \gamma$은 basis임으로 다음이 성립한다.

$$ a^1=\cdots=a^{n-k}=b^1=\cdots=b^k=0_\F $$

따라서, $f(\beta')$은 linearly independent set이다. $\qed$

[$\img(f) = \span(f(\beta'))$]  
-[$\subseteq$]  
$\img(f)$의 임의의 element를 $w$라고 하면 다음이 성립한다.

$$ \exist v \in V \st f(v) = w $$

이 떄, $\gamma \cup \beta'$이 basis임으로 어떤 $a^1,\cdots,a^{n-k},b^1,\cdots,b^k \in \F$에 대해서 다음이 성립한다.

$$ v = a^1\beta'_1+\cdots+a^{n-k}\beta'_{n-k}+b^1\gamma_1+\cdots+b^k\gamma_k $$

따라서, 다음이 성립한다.

$$ \begin{aligned} w &= f(a^1\beta'_1+\cdots+a^{n-k}\beta'_{n-k}+b^1\gamma_1+\cdots+b^k\gamma_k) \\&= a^1f(\beta'_1)+\cdots+a^{n-k}f(\beta'_{n-k}) \\&\in \span(f(\beta'))\end{aligned} $$  

$\img(f)$의 임의의 element에 대해 위가 성립함으로 다음이 성립한다.

$$ \img(f) \subseteq \span(f(\beta')) \qed $$

-[$\supseteq$]  
$\span(f(\beta'))$의 임의의 element를 $w$라고 하면 다음이 성립한다.

$$ \begin{aligned} w &= a^1f(\beta'_1)+\cdots+a^{n-k}f(\beta'_{n-k}) \\&= f(a^1\beta'_1+\cdots+a^{n-k}\beta'_{n-k}) \end{aligned} $$

이 때, $a^1\beta'_1+\cdots+a^{n-k}\beta'_{n-k} \in V$임으로 다음이 성립한다.

$$ w \in \img(f) $$

$\span(f(\beta'))$의 임의의 element에 대해 위가 성립함으로 다음이 성립한다.

$$ \span(f(\beta')) \subseteq \img(f) \qed $$

[결론]  
$f(\beta')$는 linearly independent set이면서 동시에 $\img(f)$의 generating set임으로 다음이 성립한다.

$$ f(\beta') \text{ is a basis of } \img(f) \qed $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Rank%E2%80%93nullity_theorem)  

#### 따름정리2.1
$n$차원 vector space $V/\F$와 vector space $W/\F$ 그리고 $T \in L(V;W)$가 있다고 하자.

$\beta$가 $V$의 basis이고 $T$가 injective일 때, 다음을 증명하여라.

$$ T(\beta) \text{ is an basis of } \img(T) $$

**Proof**

$T$가 injective임으로 $\ker(T)$의 basis를 $\gamma$라고 하면 다음이 성립한다.

$$ \gamma = \empty $$

따라서, Steinitz exchange lemma에 의해 다음이 성립한다.

$$ \exist \beta' \subseteq \beta \st \begin{gathered} |\beta'| = |\beta| \\ \span(\beta') = V \end{gathered}  $$

$\beta$의 subset중 cardinality가 같은 subset은 자기 자신밖에 없음으로 $\beta' = \beta$이며 명제2에 의해 $T(\beta')$은 $\img(T)$의 basis임으로 다음이 성립한다.

$$ T(\beta) \text{ is an basis of } \img(T) \qed $$

## Rank
Vector space $V,W / \mathbb F$와 $T \in L(V,W)$가 있다고 하자.

이 때, $W$의 subspace $\img(T)$의 dimension을 $\rank(T)$ 라고 한다.

$$ \rank(T) := \dim(\img(T)) $$

### 명제1(Rank-nullity Theorem)
$n$차원 vector space $V/\F$, vector space $W/\F$와 $T \in L(V; W)$가 있다고 하자.

이 때 다음을 증명하여라.

$$ \text{rank}(T) = \dim(V) - \text{nullity}(T) $$

**Proof1**

추상대수학에서 1st homorphism theorem을 통해 군 $G,H$와 group homomorphism인 $f : G \rightarrow H$가 있을 때, $G / \ker(f) \cong \text{img}(f)$임을 보였다. 이를 벡터공간으로 확장하면 다음과 같다.

벡터공간 $V,W / \mathbb F$와, $T \in L(V,W)$가 있을 때, $V / \ker(T) \cong \text{img}(T)$이다. 이를 통해 $\dim(V / \ker(T)) = \dim(\text{img}(T))$이고 정리하면 $\text{rank}(T) = \dim(V) - \text{nullity}(T)$이다.

**Proof2**

$V$의 기저를 $\beta = \Set{\beta_1,\cdots,\beta_n}$라 하고, $\ker(T)$의 기저를 $\gamma = \Set{\gamma_1,\cdots,\gamma_k}$라고 하자. 

그러면 Steinitz exchange lemma에 의해서 다음이 성립한다.

$$ \exist \beta' \subseteq \beta \st |\beta'| = n-k \land \span(\gamma\cup\beta') = V$$

이 떄, $f(\beta')$은 $\img(T)$의 basis임으로 다음이 성립한다.

$$\text{rank}(T) = \dim(V) - \text{nullity}(T) \qed $$

#### 참고
dimension theorem은 Steinitz exchange lemma에 따른 자명한 결과이다.

#### 따름명제1.1
유한 차원 vector space $V,W / \mathbb F$과 $T \in L(V; W)$가 있을 때, 다음을 증명하여라.

$$ \dim(V) = \dim(W) \land \ker(T) = \{ 0_V \} \iff T \text{ is bijective} $$

**Proof**

[$\implies$]  
-[injective]  
$\ker(T) = \{ 0_V \}$임으로 $T$는 injective이다. $\qed$

-[surjective]  
$\dim(V) = \dim(W)$임으로 Dimension Theorem에 의해 다음이 성립한다.

$$ \dim(W) = \dim(V) = \text{nullity}(T) + \text{rank}(T) = \text{rank}(T) = \dim(\text{img}(T)) $$

$\img(T)$는 $W$의 subspace이고 $\dim(\text{img}(T)) = \dim(W)$임으로 다음이 성립한다.

$$ \text{img}(T) = W $$

따라서, $W$의 임의의 element를 $w$라고 하면 다음이 성립한다.

$$ \exist v \in V \st T(v) = w \qed $$

[$\impliedby$]  
$T$가 injective임으로 다음이 성립한다.

$$ \ker(T) = \Set{0_V} \implies \nullity(T) = 0 $$

따라서, dimension theorem에 의해 다음이 성립한다.

$$ \begin{aligned} \dim(V) &= \rank(T) \\&= \dim(\img(T)) \end{aligned} $$

$T$가 surjective임으로 다음이 성립한다.

$$ \img(T) = W $$

따라서, 다음이 성립한다.

$$ \dim(V) = \dim(W) \qed $$

### 명제2
$n$차원 vector space $V,W/\F$와 $T \in L(V;W)$가 있다고 하자.

$V$의 basis가 $\beta$다음을 증명하여라.

$$ T \text{ is bijective} \implies T(\beta) \text{ is an basis of } W $$

**Proof**

$T$가 injective임으로 다음이 성립한다.

$$ T(\beta) \text{ is an basis of } \img(T) $$

$T$가 surjective임으로 다음이 성립한다.

$$ W = \img(T) $$

따라서 다음이 성립한다.

$$ T(\beta) \text{ is an basis of } W \qed $$