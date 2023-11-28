# Static Library
라이브러리란 프로그램이 동작하기 위해 필요한 외부 목적 코드들이다.예를 들어서 C++에서 iostream 헤더파일을 include 했다면, 이 프로그램이 실행하기 위해서는 iostream 라이브러리가 있어야 한다. 

이 때 정적 라이브러리는 우리가 필요로 하는 라이브러리가 링킹 후에 완성된 프로그램 안에 포함된다고 이해할 수 있다. 쉽게 말해 실행 파일 자체에 해당 라이브러리 코드가 딱 박혀서 있기 때문에 정적(static)이라고 하는 것이다. 예를 들어서 어떤 프로그램이 A라는 라이브러리와 B라는 라이브러리를 사용하고 있다면 프로그램의 어셈블리를 출력하였을 때 A와 B 라이브러리의 모든 코드들이 들어있게 된다.

어떻게 생각하면 당연한 일이다. 프로그램을 실행하기 위해선 당연히 해당 프로그램이 필요로 하는 코드들이 프로그램 안에 있어야 되기 때문이다. 정적 라이브러리가 어떻게 링크 되는지 GCC 컴파일러를 사용해서 간단한 정적 라이브러리를 만들어보면서 확인해보자.

## 정적 라이브러리 만들기
예를 들어서 foo 라는 함수를 제공하는 foo.cc 파일과 bar 라는 함수를 제공하는 bar.cc 파일이 있다고 하자.

```cpp
// bar.h
void bar();
 
// bar.cc
void bar() {}
 
// foo.h
int foo();
 
// foo.cc
#include "bar.h"
int x = 1;

int foo() {
  bar();
  x++;
  return 1;
}
```

이 두 파일들을 각각 컴파일 하면 foo.o 와 bar.o 라는 목적 코드가 생성이 된다. 만일 이 두 함수를 제공하는 정적 라이브러리를 만들기 위해서는, 이 두 목적 파일들을 하나로 묶어주기만 하면 된다. 

```
$ ar crf libfoobar.a foo.o bar.o
```

다음 명령어를 통해 libfoobar.a 라는 정적 라이브러리 파일이 만들 수 있다. 리눅스에선 통상적으로 정적 라이브러리 파일은 .a 의 확장자를 가진다. 이 libfoobar.a 은 거창한 것이 아니고 그냥 foo.o의 내용과 bar.o의 내용을 하나로 합쳐놓은 것이다. 실제로 objdump 로 libfoobar.a 의 내용을 열어보면 foo.o와 bar.o가 하나로 합쳐진 파일인걸 알 수 있다. (마치 리눅스에서 tar 로 파일들을 합친 것과 비슷하다)

```
objdump -S libfoobar.a
In archive libfoobar.a:

foo.o:     file format elf64-x86-64

Disassembly of section .text:

0000000000000000 <_Z3foov>:
   0:	f3 0f 1e fa          	endbr64 
   4:	55                   	push   %rbp
   5:	48 89 e5             	mov    %rsp,%rbp
   8:	e8 00 00 00 00       	callq  d <_Z3foov+0xd>
   d:	8b 05 00 00 00 00    	mov    0x0(%rip),%eax        ## 13 <_Z3foov+0x13>
  13:	83 c0 01             	add    $0x1,%eax
  16:	89 05 00 00 00 00    	mov    %eax,0x0(%rip)        ## 1c <_Z3foov+0x1c>
  1c:	b8 01 00 00 00       	mov    $0x1,%eax
  21:	5d                   	pop    %rbp
  22:	c3                   	retq   

bar.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <_Z3barv>:
   0:	f3 0f 1e fa          	endbr64 
   4:	55                   	push   %rbp
   5:	48 89 e5             	mov    %rsp,%rbp
   8:	90                   	nop
   9:	5d                   	pop    %rbp
   a:	c3                   	retq   
```

이 정적 라이브러리를 사용하는 방법은 간단하다.

예를 들어서 main.cc 라는 파일에서 foo 함수를 사용하고 싶다고 하자. 그렇다면 우리가 필요한 것은 foo 함수가 선언 되어 있는 헤더파일 하나 뿐이다.

```cpp
#include "foo.h"

int main() { foo(); }
```

통상적인 상황이라면 main 을 컴파일 하면서 실행파일을 생성할 때, foo.cc 코드와 bar.cc 코드를 같이 컴파일 해서 링킹해야 한다. 하지만 우리는 이미 foo.cc 와 bar.cc 가 이미 컴파일 되어 있는 libfoobar.a 라는 라이브러리가 있기 때문에 굳이 이들을 다시 컴파일 할 필요가 없다.

따라서 실행 파일을 생성 시에 링크만 해주면 된다.

```
$ g++ main.cc libfoobar.a -o main 
```

실제 main 의 내용을 objdump 로 살펴보면

```
objdump -S main
... (생략) ...
0000000000001129 <main>:
    1129:	f3 0f 1e fa          	endbr64 
    112d:	55                   	push   %rbp
    112e:	48 89 e5             	mov    %rsp,%rbp
    1131:	e8 07 00 00 00       	callq  113d <_Z3foov>
    1136:	b8 00 00 00 00       	mov    $0x0,%eax
    113b:	5d                   	pop    %rbp
    113c:	c3                   	retq   

000000000000113d <_Z3foov>:
    113d:	f3 0f 1e fa          	endbr64 
    1141:	55                   	push   %rbp
    1142:	48 89 e5             	mov    %rsp,%rbp
    1145:	e8 16 00 00 00       	callq  1160 <_Z3barv>
    114a:	8b 05 c0 2e 00 00    	mov    0x2ec0(%rip),%eax        ## 4010 <x>
    1150:	83 c0 01             	add    $0x1,%eax
    1153:	89 05 b7 2e 00 00    	mov    %eax,0x2eb7(%rip)        ## 4010 <x>
    1159:	b8 01 00 00 00       	mov    $0x1,%eax
    115e:	5d                   	pop    %rbp
    115f:	c3                   	retq   

0000000000001160 <_Z3barv>:
    1160:	f3 0f 1e fa          	endbr64 
    1164:	55                   	push   %rbp
    1165:	48 89 e5             	mov    %rsp,%rbp
    1168:	90                   	nop
    1169:	5d                   	pop    %rbp
    116a:	c3                   	retq   
    116b:	0f 1f 44 00 00       	nopl   0x0(%rax,%rax,1)
```

위 처럼 libfoobar.a의 내용이 그대로 포함되어 있는것을 볼 수 있다.

이렇게 정적 라이브러리는 링크 타임에 바인딩 된다.