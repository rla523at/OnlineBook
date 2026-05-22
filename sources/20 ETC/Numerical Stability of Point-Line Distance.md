# 점과 직선사이의 거리 계산 방식에 따른 수치안전성

## 문제 설정
임의의 점 $A$ 와 직선이 있을 때 $A$ 와 직선사이의 거리를 구해보자.

직선 위의 서로 다른 두 점을 $B,C$ 라고 하고 $B$ 에서 $A$ 로 가는 벡터를 $h$, $B$ 에서 $C$ 로 가는 벡터를 $d$ 라고 하자.

$A$ 가 직선에 수선의 발을 내렸을 때의 점을 $H$ 라고 하면 $B$ 에서 $H$ 로 가는 벡터 $b$ 는 $h,d$ 로 표현이 가능하다.

$$ b = \frac{d \cdot h}{d \cdot d} d $$

그러면 이제 점과 직선사이의 거리를 구하는 문제는 직각 삼각형의 높이를 구하는 문제로 바뀐다.

```
                    A
                    ●
                   /|
                  / |
                 /  | 
                /   |
               /    |
              /     |
           h /      |
            /       |
           /        |
          /         |
         /          |
        /           |
       /      b     |                 d
      ●------->-----●----------------->--●
      B             H                    C
```

### 두가지 계산식 

$b$ 와 $h$ 를 알고 있음으로 높이 구하는 방법은 두가지가 있다.

1. $\sqrt{h \cdot h - b \cdot b}$
2. $\sqrt{(h - b) \cdot (h-b)}$

1번은 피타고라스 방정식을 사용한 방법이고 2번은 $H$ 에서 $A$ 로 가는 벡터를 구해서 그 크기를 구하는 방법이다.

이 두 방법은 exact arithmetic에서는 동일하지만, floating-point arithmetic에서는 반올림 오차의 누적 방식이 달라 서로 다른 수치적 안정성을 가진다.

즉, 이 두 방법은 수학적으로 완전히 동일하지만 수치해석적으로는 다르다.

### 핵심 결론

결론부터 말하자면, floating-point arithmetic 하에서 2번 방식이 수치적으로 더 오차가 적은 방식이다.

아래 내용들에는 편의성을 위해 $sqrt$ 계산을 뺀 높이의 제곱을 구하고 있다.

## 수치 예시
$$ A=(96,0),\quad  B=(0,0),\quad C=(96,32)$$ 

라고 하면

$$ h=A-B=(96,0),\quad d=C-B=(96,32) $$

이다.
ㄴ
### exact arithmetic
위의 수식을 통해 계산하면 $b=(86.4, 28.8)$ 이다.

따라서
$$
h-b=(96,0)-(86.4,28.8)=(9.6,-28.8)
$$

이므로

$$
(h-b)\cdot(h-b)=9.6^2+28.8^2 = 921.6
$$

이다.

### floating-point arithmetic 

single precision 값을 사용해서 $b$ 를 계산하면

$$ b = ( 86.3999938965, 28.7999992371) $$

### 1번 방법

$$ h\cdot h= 9216 $$

이고,

$$ b\cdot b = 8294.3984375 $$

라서

$$ h\cdot h-b\cdot b = 921.6015625 $$

가 나온다.

### 2번 방법

$$ h-b = ( 9.6000061035, -28.7999992371) $$

라서

$$ (h-b) \cdot (h-b) = 921.6000366211 $$

### 절대 오차 비교

exact value가 (921.6) 이므로 absolute error는 각각

* 1번 방법:
  $$ |921.6015625-921.6|=0.0015625 $$

* 2번 방법:
  $$ |921.6000366210938-921.6|=0.00003662109375 $$

입니다.

즉, 이 예시에서는 1번 방법의 absolute error가 2번 방법보다 약 26배 크다.


## 반올림 오차 모델
부동소수점 연산에서 생기는 반올림 오차를 수학적으로 아주 간단하게 나타내는 방식이다.

$$ \operatorname{fl}(x \circ y) = (x \circ y)(1+\delta), \qquad |\delta| \le u
$$

* $\operatorname{fl}(\cdot)$
  * 실제 실수 연산 결과를 컴퓨터가 부동소수점으로 저장했을 때의 값

* $x \circ y$
  * 실수에서의 정확한 연산 결과이다.
  * $\circ$ 는 $+$, $-$, $\times$, $/$ 과 같은 binary operation 이다.

* $\delta$
  * 반올림 때문에 생긴 상대 오차이다.

