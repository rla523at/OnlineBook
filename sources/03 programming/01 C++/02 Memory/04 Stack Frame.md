# Stack Frame
함수가 호출되면 stack에 `스택 프레임(stack frame)`이라는 block of memory가 할당된다. stack frame은 함수만의 스택 영역을 구분하기 위하여 생성되는 공간으로 지역 변수, 지역 배열, 함수의 인자, 함수의 return adress등이 저장된다. 

stack frame에 할당될 메모리의 크기는 compile time에 미리 결정되며 함수가 종료되면서 소멸한다. 이 모든 과정은 자동으로 수행되며 프로그래머는 stack 영역에서의 메모리 할당과 해제에 대한 걱정을 할 필요가 없다.

## 메모리 할당
stack frame에 data를 위한 memory를 할당하는 과정은 `스택 포인터(Stack Pointer Resister; SP)`를 감소시키는 일이다. 반대로 memory를 해제하는 과정은 SP를 증가시키는 일이다.

이처럼, stack은 `후입선출(last in first out; LIFO)`구조를 가지고 있어 가장 최근에 추가된 data가 가장 먼저 제거된다. 그리고 stack에 남아있던 데이터는 향후 SP가 다시 증가하고 덮어 쓰여진다. 

이번에는 x86환경에서 돌린 코드와 함께 stack에 메모리가 어떻게 할당되는지 살펴보자.

```cpp
int main(void)
{
00A81030  push        ebp  
00A81031  mov         ebp,esp  
00A81033  sub         esp,0Ch  
  char str[9];
  str[0] = 'A';
00A81036  mov         eax,1  
00A8103B  imul        ecx,eax,0  
00A8103E  mov         byte ptr str[ecx],41h  
  str[3] = 'A';
00A81043  mov         edx,1  
00A81048  imul        eax,edx,3  
00A8104B  mov         byte ptr str[eax],41h  

  func();
00A81050  call        func (0A81000h)  

  return 0;
00A81055  xor         eax,eax  
}
```

먼저, 위의 어셈블리 코드중 `sub esp,0Ch`가 stack에 메모리를 할당하는 과정이다.

esp는 `extended stack pointer`의 약자로 현재 stack의 최하단 주소값을 저장하고 있는 레지스터이다.

따라서 `sub esp,0Ch`는 esp가 원래 가지고 있는 값에서 0Ch만큼 빼라는 명령어이고 0Ch는 16진수임으로 12Byte만큼 esp의 값을 빼라는 얘기이다. 즉, 12Byte 만큼 스택에 memory를 할당하는 코드이다.

그런데 자세히 보면 이해가 잘 되지 않는 부분이 있다. main 함수의 내용을 보면 9byte의 메모리만 필요한데 왜 12byte가 할당되었을까?

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
int main(void)
{
00A81030  push        ebp  
00A81031  mov         ebp,esp  
00A81033  sub         esp,0Ch  
  char str[9];
  str[0] = 'A';
00A81036  mov         eax,1  
00A8103B  imul        ecx,eax,0  
00A8103E  mov         byte ptr str[ecx],41h  
  str[3] = 'A';
00A81043  mov         edx,1  
00A81048  imul        eax,edx,3  
00A8104B  mov         byte ptr str[eax],41h  

  func();
00A81050  call        func (0A81000h)  

  return 0;
00A81055  xor         eax,eax  
}

void func(void)
{
00A81000  push        ebp  
00A81001  mov         ebp,esp  
00A81003  sub         esp,14h  
  char str[17];
  str[0] = 'Z';
00A81006  mov         eax,1  
00A8100B  imul        ecx,eax,0  
00A8100E  mov         byte ptr str[ecx],5Ah  
  str[16] = 'Z';
00A81013  mov         edx,1  
00A81018  shl         edx,4  
00A8101B  mov         byte ptr str[edx],5Ah  
}
```

`call func (0A81000h)`과 `push ebp`, `mov ebp,esp` 명령어를 통해 main 함수의 stack frame 영역이 새롭게 설정되며 이 중 `push ebp`, `mov ebp,esp` 명령어를 함수 `프롤로그(Prolog)`라고 한다.

여기서 ebp는 `extended base pointer`의 약자로 현재 stack frame에서 local 변수들이 저장되기 시작하는 주소보다 1byte 큰 주소를 담고 있는 레지스터이다. ebp 전에는 함수가 끝나면 실행해야될 명령어의 주소, caller 함수의 ebp등이 저장되어 있는데 자세한 내용은 뒤에서 차근차근 알아보자.

이제 각 명령어가 어떻게 main 함수의 stack frame 영역을 새롭게 설정하는지 ebp, esp의 값의 변화와 함께 살펴보자.

#### call func (0A81000h) 명령어가 실행되기전

`call func (0A81000h)` 명령어가 실행되기전 ebp와 esp의 값은 다음과 같다.

```
   ebp           esp
