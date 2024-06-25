# Type Casting

## static_cast
`static_cast<T>(val)`은 `val`의 `T`로의 형 변환 함수를 호출한다고 볼 수 있다.

따라서, bit 단위로 보면 전혀 다른 `5.0f`와 `5`를 변환해 줄 수 있는 것이다.

```cpp
  float f  = 5.0;
  int   i  = static_cast<int>(f);   // i=5
  float f2 = static_cast<float>(i); // f2=5.0
```

그래서 사용자 정의 타입과 같은 경우에는 형 변환 함수가 없을 경우 static_cast에서 적절한 변환 함수가 없다는 오류가 발생한다.
```cpp
class My_Int {
public:
  int Num = 0;
};

class My_Float {
public:
  float Num = 0.0f;
};

int main()
{
  My_Int   i = {5};
  My_Float f = {5.0};
  //My_Int i2 = { static_cast<My_Int>(f) }; // "My_Float"에서 "int"(으)로의 적절한 변환 함수가 없습니다.

  return 0;
}
```

그리고 결국 함수를 호출하는 것과 같기 때문에 컴파일 타임에 타입 체크를 수행하여 잘못된 변환을 방지할 수 있다.

잘못된 변환의 예시는 다음과 같다.
```cpp
  // different byte size
  char c = 10; // 1 byte
  // int* p = static_cast<int*>(&c); // 4 bytes

  // type casting between pointer and integer
  int64_t  i = 10;
  //int64_t* p = static_cast<int64_t*>(i); // error
  //int64_t address = static_cast<int64_t>(&i); // error
```

> Reference  
> [blog](https://blog.naver.com/wkdghcjf1234/220210906503)  

## reinterpret_cast
`reinterpret_cast<T*>(adress)`는 주어진 주소 `adress`을 `T` 타입의 변수의 시작 주소로 인식하라는 말이다.

따라서, static_cast와 다르게 동작하게 된다.
```cpp
  float f = 5.0;
  int   i = *reinterpret_cast<int*>(&f); // 1084227584
```

> Reference  
> [blog](https://blog.naver.com/wkdghcjf1234/220210906503)  

### 참고
`reinterpret_cast<T&>(val)`는 `val`을 `T` 타입의 변수의 시작 주소로 인식하라는 말이다.

```cpp
  float f = 5.0;
  int   i = reinterpret_cast<int&>(f); // 1084227584
```

## C Style casting
C style casting의 경우, static_cast가 되는 상황에서는 stataic_cast처럼 static_cast가 안되는 경우에는 reinterpret_cast처럼 동작한다.

```cpp
//static_cast
int main()
{
  float val  = 5.0;
  int   val1 = (int)(val);
  int   val2 = static_cast<int>(val);
  int   val3 = *reinterpret_cast<int*>(&val);

  //val1 == val2 != val3

  return 0;
}

//reinterpret_cast
int main()
{
  char c = 10;
  int* p1 = (int*)(&c);
  int* p2 = reinterpret_cast<int*>(&c);
  //int* p3 = static_cast<int*>(&c); compile error

  //p1 == p2

  return 0;
}
```

## const_cast

> Reference  
> [blog](https://hwan-shell.tistory.com/215)

## pointer_cast

> Reference  
> [cppreference](https://en.cppreference.com/w/cpp/memory/shared_ptr/pointer_cast)