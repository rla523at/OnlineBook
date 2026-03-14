# Basis
## 정의
Vector space $V/\F$와 $V$의 subset $\beta$가 있다고 하자.

$\beta$가 다음을 만족할 떄, $\beta$를 $V$의 `기저(basis)`라고 한다.

$$ \beta \text{ is linearly independent generating set of } V $$

### 명제1(Existence of basis)
모든 vector space가 basis를 갖음을 증명하여라.

**Proof** 

임의의 vector space를 $V/\F$라고 하자.

$V = \{ 0_V \}$인 경우 자명하게 $\beta = \emptyset$이다.

$V \neq \{ 0_V \}$인 경우 항상 $V$의 linearly independent set이 존재한다.

$V$의 linearly independent set을 $S$라고 하면 보조명제1,2에 의해 $V = \span(M)$인 linearly independent set $M$이 존재한다.

따라서 $M$은 $V/\F$의 기저가 되며 임의의 vector space가 basis를 갖음으로 모든 vector space가 basis를 갖는다. $\qed$

#### 보조명제1
Vector space $V/\F$와 $V$의 nonempty linearly independent subset $S$가 있다고 하자.

그리고 집합 $F$를 다음과 같이 정의하자.

$$F := \{ A \subseteq V | S \subseteq A \land A \text{ is linearly independent}\}$$ 

이 때, 다음을 증명하여라.

$$ (F,\subseteq) \text{ has maximal element} $$

**Proof**  

$S \in F$임으로 다음이 성립한다.

$$ F \neq \empty $$

따라서, set inclusion의 성질에 의해 다음이 성립한다.

$$ (F,\subseteq) \text{ is a poset} $$

$(F,\subseteq)$의 임의의 chain $C$라고 했을 떄, $M = \bigcup C$라고 하자.

$M$의 임의의 $n$개의 element를 $v_1,\cdots,v_n$이라하고 $\F$의 임의의 $n$개의 element를 $a_1,\cdots,a_n$이라고 할 때, $M$의 정의에 의해 다음이 성립한다.

$$ \exist C_i \in C \st v_i \in C_i \enspace i=1,\cdots,n $$

그리고 $C$는 전순서집합임으로 다음이 성립한다.

$$ \begin{aligned} & \exist C_M \in C \st C_1,\cdots,C_n \subseteq C_M \\ \implies& v_1,\cdots,v_n \in C_M \end{aligned} $$

이 떄, $C_M \in F$임으로 다음이 성립한다.

$$ \begin{aligned} & \sum_{i=1}^n a_iv_i = 0_V \implies a_1\cdots,a_n = 0_\F \\\implies& M \text{ is an linearly independent set } \\\implies& M \in F \end{aligned} $$

따라서, $M$은 $C$의 upper bound이고 임의의 chain이 upper bound를 갖음으로 Zorn's lemma에 의해 다음이 성립한다.

$$ (F,\subseteq) \text{ has maximal element} \qed $$

#### 보조명제2
Vector space $V/\F$와 $V$의 linearly independent subset $S$가 있다고 하자.

그리고 집합 $F$를 다음과 같이 정의하자.

$$F := \{ A \subseteq V | S \subseteq A \land A \text{ is linearly independent}\}$$ 

$F$의 maximal element를 $M$이라고 할 때, 다음을 증명하여라.

$$ V = \span(M) $$

**Proof**

다음을 가정하자.

$$ V \neq \span(M) $$

그러면 다음이 성립한다.

$$ \begin{aligned} & V - \span(M) \neq \empty \\ \iff \enspace & \exist v \in V - \span(M) \end{aligned} $$

위를 만족하는 $v$가 있을 떄, linearly independent set의 성질에 의해 다음이 성립한다.

$$ M \cup \Set{v} \text{ is linearly independent set } $$

또한 $S \subseteq M$임으로 다음이 성립한다. 

$$ S \subseteq (M \cup \Set{v}) $$

따라서, 다음이 성립한다.

$$ M \cup \{v\} \in F \enspace \land \enspace M \subseteq (M \cup \{v\}) $$

하지만, 이는 $M$이 maximal element라는 전제에 모순됨으로 proof by contradiction에 의해 다음이 성립한다.

$$ V = \span(M) \qed $$

