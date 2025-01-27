# Python

파이썬에서 함수 내부에서 인자로 전달된 변수에 대한 재할당(assignment)이 이루어지면, 그 변수는 함수의 지역 범위에서만 수정된다.

만약 변경 사항을 함수 밖에서도 유지하고 싶다면, 함수가 값을 반환하도록 하고 호출하는 측에서 그 값을 받아 처리해야 한다.

가변 객체(Mutable Objects)인 경우
리스트(list), 딕셔너리(dict), 집합(set) 등 가변 객체는 내부 상태를 변경할 수 있다. 이 경우, 함수 내부에서 객체의 내용이 변경되면 원본 객체에도 영향을 미친다. 하지만 변수 자체를 재할당하는 경우라면 새로운 객체를 참조하게 되고, 원본은 변경되지 않는다.

```python
def func(lst):
    lst.append(4)  # 가변 객체의 내용을 변경하는 것. 원본에 영향.
    lst = [1, 2, 3]  # lst는 이제 새로운 리스트를 참조하게 된다. 원본에 영향 없음.

my_list = [0]
func(my_list)
print(my_list)  # [0, 4]
```

lst.append(4)는 가변 객체인 my_list의 내용을 직접 변경하므로 원본이 바뀌게 된다. 그러나 lst = [1, 2, 3]에서는 lst가 완전히 새로운 리스트 객체를 참조하도록 재할당되므로, my_list에는 영향을 주지 않는다.

---

python -m pip install --upgrade pip

pip install --upgrade matplotlib

pip show matplotlib
---


## Sympy
symbolic 배열 만들기
```python
P0 = sympy.symarray("P0",3) 
```

symarray 로 만들면 numpy.ndarray 타입으로 반환되서 diff 와 같은 함수를 사용할 수 없다. 이를 해결하기 위해 numpy.ndaraay 타입을 sympy.Matrix 타입으로 변환하려면 다음과 같이하면 된다.
```python
P0 = sympy.Matrix(sympy.symarray("P0",3))
```

## 그래프 그리기
matplotlib 의 pyplot 모듈을 import 하면 된다.
```python
import matplotlib.pyplot as plt
```

<details> <summary> <h3 style="display:inline-block"> 3차원 곡선 그래프 그리기 </h3></summary>
그래프를 띄울 창을 의미하는 figure 객체를 생성하기 위해 pyplot 모듈의 figure 함수를 호출해줘야 한다.
```python
fig = plt.figure()
```
만약 그래프를 여러개의 창에 띄우고 싶다면, 원하는 창의 개수만큼 figure 객체를 생성해야 한다.


다음으론 띄워진 창에 그래프가 그려질 공간를 의미하는 Axes 객체를 생성하기 위해 figure 객체의 add_subplot 함수를 호출해야 한다.
```python
ax  = fig.add_subplot(1,1,1,projection="3d")
```
앞에 2개의 인자는 nrows, ncols 로 하나의 창에 subplot 을 가로로 세로로 몇개씩 배치하겠냐는 의미다. 그리고 세번째 인자인 index 는 몇번째 subplot 에 위치할건지를 나타낸다. 그러면 return 값으로 axes 객체를 받는다. 만약, 하나의 창에 여러개의 그래프를 띄우고 싶다면 add_subplot 에서 nrows, ncols, index 변수를 잘 조절해서 subplot 들을 추가해주면 된다.

참고로, matplotlib 최신 버전에서는 문제가 없지만, 이전 버전을 사용해야 되서 Axes3D 관련 오류가 발생할 경우 추가로 mpl_toolkits.mploft3d 모듈에 있는 Axes3D 를 import 해줘야 한다
```python
from mpl_toolkits.mplot3d import Axes3D
```

> Reference  
> [matplotlib - Figure.add_subplot](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.add_subplot.html)  

이제, 그래프를 실제로 그리기 위해서는 axes 객체의 plot 함수를 호출해야한다.
```python
ax.plot(xs1,ys1,zs1, linestyle = 'dotted', color = 'black', label = r'$f_1$')
```
xs1,ys1,zs1 은 각 각 x,y,z 좌표를 모아놓은 배열이고 label 은 Legend 에 나타날 이름을 나타낸다. 이 떄, Label 에 수식을 사용하고 싶은 경우에는 $$로 감싸준다음에 수식을 latex 형태로 수식을 작성해주면 된다. 

