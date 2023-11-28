# Compile

컴파일의 대상은 TU이다. 즉, 소스 파일이다. 헤더 파일은 특정 소스 파일이 빌드되면서 #include 전처리기 지시문에 의해 그 내용이 포함 될 뿐이다.

## Phase 7: 해석

이 단계에서는 `해석유닛(translation unit, TU)`을 `문법 분석(syntax analysis)`하여 `어셈블리 코드를 생성(code generation)`한다.

### 문법 분석

문법 분석은 원본 언어의 문법을 이해하고 원본 언어를 `추상 문법 트리(abstract syntax tree, AST)`를 만드는 단계이다. 문법 분석 단계도 `토큰화(tokenizing)`와 `파싱(parsing)`이라는 두 단계로 나뉘어 진다. 이를 의사코드로 나타내면 다음과 같다.

```
function syntaxAnalyzer(originCode) 
{
  var tokens = tokenizer(originCode)
  var AST = parser(tokens)
  return AST
}
```

#### 토큰화

토큰화 또는 `어휘 분석(lexical analysis)`, `스캐닝(scanning)`으로 불리며 문법 분석의 첫 단계이다. 토큰화는 공백이나 주석은 무시하고 원본 언어의 문자들을 언어의 문법에 정의된 기본 요소(토큰)들로 분류하는 것이다. 토큰화 되면, 토큰들은 프로그램의 기본 원소가 되며, 컴파일러의 입력도 토큰 스트림이 된다. 이를 의사코드로 나타내면 다음과 같다.

```
function tokenizer(originCode) 
{
  var tokens = [] // logic
  return tokens
}
```

토큰들은 각각 특정 분류 또는 유형으로 나뉜다. `while`은 키워드, `count`는 식별자, `<=`는 연산자라는 식이다.

아래의 예제를 보자.

```
//originCode
while (count <= 100) 
{
  count++;
}

//tokenizer에 의한 token 출력
while
(
count
<=
100
)
{
count
++
;
}
```

#### 파싱

문법 분석의 마지막 단계는 파싱 단계다. 파서는 토큰화 결과로 나온 언어 기본 요소 스트림을 언어의 문법 규칙에 맞춰 텍스트와 문법 규칙 사이의 정확한 대응 관계를 결정하는 단계다. 문법 규칙이 계층적이기 때문에 파서가 생성하는 출력은 AST라고 불리는 트리 기반 데이터 구조로 기술된다. 이를 의사코드로 나타내면 다음과 같다.

```
function parser(tokens) 
{
  var AST = {} // logic
  return AST
}
```

그리고 이전 예제의 토큰 스트림으로 AST를 만들면 다음과 같다.

```
statement
└─ whileStatement
   ├─ while
   ├─ (
   ├─ expression
   │  └─ count <= 100
   ├─ )
   └─ statement
      ├─ {
      └─ statementSequence
         ├─ statement
         │  └─ count++
         └─ ;
```

### 코드 생성

코드 생성은 AST를 통해 프로그램의 의미(semantics)를 찾고 코드를 생성한다. 코드 생성도 `가상 코드 생성(virtual code generation)`과 `대상 코드 생성(target code generation)`의 두단계로 나뉘며 이를 의사코드로 표현하면 다음과 같다.

```
function codeGenerator(AST) 
{
  var virtualCode = virtualCodeGenerator(AST)
  var targetCode = targetCodeGenerator(virtualCode)
  return targetCode
}
```

#### 가상 코드 생성

코드 생성의 첫 단계는 가상 코드 생성이다. 코드 생성을 할 때는 데이터 번역과 명령 번역이라는 두 단계에 집중하는 데, 명령 번역을 위해 우선적으로 가상 코드를 생성하게 된다. 이를 의사코드로 나타내면 다음과 같다.

```
function virtualCodeGenerator(AST) 
{
  var virtualCode = {} // logic
  return virtualCode
}
```

가상 코드 생성에 대한 예제는 다음과 같다.

```
x + g(2, y, -z) * 5 

//AST
+
├─ x
└─ *
   ├─ g
   │  ├─ 2
   │  ├─ y
   │  └─ -
   │     └─ z
   └─ 5

//가상 코드
push x
push 2
push y
push z
neg
call g
push 5
call multiply
add
```

#### 대상 코드 생성

코드 생성의 마지막 단계는 대상 코드 생성이다. 대상 코드 생성기는 가상 코드를 기반으로 대상 코드를 생성하게 되며 의사코드는 다음과 같다.

```
function targetCodeGenerator(virtualCode) 
{
  var targetCode = '' // logic
  return targetCode
}
```

### 특징

한 가지 중요한 점은 컴파일러가 어셈블리 코드를 생성할 때에는 모든 TU들을 독립적으로 생성한다는 점이다.

따라서 TU1 과 TU2가 있을 때 TU1의 어셈블리는 딱 TU1 만을 보고 결정되지 다른 TU들은 보지 않는다는 것이다. 그렇다면 문제가 생길 수 있는데 ODR 규칙에 따르면 inline이 아닌 함수의 정의는 전체 TU들에 대해 유일해야 한다. 이 떄 아래와 같이 TU1 에서 TU2에 정의된 함수 SomeFunction 사용한다면 TU1의 코드 생성 단계에서는 함수를 호출 할 때 해당 함수가 어디 있는지 알아야 하는데 해당 함수는 TU2에 정의되어 있기 때문에 도무지 알 수 없는 문제가 발생한다.

