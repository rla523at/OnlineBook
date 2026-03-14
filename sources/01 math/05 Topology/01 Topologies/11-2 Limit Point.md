# Limit Point
## 정의
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

$x \in X$가 다음을 만족할 경우, $x$를 $U$의 `limit point`라고 한다.

$$ \forall\mathcal{N_x} \in \Set{\mathcal{N_x}} , \quad \mathcal{N_x} \text{ contians a point of } U \text{ other than } x $$

### 참고1
$x \in X$이기 때문에 $x$는 $U$에 원소일수도 있고 아닐 수도 있다.

### 참고2
limit points는 `accumulation points` 또는 `cluster points`라고 부르기도 한다.

### 참고3
모든 limit point는 adherent point이다.

하지만 그 역은 성립하지 않는다.

### 명제1
Topological space $X$와 $X$의 subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ U \text{ is closed} \iff U \text{ contains all of its limit points} $$

**Proof**
[$\implies$]  
adherent point의 성질에 의해 다음이 성립한다.

$$ U \text{ is closed} \iff U \text{ contains all of its adherent points} $$

모든 limit point는 adherent point임으로, $U$가 모든 adherent point를 가지고 있다는 말은 모든 limit point를 가지고 있다는 말과 동치이다. $\qed$

[$\impliedby$]  
다음을 가정하자.

$$ U \text{ is not a closed set of } X $$

그러면 다음이 성립한다.

$$ \begin{aligned} & X-U \text{ is not a open set of } X \\\iff& \exist x \in X-U \st \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \nsubseteq X-U \\\iff& \exist x \in X-U \st \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \cap U \neq \empty \end{aligned} $$

위를 만족하는 $x\in X-U$에 대해 다음이 성립한다.

$$ \begin{aligned} & \forall\mathcal{N_x} \in \Set{\mathcal{N_x}}, \quad \mathcal{N_x} \text{ contains a point of } U \text{ other than } x \\\iff& x \text{ is ab limit point of } U  \end{aligned} $$


하지만 이는, $U$의 모든 limit point가 $U$에 포함된다는 전제에 모순이 된다.

따라서, proof by contradiction에 의해 다음이 성립한다.

$$ X-U \text{ is an open set of } X $$

그럼으로 $U$는 $X$의 closed set이다. $\qed$

> Reference  
> [math.stackexchange](https://math.stackexchange.com/questions/162018/accumulation-points-cluster-points-closed-sets?rq=1)