* $u$
  * unit roundoff이다.
  * 부동소수점 반올림으로 인해 생길 수 있는 최대 상대 오차의 대략적인 상한이다.
  * 쉽게 말하면 “한 번의 부동소수점 연산이 보통 이 정도 이하의 상대 오차를 만든다”는 기준값이다.

### 모델 기반 1번 방법 분석
표기의 편의성을 위해 $q_h = h \cdot h$ 라고하고 $q_b = b \cdot b$ 라고 하자.

반올림 오차 모델을 적용하여 구해진 결과를 각 각 $\hat{q_h}, \hat{q_b}$ 라고 하면

$$ \hat{q_h} = \operatorname{fl}(q_h) = q_h(1+\delta_1) \\
\hat{q_b} = \operatorname{fl}(q_b) = q_b(1+\delta_2)
$$

이다.

그러면 1번 방법을 통해 구한 결과 $\hat{r_1}$ 은

$$ \hat{r_1} = \operatorname{fl}(\hat{q_h} - \hat{q_b}) = (\hat{q_h} - \hat{q_b})(1 + \delta_3) $$

이다.

$$ \hat{q_h} - \hat{q_b} = q_h - q_b + q_b \delta_1 - q_h\delta_2 $$

이고 $q_h - q_b$ 는 exact arithmetic 의 결과 $r$ 이라고 두면

$$ \hat{r_1} = r + q_b\delta_1 - q_h\delta_2 + r\delta_3 + O(\delta^2) $$

가 된다.

고차항은 무시하고 삼각부등식을 통해 오차를 구해보면

$$ \lvert \hat{r_1} - r \rvert \approx \lvert q_b\delta_1 - q_h\delta_2 + r\delta_3 \rvert \le \lvert q_b \rvert \lvert \delta_1 \rvert + \lvert q_h \rvert \lvert \delta_2 \rvert + \lvert r \rvert \lvert \delta_3 \rvert $$

이고, $u$ 를 machine epsilon 수준의 unit roundoff 라고 하면 각 $\lvert \delta_i \rvert \le u$ 임으로

$$ \lvert \hat{r_1} - r \rvert \le u( \lvert q_b \rvert + \lvert q_h \rvert + \lvert r \rvert) $$

### 모델 기반 2번 방법 분석
표기의 편의를 위해 $s = h-b$ 라고 두면 반올림 오차모델에 의해

$$ \hat{s} = \operatorname{fl}(h-b) = s + e  $$

라고 표현할 수 있다. $e$ 는 $s$ 를 계산할 때 들어간 반올림 오차 벡터임으로 

$$ \lVert e \rVert \le u\lVert s \rVert $$

를 만족한다.

그러면 2번 방법을 통해 구한 결과 $\hat{r_2}$ 는

$$ \hat{r_2} = \operatorname{fl}(\hat{s} \cdot \hat{s}) = (\hat{s} \cdot \hat{s})(1 + \delta_4) = (\hat{s} \cdot \hat{s}) + (\hat{s} \cdot \hat{s})\delta_4 $$

이다.

$$ \hat{s} \cdot \hat{s} = s \cdot s + 2 s \cdot e + e \cdot e $$

이고 $r = s \cdot s$ 라고 두면

$$ \hat{r_2} = r + 2s \cdot e + e \cdot e + (\hat{s} \cdot \hat{s})\delta_4 $$

이다.

삼각부등식을 통해 오차를 구해보면

$$ \lvert \hat{r_2} - r \rvert \le 2 \lVert s \rVert \lVert e \rVert + \lVert e \rVert^2 + \lVert \hat{s} \rVert^2 \delta_4 $$

이고 $\lVert \hat{s} \rVert^2 \sim \lVert s \rVert^2$ 이고 $\lvert \delta_i \rvert \le u$ 임으로 

$$ \lvert \hat{r_2} - r \rvert \le u \lVert s \rVert ^2 $$

### 비교 및 해석
1번방식의 오차의 상한은

$$ u( \lvert b \cdot b \rvert + \lvert h \cdot h \rvert + \lvert (h \cdot h) - (b \cdot b)  \rvert) $$

2번방식의 오차의 상한은

$$ u \lvert (h-b) \cdot (h-b) \rvert $$

이다.

2번 방식은 $h$ 와 $b$ 의 차이에 의해서만 오차의 상한이 결정되지만 1번 방식은 거기에 더해 $b$ 의 크기와 $h$ 의 크기에 의해서도 상한이 영향을 받는다.