#### 참고
1. 기저는 maximal linearly independent set이다.
2. 기저의 존재성은 Zorn's lemma에 의존한다.
3. 공리(Zorn's lemma)에 의해 기저의 존재성을 보장했을 뿐 기저가 무엇인지는 알 수 없다.
4. 기저의 유일성은 보장되지 않기 때문에 기저는 여러개일 수 있다.

### 명제2(Dimension)
Vector space $V/\F$와 basis $\beta_1,\beta_2$가 있다고 하자.

$\beta_1,\beta_2$가 finite set일 때, 다음을 증명하여라.

$$ |\beta_1| = |\beta_2| $$

**proof**

$\beta_1$을 $V$의 finite linearly independent set, $\beta_2$를 $V$의 finite generating set이라고 하자.

그러면 steinitz exchange lemma에 의해 다음이 성립한다.

$$ |\beta_1| \le |\beta_2| $$

이번에는 반대로 $\beta_1$을 $V$의 finite generating set, $\beta_2$를 $V$의 finite linearly independent set이라고 하자.

그러면 steinitz exchange lemma에 의해 다음이 성립한다.

$$ |\beta_2| \le |\beta_1| $$

따라서 다음이 성립한다.

$$ |\beta_1| = |\beta_2| \qed $$

#### 참고
basis의 cardinality는 항상 동일하기 때문에, basis의 cardinality를 vector space의 `차원(dimension)`이라고 한다.

### 명제3
Vector space $V/\F$가 있다고 하자.

$G$가 $V$의 generating set일 때, 다음을 증명하여라.

$$ \exist G' \subseteq G \st G' \text{ is a basis of } V $$

**Proof**

$G$의 $0_V$가 아닌 임의의 element를 $v$라고 하고 $S = \Set{v}$라고 하자.

그러면, $S$은 linearly independent set이다.

$S$를 포함하는 $G$ linear inedependent set의 family를 $F$라고 하면 $(F,\subseteq)$는 POSET이고 $F$의 임의의 chain은 upper bound를 갖는걸 basis의 존재정리를 증명할 때와 같은 방식으로 증명할 수 있다.

따라서 Zorn's lemma에 의해 $F$는 maximal element를 갖으며 이를 $G'$이라고 하자.

$G'$의 maximality에 의해 다음이 성립한다.

$$ G \subseteq \span(G') $$

이 떄, $G$가 generating set임으로 span의 성질에 의해 다음이 성립한다.

$$ \span(G') = V $$

그럼으로 $G'$는 $V$의 basis가 되고 다음이 성립한다.

$$ \exist G' \subseteq G \st G' \text{ is a basis of } V \qed $$

#### 따름명제3.1
$n$차원 vector space $V/\F$가 있다고 하자.

$G$가 $V$의 generating set일 때, 다음을 증명하여라.

$$ |G| = n \implies G \text{ is a basis of } V $$

**Proof**

$G$가 genrating set임으로 명제3에 의해 다음이 성립한다.

$$ \exist G' \subseteq G \st G' \text{ is a basis of } V $$

이 떄, $V$가 $n$차원임으로 다음이 성립한다.

$$ |G'| = n $$

전제에 의해 $|G|$의 subset중 cardinality가 $n$인 subset은 $G$밖에 없음으로 다음이 성립한다.

$$ |G| = n \implies G \text{ is a basis of } V \qed $$

### 명제4
$n$차원 vector space $V/\F$가 있다고 하자.

$V$의 linearly independent set을 $S$라고 할 떄, 다음을 증명하여라.

$$ |S| = n \implies S \text{ is a basis of } V $$

**Proof**

$V$의 임의의 basis를 $\beta$라고 하자.

$\beta$는 generating set임으로 Steinitz exchange lemma에 의해 다음이 성립한다.

$$ \exist \beta' \subseteq \beta \st |\beta'| = 0 \land \span(S\cup\beta) = V$$

따라서 $\beta' = \empty$임으로 $\span(S)=V$이고 다음이 성립한다. 

$$ |S| = n \implies S \text{ is a basis of } V \qed $$

#### 따름명제4.1
$n$차원 vector space $V/\F$가 있다고 하자.

$V$의 subspace $U$가 있을 때, 다음을 증명하여라.

$$ \dim(U) = n \implies U = V $$

**Proof**

$U$의 basis를 $\beta$라고 하자.

$U$는 $V$의 subspace임으로 다음이 성립한다.

$$ \beta \text{ is an linearly independent set of } V $$

이 떄, $|\beta| = n$임으로 명제4에 의해 다음이 성립한다.

$$ \beta \text{ is a basis of } V $$

따라서, 다음이 성립한다.

$$ U = V \qed $$

### 명제5
Vector space $V / \mathbb F$가 있다고 하자.

$S_1, S_2$가 $V$의 subspace이고 각각의 기저가 $\beta_1,\beta_2$일 때, 다음을 증명하여라.

$$ S_1 \cap S_2 = \{ 0_V \} \implies \beta_1 \cap \beta_2 = \empty$$

**Proof**

다음을 가정하자.

$$ \beta_1 \cap \beta_2 \neq \empty $$

$\beta \in \beta_1 \cap \beta_2$라 하면 다음이 성립한다.
$$ \begin{aligned} & \beta \in \text{span}(\beta_1) \land \beta \in \text{span}(\beta_2) \\ \implies \enspace & \beta \in S_1 \cap S_2 \\ \implies \enspace & \beta = 0_V \end{aligned}  $$

하지만, basis는 linearly independent set임으로 $0_V$를 갖을 수 없음으로 모순이 발생한다. 

따라서, proof by contradiction에 의해 다음이 성립한다. 

$$ S_1 \cap S_2 = \{ 0_V \} \implies \beta_1 \cap \beta_2 = \empty \qed $$

### 명제6
$n$차원 vector space $V / \mathbb F$와 기저 $\beta = \Set{\beta_1,\cdots,\beta_n}$가 있다고 하자.

$U = \text{span}(\{\beta_1, \cdots, \beta_k \}), W = \text{span}(\{ \beta_{k+1}, \cdots \beta_n \})$라 할 때, 다음을 증명하여라.
$$V = U \oplus W$$

**Proof**

[$V \subseteq U \oplus W$]  
$$ \begin{aligned} & v \in V \\ \Rightarrow \enspace & v = a_1 \beta_1 + \cdots + a_k \beta_k + a_{k+1} \beta_{k+1} + \cdots + a_n \beta_n \\ \Rightarrow \enspace & v = u + w \\ \Rightarrow \enspace & v \in U \oplus W  \quad {_\blacksquare} \end{aligned}$$

[$U \oplus W \subseteq V$]  
$$ \begin{aligned} & x \in U \oplus W \\ \Rightarrow \enspace & x = a_1 \beta_1 + \cdots + a_k \beta_k + a_{k+1} \beta_{k+1} + \cdots + a_n \beta_n \\ \Rightarrow \enspace & x \in V  \quad {_\blacksquare} \end{aligned}$$


### 명제7
$n$차원 vector space $V / \mathbb F$가 있다고하자.

$W_1, \cdots, W_n \le V$이고 $V = W_1 \oplus \cdots \oplus W_n$일 때, $W_i$의 기저를 $\omega_i$라 하자.

이 때, 다음을 증명하여라.
$$ \bigcup_{i=1}^k \omega_i \text{ is a basis of } V$$

**Proof**

[linearly independent]  
다음과 같이 가정하자.
$$\{ \beta_1, \cdots, \beta_m \} = \bigcup_{i=1}^k \omega_i$$

명제5에 의해 $i \neq j$면, $\omega_i \cap \omega_j = \empty$임으로, 어떤 $w_i \in \text{span}(\omega_i)$에 대해  다음이 성립한다.
$$ \begin{aligned} & a^1\beta_1 + \cdots + a^m\beta_m = 0_V \\ \Rightarrow \enspace & w_1 + \cdots + w_k  = 0_V \end{aligned}  $$

이 떄, $w_1$과 $w_2+ \cdots + w_k$는 서로 다른 vector space에 있기 때문에 덧셈에 대한 역원이 될 수 없고 따라서 $w_1 = (w_2 + \cdots + w_k) = 0_V$다. 이를 반복하면 $w_i = 0$임을 알 수 있다.

즉, $a^1, \cdots, a^m = 0_{\mathbb F}$이다. $\quad {_\blacksquare}$

[$\text{span}(\bigcup_{i=1}^k \omega_i) = V$]  

명제5에 의해 $i \neq j$면, $\omega_i \cap \omega_j = \empty$임으로, 다음이 성립한다.
$$ \text{span}(\bigcup_{i=1}^k \omega_i) = \bigoplus_{i=1}^k \text{span}(\omega_i) = \bigoplus_{i=1}^k W_i = V \quad {_\blacksquare} $$


### 명제8
Vector space $V/\F$와 $V$의 subset $S$가 있다고 하자.

$V$의 모든 element가 $S$의 유한한 vector들의 lienar combination으로 유일하게 표현된다고 할 떄, 다음을 증명하여라.

$$ S \text{ is an basis of } V $$

**Proof**

[$\span(S)=V$]  
$V$의 모든 element가 $S$의 유한한 vector들의 linear combination으로 표현됨으로 자명하게 성립한다.$\qed$

[linearly independent]  
$S$의 임의의 $n$개의 element들을 $v_1,\cdots,v_n$이라고 할 때, 다음을 생각해보자.

$$ a^1v_1 + \cdots + a^nv_n = 0_V $$

이 때, 다음을 가정하자.

$$ \exist a^i \st a^i \neq 0_\F $$

그러면 $0_V$는 다음 두가지로 표현된다.

$$ 0_V = a^iv_i = 0_\F a^1 + \cdots $$

이는 유일한 표현을 갖는다는 사실에 모순임으로 proof by contradiction에 의해 다음이 성립한다.

$$ \forall a^i = 0_\F $$

따라서, 다음이 성립한다.

$$ S \text{ is an linearly independent set} \qed $$