# Permutation


## Cyclic Notation

### Cyclic notation to permutation
$n$개의 index가 $a_1,\cdots,a_n$이 주어졌다고 하자.

cyclic notation $(a_1,\cdots,a_n)$의 의미는 $a_i \mapsto a_{i+1} (i=1,\cdots n-1)$에다가 $a_n \mapsto a_1$을 의미한다.

즉, permuted index는 다음과 같다.

$$ a_2,a_3,\cdots,a_n,a_1 $$

#### Example1
index가 $1,4,2,3,5$이고 cyclic notation이 $(1,3,2)(4,5)$로 주어지면 permuted index는 $3,5,1,2,4$이 된다.

### Permuted index to cyclic notation
index $a_1,\cdots,a_n$과 permuted index $b_1,\cdots,b_n$이 주어졌을 떄, cyclic notation을 알아내는 방법은 다음과 같다.

permutation 함수를 $\sigma$라 하고, $\sigma^n =\underbrace{\sigma \circ \cdots \circ \sigma}_{n}$라고 표기할 때

1. $a_i = a_1$으로 둔다.
2. $\sigma^k(a_i) = a_i$일 때, $(a_i, \sigma(a_i), \cdots, \sigma^{k-1}(a_i))$을 쓴다.
3. cyclic notation에 들어가있지 않은 $a_j$를 찾는다.
   1. $a_j$를 찾은 경우 $a_i = a_j$로 두고 2번 과정으로 돌아간다.
   2. $a_j$가 없는 경우 끝낸다.

#### Example1
$$  \begin{array}{ccc}\text{index}&\text{permutied index} & \text{cyclic notation} \\ 1,2,3 & 1,2,3 & (1)(2)(3) \\ &2,3,1 & (123) \\& 3,1,2 & (132) \\& 3,2,1 & (13)(2) \\& 2,1,3 & (12)(3) \\& 1,3,2 & (1)(23) \end{array} $$

> Reference  
> [wiki](https://en.wikipedia.org/wiki/Permutation#Cycle_notation)  