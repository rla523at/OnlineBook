# Weighted Residual Formulation
Strong formulation의 `weighted residual formulation(WRF)`은 다음과 같다.  

$$ \text{find } u \in \mathcal U \st \forall w \in  C^\infty_c(\Omega), \quad \int_\Omega w r \thinspace dV = 0 $$

$$ \text{Where, } r(x) = \mathcal P(u) + f(x) $$

이 떄, $C^\infty_c(\Omega)$는 `테스트 함수공간(test function space)`, $w$는 `테스트 함수(test function)`, $r$은 `residual`이라한다.


## Functional Form
일반적으로 WRF은 functional $B_r,l_r$을 이용해 다음과 같이 간단하게 나타낼 수 있다.

$$ \text{find } u \in \mathcal U \st \forall w \in C^\infty_c(\Omega), \quad B_r(w,u) = l_r(w) $$

$$ \text{Where, } B_r(w,u) :=  \int_\Omega w\mathcal P(u) \thinspace dV, \quad l_r(w) := -\int_\Omega wf \thinspace dV $$

### 참고
이 때, $B_r$은 첫번째 원소에 대해서 linear map이며, $l_r$은 linear map이다.

만약, $\mathcal P$가 linear operator라면, $B_r$은 bilinear map이 된다.

> Reference  
> [Note - M. J. Zahr](https://mjzahr.github.io/content/ame40541/spr20/ch03-wres-solo.pdf)

## 참고
WRF이라고 부르는 이유는 WRF가 residual의 가중평균이 0이 되는 것과 같은 형태를 가지고 있기 때문이다. 

그렇다고 WRF의 해가 $r = 0$을 만족하는게 아니라 가중평균만 $0$으로 보낸다고 오해하면 안된다. 

Fundamental Lemma of Variation Calculus에 의해 WRF는 strong formulation과 동치이다.



### Fundamental Lemma of Variation Calculus
$\Omega \subset \R^d$와 $f \in C^0(\Omega)$가 있다고 할 때 다음을 증명하여라. 

$$ f = 0 \text{ on } \Omega \iff \forall w \in C^\infty_c(\Omega), \int_\Omega w f \thinspace dV = 0 $$

**Proof**

[$\implies$]  
$f = 0$이면 $\forall w \in C^\infty_c(\Omega), \int_\Omega w f \thinspace dV = 0$는 자명하게 참이다. $\qed$

[$\impliedby$]  
다음을 가정하자.

$$ f \neq 0 \text{ on } \Omega $$

따라서, 다음이 성립한다.

$$ \exist\Omega_h \subseteq \Omega \st f(\Omega_h)>0 \lor f(\Omega_h)<0 $$

-[$f(\Omega_h) > 0$]  
Bump function의 existence가 보장되기 때문에 다음이 성립한다.

$$ \exist w \in C^\infty_c(\Omega) \quad s.t. \quad w \in C^\infty_c(\Omega_h) \enspace \land \enspace w(\Omega_h) >0 $$

따라서 다음이 성립한다.

$$ \int_\Omega w f \thinspace dV = \int_{\Omega_h} w f \thinspace dV > 0 $$

이는 전제에 모순된다.

-[$f < 0$ on $\Omega_h$]  
위와 비슷한 방법으로 전제에 모순이 발생함을 알 수 있다.

-[결론]  
따라서, proof by contradiction에 의해 다음이 성립한다.

$$ f = 0 \text{ on } \Omega \qed $$

> Reference  
> [note - Ananthasuresh](https://mecheng.iisc.ac.in/suresh/me256/notes3_2007.pdf)  
> [math.stackexchange](https://math.stackexchange.com/questions/1792102/alternative-proof-of-fundamental-lemma-of-variational-calculus)  

denseness of smooth function proof

> Reference  
> [last answer - Mathematics](https://math.stackexchange.com/questions/1805184/the-dirac-delta-does-not-belong-in-l2)