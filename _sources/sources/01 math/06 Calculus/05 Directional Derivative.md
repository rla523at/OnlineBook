# Directional Derivative
## 정의
open subset $U \subset \R^n$과 함수 $f:U \rightarrow \R^m$이 있다고 하자.

$f$의 `directional derivative`는 다음과 같이 정의된 함수이다.

$$ Df : U \times \R^n \rightarrow \R^m \st (a,v) \mapsto \lim_{h \rightarrow 0}\frac{ f(a + h v) - f (a)}{h} $$

### 참고1(notation)
$Df(a,v)$는 $D_vf(a)$와 같이 표기하는 것이 일반적이다.

### 참고2
$D_vf(a)$는 $a$에서 $v$방향으로 변화량과 그에 따른 $f$의 변화량의 비율의 극한을 의미한다.

### 명제
open set $U \subset \R^n$과 함수 $f : U \rightarrow \R^m$이 있다고 하자.

$\R^m$의 basis를 $\beta$하고 $\forall a \in U$에서 $f$가 differentiable일 때, 다음을 증명하여라.

$$ Df\text{ is well-defined} \enspace\land\enspace \mathfrak{m_\beta}(D_vf(a)) = \mathfrak{m_\beta}(J_f(a)v) $$

**Proof**

$f$가 differentiable 함으로 다음이 성립한다.

$$ \begin{aligned} & \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a) - L_a(hv)}{\norm{hv}} = 0 \\\implies& \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a) - hL_a(v)}{h\norm{v}} = 0 \\\implies& \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a) - hL_a(v)}{h} = 0 \\\implies& \lim_{h \rightarrow 0}\frac{f(a + hv) -f (a)}{h} = L_a(v) \end{aligned} $$

이 떄, $f$가 $a$에서 differentiable 함으로, 선형변환 $L_a$은 잘 정의된다. 

따라서, 극한값이 $L_a(v)$로 존재함으로, 함수 $Df$는 잘 정의된다.

또한, $D_vf(a) = L_a(v)$이고 Jacobian matrix의 정의에 따라 $L_a(v) = J_f(a)v$



> Reference  
> {cite}`hubbard` Proposition 1.7.14.

