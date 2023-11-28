# Homogeneous Representation
## Line in Plane
### Motivation
평면상의 한 점은 coordinate를 이용해서 $\R^2$의 vector와 1:1 대응 관계를 갖는다.

그렇다면 평면상의 직선도 vector와 1:1 대응을 시킬수는 없을까?

### observation
$\R^3$의 subset $\mathbb{L}^2$를 다음과 같이 정의하자.

$$ \mathbb{L}^2 := \Set{ (a,b,c) \in \R^3 \mid (a,b) \neq (0,0) } $$

그러면 평면상의 임의의 직선을 $l$이라고 할 때, $l$을 어떤 $(a,b,c) \in \mathbb{L}^2$에 대해서 $ax+by+c=0$로 표현할 수 있다.

#### Remark
1. $k \in \R - \Set{0}$에 대해서 $ax+by+c =0$과 $ka+kb+kc=0$은 같은 직선을 나타낸다.
2. $ax+by+c = 0$은 $a'x+b'y+1=0$으로 나타낼 수 있음으로 line의 DOF는 2이다.
3. 평면의 직선의 집합을 $\mathcal{L}^2 := \Set{ax+by+c=0 \mid (a,b,c) \in \mathbb{L}^2}$

### Try1
$\mathcal{L}^2$의 임의의 element를 $l:ax+by+c=0$이라고 하자.

이 떄, $l$을 $(a,b,c) \in \mathbb{L}^2$에 대응시키기 위해 함수 $\phi$를 다음과 같이 정의하자.

$$ \phi : \mathcal{L}^2 \rightarrow \mathbb{L}^2 \st ax+by+c=0 \mapsto (a,b,c) $$

이 때, $\mathcal{L^2}$의 두 element $l_1 : x+y+1 =0, l_2 : 2x+2y+2=0$을 생각해보자.

그러면 $l_1=l_2$이지만 $\phi(l_1) = (1,1,1), \phi(l_2) = (2,2,2)$임으로 $\phi(l_1) \neq \phi(l_2)$임으로 $\phi$는 well-defined 되지 않는 문제가 생긴다.

#### Remark
1. well-defined 문제를 해결하기 위해서는 $(1,1,1),(2,2,2) \in \mathbb{L^2}$를 같게 볼 수 있어야 한다. 이는 "같다"라는 개념을 확장한것으로 볼 수 있으며 수학에서 'equivalence relation'이 이와 같은 역활을 수행할 수 있다.

### Try2
$\mathbb{L}^2$위의 equivalence relation $R$을 다음과 같이 정의하자.

$$ R := \Set{(x,y) \in \mathbb{L}^2 \mid x = ky \text{ for some } k \in \R - \Set{0}} $$

이 떄, 함수 $\phi$를 다음과 같이 정의하자.

$$ \phi : \mathcal{L^2} \rightarrow \mathbb{L^2}/R \st ax+by+c=0 \mapsto [(a,b,c)] $$

그러면 $\phi$는 well defined bijective function이다.

#### Proposition
위에서 정의한 $\phi$가 well-defined bijective function임을 증명하여라.

#### Remark
1. $\mathbb{L}^2/R$ 위에서는 자연스러운 연산이 잘 정의 되지 않는다.
$[(1,0,0)]+ [(1,0,1)] = [(2,0,1)]$이고 $[(8,0,0)]+ [(-1,0,-1)] = [(7,0,1)]$인데 $[(2,0,1)] \neq [(7,0,1)]$이다.
2. 비록 point 처럼 vector와 bijective function을 찾아내지는 못했찌만 vector의 equivalence class인 homogeneous vector와 bijective function을 찾아내었다.
3. $\mathbb{L}^2$에 $(0,0,c)$ 형태의 element는 없음으로 $\mathbb{L}^2/R$에서도 $[(0,0,c)]$ 형태의 element는 없다

### Homogeneous representation of line in plane
$\mathcal{L}^2$의 임의의 element를 $l:ax+by+c=0$이라고 하자.

이 떄, $l$의 homogeneous representation $[v_l]$은 다음과 같이 정의한다.

$$ [v_l] := \phi(l) = [(a,b,c)] \in \mathbb{L^2}/R $$

#### Remark
1. 직선 $l$의 homogenoeus representation $[v_l] = [(a,b,c)]$이 있을 떄, $(a,b)$는 $l$의 수직인 방향을 나타낸다.

### Homogenizing
https://math.stackexchange.com/questions/17009/what-does-homogenisation-of-an-equation-actually-mean