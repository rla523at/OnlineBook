# Virtual Memory Space
`가상 메모리 공간(virtual memory space)`은 각각의 프로그램마다 독립적으로 부여된 가상의 메모리 공간이다.

이 때, 부여되는 virtual memeory space의 크기는 os에 따라 다르다.

x86은 32비트 시스템이다. 주소를 가리킬 수 있는 레지스터의 크기가 32비트라는 의미이다. 즉, 이론적으로는 $2^{32}$ 대략 42억 개의 주소가 나오게 된다. 각각의 주소가 1바이트씩 가리킬 수 있으므로 각각의 프로세스는 4GB의 가상 메모리를 부여 받게 된다. 

마찬가지로 x64는 64비트 시스템이며 레지스터의 크기가 64비트이므로 이론적으로 $2^{64}$ 개의 주소를 사용할 수 있기 때문에 대략 16EB$(2^4 \times 2^{60})$byte의 가상 메모리 공간이 부여될 수 있다. 사실상 무한한 메모리 공간이라고 할 수 있는데, 아직까지 이렇게 큰 메모리 공간이 필요한 프로그램이 존재하지 않기 때문에 운영체제별로 다르긴 하지만 윈도우의 경우 프로세스마다 대략 8TB의 공간만을 부여하도록 제한하고 있다. 

그리고 이렇게 부여된 virtual memory space는 다음과 같이 4가지 공간으로 분할되어 있다.
* Null 포인터 할당 파티션
* 유저 모드 파티션
* 64KB 접근 금지 파티션
* 커널 모드 파티션

이 떄, 각각의 분할된 공간을 `파티션(partition)`이라고 한다. 단. 분할 방식은 운영체제의 구현 방식에 따라 서로 다를 수 있으며 윈도우 계열에서도 커널이 달라지면 그 구조가 조금씩 달라질 수 있다.

32bit system의 경우 다음과 같은 주소 값을 갖는다.
* Null 포인터 할당 파티션 : 0x00000000 ~ 0x0000FFFF
* 유저 모드 파티션 : 0x00010000 ~ 0x7FFEFFFF
* 64KB 접근 금지 파티션 : 0x7FFF0000 ~ 0x7FFFFFF
* 커널 모드 파티션 : 0x80000000 ~ 0xFFFFFFFF

윈도우에서는 운영체제 자체가 소유하고 있는 커널 모드 파티션에 접근을 금지한다. 이는 특정 프로세스의 수행 중인 쓰레드가 운영체제의 데이터에 접근하는 것이 불가능함을 의미한다. 따라서 사용자는 유저 모드 파티션(약 2GB)만 사용할 수 있다.

> Reference  
> {cite}`FundamentalC++`  

## Null 포인터 할당 파티션
Null 포인터 할당 파티션이란 프로그래머가 NULL 포인터 할당 연산을 수행후 발생할 수 있는 상황에 대비하기 위해 준비된 영역이다.

만일 프로세스의 특정 쓰레드가 Null 포인터 할당 연산으로 이 파티션에 접근한 뒤 읽기나 쓰기를 시도하게 되면 `접근 위반(access violation)`을 발생시킨다.

```cpp
    int* ptr = nullptr;  //NULL 포인터 할당 파티션 접근!!    
    *ptr = 1000;         //쓰기 acess 위반
    int a = *ptr;        //읽기 acess 위반
```

