# Affine Transformation

## Affine Homomorphism
두개의 affine space $A,B$와 함수 $F : A \rightarrow B$가 있다고 하자.

$F$에 의해 정의되는 $T_F:V_A \rightarrow V_B$ 함수가 다음을 만족할 때, $F$를 `affine homomorphism` 또는 `affine map`이라고 한다.

$$ T_F:V_A \rightarrow V_B \st  b-a \mapsto F(b) - F(a) \text{ is a well deifned linear map} $$

> Reference  
> [wikipedia - Affine_space#Affine_map](https://en.wikipedia.org/wiki/Affine_space#Affine_map)

### 참고1
두개의 affine space $A,B$와 affine homomorphism $F:A\rightarrow B$가 있다고 하자.

그러면 $T_F$는 well defined이기 때문에 $a,b,c,d\in A$에 대해서 다음이 성립해야 한다.

$$ a-b = c-d \implies T_F(a-b) = T_F(c-d) \iff F(a)-F(b) = F(c) - F(d) $$

> Reference  
> [wikipedia - Affine_space#Affine_map](https://en.wikipedia.org/wiki/Affine_space#Affine_map)

### 명제1
두개의 affine space $A,B$와 affine homomorphism $F:A\rightarrow B$가 있다고 하자.

$a \in A$와 $v\in V_A$에 대해서 다음이 성립한다.

$$ F(a+v) = F(a) + T_F(v) $$

**Proof**

affine homomorphism의 정의에 의해 다음이 성립한다.

$$ T_F((a+v)-a) = F(a+v) -F(a) $$

이를 정리하면 다음과 같다.

$$ F(a+v) = F(a) + T_F(v) $$

> Reference  
> [wikipedia - Affine_space#Affine_map](https://en.wikipedia.org/wiki/Affine_space#Affine_map)

## Affine Transformation
affine space $A$와 affine homomorphism $F:A\rightarrow A$가 있다고 하자.

이 떄, $F$를 `affine space endomorphism` 또는 `affine transformation`이라고 한다.

> Reference    
> [wikipedia - Affine_transformation#Definition](https://en.wikipedia.org/wiki/Affine_transformation#Definition)  

### 참고1
affine space $A$와 $p_0 \in A$가 있다고 하자.

$T \in \End(V_A)$가 있으면, $p_0$와 $T$로 부터 $L:A\rightarrow A$을 다음과 같이 정의할 수 있다.

$$ L : A \rightarrow A \st p \mapsto (F_{p_0} \circ T \circ F_{p_0}^{-1})(p) $$

$$ \begin{CD} 
A                   @>L>>     A \\ 
@V{F_{p_0}^{-1}}VV            @AA{F_{p_0}}A \\ 
V_A                 @>>T>     V_A \\
\end{CD} $$

이 때, 임의의 $p \in A$에 대해서 $\exist! v \in V_A \st p =p_0 + v$이 존재함으로, 다음이 성립한다.

$$ \begin{aligned}
L(p) &= L(p_0 +v) \\
     &= (F_{p_0} \circ T \circ F_{p_0}^{-1})(p_0 +v) \\
     &= (F_{p_0} \circ T)(v) \\
     &= F_{p_0}(T(v)) \\
     &= p_0 + T(v) \\
\end{aligned}   $$

> Reference
> [wikipedia - Affine_transformation#Structure](https://en.wikipedia.org/wiki/Affine_transformation#Structure)  


### 명제1


$V_A$의 임의의 endomorphism $T$에 대해서, 함수 $F(p_0, T)$를 다음과 같이 정의하자.

$$ F(p_0,T):A \rightarrow A \st p \mapsto f_{p_0}^{-1}(T(f_{p_0}(p))) $$

그러면 다음이 성립한다.

$$ F(p_0,T)(p) = p + T(\overrightarrow{p_0p}) $$