만약, 하나의 axes 안에 여러개의 함수를 그리고 싶다고 한다면 plot 함수를 여러번 호출해주면 된다.
```python
ax = fig.add_subplot(1, 1, 1, projection="3d")
ax.plot(xs1,ys1,zs1, label="test")
ax.plot(xs1,zs1,zs1, label="test2")
```
> Reference   
> [matplotlib - axes.Axes.plot](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.plot.html)  


그래프의 Legend 를 출력하기 위해서는 axes 객체의 legend 함수를 호출해야 한다.
```python
ax.lengend()
```
* [matplotlib - matplotlib.axes.Axes.legend](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.legend.html)  

그래프 축에 lable 을 달기 위해서는 set_xlabel, set_ylabel, set_zlabel 함수를 호출하면 된다.
```python
ax.set_xlabel('X')
ax.set_ylabel('Y')
ax.set_zlabel('Z')
```

마지막으로 그래프를 화면에 띄우기 위해서는 pyplot 모듈의 show 함수를 호출해야 한다.
```python
plt.show()
```
</details>



## foramt string 출력

* 기존 `str.format()` 방식
```python
name = "Alice"
age = 25
print("안녕하세요, 제 이름은 {}이고 나이는 {}살입니다.".format(name, age))
```

* `%` 포매팅 방식
```python
name = "Alice"
age = 25
print("안녕하세요, 제 이름은 %s이고 나이는 %d살입니다." % (name, age))
```

* f-string 방식
```python
name = "Alice"
age = 25
print(f"안녕하세요, 제 이름은 {name}이고 나이는 {age}살입니다.")
```

Python의 f-string (포매팅 문자열) 은 Python 3.6부터 도입된 기능으로, 문자열 내에 변수 또는 표현식을 간단하고 가독성 있게 삽입할 수 있는 방법이다. 

f-string은 문자열 앞에 `f` 또는 `F`를 붙이고, 중괄호 `{}` 안에 변수를 넣어 사용한다.

## 정규 표현식

### 그룹화

그룹화는 말 그대로 그룹으로 묶어주는 것이다.

예를 들어 `12` 가 1번 이상 반복되는 문자열을 찾고 싶은 상황을 생각해보자.

```py
p = re.compile("12+")
m = p.match("1212")
```

이 경우에는 `1212` 모두가 match 되길 기대하지만, 실제로 m 은 `12` 만 matching 된다.

왜냐하면 정규식 메타문자들의 효략은 대개 한 문자에만 적용되기 때문이다.

만약, 메타문자의 효력을 모두에 적용시키려면 `12` 를 그룹화 해주면 된다.

```py
p = re.compile("(12)+")
m = p.match("1212")
```

이 경우에는 의도와 같이 `1212` 모두가 match 된다.

### 비 캡처 그룹

그룹화를 위해 소괄호를 반드시 써야 하는데, 굳이 캡처하고 싶지 않을 때 사용한다.

비 캡처 그룹은 `(?:<regex>)`와 같이 사용한다.