```
// main.cpp TU 1
#include <test.h>
int main() { SomeFunction(); }

// test.h
int SomeFunction();  // 선언

// test.cpp TU 2
#include <test.h>
int SomeFunction() { return 0; }
```

이러한 문제는 실행 파일을 만들 때 링킹 단계에서 목적파일들을 재배치 시켜 우리가 정의하였던 함수들의 위치가 비로소 정해지게 함으로써 해결된다.

```
$ file b.o
b.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
```

리눅스 상에서 file 프로그램을 사용하면, 해당 파일의 대략적인 정보를 알 수 있다. file 프로그램에 따르면 우리가 생성한 목적 파일은 일반적인 ELF 파일이다. 다만 나와 있듯이 `리틀 엔디안(LSB)` 형식의 `relocatable` 파일이다. 이 재배치 가능하다(relocatable)라는 의미는 이 ELF 파일을 특정 위치에 배치할 수 있다는 의미이다. 이로써 링킹 단계에서 이 생성된 목적파일들을 재배치 시켜서 정확한 위치에 자리잡게 한다.

TU1의 목적 코드를 objdump 라는 프로그램을 사용해서 그 어셈블리를 출력한 뒤 SomeFunction() 을 호출하는 부분을 보면 다음과 같다.

```
   8:    e8 00 00 00 00           callq  d <main+0xd>
```

원래는 e8 뒤에 현재 위치로 부터 얼마만큼 떨어져 있는 곳에 있는 함수를 실행할지 그 오프셋 값이 들어가 있어야 한다. 하지만 지금은 위와 같이 그냥 0 으로 채워져있다. 왜냐하면 컴파일 단계에서는 SomeFunction이 도대체 어디에 배치될 지 알 수 없기 때문에 링킹 과정이 이루어지기 전 까지 e8 뒤에 어떤 오프셋 값을 넣어야 하는지는 알 수 없다. 그래서 위 처럼 그냥 0 으로 채워놓게 된다.

만약에 TU2를 포함하지 않고 TU1만 가지고 실행파일을 생성하려고 한다면 링커에서 오류가 발생한다. 그리고 링킹 과정에서 SomeFunction을 찾을 수 없다면 해당 부분을 채울 수 없게 된다. 따라서 종종 보이는 함수를 찾을 수 없다 라는 오류가 컴파일러 단에서 발생하는 것이 아니라 링크 단에서 발생하는 이유도 같은 맥락이다.

물론 링커 입장에서 어셈블러가 생성한 명령이 정말로 e8 00 00 00 00 일 수 도 있기 때문에 목적 파일에는 링커에게 알려주기 위해서 이 부분을 이런 식으로 고쳐라 라는 정보를 남겨놓게 된다. 이 정보를 보기 위해서는 readelf 프로그램을 사용하면 된다.

```
$ readelf -r b.o

Relocation section '.rela.text' at offset 0x230 contains 1 entry:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
000000000009  000b00000004 R_X86_64_PLT32    0000000000000000 _Z12SomeFunctionv - 4

Relocation section '.rela.eh_frame' at offset 0x248 contains 1 entry:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
000000000020  000200000002 R_X86_64_PC32     0000000000000000 .text + 0
```

readelf 프로그램은 리눅스에서 ELF 파일 정보를 보기 좋게 출력해주는 프로그램이다. -r 옵션을 주게 되면 해당 파일의 `재배치 테이블(relocation table)` 정보를 출력하게 된다. 이 재배치 테이블 중 .rela.text에선 해당 목적 파일에서 링킹 시에 수정해야할 곳의 위치와 어떠한 식으로 수정해야 할 지에 대해서 설명되어 있다.

위 경우 오프셋 9의 위치에 (정확히 e8 바로 뒤에 00 00 00 00 부분을 의미 한다) \_Z12SomeFunctionv 심볼의 정보를 R\_X86\_64\_PLT32 방식으로 덮어 씌우라고 링커에게 알려준다.


## Phase 8: 인스턴스 유닛 생성

컴파일러는 생성된 TU를 분석해서 필요로 하는 템플릿 인스턴스들을 확인한다. 템플릿들의 정의 위치가 확인이 되면 해당 템플릿들의 인스턴스화가 진행이 되고 이를 통해서 `인스턴스 유닛(instantitaion unit)`이 생성된다.

이 단계를 마치게 되면 컴파일러는 비로소 목적 코드를 생성할 수 있고 이때 생성된 목적 코드는 마지막 단계인 링킹 단계를 위해서 링커로 전달된다.

> Reference   
[모두의 코드](https://modoocode.com/319)  
[st 블로그](https://st-lab.tistory.com/176?category=872072)  
[컴파일러 chodragon 블로그](https://chodragon9.github.io/blog/compiler-theory/#%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC-%EA%B0%9C%EB%85%90)  
[Phase of translation MSDN](https://docs.microsoft.com/en-us/cpp/preprocessor/phases-of-translation?view=msvc-170)  
[C++ 표준 문서](http://eel.is/c++draft/lex.phases)