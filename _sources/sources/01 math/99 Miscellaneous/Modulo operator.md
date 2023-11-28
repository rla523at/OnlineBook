# Modulo operator

## Property1
임의의 세 정수를 $a,b,c$라고 하자. $b \neq 0$일 때, 다음을 증명하여라.

$$ (a\bmod b + c\bmod b)\bmod b  = (a+c)\bmod b$$

**Proof**

division algorithm에 의해 $a,b$를 다음과 같이 표현할 수 있다.

$$ a = q_1b + r_1, b= q_2b + r_2 $$

그러면 다음이 성립한다.

$$ (r_1 + r_2) \bmod b = ((q_1+q_2)b + r_1 + r_2) \bmod b \qed $$