> Reference  
> [greeksharifa.github - 파이썬 정규표현식(re) 사용법 - 04. 그룹, 캡처](https://greeksharifa.github.io/%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D(re)/2018/07/28/regex-usage-04-intermediate/)  

### 부정형 전방탐색(negative lookahead)
```
(?!pattern)
```

특정 패턴이 앞에 존재하지 않는 경우에만 매칭을 허용하는 Zero-width Assertion 이다.

부정형 전방탐색은 현재 위치에서 특정 패턴이 앞에 오지 않는지를 검사한다.

만약 해당 패턴이 존재하지 않으면, 전방탐색은 성공하고 정규 표현식의 나머지 부분과 매칭을 시도한다.

만약 해당 패턴이 존재한다면 바로 매칭을 실패하게 된다.

예를 들어 앞에 공백을 무시하고 return 으로 시작하지 않는 문자열을 찾으려면 regex pattern 을 다음과 같이 작성 할 수 있다.

```
(\s*?!return\\b)\s+\w+\s+
```

### Metacharacters

* .
    * 모든 단일 문자를 의미한다.
    * 단 \n 만 매치되지 않는다. 그러나 re.DOTALL 플래그를 통해 이 조차 매치시킬 수 있다.
      
* \w
    * 워드 문자를 의미한다.
    * 알파벳 대소문자 (a-z,A-Z), 숫자 (0-9), 그리고 밑줄(_)
 
* \d
    * 숫자 (0-9)를 의미한다. 

* \S
    * 모든 비 공백 문자와 일치한다.
    * [^ \t\n\r\f\v]와 동등하다

* \b
    * 단어 경계와 일치한다.
    * \w 에 일치되는 한 문자와 그 뒤에 \W 에 일치되는 한 문자가 있으면 \W 자리에서 매칭된다.
    * \b 는  escape sequence 의 백스페이스와 일치한다. 따라서 그냥 문자열에 \b 로 쓰면 마치 \n 을 썻을 때 와 같이 백스페이스로 인식이 된다. 따라서 정확히 \b 를 나타내기 위해서는 문자열 앞에 r 을 붙여주거나 \\b 로 작성해주어야 한다.

> Reference  
> [docs.python - regex](https://docs.python.org/ko/3/howto/regex.html)  

### 비탐욕적 수량자
```
(.*?)
```

일반적으로 `*` 는 가능한 많이 매칭하려는 Greedy 특성을 가지고 있다.

그러나 `*?`로 사용하게 되면 가능한 적은 수의 문자와 매칭 하려고 한다.

### match
match 메서드는 처음부터 가능한 가장 긴 부분 까지 정규식과 매치되는지 조사한다.

```python
p = re.compile("[a-z]+")
m1 = p.match( "python" )
m2 = p.match( "3 python" )
```
m2의 경우 앞의 3이 정규식과 일치하지 않음으로 None이 된다.

### search
문자열 전체를 검색하여 정규식과 매치되는 부분문자열이 있는지 조사한다.

```python
p = re.compile("[a-z]+")
m1 = p.search( "pthon" )
m2 = p.saerch( "3 python" )
```

match 메서드를 사용했을 때와 다르게 뒤에 정규식과 일치하는 부분 문자열이 있음으로, None이 되지 않는다.


> Reference
> [jump to python - 08-2 정규 표현식 시작하기](https://wikidocs.net/4308)  

### ETC
* metacharacter 가 아닌 문자에 `\` 를 추가해 주어도 아무런 영향을 주지 않는다
    * EX `\=`
    * `=` 문자 자체를 의미한다.
    * 그러나 `\=` 라고 작성해도 표현식의 의미에는 영향을 주지 않는다.

* 정규표현식에서 공백은 무시되지 않는다.
    * `\s+ (.*) \s* ;` 와 `\s+(.*)\s*;`는 다르다.

## 리스트

### append
리스트의 끝에 항목을 더하는 함수이다.

형태는 다음과 같다.

```
append(x)
```

append 함수는 다음 표현식과 동등하다.

```
a[len(a):] = [x]
```

> Reference  
> [docs.python - 5. 자료 구조](https://docs.python.org/ko/3/tutorial/datastructures.html)  

### List Comprehension
일반적으로 1부터 10까지 정수를 순서대로 가지고 있는 리스트를 생성하는 코드는 다음과 같다.

```py
numbers = []
for n in range(1, 11):
    numbers.append(n)
```

리스트를 생성하는 컴프리헨션인 리스트컴프리헨션으로 표기하면 다음과 같다.

```py
[x for x in range(1, 11)]
```

리스트를 생성하는 방식은 대괄호([])를 통해 생성하는 방법은 동일하다. 

차이점은 컴프리헨션을 사용할 경우 대괄호 내부에 코드를 작성한다는 점이다. 

리스트 컴프리헨션에서 for 문 앞에 작성된 변수가 append 메소드에 인자로 들어가게 된다.

예를 들어, 2의 배수 10개를 가지고 있는 리스트를 리스트 컴프리헨션으로 생성하면 다음과 같다.

```py
[2*x for x in range(1,11)]
```

리스트 컴프리 헨션에는 조건을 걸 수 있으며 다음과 같은 형태를 갖는다.

```py
[ (inputVar) for (var) in (range) if (condition) ]
```

예를 들어, 1부터 10까지 짝수만 순차적으로 들어있는 리스트를 생성하는 리스트 컴프리 헨션은 다음과 같다.

```py
even_numbers = [x for x in range(1, 11) if x % 2 == 0]
```

이 리스트 컴프리 헨션을 풀어 쓰면 다음과 같다.

```py
even_numbers = []
for n in range(1, 10+1):
    if n % 2 == 0:
        even_numbers.append(n)
```

## Generate Comprehension

generate comprehension 은 list comprehension 과 유사하지만 list 가 아닌 generator 객체를 생성한다

generator 는 값을 한 번에 모두 메모리에 올리지 않고 필요할 때마다 값을 생성하기 때문에 메모리 효율이 높다.

generator 는 반복자이므로 for 루프, next() 함수 등을 통해 값을 순회할 수 있다.

### for loop with if

list 에서 특정 조건을 만족하는 Elem 만 for loop를 돌고 싶다고 하자.

그러면 다음과 같이 코드를 작성할 수 있다.

```py
for elem in list:
    if condition :
        //algorithm
```

같은 동작을 하는 코드를 generate comprehension 을 이용해서 작성하면 다음과 같다.

```py
for elem in (x for x in list if condition):
    // algorithm
```

> Reference  
> [제대로 파이썬 - 리스트 컴프리헨션](https://wikidocs.net/22805)




## strip

파이썬 라이브러리에서 사용할 수 있는 내장 함수의 일부로써, 원래 문자열의 시작과 끝에서 주어진 문자를 제거한다. 

기본적으로 strip() 함수는 문자열의 시작과 끝에서 공백을 제거한 후 반환한다. 

만약, 괄호 안에 특정 값을 넣을 경우에는 공백과 함께 해당하는 문자열을 제거할 수 있다.

``` python
# 공백을 제거하는 경우
string = "     abcde     "
string.strip()
 
>>> 'abcde'
 
 
# 특정 문자열을 제거하는 경우
string = "     abcde     "
string.strip('c')
 
>>> 'abde'
```

## enumerate

enumerate() 함수는 기본적으로 인덱스와 원소로 이루어진 튜플(tuple)을 만들어줍니다. 

따라서 인덱스와 원소를 각각 다른 변수에 할당하고 싶다면 인자 풀기(unpacking)를 해줘야 합니다.

```python
for entry in enumerate(['A', 'B', 'C']):
    print(entry)

(0, 'A')
(1, 'B')
(2, 'C')

for i, letter in enumerate(['A', 'B', 'C']):
    print(i, letter)

0 A
1 B
2 C

```

## join
join 함수의 형태는 다음과 같다

```
'delimiter'.join(list)
```

예를 들어 list 에 ['a','b','c']가 있다고 하자.

그러면 `', '.join(list)`의 결과는 'a, b, c' 형태의 문자열을 만들어서 반환해준다.


## 조건부 표현식

파이썬에서는 다음과 같은 형태로 조건부 표현식(ternary operator)을 사용할 수 있다

```
[value_when_true] if [condition] else [value_when_false]
```

## Format String

파이썬에서는 다음과 같은 형태로 format string 을 사용할 수 있다.

```
format_string % (param1, ..., paramN)
```

## 변수 스코프

python 에서는 블록 스코핑을 사용하지 않고 함수나 클래스 단위의 스코핑을 사용하기 때문에 if-else 블록 외부에서도 그 안에서 생성한 변수에 접근이 가능하다.

```py
if condition:
    A = 1
else:
    A = 2

print(A)  # 옳바른 코드
```


## False 로 형변환 되는 경우

* 빈 문자열
* None
* 0

## Set
set 은 Python 에서 사용되는 데이터 타입 중 하나로, 중복을 허용하지 않고, 순서가 없는 데이터의 집합을 표현하는 데 사용된다. 

* add
  * 요소를 추가한다.
  * 이미 존재하는 요소의 경우 아무 일도 발생하지 않는다.
* remove
  * 요소를 제거한다.
  * 존재하지 않는 요소의 경우 KeyError 가 발생한다.
* discard
  * 요소를 제거한다.
  * 존재하지 않는 요소의 경우 아무 일도 발생하지 않는다.
* difference
  * set1.difference(set2) = set1 - set2
  * set1 에만 존재하는 요소들의 set 을 반환한다

### 생성

```py
my_set = set()
my_set_with_value = {1,2,3}
```



수학에서의 집합과 유사한 개념으로 생각할 수 있습니다. set은 중복된 요소를 자동으로 제거하며, 요소의 순서에는 신경 쓰지 않습니다.

