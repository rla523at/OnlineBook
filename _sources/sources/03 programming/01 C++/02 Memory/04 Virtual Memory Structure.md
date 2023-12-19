# Virtual Memory Structure
virtual memory는 분할되어 있으며, 각각의 분할된 공간을 `파티션(partition)`이라고 한다. 주소 공간의 분할 방식은 운영체제의 구현 방식에 따라 서로 다를 수 있으며 윈도우 계열에서도 커널이 달라지면 그 구조가 조금씩 달라지곤 한다.

32비트 시스템은 아래와 같이 분할되어 있다.

* Null 포인터 할당 파티션 : 0x00000000 ~ 0x0000FFFF
* 유저 모드 파티션 : 0x00010000 ~ 0x7FFEFFFF
* 64KB 접근 금지 파티션 : 0x7FFF0000 ~ 0x7FFFFFF
* 커널 모드 파티션 : 0x80000000 ~ 0xFFFFFFFF

윈도우에서는 운영체제 자체가 소유하고 있는 파티션에 접근을 금지한다. 이는 특정 프로세스의 수행 중인 쓰레드가 운영체제의 데이터에 접근하는 것이 불가능함을 의미한다. 따라서 사용자는 유저 모드 파티션(약 2GB)만 사용할 수 있다.

> Reference  
> {cite}`FundamentalC++`  

## Null 포인터 할당 파티션
0x00000000 ~ 0x0000FFFF(크기 64KB) 주소공간의 파티션은 프로그래머가 NULL 포인터 할당 연산을 수행할 경우를 대비하기 위해 준비된 영역이다. 만일 프로세스의 특정 쓰레드가 이 파티션에 대해 읽거나 쓰기를 시도하게 되면 `접근 위반(access violation)`이 발생한다.

```cpp
    int* ptr = nullptr; //NULL 포인터 할당 파티션 접근!!    
    *ptr = 1000;         //쓰기 acess 위반
    int a = *ptr;        //읽기 acess 위반
```

## 유저모드 파티션
프로세스의 주소 공간 내에서 유일하게 자유롭게 활용될 수 있는 파티션이다. 0x00010000 ~ 0x7FFEFFFF의 범위이므로, 2047MB의 크기이다. 모든 애플리케이션에 대해 프로세스가 유지해야 되는 대부분의 데이터가 저장되는 영역이며, EXE, DLL 파일이 이 파티션에 로드된다.

유저모드 파티션은 다시 Code영역, Data 영역,BSS영역, Heap영역, Stack영역 5가지로 나눌 수 있다.

### Code 영역
`텍스트 영역(text segment)`이라고도 불리는 `코드 영역(code segment)`은 실행 될 명령어가 저장 되어 있는 영역으로 CPU는 이 영역에 저장된 명령어를 하나씩 가져가서 처리한다. 또한 모든 함수는 code 영역에 저장된다. 이 영역은 오직 읽기만 가능한데, 쓰기가 금지된 이유는 코드의 안정성을 위해서이다. 만일 이 영역에 쓰기가 가능하다면 악의적인 목적으로 실행 코드 자체를 변조할 수 있다.

### Data 영역
Data 영역은 초기화된 전역 객체가 저장되는 곳이다. 이 영역의 크기는 컴파일 시 결정되며 런타임에 바뀌지 않는다. 세부적으로는 read only data 영역과 read/write data 영역으로 구분된다. read only data(.rodata) 영역은 상수 전역 변수가 저장되는 영역이다. 하지만 반드시 모든 상수 전역 변수가 .rodata에 저장되는 것은 아니며 .text에 저장되기도 한다. 이는 컴파일러 구현에 달려있다.

