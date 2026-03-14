# Equivalence Class
## 정의
집합 $S$와 동치 관계 $R \subseteq S \times S$가 있다고 하자.

$s \in S$에 대해 `동치류(equivalence class)` $[ s ]_ R$는 다음과 같이 정의된 집합이다.

$$ [s]_R := \Set{t \in S | s \sim_R t} $$

### 참고
Equivalence relation이라는 기준에 의해 관계가 있는 element를 모은 subset을 equivalence class라고 한다.

### 명제1

집합 $S$와 동치 관계 $R \subseteq S \times S$가 있을 때, $s \in S$에 대해 $[s]_ R \neq \empty$을 증명하여라.

**proof**

$$ s \in [s]_ R \quad (\because R \text { satisfy reflextive}) \quad {_\blacksquare} $$

### 명제2

집합 $S$와 동치 관계 $R \subseteq S \times S$가 있다고 하자.

$x, y \in S$ 에 대해 다음을 증명하여라.

$$ [x]_ R \cap [y]_ R \neq \empty \implies [x]_ R = [y]_ R$$

**proof**

$$ \begin{aligned} & z \in [x]_ R \cap [y]_ R \\ \Leftrightarrow \enspace & x \sim_R z \land y \sim_R z \\ \Leftrightarrow \enspace & x \sim_R z \land z \sim_R y & \quad & (\because R \text{ satisfy symmetric}) \\ \Leftrightarrow \enspace & x \sim_R y & \quad & (\because R \text{ satisfy transitive}) \quad {_\blacksquare} \end{aligned} $$

따라서, 보조명제에 의해 다음이 성립한다.

$$ [x]_ R \cap [y]_ R \neq \empty \implies [x]_ R = [y]_ R$$

#### 보조명제
다음을 증명하여라.

$$x \sim_R y \Rightarrow [x]_ R = [y]_ R$$

**proof**

[ $[x]_ R \subseteq [y]_ R$ ]  
$$ \begin{aligned} & z \in [x]_ R \\ \Rightarrow \enspace & x \sim_R z, \quad x \sim_R y \\ \Rightarrow \enspace & z \sim_R y \quad (\because R \text{ satisfy transitive}) \\ \Leftrightarrow \enspace & z \in [y]_ R \quad {_\blacksquare} \end{aligned} $$

[ $[y]_ R \subseteq [x]_ R$ ]  
$$ \begin{aligned} & z \in [y]_ R \\ \Rightarrow \enspace & z \sim_R y, \quad x \sim_R y \\ \Rightarrow \enspace & z \sim_R x \quad (\because R \text{ satisfy transitive}) \\ \Leftrightarrow \enspace & z \in [x]_ R \quad {_\blacksquare} \end{aligned} $$