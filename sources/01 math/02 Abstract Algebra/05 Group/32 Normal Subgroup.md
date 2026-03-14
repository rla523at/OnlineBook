# Normal Subgroup

## Definition
Group $G$와 subgroup $H$가 있다고 하자.

이 떄, 다음을 만족하는 $H$를 $G$의 `normal subgroup`이라고 한다.

$$ \forall g \in G, \forall h \in H, \enspace ghg^{-1} \in H $$

### Remark
1. $H$가 normal subgroup일 경우, $H \lhd G$라고 표기한다.
2. $G$가 commutative group일 경우, $G$의 모든 subgroup은 normal subgroup이다.
3. $\Set{e}$와 $G$는 항상 normal subgroup이다.
4. Group $G$와 normal subgroup $N$이 있다고 하자. 그러면 $N$의 성질에 의해 $G/N$은 quotient group이 된다. $G/N$은 일반적으로 $G$보다 단순한 구조를 가지고 있으며, 이를 통해 $G$의 복잡한 구조를 이해하는 한가지 좋은 방법을 제공한다.

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/2626414/importance-of-normal-subgroups)

### 명제1
Group $G$와 subgorup $H$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ H \lhd G \iff \forall g \in G, \enspace gH = Hg $$

**Proof**

[$\implies$]  
-[$gH \subseteq Hg$]  
$gH$의 임의의 element를 $gh$라고 하면 전제에 의해 다음이 성립한다.

$$ \exist h_2 \in H \st ghg^{-1} = h_2 \implies gh = h_2g \implies gh \in Hg \qed $$

-[$Hg \subseteq gH$]  
$Hg$의 임의의 element를 $hg$라고 하면 전제에 의해 다음이 성립한다.

$$ \exist h_2 \in H \st g^{-1}hg = h_2 \implies hg = gh_2 \implies hg \in gH \qed $$

[$\impliedby$]  
$G$의 임의의 element를 $g$, $H$의 임의의 element를 $h$라고 하면 전제에 의해 다음이 성립한다.

$$ \exist h_2 \in H \st gh = h_2g \implies ghg^{-1} = h_2 \implies ghg^{-1} \in H \qed $$

### 명제2
Group $G,G'$과 group homomorphism $\phi:G\rightarrow G'$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ \ker(\phi) \lhd G $$

**Proof**

$G$의 임의의 element를 $g$, $\ker(\phi)$의 임의의 element를 $h$라고 하면 다음이 성립한다.

$$ \phi(ghg^{-1}) = \phi(g)\phi(h)\phi(g^{-1}) = e_G \implies ghg^{-1} \in \ker(\phi) \qed $$

#### Remark
$\img(\phi) \lhd G'$이라고 해보자.

이 때, $\phi = id$로 두면 $G'$의 모든 subgroup이 normal subgroup이 된다.

하지만 반례가 있음으로 proof by contradiction에 의해 $\img(\phi)$는 $G'$의 normal subgroup이 아니다.

##### CounterExample1
$M := \Set{m \in M_{22}(\R) | \det(m) \neq 0}$라고 할 때, $(M,\times)$은 group이다.

이 때, $M$의 subgroup $H$을 다음과 같이 정의하자.

$$ H= \Braket{\begin{bmatrix} 1&1\\0&1 \end{bmatrix}} = \Set{\begin{bmatrix} 1&k\\0&1 \end{bmatrix} \mid k \in \Z} $$

그러면 $ m = \begin{bmatrix} 2&1\\1&1 \end{bmatrix} \in M$과 $ h =\begin{bmatrix} 1&1\\0&1 \end{bmatrix} \in H$에 대해 다음이 성립한다.

$$ mhm^{-1} = \begin{bmatrix} -1&1\\-1&3 \end{bmatrix} \notin H $$

따라서, $H$는 normal subgroup이 아닌 subgroup이다.

##### CounterExample2
$G = D_3:=\Set{r,s \mid r^3=s^2=e, \enspace rs = sr^{-1}}, H= \Set{1,s}$라고 하자.

이 떄, $G$의 element를 $r$, $H$의 element를 $s$라고 하면, $rsr^{-1} = r^2s \notin H$이다.

