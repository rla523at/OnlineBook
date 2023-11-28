# Stress Tensor
## 정의
Cauchy's stress principle에 의해 주어진 점 $x$와 시간 $t$에서 $t_n$은 $n$에 의해 결정된다. 

따라서 다음과 같은 함수 $\sigma$를 생각해보자. 

$$ \sigma : \R^3 \rightarrow \R^3 \quad s.t. \quad  n \mapsto t_n $$

이 때, $\sigma$는 linear map이며 이를 `응력텐서(stress tensor)`라 한다.

### 명제1
$\R^3$공간의 standard basis을 $\epsilon$이라 하자.

이 때, 다음을 증명하여라.

$$ \sigma (n^i \epsilon_i) = n^i \sigma (\epsilon_i)$$

**Proof**

```{figure} _image/0301.png
```

Normal vector가 $\epsilon_i$와 평행한 면의 면적을 $\Delta A_i$라 하고 $n$과 평행한 면의 면적을 $\Delta A_n$이라 하자.

이 때, 운동 방정식은 다음과 같다.

$$ \sum_{i=1}^3(\Delta A_it_{-\epsilon_i})  + \Delta A_n{t_{n}}  + \Delta Vf_b = \rho \Delta V a $$

이 때, $\Delta x^i \enspace i = 1, 2, 3$이 충분히 작다고 하면 다음이 성립한다.

$$ \Delta V \ll \Delta A $$

따라서 운동방정식 양변을 $\Delta A_n$으로 나눠주면 다음과 같이 간단해 진다.

$$ \sum_{i=1}^3 \left(\frac{\Delta A_i}{\Delta A_n}t_{-\epsilon_i} \right) + {t_{n}} = 0 $$

Cauchy's stress principle에 의해 다음이 성립한다.

$$ \sum_{i=1}^3 \left(\frac{\Delta A_i}{\Delta A_n}t_{\epsilon_i} \right) = {t_{n}}  $$

이 때, $\frac{\Delta A_i}{\Delta A_n} = n^i$임으로 다음이 성립한다.

$$ {t_{n}} = n^i{t_{\epsilon_i}} $$

따라서, $\sigma$의 정의에 의해 다음이 성립한다.

$$ \begin{aligned}  \sigma (n^i \epsilon_i) &= t_n \\ &= n^i t_{\epsilon_i} \\ &= n^i \sigma (\epsilon_i) \qed \end{aligned} $$

#### 따름명제1
$\R^3$공간의 임의의 기저 $\beta$가 있다고 하자.

이 때, 다음을 증명하여라.

$$ \sigma (n^i \beta_i) = n^i \sigma (\beta_i)$$

**Proof**

$B$를 $\epsilon$에서 $\beta$으로 변환하는 기저변환 행렬이라고 하면 다음이 성립한다.

$$ \beta_i = B^j_i\epsilon_j $$

따라서 명제1에 의해 다음이 성립한다.

$$ \begin{aligned} \sigma(n^i\beta_i) &= \sigma(n^iB^j_i\epsilon_j) \\ &= n^i\sigma(B^j_i\epsilon_j) \\ &= n^i \sigma (\beta_i) \qed \end{aligned} $$

#### 따름명제2
다음을 증명하여라.

$$ \sigma \text{ is an linear map} $$

**Proof**

$v_1,v_2 \in \R^3$가 있다고 하자.

$v_1 = a^i\beta_i, v_2 = b^i\beta_i \in \R^3, \enspace c \in \R$라 하면 명제1에 의해 다음이 성립한다.

$$ \begin{aligned} \sigma (v_1 + cv_2) &= \sigma ((a_i + cb_i)\beta_i) \\ &= (a_i + cb_i) \sigma (\beta_i) \\ &= \sigma (v_1) + c \sigma (v_2)\end{aligned} $$

따라서, $\sigma$는 linear map이다. $\qed$

### 명제2
$\R^3$공간의 임의의 기저 $\beta$가 있다고 하자.

$\frak m^\beta_\beta(\sigma) = T$라 할 떄, 다음을 증명하여라.

$$ \sigma(\beta_i) = t_{\beta_i} = T^j_i\beta_j $$

**Proof**

Stress tensor의 정의에 의해 다음이 성립한다.

$$ \frak m^\beta_\beta(\sigma) = \begin{bmatrix} \frak m_\beta(\sigma(\beta_1)) & \cdots & \frak m_\beta(\sigma(\beta_3)) \end{bmatrix} = \begin{bmatrix} \frak m_\beta(t_{\beta_1}) & \cdots & \frak m_\beta(t_{\beta_3}) \end{bmatrix} $$

따라서 다음이 성립한다.

$$ t_{\beta_i} = \sigma^j_i\beta_j $$

#### 참고
기저 $\beta_{1,2,3}$를 축으로 갖는 정육면체에 $\sigma$가 주어져 있다고 하자.

그러면 $T^j_i$는 $\beta_i$를 normal vector로 갖는 평면에 작용하는 stress vector의 $\beta_j$방향의 coordinate다.

이를 그림으로 표현하면 다음과 같다.

```{figure} _image/0302.png
```

### 명제3
주어진 $x,t$에서 stress tensor $\sigma$가 주어졌다고 하자.

$\R^3$공간의 임의의 basis를 $\beta$라 하고 $\frak m^\beta_\beta(\sigma) = T$라 할 떄, 다음을 증명하여라.

$$ T \text{ is symmetric.}$$

**Proof**

아래 그림과 같이 $\beta$를 축으로 하는 정육면체의 중심점에 $\sigma$가 작용하고 있다고 하자.

```{figure} _image/0303.png
```

중심점에서 torque와 angular momentum을 계산하면 다음과 같다.

$$ \begin{aligned} (M_A)_3 &= \sigma^2_1(\Delta x^2)(\Delta x^3)(\Delta x^1 / 2) + (\sigma^2_1 + \Delta \sigma^2_1)(\Delta x^2)(\Delta x^3)(\Delta x^1 / 2) \\ &- \sigma^1_2(\Delta x^1)(\Delta x^3)(\Delta x^2 / 2) - (\sigma^1_2 + \Delta \sigma^1_2)(\Delta x^1)(\Delta x^3)(\Delta x^2 / 2) \\ I_{33} &= \Delta x^1 \Delta x^2 \Delta x^3((\Delta x^1)^2 + (\Delta x^2)^2) \end{aligned} $$

중앙점에서 $x_3$ 방향의 모멘트 평형 방정식을 고려하면 다음과 같다.

$$ \begin{aligned} (M_A)_3 &= I_{33}\alpha \\ \sigma^2_1 + \Delta \sigma^2_1 - \sigma^1_2 - \Delta \sigma^1_2 &= \alpha ((\Delta x^1)^2 + (\Delta x^2)^2) \end{aligned} $$

미소 값을 무시하면 다음과 같다.

$$ \begin{gathered} \sigma^2_1 - \sigma^1_2 = 0 \\ \therefore \sigma^2_1 = \sigma^1_2 \end{gathered} $$

다른 방향으로의 모멘트 평형 방정식을 고려하면 다음의 결론을 얻을 수 있다.

$$ \sigma^3_1 = \sigma^1_3, \sigma^3_2 = \sigma^2_3 $$

따라서 $\sigma$는 대칭이며 6개의 독립적인 응력성분을 갖는다.

> Q. standard basis 말고는 성립안하는 특징인가?

> Reference  
[book] (Lai et al) Introduction to Continuum Mechanics Chapter4.4  