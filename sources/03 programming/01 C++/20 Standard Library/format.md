# format

## std::format

args 에 wide character 가 있으면 fmt 도 wformat_string 이어야 한다.
```cpp
TEST( format, wstirng_view )
{
   const std::wstring_view wstr_view = L"Char arr";

   //const auto result = std::format( "{}", wstr_view ); //불가능
   const auto result = std::format( L"{}", wstr_view );

   const std::wstring ref    = L"Char arr";
   EXPECT_EQ( result, ref );
}
```

> Reference  
> [cppreference - format](https://en.cppreference.com/w/cpp/utility/format/format)  
> [tango1202.github - modern-cpp-stl-formatting](https://tango1202.github.io/cpp-stl/modern-cpp-stl-formatting/)  

## std::vformat
format string 이 동적으로 결정되어야 할 때 사용한다.

## Standard format specification
For basic types and string types, the format specification is based on the format specification in Python
* [cppreference - format/spec](https://en.cppreference.com/w/cpp/utility/format/spec)

In most of the cases the syntax is similar to the old %-formatting, with the addition of the {} and with : used instead of %
* [docs.python - format-examples](https://docs.python.org/3/library/string.html#format-examples)

```
fill-and-align(optional) sign(optional) #(optional) 0(optional) width(optional) precision(optional) L(optional) type(optional)		
```

### Fill and align
```
{:[fill character][align options][width]}
```

> Reference  
> [cppreference - format/spec](https://en.cppreference.com/w/cpp/utility/format/spec)  

### Sign, #, and 0

* The # option causes the alternate form to be used for the conversion.
  * For integral types, when binary, octal, or hexadecimal presentation type is used
    * the alternate form inserts the prefix (0b, 0, or 0x) into the output value after the sign character (possibly space) if there is one, or add it before the output value otherwise.
* The 0 option pads the field with leading zeros

```cpp
    std::cout << std::format("{:06x}", c);  //000078
    std::cout << std::format("{:#06x}", c); //0x0078
```



\# option causes the alternate form For integral types, when binary, octal, or hexadecimal presentation type is used, the alternate form inserts the prefix (0b, 0, or 0x) into the output value after the sign character (possibly space) if there is one, or add it before the output value otherwise.

> Reference  
> [cppreference - format/spec](https://en.cppreference.com/w/cpp/utility/format/spec)  

### Type

* x
  * 16진수 표현
* b
  * 2진수 표현

> Reference  
> [cppreference - format/spec](https://en.cppreference.com/w/cpp/utility/format/spec)  