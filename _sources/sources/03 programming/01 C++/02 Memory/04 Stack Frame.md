# Stack Frame
함수가 호출되면 stack에 `스택 프레임(stack frame)`이라는 block of memory가 할당된다. stack frame은 함수만의 스택 영역을 구분하기 위하여 생성되는 공간으로 지역 변수, 함수의 인자, 지역 배열, 함수의 return adress등이 저장된다. 

stack frame에 할당될 메모리의 크기는 compile time에 미리 결정되며 함수가 종료되면서 소멸한다. 이 모든 과정은 자동으로 수행되며 프로그래머는 stack 영역에서의 메모리 할당과 해제에 대한 걱정을 할 필요가 없다.

## Data 저장
stack frame에 data를 위한 memory를 할당하는 과정은 `스택 포인터(Stack Pointer Resister; SP)`를 감소시키는 일이다. 반대로 memory를 해제하는 과정은 SP를 증가시키는 일이다.

이처럼, stack은 `후입선출(last in first out; LIFO)`구조를 가지고 있어 가장 최근에 추가된 data가 가장 먼저 제거된다.

stack에 남아있던 데이터는 향후 SP가 다시 증가하고 덮어 쓰여진다. 

```cpp
int main() {
  char str[13];
  str[0] = 'A';
  stack();
  return 0;
}
```

```
//...
00691023  sub         esp,10h
//...
```

main 함수에서 지연변수 str을 stack에 어떻게 저장하는지 살펴보자. 지역변수 str을 위한 memory를 stack에 할당하기 위해 esp를 이동시키게 된다.
```
00691023  sub         esp,10h  // esp = 0x00affb7c --> 0x00affb6c
```

`sub esp,10h`는 esp가 가지고 있는 값에서 10h만큼 빼라는 명령어이고 10h는 16진수 10 즉, 16만큼 esp의 값을 빼라는 얘기이다. esp에는 16진수 00affb7c가 들어 있음으로 00affb6c가 들어가게 된다. 

근데, 여기서 str은 13byte의 메모리만 필요한데 왜, 16byte의 메모리가 할당되었을까? 

이는 32bit 프로그램에서는 메모리 할당이 32bit(4byte)단위로 할당되기 때문에 16byte가 할당된 것이다.

