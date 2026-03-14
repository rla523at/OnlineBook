# Relocation
TU에서 생성된 목적 코드들은 링킹 과정 전까지 심볼들의 위치를 확정할 수 없기 때문에 추후에 심볼들의 위치가 확정이 되면 값을 바꿔야 할 부분들을 적어놓은 `재배치 테이블 (Relocation Table)`을 생성한다.

예를 들어서 아래와 같은 코드를 보자.

```
static int a = 3;
int b = 3;
static int c;

static int func2() {
  c += a + b;
  return c;
}

int func3() {
  b += c;
  return b;
}

int func() {
  a += func2();
  a += func3();
  return a;
}
```

objdump 에 -r 옵션을 주면 재배치가 필요한 부분들을 보여준다.

예를 들어서 func2 의 목적 코드가 어떤 식으로 생겼는지 살펴보자.

```
objdump -Sr s.o

s.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <_ZL5func2v>:
static int a = 3;
int b = 3;
static int c;

static int func2() {
   0:    f3 0f 1e fa              endbr64 
   4:    55                       push   %rbp
   5:    48 89 e5                 mov    %rsp,%rbp
  c = a + b;
   8:    8b 15 00 00 00 00        mov    0x0(%rip),%edx        ## e <_ZL5func2v+0xe>
            a: R_X86_64_PC32    .data-0x4
   e:    8b 05 00 00 00 00        mov    0x0(%rip),%eax        ## 14 <_ZL5func2v+0x14>
            10: R_X86_64_PC32    b-0x4
  14:    01 d0                    add    %edx,%eax
  16:    89 05 00 00 00 00        mov    %eax,0x0(%rip)        ## 1c <_ZL5func2v+0x1c>
            18: R_X86_64_PC32    .bss-0x4
  return c;
  1c:    8b 05 00 00 00 00        mov    0x0(%rip),%eax        ## 22 <_ZL5func2v+0x22>
            1e: R_X86_64_PC32    .bss-0x4
}
  22:    5d                       pop    %rbp
  23:    c3                       retq   
```

내부 및 외부 링크 방식인 a, b, c 들은 데이터 섹션과 BSS 섹션의 위치가 확정되기 전까지 어디에 위치할 지 모르기 때문에 추후에 재배치 시켜야만 한다.

먼저, a 의 값을 읽어들이는 부분부터 보자.

```
  8:    8b 15 00 00 00 00        mov    0x0(%rip),%edx        ## e <_ZL5func2v+0xe>
```

먼저 `0x0(%rip),%edx` 어셈블리는 C 코드로 바꿔서 생각했을 때 `%edx = *(int*)(%rip + 0x0)` 이라고 보면 된다. 즉 RIP 레지스터에다 0만큼 더한 주소값에 있는 데이터를 읽으라는 의미이다. 여기서 a 의 상대적 위치가 결정되지 않았기 때문에 일단 0으로 대체되어 있다.

만일 a 가 어디에 배치되는지 위치가 확정이 된다면,

```
$ readelf -r s.o
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
00000000000a  000300000002 R_X86_64_PC32     0000000000000000 .data - 4
```

위와 같이 해당 부분을 R\_X86\_64\_PC32의 형태로 재배치 하라고 써져 있다. (위 objdump 출력에도 나와있다.) 레퍼런스에 따르면 R\_X86\_64\_PC32은 해당 부분 4 바이트 영역을 S + A - P를 계산한 값으로 치환해라 라는 의미이다.

이 때 S, A, P 는 각각

-   S : 재배치 후에 해당 심볼의 실제 위치
-   P : 재배치 해야하는 부분의 위치
-   A : 더해지는 값으로, 재배치 테이블에서 그 값을 확인할 수 있다.

일단 readelf를 통해 확인했을 때 일단 A 의 값은 -4 임을 알 수 있다(Addend 부분). 나머지 부분은 링킹 후에 심볼들의 위치가 정해져야지 알 수 있다.

따라서 간단히 int main{} 만 있는 파일과 같이 링크해보도록 하자. nm 을 통해서 우리의 경우 a는 4010에 위치 되어 있는 것을 확인할 수 있다.

```
$ nm s
0000000000004010 d _ZL1a
```

따라서 S는 0x4010이 된다.

func2 는 1138 에 위치해있으므로 (마찬가지로 nm 을 통해서 확인 가능)

```
$ nm s
0000000000001138 t _ZL5func2v
```

P에서 A를 더한값이 재배치 해야 되는 부분 즉 func2의 위치인 0x1138이 되어야 함으로 P는 4를 더한 만큼인 0x1142가 된다. 따라서 P는 0x1142 이므로, S + A - P = 0x4010 - 0x4 - 0x1142 = 0x2ECA가 된다.

