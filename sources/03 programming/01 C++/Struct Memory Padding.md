# Struct Memory Padding
대부분의 컴파일러는 성능 향상을 위해 CPU가 접근하기 쉽게 메모리를 정렬해서 data에  할당하는데 이를 `구조체 패딩(struct memory padding)`이라고 한다.

struct memory padding이 어떻게 성능을 향상시키는지 다음 예시 코드를 보자.
```cpp
class A1
{
    char a;
    int  b;
}
```

struct memory padding이 없는경우 offset과 size는 다음과 같다.
```cpp
class A1
{
    char a; // offset 0byte, size 1byte
    int  b; // offset 1byte, size 4byte
}
```

이 때, 32bit os에서 이 class의 객체에서 `int b`변수를 읽어오는 경우를 생각해보자.

32bit os에서는 cpu에서 메모리를 읽을 때 4byte씩 읽는다. 따라서, `int b`변수를 읽기 위해서는 객체의 시작주소를 0이라고할 때, [1,4]에 저장된 데이터를 읽어야 된다. 따라서 CPU는 [0,3]를 읽고 4에 저장된 데이터를 읽기 위해 [4,7]을 한번 더 읽어야 된다.

이번에는 struct memory padding이 4byte packing으로 이루어진 경우를 보자.
```cpp
class A1
{
    char a; // offset 0byte, size 1byte
    int  b; // offset 4byte, size 4byte 
}
```

이번에 `int b`변수를 읽기 위해서는 시작주소를 0이라고할 때 CPU는 [4,7]를 한번만 읽으면 된다. 

이처럼 메모리를 정렬해서 data에 할당하면 메모리를 읽는 횟수를 줄여 성능을 향상시킬 수 있다. 그러나 struct memory padding을 하면 중간중간 낭비되는 빈 메모리 공간이 생기게 되는데 이를 `패딩 비트(padding bit)`라고 한다.

> Reference  
> [wnsgml972 - c_struct_padding](https://wnsgml972.github.io/c/2019/11/21/c_struct_padding/)  
> [tipsware - 클래스의 크기](https://blog.naver.com/tipsware/221090063784)  


## Struct Memory Padding Rule
* 구조체에 선언된 변수의 순서대로 packing 된다.

다음 예시 코드를 보자.
```cpp
TEST(class, struct_member_alignment1)
{
  class A1
  {
    char a; //offset 0, size 1
    int  b; //offset 4, size 4
    char c; //offset 8, size 1
  };
  class A2
  {
    char a; //offset 0, size 1
    char c; //offset 1, size 1
    int  b; //offset 4, size 4
  };

  EXPECT_EQ(sizeof(A1), 12);
  EXPECT_EQ(sizeof(A2), 8);
}
```

여기서 class A1과 A2는 모두 4byte packing이 된다. 하지만 class의 크기는 다르다.

A1에서는 char-int-char 순서로 선언되어 있어서 3개의 4byte packing(cahr)(int)(char)에 data가 들어가게 되어 총 12byte의 크기를 갖게 된다.

하지만 A2에서는 char-char-int 순서로 선언되어 있어서 2개의 4byte packing(char,char)(int)에 data가 들어가게 되어 총 8byte의 크기를 갖게 된다.


* 구조체가 가진 자료형 중 가장 큰 자료형을 기준으로 packing 된다.

다음 예시 코드를 보자.
```cpp
TEST(class, struct_member_alignment1)
{
  class A1
  {
    char a;
    int  b;
    char c;
  };
  class A2
  {
    char a;
    char c;
    int  b;
  };

  EXPECT_EQ(sizeof(A1), 12);
  EXPECT_EQ(sizeof(A2), 8);
}
TEST(class, struct_member_alignment2)
{
  class A1
  {
    char      a;
    long long b;
    char      c;
  };
  class A2
  {
    char      a;
    char      c;
    long long b;
  };

  EXPECT_EQ(sizeof(A1), 24);
  EXPECT_EQ(sizeof(A2), 16);
}
```
struct_member_alignment1에서는 가장 큰 자료형이 int(4byte)임으로 4byte packing이 된다. 하지만 struct_member_alignment2에서는 가장 큰 자료형이 long long(8byte)임으로 8byte packing이 된다.

하지만, MSVC의 경우 최대 8byte packing까지 된다.

```cpp
TEST(class, struct_member_alignment3)
{
  class Data
  {
    long long a;
    long long b;
    long long c;
  };
  class A1
  {
    char a;
    Data b;
    char c;
  };
  class A2
  {
    char a;
    char c;
    Data b;
  };

  EXPECT_EQ(sizeof(Data), 24);
  EXPECT_EQ(sizeof(A1), 40);
  EXPECT_EQ(sizeof(A2), 32);
}
```

Data class는 24byte이지만 class A1과 class A2는 8byte packing이 된다.