> Reference  
> {cite}`FundamentalC++`  
> [jungwoong - 윈도우 메모리 구조](https://jungwoong.tistory.com/44)

## 유저 모드 파티션
유저 모드 파티션이란 프로그래머가 자유롭게 활용할 수 있는 영역입니다.

모든 애플리케이션에 대해 프로세스가 유지해야 되는 대부분의 데이터가 저장되는 영역이며, EXE, DLL 파일이 이 파티션에 로드된다.

유저모드 파티션은 다시 Code영역, Data 영역,BSS영역, Heap영역, Stack영역 5가지로 나눌 수 있다.

### Code 영역
`코드 영역(code segment)`은 `텍스트 영역(text segment)`이라고도 불리며 실행 될 명령어가 저장 되어 있는 영역이다. 

CPU는 이 영역에 저장된 명령어를 하나씩 가져가서 처리한다. 또한 모든 함수는 code 영역에 저장된다. 이 영역은 안전성을 위해 오직 읽기만 가능한데, 만일 이 영역에 쓰기가 가능하다면 악의적인 목적으로 실행 코드 자체를 변조할 수 있기 때문이다.

### Data 영역
`데이터 영역(Data segment)`은 초기화된 전역 객체가 저장되는 영역이다. 

세부적으로는 read only data 영역과 read/write data 영역으로 구분된다. read only data(.rodata) 영역은 상수 전역 변수가 저장되는 영역이다. 하지만 반드시 모든 상수 전역 변수가 .rodata에 저장되는 것은 아니며 .text에 저장되기도 한다. 이는 컴파일러 구현에 달려있다.

실행 파일의 Data영역에는 초기화된 전역 객체들의 값이 저장되어 있으며 이후 프로세스가 시작되어 실행 파일에 내용이 가상 메모리에 올라가게 되면 OS는 실행 파일의 Data영역을 그대로 가상 메모리의 Data segment에 복사한다.

> Reference  
[stackoverflow - use-of-readonly-memory-in-data-segment](https://stackoverflow.com/questions/39252410/use-of-readonly-memory-in-data-segment)  

### BSS 영역
`block starting symbol (BSS)` 영역은 초기화 되지 않은 전역 객체가 저장되는 영역이다. 

실행 파일의 BSS영역에는 어떤 크기의 초기화 되지 않은 전역 객체가 존재하는지 저장되어 있다. 이후 프로세스가 시작되어 실행 파일에 내용이 가상 메모리에 올라가게 되면 실행파일의 BSS영역에 있는 정보를 바탕으로 초기화 되지 않은 전역 객체들의 크기만큼 가상메모리의 한 영역의 메모리를 모두 0으로 채운다. 이를 가상 메모리의 BSS 영역이라고 한다.

#### 실행파일의 BSS영역
예를 들어, 실행 파일에 초기화되지 않은 32MB 크기의 배열 객체가 있다고 가정해보자. 이것을 그대로 저장한다면 실행 파일은 32MB 이상이 될 것이다. 이것은 무척 낭비가 심하다. 실행 파일의 BSS 영역에는 단지 "32MB 크기의 초기화되지 않은 배열 객체가 있다."는 정보만 기록하고 있으면 된다. 따라서 32MB의 크기를 포함할 필요가 없다. 

> Reference  
> {cite}`FundamentalC++`  

### Heap 영역
`힙 영역(heap segment)`은 run time에 `동적 할당(dynamic allocation)` 된 메모리가가 저장되는 영역이다. 

heap 영역에 memory를 할당/해제는 경우 run time 오버헤드가 존재한다. 왜냐하면 다음과 같은 일들을 하기 때문이다.
* heap memory API를 사용해서 heap memory 할당할 때
  * 할당 내역 테이블 작성
  * 미할당 영역이 부족할 시 virtual memory API를 통해 heap memory 증가
  * multi thread 환경에서 동기화
* heap memory API를 사용해서 heap memory 해제할 떄
  * 할당 내역 테이블 참고
  * 미할당 영역으로 변경
  * 주변 미할당 영역과 병합
  * multi thread 환경에서 동기화

multi thread 환경에서 동기화를 하는 이유는 heap 영역은 여러 thread에서 공유하는 memory이기 때문이다. 따라서 할당,해제가 발생할 때 HeapLock과 HeapUnlock 함수를 호출하여 순차적인 접근이 일어나도록 한다. 이로인해 여러 쓰레드에서 동시에 이 작업을 요청할 경우 대기에 의해 속도가 느려질 수 있다.

heap에 저장된 data의 주소는 compile time에 알 수가 없다. 따라서, run time에 결정되는 pointer를 통해서 접근해야 한다. 하지만 Cache, RAM, swap file에 의한 영향을 무시한다면 stack에 있는 data와 heap에 있는 data에 접근하는 속도는 거의 동일하다. 하지만 heap에 저장된 여러 data들은 memory상에 연속성이 보장되지 않기 때문에 cache 친화적이지 않다. data 하나가 memory 연속성이 없다는 얘기가 아니라 data 여러개가 memory 연속성이 없다는 얘기이다. 예를 들어 heap에 배열 2개를 할당하면 주소값이 연속되지 않을 수 있다. 다음 예시 코드를 보자.

```cpp
int main() {
  vector<char> str3(5); // str3.data() = 0x0131ddb0
  vector<char> str4(9); // str4.data() = 0x0131ccb8
  return 0;
}
```

str3와 str4가 memory에 어디에 위치하는지 보면 연속적인 위치에 있지 않다는걸 알 수 있다. 이렇게 불연속적으로 위치할 경우 순차적으로 str3, str4를 접근할 때 cache miss, page fault등이 발생할 수 있으며 이로인해 접근속도가 매우 느려질 수 있다.

> Reference  
> [educative - stack-vs-heap](https://www.educative.io/blog/stack-vs-heap)  
> [stackoverflow - is-accessing-data-in-the-heap-faster-than-from-the-stack](https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack)  

### Stack 영역
`스택 영역(stack segment)`은 함수와 관련된 값들이 저장되는 영역이다.

stack segment는 스레드당 하나씩 생성된다. 컴파일러 설정에 따라 다를 수 있지만 보통 스레드가 생성되는 순간 운영체제는 virtual memory의 남은 영역 중에서 스레드당 1MB~4MB 정도의 virtual memory 영역을 스레드 stack 공간으로 예약한다.

stack segment에 memory를 할당하는 과정은 `스택 포인터(Stack Pointer Resister; SP)`를 감소시키는 일이다. 반대로 memory를 해제하는 과정은 SP를 증가시키는 일이다. 이처럼, stack은 가장 최근에 할당된 memory가 가장 먼저 해제되는 `후입선출(last in first out; LIFO)`구조를 가지고 있다. 

그리고 stack의 memory 할당/해제 과정은 run time 오버헤드가 거의 없다. 왜냐하면 stack 영역에 할당되는 data는 compile time에 크기가 결정되며 stack에서 memory를 할당하고 해제하는 과정은 미리 알고 있는 data 크기만큼 SP를 감소시키거나 증가시키는 과정에 지나지 않기 때문이다. 또한 모든 memory 할당/해제 과정은 compiler에 의해 자동으로 수행되기 때문에 프로그래머는 걱정 할 필요가 없다.

stack에 저장된 data의 주소는 compile time에 알 수 있기 때문에, compile시 hardcoding되어 runtime에 매우 빠르게 접근이 가능하다. 또한 stack에 저장된 data들은 memory상에 연속적으로 저장되기 때문에 stack에 저장된 data들에 연속적으로 접근하는 행위는 cache 친화적이다. 다음 예시 코드를 보자.
```cpp
int main() {
    char str1[5]; // &str1 = 0x010ffc38
    char str2[8]; // &str2 = 0x010ffc30, &str2[7] = 0x010ffc37
  return 0;
}
```

str2의 주소값과 str1의 주소값이 연속으로 존재하는 걸 알 수 있다.

참고로, SP가 증가함에 따라 SP보다 작아 범위는 벗어났지만 stack에 남아있던 데이터는 향후 SP가 다시 감소하고 덮어 쓰여진다. 

> Reference  
> [educative - stack-vs-heap](https://www.educative.io/blog/stack-vs-heap)  
> [stackoverflow - is-accessing-data-in-the-heap-faster-than-from-the-stack](https://stackoverflow.com/questions/24057331/is-accessing-data-in-the-heap-faster-than-from-the-stack)  

## 64KB 접근 금지 파티션
64KB 접근 금지 파티션은 Private영역인 커널 모드 파티션과 Shared 영역인 유저 모드 파티션을 구분하기 위한 영역이다.

NULL 포인터 영역과 같이 접근을 시도할 경우 Access Violation을 일으킨다.

> Reference  
> {cite}`FundamentalC++`  

## 커널 모드 파티션  
커널 모드 파티션은 스레드 스케줄링, 메모리 관리, 파일시스템 지원, 네트워크 지원들을 구현하는 모든 코드와 모든 디바이스 드라이버들등 운영체제를 구성하는 코드들이 위치하는 영역이다.

primary 메모리에 한 번 로드 되며, 모든 프로세스가 공통으로 사용한다. 프로세스가 이 영역을 직접 접근하는 것은 막고 있다. API함수를 통하여 얻은 핸들을 통해서만 이 영역을 사용할 수 있다. 프로세스에서 이 주소공간에 읽거나 쓰기를 시도하게 되면 접근 위반이 발생한다. 따라서 이 파티션의 코드와 데이터는 완벽하게 보호된다. 

> Reference  
> {cite}`FundamentalC++`  


## stack vs heap
* https://arca.live/b/programmer/67268686
* https://m.todayhumor.co.kr/view.php?table=programmer&no=5516
* https://fortran-lang.discourse.group/t/why-stack-is-faster-than-heap-and-what-exactly-is-stack/2130
* https://stackoverflow.com/questions/27291991/heap-vs-stack-in-regard-to-read-write-speed 
* https://stackoverflow.com/questions/161053/which-is-faster-stack-allocation-or-heap-allocation
* https://zijishi.xyz/post/cpp/understanding-performance-cpp-stack-heap/
* https://publicwork.wordpress.com/2019/06/27/stack-allocation-vs-heap-allocation-performance-benchmark/
* [manx - 프로세스 주소공간](https://velog.io/@manx/OS-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%A3%BC%EC%86%8C%EA%B3%B5%EA%B0%84)
* [techref - stack heap 차이점](https://blog.naver.com/PostView.nhn?blogId=techref&logNo=222274484731)  