```
0000000000001138 <_ZL5func2v>:
static int a = 3;
int b = 3;
static int c;

static int func2() {
    1138:    f3 0f 1e fa              endbr64 
    113c:    55                       push   %rbp
    113d:    48 89 e5                 mov    %rsp,%rbp
  c = a + b;
    1140:    8b 15 ca 2e 00 00        mov    0x2eca(%rip),%edx        ## 4010 <_ZL1a>
    1146:    8b 05 c8 2e 00 00        mov    0x2ec8(%rip),%eax        ## 4014 <b>
    114c:    01 d0                    add    %edx,%eax
    114e:    89 05 c8 2e 00 00        mov    %eax,0x2ec8(%rip)        ## 401c <_ZL1c>
  return c;
    1154:    8b 05 c2 2e 00 00        mov    0x2ec2(%rip),%eax        ## 401c <_ZL1c>
}
    115a:    5d                       pop    %rbp
    115b:    c3                       retq   
```

실제로 해당 부분이 ca 2e 00 00 으로 바뀐 것을 확인할 수 있다. 리틀 엔디언임을 고려하면 해당 값이 0x2ECA 임을 알 수 있다. `0x2eca(%rip)`의 의미는 RIP 레지스터에서 0x2ECA만큼 떨어진 곳에서 4 바이트 만큼 읽어서 EDX 레지스터에 저장하라는 의미 이다. 해당 명령어를 실행할 때 RIP에는 다음에 실행할 명령어의 위치가 들어가 있으니,  0x1146 + 0x2ECA = 0x4010 에 위치한 곳의 4바이트를 읽어들인다. 즉 정확히 변수 a가 위치해있는 곳을 읽게 된다.

이번에는 func3를 보자.

```
  45:    48 89 e5                 mov    %rsp,%rbp
  a += func2();
  48:    e8 b3 ff ff ff           callq  0 <_ZL5func2v>
  4d:    8b 15 00 00 00 00        mov    0x0(%rip),%edx        ## 53 <_Z4funcv+0x13>
            4f: R_X86_64_PC32    .data-0x4
  53:    01 d0                    add    %edx,%eax
  55:    89 05 00 00 00 00        mov    %eax,0x0(%rip)        ## 5b <_Z4funcv+0x1b>
            57: R_X86_64_PC32    .data-0x4
  a += func3();
  5b:    e8 00 00 00 00           callq  60 <_Z4funcv+0x20>
            5c: R_X86_64_PLT32    _Z5func3v-0x4
  60:    8b 15 00 00 00 00        mov    0x0(%rip),%edx        ## 66 <_Z4funcv+0x26>
            62: R_X86_64_PC32    .data-0x4
  66:    01 d0                    add    %edx,%eax
  68:    89 05 00 00 00 00        mov    %eax,0x0(%rip)        ## 6e <_Z4funcv+0x2e>
            6a: R_X86_64_PC32    .data-0x4
  return a;
  6e:    8b 05 00 00 00 00        mov    0x0(%rip),%eax        ## 74 <_Z4funcv+0x34>
            70: R_X86_64_PC32    .data-0x4
}
  74:    5d                       pop    %rbp
  75:    c3                       retq   
```

먼저 static 함수인 func2 를 호출하는 부분부터 보자.

```
a += func2();
  48:    e8 b3 ff ff ff           callq  0 <_ZL5func2v>
```

놀랍게도 이 부분의 경우 재배치가 지정되지 않고 실제 func2의 위치가 그대로 들어가 있음을 알 수 있다. 왜냐하면 이 callq의 경우 현재의 RIP에서 상대 위치를 받는데, 0xFFFFFFB3 가 2 의 보수 표현법으로 -0x4D 이므로, 정확히 주소값 0을 의미한다(0x4D - 0x4D = 0). 그리고 실제로 0번에 func2 함수가 위치하고 있다. 그 이유는 static 함수의 경우 내부 링크 방식이기 때문에 TU밖에서 참조될 일이 없기 때문이다. 이 때문에 컴파일 타임에 함수의 위치를 확정시킬 수 있다.

반면에 외부 링크 방식으로 된 일반적인 func3 함수를 호출하는 부분을 보자.

```
a += func3();
  5b:    e8 00 00 00 00           callq  60 <_Z4funcv+0x20>
            5c: R_X86_64_PLT32    _Z5func3v-0x4
```

이 경우 R\_X86\_64\_PLT32의 형태로 링크를 하고 있다. R\_X86\_64\_PLT32의 경우 `프로시져 링크 테이블(procedure linkage table)`을 사용한 링킹 방식으로 `동적 링크 방식(dynamic linking)`에서 사용된다. 하지만 동적 링크 방식을 사용하지 않았을 경우, 그냥 R\_X86\_64\_PC32 와 동일하다고 보면 된다. 이 예제에서는 실행 파일을 생성하기 위해 동적 링크 방식을 사용하지 않았으므로 그냥 R\_X86\_64\_PC32 와 같은 식으로 계산된다.

실제로 완성된 코드를 살펴보자면

```
    1194:    e8 c4 ff ff ff           callq  115d <_Z5func3v>
```

와 같이 되어 있는데, 00 00 00 00 부분이 현재의 RIP 로 부터 상대적 위치값으로 변경되어 있음을 알 수 있다.