# Permutation Representation
## Definition
Group $G$와 set $X$ 그리고 $X$위의 symmetric group $S_X$가 있다고 하자.

이 떄, group homomorphism $\rho: G \rightarrow S_X$를 $X$의 `permutation representation`이라고 한다.

### 명제1
Group $G$와 set $X$ 그리고 $X$위의 symmetric group $S_X$가 있다고 하자.

$X$의 permutation representation를 $\rho$라고 할 때, 다음을 증명하여라.

$$ \cdot : G \times X \rightarrow X \st g \cdot x = \rho_g(x) \text{ is a group action}  $$

**Proof**

[identity]  
$\rho$는 group homomorphism임으로 $\rho_{e_G} = id_X$이다. 

따라서 다음이 성립한다.

$$e_G \cdot x = \rho_{e_G}(x) = x \qed$$  

[compatibility]  
$G$의 임의의 element를 $g_1,g_2$, $X$의 임의의 element를 $x$라고 하자.

$\rho$는 group homomorphism임으로 다음이 성립한다.

$$\rho_{g_1} \circ \rho_{g_2} = \rho_{g_1g_2}$$ 

따라서 다음이 성립한다.

$$ g_1 \cdot g_2 \cdot x = \rho_{g_1}(\rho_{g_2}(x)) = (\rho_{g_1} \circ \rho_{g_2})(x) = \rho_{g_1g_2}(x) = (g_1g_2)\cdot x \qed $$

### 참고
모든 permutation representation은 homomorphism인것은 아니다.

Cayley Theorem에 나오는 것과 같은 특정 permutation representation만 homomorphism이다.