> Reference
> [eliez3r - 스택프레임(Stack Frame) 이란?](https://eliez3r.github.io/post/2019/10/16/study-system.Stack-Frame.html)  

## stack frame의 생성과 소멸 과정
stack frame이 생성되고 소멸되는 과정을 간단하게 살펴보자. 아래 그림에서 흰색으로 표현한 것이 stack frame이다.

```{figure} _image/0401.png
```

1. 10번째 줄에 main 함수가 시작할 때, main 함수에 대한 stack frame이 생성된다.
2. 15번째 줄에서 add 함수를 호출하여 4번째 줄에 add함수가 시작할 때, add 함수에 대한 stack frame이 생성된다.
3. add 함수가 끝난 뒤 8번째 줄에서 add 함수의 stack frame이 소멸한다.
4. main 함수가 끝난 뒤 20번째 줄에서 main 함수의 stack frame이 소멸한다.

### stack frame의 생성 과정
이번에는 stack frame이 생성과 초기화되는 과정을 조금 더 자세하게 보도록 하자. 

```cpp
//exe_common.inl
static __declspec(noinline) int __cdecl __scrt_common_main_seh(){
  //...
  int const main_result = invoke_main();
  //...
}
```

```
//...
00E91206  push        dword ptr [eax]  
00E91208  call        main (0E91030h)  
00E9120D  add         esp,0Ch  
//...
```

```cpp
int main() {
  char str[13];
  str[0] = 'A';
  stack();
  return 0;
}
```

```
00691020  push        ebp                     
00691021  mov         ebp,esp                 
//...                   
```

main 함수를 호출하는 caller 함수와 main 함수 그리고 각각 해당하는 어셈블리어 코드의 일부이다.

`call main (0E91030h)`과 `push ebp`, `mov ebp,esp` 명령어를 통해 main 함수의 stack frame 영역이 새롭게 설정되며 이 중 `push ebp`, `mov ebp,esp` 명령어를 함수 `프롤로그(Prolog)`라고 한다.

여기서 ebp는 `extended base pointer`의 약자로 현재 stack frame의 base 주소를 담고 있는 레지스터이고 esp는 `extended stack pointer`의 약자로 현재 stack의 최상단 주소값을 저장하고 있는 레지스터이다.

각 명령어가 어떻게 main 함수의 stack frame 영역을 새롭게 설정하는지 ebp, esp의 값의 변화와 함께 살펴보자.

```
                                                  ebp           esp
00E91208  call        main (0E91030h)         0x00affbc4    0x00affb84 
```

```
                                                  ebp           esp
                                              0x00affbc4    0x00affb80
00691020  push        ebp                                   0x00affb7c
00691021  mov         ebp,esp                 0x00affb7c              
```

`call main`명령어가 실행되기 전에 ebp와 esp 값을 살펴보자.

먼저 ebp,esp의 정의에 따라 ebp값 0x00affbc4은 caller 함수의 stack frame의 base 주소이며 esp값 0x00affb84는 caller 함수의 stack frame의 최상단 주소이다.

`call main`명령어가 실행되어 main 함수로 들어오게 되면 esp 값이 0x00affb80으로 4byte 줄어든것을 알 수 있다. 

stack에 메모리를 할당받아 data를 저장하는 과정은 data 값을 memory에 저장하고 esp를 data 크기만큼 이동하는 과정에 불과하다. 그리고 stack은 주소가 작아지는 방향으로 메모리를 할당(esp를 이동)한다. 즉, stack에 4byte data가 주소값 [0x00AFFB83, 0x00AFFB80]에 저장 되었고 그로 인해 esp가 4byte 줄어든 것이다. 주소값 [0x00AFFB83, 0x00AFFB80]에 어떤 data가 저장되어 있는지 메모리를 확인해보자.

```
//4byte 단위
0x00AFFB80  0d 12 e9 00

//1byte 단위
0x00AFFB80  0d
0x00AFFB81  12
0x00AFFB82  e9
0x00AFFB83  00
```

저장된 00e9120d는 main함수 호출이 끝난뒤에 돌아가야 될 return adress임을 알 수 있다. 즉, call main 명령어가 실행되면 return adress가 stack에 저장되며 여기서부터 main 함수의 stack frame이 시작되는 것이다.

다음으로, `push ebp`는 ebp에 들어 있는 값을 stack에 할당하는 명령어이다. 따라서, `push ebp`를 하면 ebp에 들어있는 4byte(32bit) data인 0x00affbc4가 주소값 [0x00AFFB7F, 0x00AFFB7C]에 저장되고 esp의 값이 0x00affb7c가 된다. `push ebp`를 한 후의 메모리 상태를 보면 다음과 같다.

```
//4byte 단위
0x00AFFB7C  c4 fb af 00  
0x00AFFB80  0d 12 e9 00

//1byte 단위
0x00AFFB7C  c4
0x00AFFB7D  fb
0x00AFFB7E  af
0x00AFFB7F  00
```

즉, `push ebp`를 통해 caller 함수의 ebp값이 stack에 저장된다.

다음으로 `mov ebp,esp`는 esp에 들어 있는 값을 ebp에 저장하는 명령어이다. 따라서, ebp의 값은 0x00affb7c이 되며 주소값 0x00AFFB7C이 main 함수의 stack frame에 base 주소가 된다.

정리하자면 `call main (0E91030h)` 명령어가 실행되면 main 함수의 return adress가 저장되며 여기서 부터 main 함수의 stack frame이 시작된다. 그리고 `push ebp` 명령어를 통해 caller 함수의 ebp를 stack에 저장해두고 `mov ebp,esp`를 통해 main 함수의 ebp를 설정한다. 그리고 이 과정이 새로운 stack frame을 시작하고 설정하는 단계이다.

```{figure} _image/0402.png
```

> Reference  
> [eliez3r - 스택프레임(Stack Frame) 이란?](https://eliez3r.github.io/post/2019/10/16/study-system.Stack-Frame.html)  
> [zxwnstn - 레지스터 ESP, EBP와 명령어 POP, PUSH](https://blog.naver.com/zxwnstn/221513370836)  
> [jungwoong  - 스레드 스택 메모리 구조](https://jungwoong.tistory.com/45)  

### stack frame의 소멸 과정
함수 `에필로그(epilog)`는 callee 함수가 끝나서 다시 caller 함수로 돌아갈 때 stack을 정리하는 과정이다. 다시말해, 스택을 다시 callee함수를 호출하기 전 상태로 되돌리는 과정을 말한다.

epilog는 다음과 같은 명령어로 이루어져 있다.

```
0055104A  mov         esp,ebp  
0055104C  pop         ebp  
0055104D  ret                
```

여기서 `ret` 명령어는 2개의 명령어로 이루어져 있는데, pop eip, jmp eip 이다.

eip는 `Extended Instruction Pointer`의 약자로 다음 수행해야 될 code의 주소를 저장하는 register이다. 즉, CPU가 다음으로 해야 하는 일이 무엇인지를 기억하는 register이다.

`mov esp,ebp`명령어를 통해 esp가 다시 ebp 위치로 돌아옴으로써 ebp이후로 stack에 할당된 모든 memory가 해제된다.

다음으로 `pop ebp`명령어를 통해 ebp가 다시 caller의 ebp값을 가지게 되고 esp는 4byte 증가하면서 caller의 esp를 저장했던 memory도 해제가 된다.

다음으로 `pop eip`명령어를 통해 eip에 return adress값이 들어가게 되고 esp는 4byte 증가하면서 return adress를 저장했던 memory도 해제되면서 main의 stack frame이 전부 해제되게 된다.

마지막으로 `jmp eip`명령어를 통해 return adress로 돌아가서 명령어를 다시 수행한다.

```{figure} _image/0403.png
```

> Reference  
> [eliez3r - 스택프레임(Stack Frame) 이란?](https://eliez3r.github.io/post/2019/10/16/study-system.Stack-Frame.html)  
> [sthellfire - EIP는 무엇인가?](https://blog.naver.com/sthellfire/220639968366)


#### 참고
* stack 영역은 다른 영역들과 달리 높은 주소에서 낮은 주소로 자라나는 형태를 가진다. 이러한 이유는 stack 영역이 엄청나게 커지더라도 운영체제의 핵심인 커널영역을 침범할 수 없게 하기 위해서다. 
* stack 영역을 초과하면 stack overflow가 발생한다.
  * [bozeury - 스택의 최대 할당 크기](https://bozeury.tistory.com/90) 
* debug 모드로 하면 stack에 debug 정보가 들어가서 엄청나게 많은 메모리를 할당함
  * [stackoverflow - why-is-so-much-space-allocated-on-the-stack](https://stackoverflow.com/questions/15806673/why-is-so-much-space-allocated-on-the-stack)
* 어셈블리어는 visual studio2022에서 release x86 최적화 전부 끈 환경으로 visual studio에서 제공하는 디스어셈블리를 확인한 결과이다.
* 메모리결과는 visual studio2022에서 release x86 최적화 전부 끈 환경으로 visual studio에서 제공하는 메모리를 확인한 결과이다.

 
> Reference  
> [educative - stack-vs-heap](https://www.educative.io/blog/stack-vs-heap)  
> [tcpschool - c_memory_stackframe](https://tcpschool.com/c/c_memory_stackframe) 