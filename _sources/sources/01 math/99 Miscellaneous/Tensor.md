# Tensor
vector space $V / \mathbb F$가 있을 때, $p-$contravariant, $q-$covariant tensor $T$는 다음과 같이 정의된 multi-linear form이다.
$$ T : \underbrace{V^* \times \cdots \times V^*}_{p} \times \underbrace{V \times \cdots \times V}_{q} \rightarrow \mathbb F $$

이를 $(p,q)$ tensor 라고도 한다. 

$(p,q)$ tensor들이 모여 있는 공간은 tensor product에 의해 정의된 vector space이며 다음과 같다.
$$ T \in \underbrace{V \otimes \cdots \otimes V}_{p} \otimes \underbrace{V^* \otimes \cdots \otimes V^*}_{q} = T^q_p(V) $$

### 명제1
vector space $V / \mathbb F$가 있을 때, 함수 $\phi^k_l$를 다음과 같이 정의하자.
$$ \phi^k_l : L(\underbrace{V^*, \cdots , V^*}_{l}, \underbrace{V, \cdots , V}_{k} ; V) \rightarrow T^k_{l+1}(V) \quad s.t. \quad L \mapsto \phi^k_l(L) $$

$$ \text{Where, } \phi^k_l(L)(v^1, \cdots, v^{l+1},v_1, \cdots, v_k) = v^{l+1}(L(v^1, \cdots, v^l,v_1, \cdots, v_k))$$

이 때, 다음을 증명하여라.
$$ \phi^k_l \text{ is a vector space isomorphism}$$

**Proof**

$\{v^{l}_k\} = (v^1, \cdots, v^{l},v_1, \cdots, v_k)$라 하자.

[$\phi^k_l \in L(L; T^k_l)$]  
$L_1, L_2 \in L$이라 하자.
$$ \begin{aligned} (\phi^k_l(L_1 + aL_2))\{v^{l+1}_k\} &= v^{l+1}((L_1 + aL_2)\{v^{l+1}_k\}) \\ &= v^{l+1}(L_1\{v^{l+1}_k\}) + av^{l+1}(L_2\{v^{l+1}_k\}) \\ &= (\phi^k_l(L_1))\{v^{l+1}_k\} + a(\phi^k_l(L_2))\{v^{l+1}_k\} \end{aligned}  $$

[$\phi^k_l$ is bijective]    
$\ker(L) = \{ 0_{L} \}$이고, $\dim(L) = \dim(T^k_{l+1})$임으로 dimension theorem에 의해 bijective이다. $\quad {_\blacksquare}$

> 참고  
> [Tensor - Wiki](https://en.wikipedia.org/wiki/Tensor)  
> [Book] (John M Lee) Riemannian Manifolds_ An Introduction to Curvature chap2  
>

# Dyad
$\bf u,v,w$를 벡터라고 하자.
$$ \bf (u \otimes v) \cdot w = u(v \cdot w) $$

위의 관계식을 만족하는 $\bf u \otimes v$를 `dyad`라고 한다. 이 때, $\otimes$를 `텐서곱(tensor product)`이라 한다.

### 명제1
dyad는 선형변환임을 증명하여라.

### 명제2
dyad는 교환법칙이 성립하지 않음을 증명하여라.

### 명제3
$\bf u,v,w,x$를 벡터라고 할 때 다음을 증명하여라.
$$ (\bf (u \otimes v) \cdot w) \otimes x = (u \otimes v) \cdot (w \otimes x) $$

### 명제4
$\bf u,v,w,x$를 벡터라고 할 때 다음을 증명하여라.
$$ \bf (u \otimes v)\cdot(w \otimes x) = (v \cdot w)(u \otimes x) $$

### 명제5
$\bf u,v,w$를 벡터라고 할 때 다음을 증명하여라.
$$ \bf u \cdot (v \otimes w) = (u \cdot v) w $$

# Tensor Contraction

> 참고  
> [Tensor contraction - Wiki](https://en.wikipedia.org/wiki/Tensor_contraction#cite_note-natural_iso-1)  
> [텐서란 무엇인가? 텐서의 이해, 표기법, 연산 완전 정리 - 피그티의 기초물리 tistory](https://elementary-physics.tistory.com/155)   
> 

# Cartesian Tensor

### 명제1
$\bf I$가 Cartesian Tensor의 Identity tensor라 할 때, 다음을 증명하여라.

$$ \mathbf I = \mathbf e_i \mathbf e_i $$

**Proof**

$\mathbf I = \mathbf e_i \mathbf e_i$라 가정하고 $\bf u$를 임의의 벡터라 하자.

$$ \mathbf{I \cdot u} = u_j (\mathbf e_i \otimes \mathbf e_i) \cdot \mathbf e_j = u_j \delta_{ij} \mathbf e_i = u_i \mathbf e_i \quad {_\blacksquare} $$

### 명제2
$\bf A$가 Cartesian Tensor라 할 때, 다음을 증명하여라.
$$ A_{ij} = (\mathbf A \cdot \mathbf e_j) \cdot \mathbf e_i $$

**Proof**

$$ \mathbf A \cdot \mathbf e_j \cdot \mathbf e_i = A_{kl}((\mathbf e_k \otimes \mathbf e_l) \cdot \mathbf e_j) \cdot \mathbf e_i = A_{kl} \delta_{lj} \delta_{ki} = A_{ij} \quad {_\blacksquare} $$

### 명제3
$\bf A$가 Cartesian Tensor, $\bf x$가 벡터라고 할 때 다음을 증명하여라.
$$ \frac{\partial \mathbf x^T \bf Ax}{\partial \mathbf x} = (\mathbf A^T + \bf A )x  $$

**Proof**  


# Isotropic Tensor
[Isotropic Tensor](https://farside.ph.utexas.edu/teaching/336L/Fluid/node252.html)  
[2nd order isotropic tensor](https://www.weizmann.ac.il/chembiophys/bouchbinder/sites/chemphys.bouchbinder/files/uploads/Courses/2019/TA2-IndexGymnastics.pdf)  
[4th order isotropic tensor](https://math.stackexchange.com/questions/3589647/general-form-of-an-isotropic-fourth-rank-tensor)  

---
> 참고  
 
[텐서 - 나무위키](https://namu.wiki/w/%ED%85%90%EC%84%9C)  
[What are the Differences Between a Matrix and a Tensor? - StackExchange](https://math.stackexchange.com/questions/412423/what-are-the-differences-between-a-matrix-and-a-tensor)  
[텐서 - 전파거북이 블로그](https://ghebook.blogspot.com/2011/06/tensor.html)


> 참고  
> [Universal property - Wiki](https://en.wikipedia.org/wiki/Universal_property)  
> [Tensor Proudct - Wiki](https://en.wikipedia.org/wiki/Tensor_product#Universal_property)  
> [tensor-products-universal-property-and-a-particular-identification - Mathematics](https://math.stackexchange.com/questions/2674549/tensor-products-universal-property-and-a-particular-identification)  
> [proof-of-universal-mapping-property-for-tensor-product-of-vector-spaces - Mathematics](https://math.stackexchange.com/questions/2713003/proof-of-universal-mapping-property-for-tensor-product-of-vector-spaces)  
> [change-of-basis-of-tensors - Mathmatics](https://math.stackexchange.com/questions/3451369/change-of-basis-of-tensors)