따라서, $H$는 normal subgroup이 아닌 subgroup이다.

### 명제3
Group $G,G'$과 group epimomorphism $\phi:G\rightarrow G'$이 있다고 하자.

그리고 set $\mathcal{A,B}$와 function $\varphi$를 다음과 같이 정의하자.

$$ \begin{gathered} \mathcal{A} := \Set{H \subseteq G \mid \ker(\phi) \subseteq H \lhd G}, \enspace \mathcal{B} := \Set{ H' \subseteq G' \mid H' \lhd G'} \\ \varphi : \mathcal{A} \rightarrow \mathcal{B} \st H \mapsto \phi(H) \end{gathered} $$

이 떄, 다음을 증명하여라.

$$ \varphi \text{ is a bijective } $$

**Proof**

[$\phi(H) \in \mathcal{B}$]   
$\mathcal{A}$의 임의의 element를 $H$라고 하고 $G'$의 임의의 element를 $g'$, $\phi(H)$의 임의의 element를 $\phi(h)$라고 하자.

$\phi$가 epimorphism임으로 다음이 성립한다.

$$ \exist g \in G \st g' = \phi(g) $$

그리고 $H\lhd G$임으로 다음이 성립한다.

$$ g'h(g')^{-1} = \phi(g)\phi(h)\phi(g)^{-1} = \phi(ghg^{-1}) \in \phi(H) $$

그럼으로 $\phi(H) \lhd G'$이고 $\phi(H) \in \mathcal{B}$이다. $\qed$

[well-defined]  
$\mathcal{A}$의 임의의 element를 $H_1,H_2$라고 하자.

그러면 다음이 성립한다.

$$ H_1 = H_2 \implies \phi(H_1) = \phi(H_2) \implies \varphi(H_1) = \varphi(H_2) \qed $$

[injective]  
$\mathcal{A}$의 임의의 element를 $H_1,H_2$라고 하면 다음이 성립한다.

$$ \varphi(H_1) = \varphi(H_2) \implies \phi(H_1) = \phi(H_2) \implies \preimg(\phi(H_1)) = \preimg(\phi(H_2)) $$

이 때, $\ker(\phi) \subseteq H_1,H_2$임으로 다음이 성립한다.

$$ \preimg(\phi(H_1)) = \preimg(\phi(H_2)) \implies H_1 = H_2 $$

[surjective]  
$\mathcal{B}$의 임의의 element를 $H'$라고 하자. 

$\preimg(H') = H$라고 하면 $e_{G'} \in H'$임으로 $\ker(\phi) \subseteq H$이다.

$G$의 임의의 element를 $g$, $H$의 임의의 element를 $h$라고 하면 $\phi(h) \in H'$이고 $H' \lhd G'$임으로 다음이 성립한다.

$$ \phi(g)\phi(h)\phi(g)^{-1} = \phi(ghg^{-1}) \in H' \implies ghg^{-1} \in H \implies H \lhd G $$

따라서, $\exist H \in \mathcal{A} \st \phi(H) = H'$이다. $\qed$

### 명제4
Group $G$와 subset $H$, normal subgroup $N$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ N \le H \le G \implies N \lhd H $$

**Proof**

전제에 의해 $N$은 $H$의 subgroup이다.

그리고 $N$의 임의의 element를 $n$ $H$의 임의의 element를 $h$라고 하면 $h \in G$이고 $N \lhd G$임으로 다음이 성립한다.

$$ hnh^{-1} \in N \qed $$

### 명제5
Group $G$와 subgroup $H$, normal subgroup $N$이 있다고 하자.

$HN := \Set{hn \mid h \in H, \enspace n \in N}$이라고 할 때, 다음을 증명하여라.

$$ HN \le G $$

**Proof**

$HN=NH$이면 $HN \le G$임으로 $HN=NH$임을 보이자.

[$HN \subseteq NH$]  
$HN$의 임의의 element를 $hn$이라고 하자. 

$N \lhd G$임으로 다음이 성립한다.

$$ \exist n_2 \in N \st  hnh^{-1} = n_2 $$