> Reference  
[stackoverflow - use-of-readonly-memory-in-data-segment](https://stackoverflow.com/questions/39252410/use-of-readonly-memory-in-data-segment)  

### BSS 영역
`block starting symbol (BSS)` 영역은 초기화 되지 않은 전역 객체가 저장되는 곳이다. 초기화되지 않았다고 해서 쓰레기 값이 들어있는 것은 아니고 일반적으로 메모리가 모두 0으로 채워진다.

초기화 여부는 객체 자체의 내용을 저장하느냐, 빈 객체가 있다는 사실을 저장하느냐의 차이다. 실행 파일의 데이터 영역에는 실제 초기화된 값들이 저장되어 있지만, 실행 파일의 BSS 영역에는 어떤 객체들이 필요한지가 저장되어 있다.

만약 실행 파일에 초기화되지 않은 32MB 크기의 배열 객체가 있다고 가정해보자. 이것을 그대로 저장한다면 실행 파일은 32MB 이상이 될 것이다. 이것은 무척 낭비가 심하다. 실행 파일의 BSS 영역에는 단지 "32MB 크기의 초기화되지 않은 배열 객체가 있다."는 정보만 기록하고 있으면 된다. 따라서 32MB의 크기를 포함할 필요가 없다.

이제 프로세스가 시작되면서 실행 파일의 내용이 가상 메모리에 올라가게 된다. OS는 실행 파일의 Code와 Data영역에 해당하는 부분을 그대로 가상 메모리에 복사하지만, BSS 영역에 해당하는 부분에 대해서는 조금 다른 처리를 수행한다. 배열 객체의 크기인 32MB의 메모리를 확보하고 0으로 채워 넣는 것이다. 초기화 되지 않은 전역 객체는 수없이 많을 수 있다. 따라서 하나의 영역에 몰아놓고, 통째로 해당 영역을 0으로 초기화하는 것이 효율적이다. 그래서 초기화되지 않은 전역 객체에 대해서는 하나의 영역을 만들게 된 것이고, 그것이 바로 가상 메모리의 BSS 영역이다.

> Reference  
> {cite}`FundamentalC++`  

### Heap 영역
Heap 영역은 동적 메모리가 저장되는 곳이다. 따라서 heap 영역의 크기는 런 타임(run time)에 사용자가 동적 할당하는 메모리 크기에 의해 결정된다. 이렇게 런 타임에 메모리를 할당받는 것을 메모리의 `동적 할당(dynamic allocation)`이라고 한다.

반면 Heap은 런타임에 동적으로 할당되기 때문에 메모리 할당 & 해제 관리에 자원이 소모된다.
또한, Thread Safe하지 않아 동기화 작업을 거쳐야 한다. 메모리 공간이 정적이지 않고, 크기가 변할 수 있어 스택보다 더 느리다.

> Reference  
> [educative - stack-vs-heap](https://www.educative.io/blog/stack-vs-heap)  
> 

### Stack 영역
stack 영역은 스레드당 하나씩 생성된다. 컴파일러 설정에 따라 다를 수 있지만 보통 기본적으로 스레드당 1MB~4MB 정도의 virtual memory 영역이 스레드 stack 공간으로 예약된다. 스레드가 생성되는 순간 운영체제는 virtual memory의 남은 영역 중에서 적당한 곳을 골라서 스레드 Stack으로 예약해준다. 

함수가 호출되면 stack에 `스택 프레임(stack frame)`이라는 block of memory가 할당된다. stack frame은 함수만의 스택 영역을 구분하기 위하여 생성되는 공간으로 지역 변수, 함수의 인자, 지역 배열, 함수의 return adress등이 저장된다. stack frame에 할당될 메모리의 크기는 compile time에 미리 결정되며 함수가 종료되면서 소멸한다. 이 모든 과정은 자동으로 수행되며 프로그래머는 stack 영역에서의 메모리 할당과 해제에 대한 걱정을 할 필요가 없다.

stack frame이 생성되고 제거되는 걸 간단하게 살펴보자. 아래 그림에서 흰색으로 표현한 것이 stack frame이다.

```{figure} _image/0401.png
```

1. 10번째 줄에 main 함수가 시작할 때, main 함수에 대한 stack frame이 생성된다.
2. 15번째 줄에서 add 함수를 호출하여 4번째 줄에 add함수가 시작할 때, add 함수에 대한 stack frame이 생성된다.
3. add 함수가 끝난 뒤 8번째 줄에서 add 함수의 stack frame이 소멸한다.
4. main 함수가 끝난 뒤 20번째 줄에서 main 함수의 stack frame이 소멸한다.

이번에는 stack frame에 데이터를 저장하는 과정을 조금 더 자세하게 보도록 하자. 

stack frame에 데이터를 저장할 때 `스택 포인터(Stack Pointer Resister; SP)`를 이동시킨 뒤 SP위치에 데이터를 저장한다. 함수가 끝나면 사용한 메모리를 반납하는게 아니라 SP를 감소시키기만 한다. stack에 남아있던 데이터는 향후 SP가 다시 증가하면 덮어 쓰여진다. 이처럼, stack은 `후입선출(last in first out; LIFO)`구조를 가지고 있어 가장 최근에 추가된 data가 가장 먼저 제거된다.

stack frame이 할당되고 소멸될 때 SP가 어떻게 동작하는지는 [eliez3r - 스택프레임(Stack Frame) 이란?](https://eliez3r.github.io/post/2019/10/16/study-system.Stack-Frame.html)에 잘 나와있다.

stack 영역은 다른 영역들과 달리 높은 주소에서 낮은 주소로 자라나는 형태를 가진다. 이러한 이유는 stack 영역이 엄청나게 커지더라도 운영체제의 핵심인 커널영역을 침범할 수 없게 하기 위해서다. 

stack 영역을 초과하면 stack overflow가 발생한다.




 
> Reference  
> [bozeury - 스택의 최대 할당 크기](https://bozeury.tistory.com/90)  
> [educative - stack-vs-heap](https://www.educative.io/blog/stack-vs-heap)  
> [manx - 프로세스 주소공간](https://velog.io/@manx/OS-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%A3%BC%EC%86%8C%EA%B3%B5%EA%B0%84)
> https://tcpschool.com/c/c_memory_stackframe  
> [eliez3r - 스택프레임(Stack Frame) 이란?](https://eliez3r.github.io/post/2019/10/16/study-system.Stack-Frame.html)  
> [weweweme - 스택 프레임(Stack Frame)이란](https://velog.io/@weweweme/%EC%8A%A4%ED%83%9D-%ED%94%84%EB%A0%88%EC%9E%84Stack-Frame%EC%9D%B4%EB%9E%80)  
> [furysecurity - 스택 프레임(Stack Frame) 실습](https://furysecurity.tistory.com/14)  
> [jungwoong  - 스레드 스택 메모리 구조](https://jungwoong.tistory.com/45)  
> [techref - stack heap 차이점](https://blog.naver.com/PostView.nhn?blogId=techref&logNo=222274484731)


## 64KB 접근 금지 영역
유저모드 파티션과 커널 모드 파티션 중간의 64KB 크기 메모리 영역을 정하였다. 이 영역은 Private영역과 Shared영역을 구분하기 위한 완충지대를 둔 것이다. NULL 포인터 영역과 같이 접근을 시도할 경우 Access Violation을 일으킨다.

> Reference  
> {cite}`FundamentalC++`  

## 커널 모드 파티션  
운영체제를 구성하는 코드들이 위치하며 시스템 내의 모든 프로세스들이 공유한다. 스레드 스케줄링, 메모리 관리, 파일시스템 지원, 네트워크 지원들을 구현하는 모든 코드와 모든 디바이스 드라이버들이 커널 모드 파티션에 로드된다. 이것은 물리적 메모리에 한 번 로드 되며, 모든 프로세스가 공통으로 사용한다는 뜻인데, 프로세스가 이 영역을 직접 접근하는 것은 막고 있다. API함수를 통하여 얻은 핸들을 통해서만 이 영역을 사용할 수 있다. 프로세스에서 이 주소공간에 읽거나 쓰기를 시도하게 되면 접근 위반이 발생한다. 따라서 이 파티션의 코드와 데이터는 완벽하게 보호된다. 

> Reference  
> {cite}`FundamentalC++`  


## stack vs heap
* https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack
  * Accessing from heap is slightly slower cause of the indirection
  * Allocation and deallocation
    * For global data (including C++ namespace data members), the virtual address will typically be calculated and hardcoded at compile time
    * For stack-based data, the stack-pointer-register-relative address can also be calculated and hardcoded at compile time
    * Then the stack-pointer-register may be adjusted by the total size of function arguments, local variables, return addresses and saved CPU registers as the function is entered and returns
    * Both of the above are effectively free of runtime allocation/deallocation overhead, while heap based overheads are very real and may be significant for some applications...
    * For heap-based data, a runtime heap allocation library must consult and update its internal data structures to track which parts of the block(s) aka pool(s) of heap memory it manages are associated with specific pointers the library has provided to the application, until the application frees or deletes the memory. If there is insufficient virtual address space for heap memory, it may need to call an OS function like sbrk to request more memory (Linux may also call mmap to create backing memory for large memory requests, then unmap that memory on free/delete).
  * Access
    * Because the absolute virtual address, or a segment- or stack-pointer-register-relative address can be calculated at compile time for global and stack based data, runtime access is very fast
    * With heap hosted data, the program has to access the data via a runtime-determined pointer holding the virtual memory address on the heap, sometimes with an offset from the pointer to a specific data member applied at runtime. That may take a little longer on some architectures.
    * For the heap access, both the pointer and the heap memory must be in registers for the data to be accessible (so there's more demand on CPU caches, and at scale - more cache misses/faulting overheads)
  * Layout
    * global variables, stack-based variables, so the caching is very efficient.
    * When you use heap based memory, successive calls to the heap allocation library can easily return pointers to memory in different cache lines, especially if the allocation size differs a fair bit (e.g. a three byte allocation followed by a 13 byte allocation) or if there's already been a lot of allocation and deallocation (causing "fragmentation"). This means when you go to access a bunch of small heap-allocated memory, at worst you may need to fault in as many cache lines (in addition to needing to load the memory containing your pointers to the heap). The heap-allocated memory won't share cache lines with your stack-allocated data - no synergies there.
    * 
* https://arca.live/b/programmer/67268686
  * "스택이 힙보다 빠르다"고 하는 건, 사실 "스택 할당된 변수에 접근하는 것이 힙 할당 변수에 접근하는 것보다 빠르다"라고 해야 해.
  * 스택 메모리 영역이 힙 메모리 영역보다 그 자체로 빠른 건 절대로 아니란 거야. 생각해보면 스택이나 힙이나 가상메모리 영역에 불과하고, 하드웨어로 내려가면 똑같이 RAM의 어딘가에 저장된 페이지에 불과하니까 둘의 속도가 다를 이유는 전혀 없어.
  * 스택에 할당된 변수들은 주소를 정말 간단하게 계산할 수 있어. 변수의 offset을 알고 있고 스택 바닥 주소를 알고 있다면, 그냥 이 둘을 더하면 해당 변수의 정확한 주소가 나오는 거잖아?
  * 힙은 결국 OS가 관리하는 거니까, 어셈블리를 컴파일하는 단계에서는 절대로 이 변수가 힙의 어떤 offset에 할당될지 알 수 없어. 그래서 OS에서 할당받은 주소를 스택에 할당된 변수에 저장해두는 거고, 실제로 힙에 접근할 때는 add 이후 load로 변수의 주소값을 읽어들인 뒤에 다시 그 주소를 load하는 게 최선이라는 거지.
  * 스택 할당이 힙할당보다 한참 싸고, 접근할때 인스트럭션도 덜 들어가고, 캐시히트 측면에서도 유리하다는거네
* https://m.todayhumor.co.kr/view.php?table=programmer&no=5516
  * 스택은 이미 할당 되어 있는 공간을 사용하는 것이고, 힙은 따로 할당해서 사용하는 공간이죠.
  * 스택에서 할당의 의미는 단순히 스택 내에서 가리키고 있는 포인터의 위치를 바꾼다라는 매우 단순한 CPU instruction(단순히 덧셈과 뺄셈 연산, 일반적으로 단일 instruction)이다.
  *  힙 할당에서 나타나는 요청된 chunk의 크기, 현재 메모리의 fragmentation 상황 등등등 다양한 것을 고려하기 때문에 더 많은 CPU instruction이 필요합니다.
* https://fortran-lang.discourse.group/t/why-stack-is-faster-than-heap-and-what-exactly-is-stack/2130
  * The difference is that the compiler generally knows where to find variables that are on the stack when it compiles the code.
  * The heap is allocated dynamically as the program runs, so the compiler doesn’t know where the variables are coming from, reducing optimization opportunities
  * stack variables are often used enough that they are kept in the CPU cache
  * Stack variables cannot be accessed from another thread, so the compiler can assume they aren’t going to change.
  * Stack variables do not require allocating on the heap, which is managed by a memory allocator, which is a slow process
  * Stack variables do not need to be “freed”, which is also a slow process
* https://stackoverflow.com/questions/27291991/heap-vs-stack-in-regard-to-read-write-speed 
  * There is no difference in the access speed, its the same memory after all.
* https://stackoverflow.com/questions/161053/which-is-faster-stack-allocation-or-heap-allocation
  * Stack allocation is much faster since all it really does is move the stack pointer.
  * Using memory pools, you can get comparable performance out of heap allocation, but that comes with a slight added complexity and its own headaches.
  * It literally only uses a single instruction on most architectures, in most cases, That moves the stack pointer down by 0x10 bytes and thereby "allocates" those bytes for use by a variable.
  * heaps are often shared among the threads in a process. For such heaps, the allocation and deallocation functions have to protect the shared internal administrative data structure from the data race. As a result, heap allocations and deallocations may have additional overhead due to internal synchronization operations.



> Reference  
> 
> 
> 
> 
> https://publicwork.wordpress.com/2019/06/27/stack-allocation-vs-heap-allocation-performance-benchmark/
> https://fortran-lang.discourse.group/t/why-stack-is-faster-than-heap-and-what-exactly-is-stack/2130
> https://stackoverflow.com/questions/27291991/heap-vs-stack-in-regard-to-read-write-speed
> https://zijishi.xyz/post/cpp/understanding-performance-cpp-stack-heap/