즉, $h$ 와 $b$ 의 차이가 앞도적으로 크다면 1번과 2번방식이 큰 차이가 없을수 있지만 $h$ 와 $b$ 의 차이는 크지 않은데 $h$ 와 $b$ 의 크기가 큰 경우에는 오차의 크기는 1번 방식에서 크게 증가할 수 있다.

따라서, 2번 방식이 수치적으로 오차가 더 작은 방식이다.

## 요약

반올림 오차를 조심해야할 대표적인 패턴 
* 서로 매우 가까운 두 수를 빼는 경우
  예: $x-y$ 에서 $x \approx y$

* 큰 두 값의 차로 작은 결과를 만드는 경우
  예: $|b|^2-|a|^2$

반올림 오차 피하기
* 작은 값을 원한다면 가능하면 그 작은 값을 직접 계산하라. 큰 값 두 개를 따로 구해서 마지막에 빼지 말자.
* 수학적으로 동치인 식이 여러 개 있으면, “비슷한 큰 수의 차”를 만들지 않는 형태를 우선 선택하라.

## 테스트 코드

```py
from fractions import Fraction
import numpy as np

def make_fraction_point(coords):
    return tuple(Fraction(str(x)) for x in coords)

def make_float_point(coords):
    return tuple(np.float32(x) for x in coords)

def sub(u, v):
    return tuple(x - y for x, y in zip(u, v))

def scale(s, v):
    return tuple(s * x for x in v)

def dot(u, v):
    return sum(x * y for x, y in zip(u, v))

def projection_and_distance_sq(A, B, C):
    # h = B -> A
    # d = B -> C
    h = sub(A, B)
    d = sub(C, B)

    dd = dot(d, d)
    if dd == 0:
        raise ValueError("B와 C는 서로 다른 점이어야 합니다.")

    # b = projection of h onto d
    coeff = dot(d, h) / dd
    b = scale(coeff, d)

    # Method 1: h·h - b·b
    method1 = dot(h, h) - dot(b, b)

    # Method 2: (h-b)·(h-b)
    h_minus_b = sub(h, b)
    method2 = dot(h_minus_b, h_minus_b)

    return b, method1, method2

def fmt_fraction(x):
    if x.denominator == 1:
        return str(x.numerator)
    return f"{x.numerator}/{x.denominator}"

def fmt_fraction_vec(v):
    return "(" + ", ".join(fmt_fraction(x) for x in v) + ")"

def fmt_float(x):
    return format(x, ".17g")

def fmt_float_vec(v):
    return "(" + ", ".join(fmt_float(x) for x in v) + ")"

def scalar_abs_error(exact, approx):
    # exact: Fraction
    # approx: float
    approx_frac = Fraction.from_float(float(approx))
    return float( abs(approx_frac - exact) )


# -------------------------------------------------
# 사용자 입력: 원하는 점으로 바꾸면 된다.
# Exact Arithmetic을 위해 문자열 또는 정수 사용 권장
# -------------------------------------------------

A_input = ("96", "0")
B_input = ("0",  "0")
C_input = ("96", "1")

# -------------------------------------------------
# Exact Arithmetic
# -------------------------------------------------

A_exact = make_fraction_point(A_input)
B_exact = make_fraction_point(B_input)
C_exact = make_fraction_point(C_input)

b_exact, method1_exact, method2_exact = projection_and_distance_sq(
    A_exact, B_exact, C_exact
)

# -------------------------------------------------
# Floating-point Arithmetic
# -------------------------------------------------

A_float = make_float_point(A_input)
B_float = make_float_point(B_input)
C_float = make_float_point(C_input)

b_float, method1_float, method2_float = projection_and_distance_sq(
    A_float, B_float, C_float
)

# -------------------------------------------------
# 출력
# -------------------------------------------------

print("A =", A_input)
print("B =", B_input)
print("C =", C_input)
print()

print("Exact Arithmetic 에서")
print("b =", fmt_fraction_vec(b_exact))
print("1번 방법 값 =", fmt_fraction(method1_exact))
print("2번 방법 값 =", fmt_fraction(method2_exact))
print(f"약 ={float(method1_exact)}")

print("Floating-point Arithmetic 에서")
print("b =", fmt_float_vec(b_float))
print(f"1번 방법 값 ={fmt_float(method1_float)}, 오차 약= {scalar_abs_error(method1_exact, method1_float)}" )
print(f"2번 방법 값 ={fmt_float(method2_float)}, 오차 약= {scalar_abs_error(method2_exact, method2_float)}" )

```