따라서, 다음이 성립한다.

$$ hn = hnh^{-1}h = n_2h \in NH \qed $$

[$NH \subseteq HN$]  
$NH$의 임의의 element를 $nh$라고 하자. 

$N \lhd G$임으로 다음이 성립한다.

$$ \exist n_2 \in N \st  h^{-1}nh = n_2 $$

따라서, 다음이 성립한다.

$$ nh = hh^{-1}nh = hn_2 \in HN \qed $$

#### 따름명제
Group $G$와 subgroup $H$, normal subgroup $N$이 있다고 하자.

$HN := \Set{hn \mid h \in H, \enspace n \in N}$이라고 할 때, 다음을 증명하여라.

$$ N \lhd HN $$

**Proof**

$N \subseteq HN \le G$임으로 $N \lhd HN$이다. $\qed$

### 명제6
Group $G$와 subgroup $H$, normal subgroup $N$이 있다고 하자.

$HN := \Set{hn \mid h \in H, \enspace n \in N}$이라고 할 때, 다음을 증명하여라.

$$ HN \le \braket{H \cup N} $$

**Proof**

[$HN \subseteq \braket{H \cup N}$]    
$HN$의 임의의 element를 $hn$이라고 하자.

$h,n \in \braket{H \cup N}$이고 $\braket{H \cup N} \le G$임으로 $hn \in \braket{H \cup N}$이다. $\qed$

[$\braket{H \cup N} \subseteq HN$]      
$H \cup N$을 포함하는 $G$의 subgroup들의 family를 $\mathcal{H}$라고 하자.

$HN \le G$이고 $H \cup N \subseteq HN$임으로 $HN \in \mathcal{H}$ 이다.

따라서, $\braket{H \cup N} = \bigcap \mathcal{H}$임으로 $\braket{H\cup N} \subseteq HN$이다. $\qed$

#### Reamark
1. $HN$은 $H$와 $N$을 포함하는 가장 작은 subgroup이다.

### 명제7
Group $G$와 subgroup $H$, normal subgroup $N$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ H \cap N \lhd H $$

**Proof**

$H \cap N$은 $H$의 subgroup임은 자명하다.

$H$의 임의의 element를 $h$, $H \cap N$의 임의의 element를 $x$라고 하면 다음이 성립한다.

$$  \begin{gathered} hxh^{-1} \in H \\ hxh^{-1} \in N \end{gathered} \implies hxh^{-1} \in N \cap H \implies H \cap N \lhd H \qed $$

### 명제8
Group $G,G'$과 group homomorphism $\phi : G \rightarrow G'$이 있다고 하자.

$N \lhd G$이고 $\varphi: G/N \rightarrow G' \st gN \mapsto \phi(g)$일 때, 다음을 증명하여라.

$$\varphi \text{ is well defined} \iff N \subseteq \ker(\phi)$$

**Proof**

[$\implies$]  
$N$의 임의의 element를 $n$, $G$의 임의의 element를 $g$라고 하자.

이 때, $g'=gn$이라고 하면 $gN = g'N$임으로 다음이 성립한다.

