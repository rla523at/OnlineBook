# Preprocessing

## Phase 1: 문자들 해석하기

소스 파일에 있는 문자들을 해석한다. 기본적으로 C++ 코드에서는 총 96개의 문자들로 이루어진 basic source character set이 있는데, 이들은 다음과 같이 구성되어 있다.

-   5 종류의 공백 문자들 (스페이스, 탭, 개행 문자 등등)
-   10 종류의 숫자들 (0 부터 9 까지)
-   52 종류의 알파벳 대소문자
-   29 종류의 특수 문자들 (\_, {, + 등등)

이 기본 문자 셋에 포함되어 있지 않은 다른 모든 문자들은 $\\backslash$u 를 통해 유니코드 값으로 치환되거나, 컴파일러에 의해서 따로 해석된다. (적어도 GCC 의 경우 유니코드를 지원하므로 따로 치환되는 것은 아닌 것 같다.)

## Phase 2: $\backslash$ 문자 해석하기

백슬래시 ($\\backslash$) 문자가 문장 맨 끝 부분에 위치해있다면, 해당 문장과 바로 다음에 오는 문장을 하나로 합치고 개행 문자는 삭제한다.

## Phase 3: 전처리 토큰들로 분리하기

소스 파일을 주석(comment), 공백 문자, 전처리 토큰(preprocessing token)들로 분리한다. 전처리 토큰은 C++에서 가장 기본적인 문법 요소로 컴파일러가 사용하는 컴파일러 토큰의 근간이 된다. [전처리 토큰](http://eel.is/c++draft/lex.pptoken)은 다음을 포함한다.

-   [헤더 이름](http://eel.is/c++draft/lex.header#nt:header-name)
-   [키워드](http://eel.is/c++draft/lex.key)
-   [식별자](http://eel.is/c++draft/lex.name#nt:identifier)
-   문자/문자열 리터럴
-   연산자들 (+, #) 등...

이 단계에서 raw string literal을 확인해서 만일 1~2단계를 거치면서 해당 문자열 안의 내용이 바뀌었다면 그 변경은 취소된다. 또한 주석은 모두 공백 문자 하나로 변경된다. 참고로 컴파일러가 전처리기 토큰을 인식할 때에는 가능한 가장 긴 전처리 토큰을 만드려고 한다. 이러한 규칙을 `maximal munch` 라고 한다.

예를 들어서 아래의 코드를 보자.

```
int a = bar+++++baz
int bar = 0xE+foo
```

첫번째 줄은 `(bar++) + (++baz)`를 의도한 것이겠지만, maximal munch 규칙에 따라 컴파일러가는 가장 긴 전처리 토큰을 구성하려고 하기 때문에 `bar++ ++ +baz`로 해석되어 컴파일 오류가 발생한다. 마찬가지로 두번째줄은 `(0xE) + (foo)`를 의도한 것이겠지만 컴파일러의 경우 `0xE+ foo`로 해석해 오류가 발생한다. 그 이유는 부동 소수점 리터럴의 경우 `0xE+10`와 같이 E를 통해서 지수를 지정할 수 있기 때문이다.

## Phase 4: 전처리기 실행 단계

전처리기를 실행하는 단계로 아래의 일들을 수행한다.

-   #include 에 지정된 파일의 내용을 복사
-   #define 에 정의된 매크로를 사용해서 코드를 치환
-   #if, #ifndef 와 같은 구문들을 실행해서 코드를 치환
-   #pragma 와 같은 컴파일러 명령문들을 해석

이때, #include로 복사된 헤더 파일은 다시 Phase 1-4 단계를 거치며 이 과정은 소스 파일에 더이상의 전처리기 지시문이 없을 때 까지 지속된다.

이 단계가 끝난 소스 파일을 `해석 유닛(translation unit, TU)`이라고 한다. [C++표준](https://eel.is/c++draft/lex.separate), [wiki](https://en.wikipedia.org/wiki/Translation_unit_(programming))

### #define

#define 전처리기 지시문의 형식은 다음과 같다.

```
#define identifier token-string
```

#define은 컴파일러에게 소스 파일에서 identifier가 나타날 때마다 token-string으로 대체 하도록 지시하는 역활을 한다. 물론 identifier가 주석이나 문자열에 사용될 경우에는 대체를 하지 않는다. 대체가 되는 경우는 identifier가 토큰으로 사용될 경우뿐이다.

identifier는 함수 형식처럼 사용될 수도 있다.

```
#define identifier(identifier, ..., identifier) token-string
```

하지만 identifier를 함수 형식처럼 사용할 경우 아래 예시와 같이 의도와 다르게 동작할 수 있어 사용에 주의를 기울여야 한다.

```
#define Multiply(x,y) x * y

void main(void)
{
  const auto val1 = Multiply(2,3); 
  const auto val2 = Multiply(2,3) / Multiply(2,3); 
  //val1 = 2 * 3  = 6
  //val2 = 2 * 3 / 2 * 3 = 9 
  //val2는 원래 1을 예상하고 작성했겠지만 의도와 다르게 동작하는 경우가 생길 수 있음.
}
```

## Phase 5: 실행 문자 셋으로 변경하기

모든 문자들은 이전의 소스 코드 문자 셋에서 `실행 문자 셋(execution character set)`의 문자들로 변경된다. 마찬가지로 이전의 escaped 된 문자들도 실행 문자 셋의 문자들로 변경된다.

## Phase 6: 인접한 문자열 합치기

이 단계에서 인접한 문자열들이 하나로 합쳐진다. 예를 들어서

```
std::cout << "abc"
             "def";
```

의 경우

```
std::cout << "abcdef";
```

로 된다.

여기까지가 바로 전처리기 과정이다.