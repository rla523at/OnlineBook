# Macro

## Multi Line Macro
#define 선언 시 여러 줄에 나눠서 하고 싶은 경우 끝에 \ 를 붙여주면 된다.

```cpp
//#define T "New Content\n"

#define T \
"New \
Content\n"\
```

이 때, 주의할 점은 \ 뒤에 공백이 오면 compile 오류가 발생한다.

## Variadic Macros

### __VA_ARGS__

매크로의 선언부에서 가변인자들은 `...` 으로 표현되며 정의부에서는 `__VA_ARGS__` 로 표현된다.

```
#define MACRO_NAME(...) function(__VA_ARGS__)
```

> Reference    
> [wikipedia - Variadic_macro_in_the_C_preprocessor](https://en.wikipedia.org/wiki/Variadic_macro_in_the_C_preprocessor)  
> [The GNU C Preprocessor - Variadic Macros](http://tigcc.ticalc.org/doc/cpp.html#SEC13)  

### __VA_OPT__

C++20 부터 지원되는 매크로로 만약 가변인자의 개수가 0 개인 경우 `__VA_OPT__` 로 둘러싸인 부분은 무시된다.

```
#define REQUIRE( requirement, format_string, ... ) \
//...
    ms::exception::print( format_string __VA_OPT__(, __VA_ARGS__ ) ); \
//...
```
단, `__VA_OPT__` 매크로를 사용하고 싶은 경우 속성 >> C/C++ >> 전처리기 >> 표준 준수 전처리기 사용 >> 예(/Zc:preprocessor) 옵션을 켜줘야 한다.

> Reference    
> [wikipedia - Variadic_macro_in_the_C_preprocessor](https://en.wikipedia.org/wiki/Variadic_macro_in_the_C_preprocessor)  
> [The GNU C Preprocessor - Variadic Macros](http://tigcc.ticalc.org/doc/cpp.html#SEC13)  
> [learn.microsoft - MSVC 새 전처리기 개요](https://learn.microsoft.com/ko-kr/cpp/preprocessor/preprocessor-experimental-overview?view=msvc-170)  
