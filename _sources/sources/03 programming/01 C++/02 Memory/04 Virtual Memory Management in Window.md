# Virtual Memory Management in Window
메모리를 관리하는 것은 전적으로 운영체제의 몫이다. 프로세스가 가상 메모리를 할당 받기 위해서는 해당 운영체제에게 메모리 요청을 해야만한다. Windows에서 제공하는 메모리 관련 Windows API는 아래 그림과 같다.

```{figure} _image/0401.png
```

위 그림을 보면 Heap Memory API는 Virtual Memory API를 통해 구현되어있고 Local/Global Memory API와 CRT Memory fuctions and operations는 Heap Memory API로 구현되어있음을 알수 있다. 결국 메모리 맵 파일 API를 제외하고 모든 API는 Virtual Memory API를 이용하고 있다는 말이다. 메모리 맵 파일을 제외한 모든 메모리 관련 API 또는 오브젝트들은 Virtual Memory API을 이용하여 특정 용도에 맞도록 보다 쉽게 구현된 것들이다. Windows 운영체제가 Virtual Memory API를 기반으로 만들어졌으니 당연한 일일 것이다.

## Virtual memory API
User Mode 에서 사용할 수 있는 가장 낮은 수준의 API이다. 따라서 Windows 메모리 관리의 가장 기본이 되는 API다.

Windows는 기본적으로 페이지 단위의 가상 메모리 관리 기법을 사용한다. 이러한 Windows의 관리 기법을 개발자가 동일한 수준에서 수행할 수 있도록 virtual memory API를 지원한다. 따라서 page 단위의 메모리에 할당 유형(commited, reserved, free)과 가상 메모리 접근 등급을 직접 결정할 수 있다.

