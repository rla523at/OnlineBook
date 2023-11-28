# 명제
$A,B \in M_{nn}(\mathbb F)$일 때, 다음을 증명하시오.
$$ AB=I \Rightarrow A^{-1} = B $$

**Proof**

Determinant의 성질에 이해 다음이 성립한다.
$$ \begin{aligned} & \det(AB) = \det(A)\det(B) = \det(I) = 1 \\ \Rightarrow\enspace& \det(B) \neq 0 \\ \Rightarrow\enspace& \exist B^{-1} \end{aligned} $$

따라서 다음이 성립한다.
$$ \begin{aligned} BA &= BAI \\&= BABB^{-1} \\&= BB^{-1} \\&= I \end{aligned}  $$

즉, $AB = BA = I$임으로 $A^{-1} = B$이다. $\quad\tiny\blacksquare$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/852387/if-ab-i-then-ba-i-is-my-proof-right)