# Text Encoding
텍스트 인코딩은 문자를 컴퓨터가 이해할 수 있는 이진 데이터(0과 1의 조합)로 변환하는 방식이다.

컴퓨터는 모든 데이터를 바이너리 형식으로 처리하기 때문에, 사람이 읽을 수 있는 텍스트(예: 알파벳, 숫자, 한글)를 각각의 바이너리 문자 코드로 매핑하는 과정이 필요하다.

이 매핑을 텍스트 인코딩이라고 부르며 다양한 인코딩 방식이 있다.

## ASCII

ASCII (American Standard Code for Information Interchange) 는 7비트로 구성된 인코딩 방식으로 영어 알파벳, 숫자, 기호만 매핑 가능하다.

## ANSI
"ANSI"라는 용어는 엄밀히 말하면 특정 인코딩을 지칭하지 않지만, Windows 운영체제에서는 종종 "로케일 기반 인코딩"을 의미하는 용어로 사용된다.

ASCII(7비트 인코딩)의 확장판으로, 8비트(256개 문자)로 구성된 인코딩 방식이다.

128개 이하의 문자는 ASCII 문자 집합과 동일하고, 나머지 128개 문자는 각 코드 페이지에 맞춰 정의된다.

각 코드 페이지는 특정 언어와 지역에 맞는 문자를 정의하며 예를 들어 CP1252는 서유럽 언어를 위한 코드 페이지이며 CP949는 한국어를 지원하는 코드 페이지이다.

## Unicode
전 세계 모든 문자를 하나의 통일된 코드 체계로 정의한 표준이다.

코드 포인트는 숫자로 표현되며 이를 저장하거나 전송하기 위해서는 특정한 인코딩 방식이 필요하다.

예를 들어 코드 포인트 U+0041은 문자 A에 할당되며 코드 포인트 U+AC00은 한국어 문자 가에 해당한다.

## UTF-8
유니코드를 위한 가변 길이 문자 인코딩 방식 중 하나이다.

UTF-8 인코딩은 유니코드 한 문자를 나타내기 위해 1바이트에서 4바이트까지를 사용하며 영어 문자는 1바이트, 다른 언어의 문자는 2~4바이트를 사용한다.

## UTF-16
유니코드를 위한 가변 길이 문자 인코딩 방식 중 하나이다.

주로 사용되는 기본 다국어 평면 (BMP, Basic multilingual plane)에 속하는 문자들은 그대로 16비트 값으로 인코딩이 되고 그 이상의 문자는 특별히 정해진 방식으로 32비트로 인코딩이 된다.

## UTF-32
유니코드를 위한 고정 길이 문자 인코딩 방식이다.

이것은 모든 유니코드 문자를 4바이트로 표현한다.

## BOM
BOM(Byte Order Mark) 은 유니코드 텍스트 파일의 시작 부분에 인코딩 방식을 표시하는 특별한 비트 값이다.

예를 들면 다음과 같다.

| **인코딩 형식**   | **BOM 값**                | **설명**                      |
|------------------|--------------------------|------------------------------|
| **UTF-8**        | EF BB BF                 | UTF-8 파일임을 표시            |
| **UTF-16 LE**    | FF FE                    | Little Endian UTF-16          |
| **UTF-16 BE**    | FE FF                    | Big Endian UTF-16             |
| **UTF-32 LE**    | FF FE 00 00              | Little Endian UTF-32          |
| **UTF-32 BE**    | 00 00 FE FF              | Big Endian UTF-32             |

> Reference  
> [learn.microsoft - byte-order-and-byte-order-marks](https://learn.microsoft.com/en-us/globalization/encoding/unicode-standard#byte-order-and-byte-order-marks)  

## char

##  Locale

locale name 은 [여기](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c)에서 Language tag 를 살펴보면 된다.

> Reference  
> [learn.microsoft - UCRT Locale names, Languages, and Country/Region strings](https://learn.microsoft.com/en-us/cpp/c-runtime-library/locale-names-languages-and-country-region-strings?view=msvc-170)    

## String Literal

The encoding of unprefixed string literals in C++ is platform dependent.

MSVC 에서는 unprefixe string literal 의 encoding 방식으로 시스템의 code page 를 사용하는 것 같다.

시스템의 code page 는 Win32 API 의 GetACP 함수로 확인할 수 있다.

execution-charset-set 에 무엇을 설정하느냐에 따라 str = "가"; 가 실행될 때 encoding 방식이 결정된다.

이로 인해 str 에 저장되는 bit 가 결정된다.

```cpp
void print(const std::string& str)
{
for (const char c : str)
{
std::bitset<8> bs(c);
std::cout << std::uppercase << std::hex << bs.to_ulong() << " " ;
}
std::cout << "\n";
}

int main(void){
  std::string str;
  str = "가";

  print(str);
}
```

> Reference  
> [cppreference - string_literal](https://en.cppreference.com/w/cpp/language/string_literal)    
> [stackoverflow - What is the encoding of unprefixed string literals in C++?](https://stackoverflow.com/questions/75172318/what-is-the-encoding-of-unprefixed-string-literals-in-c)  
> [learn.microsoft - source-charset-set-source-character-set](https://learn.microsoft.com/ko-kr/cpp/build/reference/source-charset-set-source-character-set?view=msvc-170)  
> [learn.microsoft - execution-charset-set-execution-character-set?view=msvc-170](https://learn.microsoft.com/en-us/cpp/build/reference/execution-charset-set-execution-character-set?view=msvc-170)  