Virtual memory API를 사용하여 메모리를 사용할 때는 시스템에 따른 페이지 크기로만 가능하므로, 불과 몇 바이트의 작은 크기를 사용할 때는 효과적이지 못하다. 예를들어 32 byte를 할당하기 위해서는 1 page(4KB = 4096byte)를 전부 물리 메모리에 맵핑 시켜야한다. 또한 virtual memory API는 각 page들을 관리하기 위해 `페이지 디렉토리(page directory)`라는 것이 있고, 이는 4KB의 추가적인 물리 메모리를 소모한다([참고1 - stackoverflow](https://stackoverflow.com/questions/29945171/difference-between-page-table-and-page-directory), [참고2 - stackoverflow](https://stackoverflow.com/questions/26858196/why-does-page-directory-entries-need-20-bits-to-point-210-page-tables)). 따라서 대용량 구조체 또는 객체의 배열을 사용하고자 할 때 사용한다.

Virtual memory API와 관련된 자세한 사항들은 [VirtualAlloc - MSDN](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc), [VirtualFree - MSDN](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualfree), [윈도우 메모리 구조 - 보안 세상 블로그](https://securitynewsteam.tistory.com/99)를 참고하면 된다.

## Heap memory API
Virtual memory API가 작은 크기의 메모리 영역을 관리하는데 비효율적이기 때문에 이를 보완한 heap memory API를 제공한다.

Heap memory API는 커널 오브젝트인 heap 오브젝트를 이용한다. Heap 오브젝트는 크기가 작은 메모리 영역을 효율적으로 사용기 위해 미리 내부적으로 virtual memory API를 활용해 가상 메모리를 예약해 두고 프로그램으로부터 할당 요구가 있을 때마다 heap manager가 자동적으로 virtual memory API를 이용해 내부적으로 페이지 단위의 메모리 할당하고 요구에 맞게 메모리를 잘라 메모리 블록을 확보 한다.

Heap memory API을 사용하면 할당 단위나 페이지 경계와 같은 특성을 고려할 필요 없이 수행하고자 하는 작업에 좀 더 집중할 수 있는 장점이 있지만 heap manager에 의해 메모리가 자동적으로 제어됨으로 직접적인 제어권을 잃는다는 단점이 있다.

Heap memory API에서 메모리 할당과 해제 작업은 다중 스레드 환경에서 동기화로 이루어진다. 어느 한 시점에서 heap 메모리를 할당하는 작업은 하나의 스레드에게만 허용된다. 이를 보장하기 위해 HeapAlloc, HeapRealloc, HeapSize, HeapFree 등의 함수들은 내부적으로 HeapLock과 HeapUnlock 함수를 호출하여 순차적인 접근이 일어나도록 한다. 이로인해 여러 쓰레드에서 동시에 이 작업을 요청할 경우 대기에 의해 속도가 느려질 수 있다.

Heap memory API에 관한 자세한 내용은 [heapapi.h header overview - MSDN](https://docs.microsoft.com/en-us/windows/win32/api/heapapi/)를 참고하면 된다.

### 프로세스 기본 힙

프로세스가 초기화되면 가장 기본이 되는 heap 오브젝트인 `프로세스 기본 힙(process default heap)`을 생성한다. 프로세스 기본 힙은 가상 메모리 공간에 1MB의 영역을 예약한다. (참고로, 기본적으로 할당되는 쓰레드 스택의 크기도 1MB 이다. 윈도우 개발환경에서 VS IDE를 쓴다면 프로젝트 설정에서 Heap reserved size를 조절할 수도 있다.) 프로세스의 기본 힙에 대한 핸들을 얻으려면 GetProcessHeap 함수를 호출하면 된다.

```cpp
HANDLE GetProcessHeap();
```

다양한 크기의 메모리 할당/해제가 빈번한 프로그램을 생각해보면, 프로세스 기본 힙의 단편화에 대한 걱정을 떨쳐 버릴수가 없다. 예를 들어 List와 Tree를 사용한다고 가정하면 page 내 영역에서 각 자료구조의 노드들이 아주 복잡하게 분산되어 있을 것이다. 한 페이지 내에 다 들어갈 크기의 List를 사용하기 위해 몇 개의 페이지를 참조해야 하는 경우가 발생한다. 물리 메모리 공간이 부족하여 hdd swapping이라도 발생하면 이로 인해 발생하는 성능 하락은 무시하지 못할 것이다. 또한 List나 Tree의 각 node에 할당된 메모리를 해제하려 할 시, 모든 node를 iterate하며 각 node에 대해 해제를 해줘야 한다. 삽입과 삭제시에도 마찬가지다. 혹시나 모를 실수로 memory leak이 발생할 수 있는 가능성이 있다.

### 사용자 힙
프로세스 기본 힙 이외에 Heap memory API(HeapCreate)를 통해 프로세스에 추가로 생성한 힙을 `사용자 힙(private heap)`이라고 한다.

```cpp
HANDLE HeapCreate(
    DWORD fdwOptions,         // 힙 생성 옵션
    SIZE_T dwInitialSize,     // 힙 초기 크기
    SIZE_T dwMaximumSize);    // 힙 최대 크기
```

각 인수와 사용가능한 옵션에 대해서는 [heapapi-heapcreate - MSDN](https://docs.microsoft.com/ko-kr/windows/win32/api/heapapi/nf-heapapi-heapcreate)을 참고하면 된다.

사용자 힙의 장점은 아래 글들에 잘 정리되어 있다.  
[Process Default & Private heap - 수까락 블로그](http://egloos.zum.com/sweeper/v/1791208)    
[프로세스의 힙 메모리 - 웅이의 블로그](https://jungwoong.tistory.com/47)    
[heap에 관한 화제 - kiaak 블로그](https://kiaak.tistory.com/entry/Windowsheap%EC%97%90-%EA%B4%80%ED%95%9C-%ED%99%94%EC%A0%9C)  

## C/C++의 CRT memory function / operation
운영체제(윈도우, 리눅스)에 따라서 그리고 각각의 버전에 따라서 메모리를 관리하는 방법은 달라질 수 있다. 그럼에도 C/C++의 malloc/free 함수나 new/delete 연산자를 사용하게 되면 운엥체제의 차이를 뛰어넘어 메모리를 아주 쉽게 할당/해제 할 수 있다. 그 이유는 CRT Library 내부적에서 각각의 운영체제에 맞는 Heap memory API를 호출하기 때문이다.

참고로 Window에서 CRT는 프로세스 기본 힙을 이용해서 heap memory를 할당한다. C:\Program Files (x86)\Windows Kits\10\Source\10.0.19041.0\ucrt\heap의 malloc_base.cpp을 보면 __acrt_heap 핸들을 통해서 heap을 할당받는것을 알 수 있다. 그리고 동일 경로에 heap_handle.cpp을 보면 __acrt_heap이 GetProcessHeap 함수를 통해 할당 받은 프로세스 기본 힙의 핸들임을 알 수 있다.


```cpp
//malloc_base.cpp
void* const block = HeapAlloc(__acrt_heap, 0, actual_size);

// heap_handle.cpp
// Initializes the heap.  This function must be called during CRT startup, and
// must be called before any user code that might use the heap is executed.
extern "C" bool __cdecl __acrt_initialize_heap()
{
    __acrt_heap = GetProcessHeap();
    if (__acrt_heap == nullptr)
        return false;

    return true;
}
```

### malloc & free

```cpp
void* malloc(size_t size);
void free(void* memblock);
```

malloc은 size를 인자로 받아서 size 바이트만큼 메모리 블록을 할당한 후 해당 블록의 포인터를 반환하는 함수이다. 만일 할당할 수 있는 충분한 메모리가 존재하지 않을 경우에는 NULL을 반환하게 된다. 할당된 메모리는 프로세스가 종료되거나 직접 해제되기 전까지 계속해서 유효하게 접근될 수 있다. 이런 상세 요구를 만족하기 위하여 CRT Library는 heap memory API를 사용한다.

Heap 영역은 할당과 미할당 영역으로 구분될 수 있다. malloc 호출시 미할당 영역에서 요청된 크기만큼을 반환하면서 반환된 영역은 할당 영역으로 변경된다. malloc 요청이 많아질 경우 할당 영역은 늘어나고, 미할당 영역은 줄어들 것이다. Heap manager가 malloc 요청에 대해서 반환할 미할당 영역을 찾기 어려울 때 내부적으로 virtual memory API를 통해 가상 메모리를 할당하여 heap 자체를 늘린다.

free는 malloc을 통해서 할당된 메모리를 해제하는 함수이다. 인자로는 malloc을 통해서 반환된 포인터를 그대로 넣어주면 된다. free가 호출될 경우 인자 memblock이 가리키는 heap의 할당 영역을 찾아낸 뒤에 미할당 영역으로 변경해놓는다. 만일 인근에 미할당 영역이 접해있을 경우 미할당 영역을 하나로 합치기도 한다.

malloc은 인자로 할당을 원하는 메모리 크기를 받는데 비해서 free는 해제할 메모리 크기를 인자로 받지 않는다. 그럼에도 할당된 메모리를 제대로 해제할 수 있다. 그 이유는 HeapAlloc 함수에서 할당된 내역을 heap manager가 저장해놓고 있다가 HeapFree를 호출할 때 블록의 포인터만 인자로 넘겨줘도 해당 블록의 크기를 확인할 수 있고 깨끗하게 해제할 수 있기 때문이다. malloc과 free도 내부적으로 이 함수들을 호출함으로 같은 맥락으로 잘 작동할 수 있는 것이다.

Heap manager는 메모리 블록을 할당할 때마다 heap 할당 내역을 저장하게 된다. `할당 내역 테이블`에는 할당된 메모리 주소와 크기가 저장되어 있으며 HeapFree를 호출하게 될 경우 heap 관리자는 인자로 넘어온 메모리 주소를 할당 내역에서 검색하여 존재할 경우 할당된 크기만큼 해당 메모리 블록을 해제하여 미할당 영역으로 변경한다.

만일 free의 인자로 할당된 적이 없는 메모리 주소를 넘기게 될 경우 어떤 일이 벌어질까? [MSDN](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/free?view=msvc-170)에 의하면 다음의 할당 요청에 영향을 줄 수 있다고 되어있다. 이로 인해서 에러가 발생할 수 있다. 그러나 에러가 free 즉시 발생하는 것은 아니기 때문에 디버깅을 하는데 상당한 시간을 허비할 수 있어 주의해야 한다.

메모리 할당 요청 함수로 malloc을 살펴보았지만 할당 요청 함수가 malloc만 있는 것은 아니다. calloc, realloc 등도 있으며 필요에 따라서 적절히 선택하여 사용할 수 있다. 그러나 이와 같은 메모리 할당 함수를 이용하여 할당된 메모리를 해제하는 함수는 오직 free 하나 뿐이다.

> Reference  
> {cite}`FundamentalC++`

### new & delete
new와 delete는 C++에서 새롭게 도입된 메모리 할당 해제 연산자이다. C에서 사용하던 malloc과 free를 대신해서 사용할 수 있다. 그러나 반대로 new와 delete를 malloc과 free로 대신해서 사용할 수는 없다. 그 이유는 new와 delete가 단순하게 메모리 할당과 해제만 하는 것이 아니기 때문이다.

먼저 malloc/free와 new/delete 사용법의 차이를 보자.

```cpp
auto* ptr1 = (int*)malloc(sizeof(int));
*ptr1 = 1;

auto* parr1 = (int*)malloc(sizeof(int) * 2);
parr1[0] = 0;
parr1[1] = 1;

auto* ptr2 = new int;
*ptr2 = 1;

int* parr2 = new int[2];
parr2[0] = 0;
parr2[1] = 1;

free(ptr1);
free(parr1);

delete p2;
delete [] parr2;
```

malloc의 경우 인자로 할당할 메모리 크기를 알려주기 위해서 `sizeof(type)`과 같은 방법을 사용해야 된다. 또한 malloc에 의해서 반환된 포인터는 void*로서 메모리 덩어리를 가리키는 주소일 뿐이다. 따라서 의미 있는 포인터로 사용되기 위해서는 반드시 타입 변환을 수행해야 한다. 하지만 new의 경우에는 이러한 과정이 필요 없다. 단일 객체의 메모리 해제시 free와 delete는 별 차이가 없지만 배열 객체의 메모리 해제에서는 delete 뒤에 [] 기호가 쓰이는 차이점이 있다.

new와 delete는 malloc과 free를 단순히 쓰기 편하도록 대체하기 위해서 만든 것은 아니다. 사실 new는 내부적으로 malloc을 호출하고, delete도 역시 내부적으로 free를 호출한다. 결국 CRT Library에서 동적 메모리를 할당 해제하는 함수는 malloc과 free라고 할 수 있다. 즉, C++에서 새롭게 도입된 new와 delete는 메모리 할당과 해제는 malloc과 free에게 맡기고 동적으로 생성된 객체의 생성자와 소멸자를 호출하는 일을 추가적으로 한다. 결국 동적으로 객체를 생성한다면 반드시 new와 delete를 사용해야 하고 이것은 절대로 malloc과 free가 대신할 수 없다.

생성자와 소멸자는 C++에 클래스가 도입되면서 나타났다. 클래스 객체는 기본적으로 생성되면서 생성자가 호출되고, 사라지면서 소멸자가 호출되도록 만들어졌다. 클래스 객체가 전역 객체이거나 함수의 지역 객체일 경우 컴파일러는 어떤 클래스 타입의 객체가 생성되는지 알 수 있다. 따라서 객체 생성 과정에 생성자가 호출되도록 어셈블리를 추가한다. 이제 소멸자를 살펴보자. 지역 객체는 main이 끝나는 시점에, 전역 객체는 프로그램이 끝나는 시점에 각각 소멸자가 호출된다. 컴파일러가 타입을 알고 있기 때문에 가능한 일이다.

그러나 클래스 객체를 malloc을 사용하여 생성할 경우 생성자를 호출할 수가 없다. malloc은 오직 인자로 받은 크기만큼 메모리를 할당하고, 해당 메모리 덩어리를 가리키는 void*만을 반환하는 함수이기 때문이다. 즉, malloc이 호출되는 순간에는 어떤 클래스 타입의 객체가 생성되는지를 알 수가 없다. 알 수 없는 클래스의 생성자를 호출할 수는 없다. free를 이용하여 동적으로 할당된 객체를 해제할 때도 소멸자를 호출할 수가 없다. 왜냐하면 free는 오직 인자로 받은 포인터의 주소를 가지고 Heap manager를 통해 해당 포인터가 가리키는 메모리 블록을 해제하는 일만 하기 때문이다. 결국 기존의 malloc과 free로는 동적 할당으로 생성되는 클래스 객체의 생성자와 소멸자를 호출할 방법이 없었고, 이를 해결하기 위해서 new와 delete를 만들게 된 것이다.

new는 기본적으로 malloc을 통하여 메모리를 할당 받고, 할당 받은 메모리 영역에 대하여 생성자를 호출하는 역할을 수행해야 한다. 따라서 반드시 타입을 알아야만 하기 때문에 new 뒤에는 생성하고자 하는 객체의 타입이 들어가게 된다. 내부적으로는 타입의 크기를 알아내서 malloc을 호출하게 되어 있다. delete 또한 마찬가지로 뒤에 오는 포인터의 타입이 클래스일 경우 해당 클래스의 소멸자를 호출한 후에 free를 호출하여 메모리를 해제하게 된다. 컴파일러는 대상 포인터의 타입만을 체크하여 해당 타입과 일치하는 클래스의 소멸자를 호출한 뒤에 메모리 영역을 해제하는 어셈블리를 작성한다.

> Reference  
> {cite}`FundamentalC++`

### new [] & delete []
먼저 아래의 코드를 보자.

```cpp
class A
{
public:
	A(void) { std::cout << "constructor\n"; };
    ~A(void) { std::cout << "destructor\n"; };
};

int main(void)
{
    A* Aarr = new A[2];
    delete [] Aarr;
}
```

코드를 실행할 경우 생성자가 2번, 소멸자가 2번 호출된것을 확인할 수 있다. 당연하다고 생각할 수 있지만 이 부분은 굉장히 복잡한 과정을 거쳐서 이루어진다. 한번 생각해보자. 컴파일러는 어떻게 생성자와 소멸자를 2번씩 호출해야 된다는 것을 알 수 있을까? 

생성자를 두 번 호출하는 것은 사실 쉽다. 코드에 친절하게 []안에 몇번 호출해야 되는지 쓰여있기 때문이다. 하지만 문제는 소멸자를 두 번 호출하는 것이다. delete []에 전달되는 정보는 오직 배열 객체를 가리키는 포인터이다. 포인터는 말 그대로 메모리 덩어리를 가리키는 주소일 뿐이지 이것을 통해서 배열 요소의 개수를 알아낼 수는 없다. 즉, 소멸자를 2번 부르기 위해서 컴파일러는 마법을 부려야만 하는 것이다.

Heap manager는 메모리 할당 내역을 관리하고 있으며 malloc이 호출될 경우 할당 내역에 추가되고, free가 호출되면 할당 내역에서 삭제된다. 할당 내역을 관리하는 자료구조는 당연히 컴파일러마다 다를 것이지만 결국 할당 내역을 통해서 검색, 추가, 삭제하는 과정은 동일하다고 할 수 있다.

배열 타입의 new [], delete []도 내부적으로는 malloc과 free를 호출한다. 즉, heap manager의 할당 내역의 변화를 일으키는 것은 직접적으로 new와 delete가 아니라 내부에서 호출되는 malloc과 free다. 여기서 중요한 점은 단일 객체 타입의 new와 delete는 malloc과 free를 그대로 호출하는데, 배열 타입의 new [], delete []는 malloc과 free를 그대로 호출하지 않는다는 사실이다. 당연히 배열 타입이니까 다른 면이 있겠지만, 가장 큰 차이점이라고 한다면 할당 되고 해제되는 메모리의 구조 자체가 다르다는 점이다.

> Reference  
> {cite}`FundamentalC++`

#### 단일 타입 new의 처리 과정
기본 타입인 int에 대해서 new를 호출할 경우 내부적으로는 malloc이 호출된다. malloc은 heap manager에게 메모리 할당을 요청하며, heap manager는 관리하고 있는 힙에서 적절히 4바이트 만큼의 메모리 블록을 찾은 뒤에 해당 블록의 주소를 반환할 것이다. 동시에 heap manager는 해당 블록의 주소를 할당 내역에 추가한다. malloc을 통해서 할당 받은 메모리 블록의 주소를 new는 그대로 다시 반환한다.

> Reference  
> {cite}`FundamentalC++`

#### 소멸자가 존재하는 클래스 타입의 배열 객체를 new[]를 통해서 할당하는 과정
new []를 호출하면 내부에서 malloc을 호출하는 것은 같지만, malloc으로 넘기는 인자에 변화를 준다. new[]의 경우 필요한 메모리에 추가적인 메모리를 더해 malloc에 요청한다. heap manager는 요청 받은 만큼 힙에서 찾아내서 반환을 해줄 것이다. 그리고 추가적으로 할당한 메모리 부분에 배열의 요소 개수가 저장된다. 이것이 바로 배열 타입 객체의 소멸자를 배열 요소 개수만큼 호출하기 위한 마법의 근원이다.

> Reference  
> {cite}`FundamentalC++`

#### delete [] 호출 후 과정
delete []의 의미는 해당 객체가 배열 타입으로 할당되었음을 나타내는 것이다. delete []가 호출 되면 컴파일러는 ptr이 가르키는 객체가 배열 타입임으로 메모리 주소에서 추가적으로 할당된 부분을 찾아내고 그를 통해 요소의 개수를 알아낸다. 배열 요소의 개수를 알아내면 그만큼 반복하면서 소멸자를 호출하면 된다. 동시에 메모리를 해제하기 위하여 내부적으로 free를 호출할 때도 ptr가 가르키는 주소에서 추가적으로 할당된 부분을 뺀 값을 인자로 넘기게 되어있다. 당연히 힙 관리자는 할당 내역에서 할당 정보를 찾을 수 있고, 정확하게 해재할 수 있다.

delete []가 아니라 delete를 호출할 경우 컴파일러는 단순히 소멸자는 한번만 호출할 것이며 ptr 주소를 그대로 free에 인자로 넘기게 된다. 그러나 ptr에는 추가적으로 할당된 메모리 주소가 배제되어 있기 때문에 이 값으로 할당 내역 테이블에서 검색할 경우 주소를 찾을 수 없기 때문에 메모리는 제대로 해제되지 않게 된다. 이런 상황을 보통 힙 충돌이 발생했다고 하는데 힙 충돌은 즉시 오류를 발생시킬 수도 있지만, 보통 나중에 더 큰 오류의 원인으로 작용하기 때문에 디버깅을 상당히 어렵게 만든다.

> Reference  
> {cite}`FundamentalC++`

#### 소멸자가 정의되지 않는 타입에 대한 new []할당
할당된 메모리 블록의 포인터를 통해서 블록의 크기를 알기 위해 VC++의 경우 _msize 함수를 제공한다.

```cpp
size_t _msize(void* memblock);
```

MSDN에 의하면 힙에 할당된 메모리 블록의 크기를 바이트 단위로 반환한다고 설명되어 있다. 이 함수를 통해서 소멸자가 정의되지 않은 타입에 대한 new []할당에 특성을 알아보자.

```cpp
class A {};

class B
{
    ~B() = default;
};

class C
{
    ~C() {};
};

int main(void)
{
    A* Aarr = new A[3];
    B* Barr = new B[3];
    C* Carr = new C[3];

    std::cout << "size of Aarr : " << _msize(Aarr) << "\n";
    std::cout << "size of Barr : " << _msize(Barr) << "\n";
    std::cout << "size of Carr : " << _msize(Carr) << "\n"; // throw exception!
}
```

위의 코드에서 A와 B 클래스는 default 소멸자가 존재하고 C 클래스는 사용자 정의 소멸자가 존재한다. 이 때, 각각의 클래스에 대해 new []를 호출할 경우 Aarr와 Barr는 소멸자를 부를 필요가 없기 때문에 배열의 요소 개수를 저장할 추가 영역이 불필요하다. 하지만 Carr의 경우 소멸자가 존재하기 때문에 배열의 요소 개수를 저장할 추가영역이 필요하고 이로 인해 new []를 호출하여 반환된 주소(Carr의 값)가 실제 malloc을 호출하여 반환된 주소와는 다르다. 따라서 Carr 주소에 대한 할당 내역 테이블이 존재하지 않게 되고 _msize가 예외를 발생시킨다.

결론적으로 default 소멸자만 있을 경우 소멸자를 호출할 필요가 없기 때문에 배열의 요소 개수를 저장할 추가 영역 할당등의 작업이 불필요하게 된다. 따라서 단일 객체 할당을 위한 new와 크게 다를 것이 없다. 이로인해 delete와 delete []의 구분이 무의미해진다.

> Reference  
> {cite}`FundamentalC++`  
[윈도우 메모리 구조 - 보안 세상 블로그](https://securitynewsteam.tistory.com/99)    
[Window Heap 관리자](https://dnsdudrla97.github.io/window_allocator_/)  
[heap에 관한 화제 - kiaak 블로그](https://kiaak.tistory.com/entry/Windowsheap%EC%97%90-%EA%B4%80%ED%95%9C-%ED%99%94%EC%A0%9C)    
[Process Default & Private heap - 수까락 블로그](http://egloos.zum.com/sweeper/v/1791208)  
[프로세스의 힙 메모리 - 웅웅이의 블로그](https://jungwoong.tistory.com/47)    
[HeapAlloc과 HeapFree에 대하여 - 김성엽 블로그](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=tipsware&logNo=220964497321)  
[Managing heap memory - MSDN](https://docs.microsoft.com/en-us/previous-versions/ms810603(v=msdn.10)?redirectedfrom=MSDN)  
[윈도우 메모리 구조 - 웅웅이의 블로그](https://jungwoong.tistory.com/44?category=1067195)
 