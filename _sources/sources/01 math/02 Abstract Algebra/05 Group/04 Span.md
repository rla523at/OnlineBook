# Span
## 정의
Group $G$와 $G$의 subset $U$가 있다고 하자.

$U$를 포함하는 $G$의 subgroup의 family를 $H$라고 할 때, $U$에 의한 `생성(span)`은 다음과 같이 정의된 집합이다.

$$ \span(U) := \bigcap H $$

### 참고
$\span(H)$는 $H$를 포함하는 $G$의 최소 subgroup이며 이를 $H$에 의해 생성된 subgroup이라고도 한다.

### 명제1
Group $G$와 $G$의 subset $U$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \span(U) \le G $$

**Proof**  

$U$를 포함하는 $G$의 subgroup의 family를 $H$라고 하자.

$H$의 임의의 element를 $H_i$라고 하고 $\span(H)$의 임의의 element를 $x,y$라고 하면 다음이 성립한다.

$$ x,y \in H_i $$

[closed]   
이 떄, $H_i$는 subgroup임으로 다음이 성립한다.

$$ x*y \in H_i $$

따라서, $\span(H)$의 정의에 의해 다음이 성립한다.

$$ a*b \in \span(H) \qed $$

[inverse]    
이 떄, $H_i$는 subgroup임으로 다음이 성립한다.

$$ \exist x^{-1} \in H_i $$

따라서, $\span(H)$의 정의에 의해 다음이 성립한다.

$$ \exist x^{-1} \in \span(H) \qed $$

[결론]
따라서, subgroup의 성질에의해 다음이 성립한다.

$$ \span(H) \le G \qed $$

### 명제2
Group $G$와 $G$의 subset $U$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \span(U) \text{ is a smallest subgroup of } G \text{ which that contains } U $$

**Proof**

$U$를 포함하는 $G$의 subgroup의 family를 $H$라고 하자.

정의에 의해 $\span(U) = \bigcap H$임으로, $H$의 임의의 element를 $H_i$라고 하면 다음이 성립한다.

$$ \span(U) \subseteq H_i $$

또한 명제1에 의해 $\span(U)$는 subgroup임으로 smallest subgroup이다. $\qed$

### 명제3
Abilian group $A$가 있다고 하자.

$A$의 임의의 element를 $x$라고 할 때, 다음을 증명하여라.

$$ \span(\{ x \}) = \{ nx | n \in \Z \}$$

이 때, $nx$는 다음과 같이 정의한다.

$$nx = \begin{cases} \underbrace{x + \cdots + x}_{n} & 0 < n \\ 0_G & n= 0 \\ \underbrace{ x^{-1} + \cdots + x^{-1} } _ {|n|} & n < 0 \end{cases}$$

**Proof**  

[$\{ nx | n \in \Z \}$ is group ]  
-[closed]  
$\Set{ nx | n \in \Z}$의 임의의 element를 $a,b$라고 하면 다음이 성립한다.

$$ a+b = n_ax +n_bx = (n_a+n_b)x \in \Set{ nx | n \in \Z} \qed $$

-[identity]  
$n=0$으로 두면 $0_G$임으로 $0_G \in \Set{ nx | n \in \Z}$이다. $\qed$

-[inverse]  
$\Set{ nx | n \in \Z}$의 임의의 element를 $a$라고 하면 다음이 성립한다.

$$ \exist n_a \in \Z \st a = n_ax $$

그러면 $-n_a \in \Z$임으로 $-a = -n_ax$라고 하면 다음이 성립한다.

$$-a \in \Set{ nx | n \in \Z}$$

$a+(-a) = (-a) + a = 0_G$임으로 다음이 성립한다.

$$ \exist a^{-1} \in \Set{ nx | n \in \Z} \qed $$

[$\{ nx|n \in \Z\}$ is smallest]  
$\Set{x}$를 포함하는 $A$의 subgroup의 family를 $H$라고 하자.

$H$의 임의의 element를 $H_i$, $\Set{ nx|n \in \Z}$의 임의의 element를 $a$라고 하면 $H_i$가 subgroup이기 때문에 다음이 성립한다.

$$ a \in H_i \implies \Set{ nx|n \in \Z} \subseteq H_i $$

$\Set{ nx|n \in \Z}$는 $G$의 subgroup임으로 다음이 성립한다.

$$ \{ nx|n \in \Z\} \text{ is a smallest subgroup of } G \text{ such taht contains } \Set{x} \qed $$

### 명제4
Abilian group $A$가 있다고 하자.

$A$의 임의의 elements를 $x,y$라고 할 때, 다음을 증명하여라.

$$ \span( \{ x,y\}) = \Set{nx + my | n,m \in \Z} $$

**Proof**  

[$\Set{nx + my | n,m \in \Z}$ is a group]  
-[closed]  
$\Set{nx + my | n,m \in \Z}$의 임의의 element를 $a,b$라고 하면 다음이 성립한다.

$$ a+b = n_ax + m_ay + n_bx + m_by = (n_a+n_b)x + (m_a+m_b)y \in \Set{nx + my | n,m \in \Z} \qed $$

-[identity]  
$n,m=0$으로 두면 $0_G$임으로 $0_G \in \Set{nx + my | n,m \in \Z}$이다. $\qed$

-[inverse]  
$\Set{nx + my | n,m \in \Z}$의 임의의 element를 $a$라고 하면 다음이 성립한다.

$$ \exist n_a,m_a \in \Z \st a = n_ax + m_ay $$

그러면 $-n_a,-m_a \in \Z$임으로 $-a = -n_ax-m_ay$라고 하면 다음이 성립한다.

$$-a \in \Set{nx + my | n,m \in \Z}$$

$a+(-a) = (-a) + a = 0_G$임으로 다음이 성립한다.

$$ \exist a^{-1} \in \Set{nx + my | n,m \in \Z} \qed $$

[$\{ nx|n \in \Z\}$ is smallest]  
$\Set{x,y}$를 포함하는 $A$의 subgroup의 family를 $H$라고 하자.

$H$의 임의의 element를 $H_i$, $\Set{nx + my | n,m \in \Z}$의 임의의 element를 $a$라고 하면 $H_i$가 subgroup이기 때문에 다음이 성립한다.

$$ a \in H_i \implies \Set{nx + my | n,m \in \Z} \subseteq H_i $$

$\Set{nx + my | n,m \in \Z}$는 $G$의 subgroup임으로 다음이 성립한다.

$$ \{ nx|n \in \Z\} \text{ is a smallest subgroup of } G \text{ such taht contains } \Set{x,y} \qed $$
