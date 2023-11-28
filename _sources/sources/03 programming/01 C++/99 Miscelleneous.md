# Miscellaneous
## Checking TypeName
Compile error message를 통해 void가 아닌 typename을 알아낼 수가 있다.

먼저 template 함수를 다음과 같이 정의하자.

```cpp
template <typename T> void type_checker(T& val)
{
	static_assert(std::is_void_v<T>, " ");
}
```

그 다음에 cpp파일을 compile하면 다음 예시처럼 error message에 type이 나온다.

```
instantiation of "void Mec::type_checker(T &) [with T=Mec::DataArchive]" at line 3657
```

## Iterator

> Reference   
> [blog](https://www.internalpointers.com/post/writing-custom-iterators-modern-cpp)