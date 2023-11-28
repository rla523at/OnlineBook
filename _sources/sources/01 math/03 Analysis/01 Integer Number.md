# Integer Number
## 정의
$\N$의 additive inverse의 집합을 $\N^-$이라 하자.

$$ \N^- := \Set{-n | \forall n \in \N, n + (-n) = 0} $$

이 때, 다음과 같이 정의된 집합 $\Z$를 `정수(interger number)`라고 한다.

$$ \Z = \N \cup \Set{0} \cup \N^- $$

### 명제1
$\Z$와 $\Z$의 not empty bounded below subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ U \text{ has an least element} $$

**Proof**

$U$가 bounded below임으로 다음이 성립한다.

$$ \exist m \in \Z \st \forall u \in U, \quad m \le u $$

이 떄, 집합 $V$를 다음과 같이 정의하자.

$$ V := \Set{ u-m | u \in U} $$

그러면 $\N$은 well ordered set이고 $V$는 $\N$의 subset임으로 다음이 성립한다.

$$ V \text{ has an least element } V_m $$

$V_m$은 least element임으로 다음이 성립한다.

$$ \begin{gathered} \forall u \in U, \quad V_m \le u -m \\ \exist u_m \in U \st u_m -m = V_m \end{gathered}  $$

따라서 다음이 성립한다.

$$ \begin{aligned} & \forall u \in U, \quad V_m \le u-m \\\iff & \forall u \in U, \quad u_m-m \le u-m \\\iff & \forall u \in U, \quad u_m \le u \end{aligned} $$

그럼으로, $u_m$은 $U$의 least element이다. $\qed$

> Reference  
> [proofwiki](https://proofwiki.org/wiki/Set_of_Integers_Bounded_Below_by_Integer_has_Smallest_Element)

### 명제2
$\Z$와 $\Z$의 not empty bounded above subset $U$가 있다고 하자.

이 떄, 다음을 증명하여라.

$$ U \text{ has an greatest element} $$

**Proof**

$U$가 bounded above임으로 다음이 성립한다.

$$ \exist M \in \Z \st \forall u \in U, \quad u \le M $$

이 떄, 집합 $V$를 다음과 같이 정의하자.

$$ V := \Set{ M-u | u \in U} $$

그러면 $\N$은 well ordered set이고 $V$는 $\N$의 subset임으로 다음이 성립한다.

$$ V \text{ has an least element } V_m $$

$V_m$은 least element임으로 다음이 성립한다.

$$ \begin{gathered} \forall u \in U, \quad V_m \le M-u \\ \exist u_m \in U \st M-u_m = V_m \end{gathered}  $$

따라서 다음이 성립한다.

$$ \begin{aligned} & \forall u \in U, \quad V_m \le M-u \\\iff & \forall u \in U, \quad M-u_m \le M-u \\\iff & \forall u \in U, \quad u \le u_m \end{aligned} $$

그럼으로, $u_m$은 $U$의 greatest element이다. $\qed$

> Reference  
> [proofwiki](https://proofwiki.org/wiki/Set_of_Integers_Bounded_Above_by_Integer_has_Greatest_Element)

### 명제3
다음을 증명하여라.

$$ \forall m,n \in \Z, \quad \exist z \in \Z \st zm \le n $$

**Proof**

[$0<m,0<n$]  
$z \in \N^-$이면 자명하게 성립한다. $\qed$

[$0<m,n<0$]  
$\Z$의 정의에 의해 다음이 성립한다.

$$ \exist z \in Z \st z \le n$$

전제에 의해 $0<m$이고 $z \in \N^-$임으로 다음이 성립한다.

$$ zm \le z $$

따라서 위 결과를 종합하면 다음이 성립한다

$$ \exist z \in Z \st zm \le n \qed $$

[$m<0,0<n$]  
$z \in \N^+$이면 자명하게 성립한다. $\qed$

[$m<0,n<0$]  
$\Z$의 정의에 의해 다음이 성립한다.

$$ \exist z \in \N \st |n| \le z $$

그리고 다음이 성립한다.

$$ \forall z \in \N, \enspace \forall m \in \Z - \Set{0}, \quad  z \le |mz| $$

따라서 위 결과를 종합하면 다음이 성립한다

$$ |n| \le |zm| $$

전제에 의해 $n,mz \in \N^-$임으로 다음이 성립한다.

$$ \exist z \in Z \st zm \le n \qed $$

### 명제4
다음을 증명하여라.

$$ \forall m,n \in \Z, \quad \exist z \in \Z \st m \le zn $$

**Proof**

[$0<m,0<n$]  
$\Z$의 정의에 의해 다음이 성립한다.

$$ \exist z \in \N \st m \le z$$

전제에 의해 $n \in \N$임으로 다음이 성립한다.

$$ z \le zn $$

따라서 위 결과를 종합하면 다음이 성립한다

$$ \exist z \in \Z \st m \le zn \qed $$

[$0<m,n<0$]  
$\Z$의 정의에 의해 다음이 성립한다.

$$ \exist z \in \N^- \st m \le |z| $$

전제에 의해 $n \in \N^-$임으로 위를 만족하는 $z$에 대해 다음이 성립한다.

$$ |z| \le zn $$

따라서 위 결과를 종합하면 다음이 성립한다

$$ \exist z \in \Z \st m \le zn \qed $$

[$m<0,0<n$]  
$z \in \N^+_0$이면 자명하게 성립한다. $\qed$

[$m<0,n<0$]  
$z = 0$이면 자명하게 성립한다. $\qed$