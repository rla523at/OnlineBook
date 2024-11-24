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

### Fill and align

```
{:[fill character][align options][width]}
```

> Reference  
> [cppreference - format/spec](https://en.cppreference.com/w/cpp/utility/format/spec)  
