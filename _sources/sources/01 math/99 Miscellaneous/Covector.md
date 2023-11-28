# Covector

### 명제1
유한차원 벡터공간 $V/\Bbb F$와 기저 $\beta, \gamma$가 있을 때, $\beta$에서 $\gamma$로 변할 때 기저변환행렬을 $P$라 하자.

$v^* = a_i\beta^i = b_i\gamma^i$가 있을 때, 다음을 증명하여라.
$$ b_i = a_jP^i_j $$

**Proof1**

$$ \begin{aligned} & v^*(\gamma_i) = b_j\gamma^j(\gamma_i) = a_j\beta^j(\gamma_i) \\ \Rightarrow \enspace & b_i = a_j P^i_k \beta^j(\beta_k) = a_j P^i_k \delta ^j_k = a_jP^i_j \quad {_\blacksquare} \end{aligned} $$

**Proof2**

$v^* \in V^*$면 $v^* : V \rightarrow \Bbb F$임으로 $v^* = v^* \circ Id_V$이다. 따라서 다음이 성립한다.
$$ [v^*]^1_\beta = [v^*]^1_\gamma[Id_V]^\gamma_\beta $$

Dual space의 명제 2번에 의해 다음과 같이 정리할 수 있다.
$$ \begin{aligned} & ([v^*]_\beta)^T = ([v^*]_\gamma)^T [Id_V]^\gamma_\beta \\ \Rightarrow \enspace & [v^*]_\beta = ([Id_V]^\gamma_\beta)^T [v^*]_\gamma \\ \Rightarrow \enspace & [v^*]_{\gamma^*} = ([Id_V]_\gamma^\beta)^T[v^*]_{\beta^*} \end{aligned} $$

이를 통해 기저가 $\beta^*$ 에서 $\gamma^*$로 변할 때 좌표변환행렬과 기저가 $\beta$에서 $\gamma$로 변할 때 나타나는 기저변환행렬이 동일함을 알 수 있다. 즉, 기저가 $\beta$에서 $\gamma$로 바뀔 때 기저가 변하는 방식과 dual basis에서
좌표가 변하는 방식이 동일하다. 따라서 $v^* \in V^*$를 `covector`라고 한다. 


### 명제2
$n$차원 벡터공간 $V/\Bbb F$와 기저 $\beta$, dual space $V^*$와 $\beta$의 dual basis $\beta^*$가 있을 때 다음을 증명하여라.

$$ v^* \in V^* \Rightarrow [v^*]_\beta^1 = ([v^*]_{\beta^*}) ^T $$

이 떄, $1$은 $\R$의 기저이다.

**Proof**

$v^* = a_i\beta^i$라 하자.
$$ v^*(\beta_j) = a_j \Rightarrow [v^*]_\beta^1 = \begin{bmatrix} a_1 & \cdots & a_n \end{bmatrix} \quad {_\blacksquare} $$

## covector의 사용
> 참고
> [Book] (Dullemond & Peeters) Introudction to Tensor Calculus chap 2.1

### 명제1
유한차원 벡터공간 $V/\Bbb F$가 있을 때, 기저를 $\beta, \gamma$, dual basis를 $\beta^*, \gamma^*$라 하자. $v \in V, v^* \in V^*$에 대해서 다음이 성립한다.

$$ \begin{array}{l l} \beta \rightarrow \gamma & \gamma = P^T\beta \\ \beta \rightarrow \gamma & [v]_{\gamma} = P^{-1} [v]_{\beta}\\ \beta^* \rightarrow \gamma^* & \gamma^* = P^{-1} \beta^* \\ \beta^* \rightarrow \gamma^* & [v^*]_{\gamma^*} = P^T [v^*]_{\beta^*} \end{array} $$
$$ \text{Where, } P = [Id_V]^\beta_\gamma  $$

> 참고  
> [Dual Space - 피그티의 기초물리 블로그](https://elementary-physics.tistory.com/16)  
> [What are covectors - eigenchris Youtube](https://www.youtube.com/watch?v=LNoQ_Q5JQMY)  
> [how-are-co-vectors-not-just-row-vectors - Mathmatics](https://math.stackexchange.com/questions/3295875/how-are-co-vectors-not-just-row-vectors)  
> [Linear_form - Wiki](https://en.wikipedia.org/wiki/Linear_form)  
> [what-is-the-difference-between-a-dual-vector-and-a-reciprocal-vector - Pysics](https://physics.stackexchange.com/questions/509334/what-is-the-difference-between-a-dual-vector-and-a-reciprocal-vector)  
> reciprocal vector, covariant components, metric tensor in euclidean space  
 
> [Covariance and Contravarient - Wiki](https://en.wikipedia.org/wiki/Covariance_and_contravariance_of_vectors)  
> [dual basis - Mathmatics](https://math.stackexchange.com/questions/1286100/how-do-i-find-a-dual-basis-given-the-following-basis)  
> [linear form - Wiki](https://en.wikipedia.org/wiki/Linear_form#Basis_of_the_dual_space)    
> [What is a covector and what is it used for? - Mathmatics](https://math.stackexchange.com/questions/240491/what-is-a-covector-and-what-is-it-used-for)  