$$ \varphi(g'N) = \phi(g') = \phi(g)\phi(n) = \phi(g) \implies \phi(n) = e_{G'} \implies n \in \ker(\phi) $$

[$\impliedby$]  
$G/N$의 임의의 element를 $g_1N,g_2N$이라고 하면 다음이 성립한다.

$$ g_1N = g_2N \implies \exist n \in N \st g_2 = g_1n \implies \phi(g_2) = \phi(g_1n) = \phi(g_1) \implies \varphi(g_1N) = \varphi(g_2N) $$

### 명제9(First Isomorphism Theorem)
Group $G,G'$과 group homomorphism $\phi: G \rightarrow G'$이 있다고 하자.

이 때, 다음을 증명하여라.

$$ G/\ker(\phi) \cong \img(\phi) $$

**Proof**

$H = \ker(\phi)$라고 할 때, 함수 $\varphi$를 다음과 같이 정의하자.

$$ \varphi : G/H \rightarrow \img(\phi) \st gH \mapsto \phi(g) $$

[well defined]  
$\ker(\phi) \lhd G$이고 $\ker(\phi) \subseteq \ker(\phi)$임으로 명제8에 의해 $\varphi$는 well defined이다. $\qed$

[group homomorphism]   
$G$의 임의의 element를 $g_1,g_2$라고 하면 다음이 성립한다.

$$ \varphi(g_1Hg_2H) = \varphi(g_1g_2H) = \phi(g_1g_2) = \phi(g_1)\phi(g_2) = \varphi(g_1H)\varphi(g_2H) \qed $$

[injective]  
$G$의 임의의 element를 $g_1,g_2$라고 하면 다음이 성립한다.

$$ \varphi(g_1H) = \varphi(g_2H) \implies \phi(g_1) = \phi(g_2) \implies \phi(g_1)\phi(g_2^{-1}) = e_{G'} \implies g_1g_2^{-1} \in H \implies g_1H = g_2H \qed $$  

[surjective]  
$\img(\phi)$의 임의의 element를 $g'$이라고 하면 다음이 성립한다.

$$ \exist g \in G \st g' = \phi(g) \implies \exist gH \in G/H \st \varphi(gH) = g' \qed $$

#### Remark
1. group homomorphism이 있으면 isomorphic한 두 group을 찾아 낼 수 있다.
2. 그림으로 나타내면 다음과 같다.
```{figure} _image/3201.jpg
```

#### Example
1. $G = (\R,+)$ 그리고  $G'=(\C-\Set{0}, \times)$의 subgroup $S^1= \Set{z \in \C \mid |Z| = 1}$ 이 있다고 하자. $\varphi : (\R,+) \rightarrow S^1 \st x \mapsto e^{2\pi i x}$이라고 하면 $\varphi$는 group homomorphism이고 $\ker(\varphi)=\Z$이다. 따라서 first isomorphism theorem에 의해 $\R/\Z \cong S^1$이다.
2. Group $G$와 normal subgroup $N$이 있다고 하자. $\pi : G \rightarrow G/N \st g \mapsto gN$이라고 하면 $\pi$는 group homomorphism이고 $\ker(\pi) = N, \img(\pi) = G/N$이다.

### 명제10(Second Isomorphism Theorem)
Group $G$와 subgroup $H$ 그리고 normal subgroup $N$이 있다고 하자.

$HN = \braket{H \cup N}$일 때, 다음을 증명하여라.

$$ H/ H\cap N \cong HN/N $$

**Proof**

두 homeomorphism $i,\pi$를 다음과 같이 정의하자.

$$ i : H \rightarrow G \st h \mapsto h, \enspace \pi : G \rightarrow G/N \st g \mapsto gN $$

이 때, $\phi := \pi \circ i$는 group homomorphism이고 다음이 성립한다.

$$ \begin{gathered} \ker(\phi) = H \cap N \\ \img(\phi) = \Set{hN \mid h \in H} = \Set{hnN \mid h \in H, n\in N} = HN / N \end{gathered} $$

따라서, first isomorphism theorem에 의해 다음이 성립한다.

$$ H/ H\cap N \cong HN/N \qed $$

#### Remark
1. $N$이 $H$의 subset이 아니기 때문에 $N$은 $H$의 normal subgroup이 아니다. 
2. $N$이 $HN$의 subset임으로 $N$은 $HN$의 normal subgroup이다.
3. 이를 그림으로 나타내면 다음과 같다.
```{figure} _image/3202.jpg
```

### 명제11
Group $G,G'$과 normal subgroup $H \lhd G, H' \lhd G'$ 그리고 group homomorphism $\phi : G \rightarrow G'$가 있다고 하자.

$\varphi : G/H \rightarrow G'/H \st gH \mapsto \phi(g)H'$일 때, 다음을 증명하여라.

$$ \varphi \text{ is well defined} \iff \phi(H) \subseteq H' $$

**Proof**

[$\implies$]  
$G$의 임의의 element를 $g$, $H$의 임의의 element를 $h$라고 하면 다음이 성립한다.

$$ gH = ghH \implies  \varphi(gH) = \varphi(ghH) \implies \phi(g)H' = \phi(gh)H' \implies \phi(g)H' = \phi(g)\phi(h)H' \implies \phi(h) \in H' \qed $$

[$\impliedby$]  
$G/H$의 임의의 두 element를 $g_1H,g_2H$라고 하면 다음이 성립한다.

$$ g_1H = g_2H \implies g_1g_2^{-1} \in H \implies \phi(g_1)\phi(g_2^{-1}) \in H' \implies \phi (g_1)H' = \phi(g_2)H' \implies \varphi(g_1H) = \varphi(g_2H) \qed $$


### 명제12(Third Isomorphism Theorem)
Group $G$와 normal subgroup $H,K$가 있다고 하자.

$K$가 $H$의 subgroup일 때, 다음을 증명하여라.

$$ G/K \Big/ H/K \cong G/H $$

**Proof**

함수 $\phi : G/K \rightarrow G/H \st gK \mapsto gH$라고 하자.

[well-defined]  
$G=G, G'= G, \phi = id, H = H, H' = K$로 두면 명제 11에 의해 well-defined이다. $\qed$

[homomorphism]  
$G/K$의 임의의 두 element를 $g_1K,g_2K$라고 하면 다음이 성립한다.

$$ \phi(g_1Kg_2K) = \phi(g_1g_2K) = g_1g_2H = g_1Hg_2H = \phi(g_1K)\phi(g_2K) \qed $$

그리고 $\ker(\phi) = H/K$이고 $\img(\phi) = G/H$임으로 first isormorphism theorem에 의해 다음이 성립한다.

$$ G/K \Big/ H/K \cong G/H \qed $$

#### Remark
1. 이를 그림으로 나타내면 다음과 같다.
```{figure} _image/3203.jpg
```

## Definition(Simple)
Group $G$가 있다고 하자.

이 때, $G$의 normal subgroup이 $\Set{e},G$밖에 없는 경우, $G$를 `simple group`이라고 한다.

## Definition(Maximal Normal Subgroup)
Group $G$와 normal subgroup $M$이 있다고 하자.

$M \neq G$일 때, 다음을 만족하는 $M$을 `maximal normal subgroup`이라고 한다.

$$ \exist N \lhd G \st M \subseteq N \implies N = G $$

### 명제1
Groupg $G$와 normal subgroup $N$이 있다고 하자.

이 떄, 다음을 증명하여라.

$$ G/N \text{ is simple} \iff N \text{ is maximal normal subgroup} $$

**Proof**

group epimorphism $\pi$를 다음과 같이 정의하자.

$$ \pi : G \rightarrow G/N \st g \mapsto gN $$

이 떄, set $\mathcal{A,B}$를 다음과 같이 정의하자.

$$ \mathcal{A} := \Set{H \subseteq G \mid \ker(\pi) \subseteq H \lhd G }, \enspace \mathcal{B} := \Set{ H \subseteq G/N \mid H \lhd G/N} $$

그러면 $\phi : \mathcal{A} \rightarrow \mathcal{B} \st H \mapsto \pi(H)$는 bijective function이다.

[$\implies$]  
$G/N$이 simple 임으로 $\mathcal{B} = \Set{\Set{N},G/N}$이다.

이 때, $\phi(N) = \pi(N) =  \Set{gN \in G/N \mid g \in N } = \Set{N}$이고 $\phi(G) = \Set{gN \in G/N \mid g \in G} = G/N$임으로 $\mathcal{A} = \Set{N, G}$이다.

즉, $\ker(\pi) = N$을 포함하는 $G$의 normal subgroup이 $N,G$밖에 없다는 말임으로 $N$은 maximal normal subgroup이다. $\qed$

[$\impliedby$]  
$N$이 maximal normal subgroup이고 $\ker(\pi) = N$임으로 $\mathcal{A} = \Set{N, G}$이다.

이 때, $\phi(N) = \Set{N}$이고 $\phi(G) = G/N$임으로 $\mathcal{B} = \Set{\Set{N},G/N}$이다.

따라서, $G/N$은 simple이다. $\qed$