0x006ffb2c    0x006ffb20 
```

이를 해석해보면 main 함수의 stack frame에서 local 변수들을 위해 할당된 메모리 주소는 (0X006FFB2C, 0X006FFB20]이고 12byte의 local 변수 데이터는 [0X006FFB2B, 0X006FFB20] 주소에 저장되어 있다.

#### call func (0A81000h) 명령어가 실행된 후

다음으로 `call func (0A81000h)` 명령어가 실행된 후에 ebp와 esp의 값은 다음과 같다.

```
   ebp           esp
0x006ffb2c    0x006ffb1c 
```

이를 통해 esp가 4byte 줄어든걸 알 수 있는데, 그렇다면 새롭게 할당된 4byte의 stack 메모리 [0x006FFB1C, 0x006FFB1F]에는 어떤 값이 들어가 있는지 메모리를 확인해보자.

```
0x006FFB1C  50 
0x006FFB1D  10 
0x006FFB1E  A8 
0x006FFB1F  00 
```

이를 해석해보면 높은 memory부터 값이 저장되니 00A81050 이라는 4byte 데이터가 저장되어 있다. 이 주소는 func() 함수가 호출이 끝나면 돌아갈 주소이다! 즉, 함수를 호출하게 되면 이 함수가 끝나고 돌아갈 주소를 가장 먼저 stack에 저장한다. 

그리고 여기서부터 func 함수의 stack frame이 시작된다.

#### push ebp 명령어가 실행된 후

다음으로, `push ebp`는 명령어가 실행된 후에 ebp와 esp 값을 살펴보자.

```
   ebp           esp
0x006ffb2c    0x006ffb18 
```

이 때, `push ebp`는 ebp에 들어 있는 4byte 데이터를 stack에 저장하라는 명령어이다.

stack에 데이터를 저장하는 과정은 앞에서 살펴봤듯이 esp를 증가시키고 새롭게 할당된 memory에 데이터를 저장하는 과정이다. 따라서, `push ebp`를 하면 esp가 4byte 줄어들어서 0x006FFB1C에서 0x006FFB18가 되며 ebp가 4byte 들어있는 4byte data인 0X006FFB2C가 새롭게 할당된 [0x006FFB18, 0x006FFB1B]에 저장된다.

메모리를 확인해보면 다음과 같다.

```
0x006FFB18  2c  
0x006FFB19  fb  
0x006FFB1A  6f  
0x006FFB1B  00  
```

결론적으로, `push ebp`를 통해 caller 함수인 main 함수의 ebp값이 calle 함수인 func 함수의 stack에 저장된다.

#### mov ebp,esp 명령어가 실행된 후

다음으로, `mov ebp,esp`는 명령어가 실행된 후에 ebp와 esp 값을 살펴보자.

```
   ebp           esp
0x006ffb18    0x006ffb18 
```

`mov ebp,esp`는 esp에 들어 있는 값을 ebp에 저장하는 명령어이다. 따라서, ebp의 값은 0x006FFB18 되며 주소값 0x006FFB18이 func 함수의 stack frame에 base 주소가 된다.

#### sub esp,14h 명령어가 실행된 후

다음으로, `sub esp,14h`는 명령어가 실행된 후에 ebp와 esp 값을 살펴보자.

```
   ebp           esp
0x006ffb18    0x006ffb04 
```

`sub esp,14h`는.func 함수의 stack frame에 20Byte의 메모리를 할당하는 과정이며 이는 지역 변수인 str을 저장하기 위해 필요한 메모리를 할당하는 것이다.


#### 총정리
정리하자면 `call func (0A81000h)` 명령어가 실행되면 func이 종료되고 실행되야 할 명령어의 주소가 저장되며 여기서 부터 func 함수의 stack frame이 시작된다. 그리고 `push ebp` 명령어를 통해 caller 함수의 ebp를 stack에 저장해두고 `mov ebp,esp`를 통해 func 함수의 ebp를 설정한다.  그리고 local 변수를 저장하기 위한 memory를 한번에 할당한다. 이 과정이 새로운 stack frame을 시작하고 메모리를 할당하는 과정이다.

이를 그림으로 정리하면 다음과 같다.

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