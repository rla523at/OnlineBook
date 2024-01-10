# Virtual Memeory
`가상메모리(virtual memory) 기법`은 "프로그램이 실행되기 위해서 프로그램에 필요한 모든 메모리를 한번에 할당 받을 필요는 없다."는 점을 활용해 secondary memory를 primary memory의 일부처럼 사용하여 작고 빠른 primary memory와 크고 느린 secondary memory를 병합함으로써 primary memory보다 크고 secondary memory보다 빠른 가상의 memory를 갖고 있는 것과 같은 환상을 제공한다.

## 동작원리
메모리 접근은 순차적이고 지역적이기 때문에 명령어 수행에 필요한 메모리는 프로그램에 필요한 모든 메모리 용량에 비해 훨씬 작다. 단독 기계 명령어를 실행하기 위해서는 다음 3가지의 메모리 접근이 필요하다.
* 메모리에서 명령어 읽기
* 메모리에서 명령어를 수행하는데 필요한 데이터 읽기
* 명령어 수행 후 결과를 메모리에 기록

매번 메모리에 접근하는데 필요한 실제 바이트 수는 CPU 아키텍쳐 및 실제 명령어와 데이터 유형에 따라 달라진다. 그러나 예를 들어 각 메모리 접근 유형마다 100 바이트의 메모리가 필요하다고 가정하면 위의 경우 300 바이트만 메모리에 올라와 있어도 명령어를 수행할 수 있다.

이런 순차적이고 지역적인 특성을 활용하면 전체 프로그램에 필요한 메모리가 아닌 명령어 수행에 필요한 최소 메모리만 primary memory에 할당 되고 나머지는 secondary memory에 할당 되도 프로그램을 실행할 수 있다. 

## Memory Mapping
virtual memory 기법에서는 필요한 virtual memory만 primary memory에 할당해야 하기 때문에 virtual memory를 여러 개의 작은 블록 단위로 나누고 메모리 할당이 필요한 블록만 primary memory에 할당하고 나머지는 secondary memory의 `paging file` 혹은 `swap file`이라 불리는 파일에 할당한다.

이 때, 실제로 data에 접근하기 위해서는 virtual memory의 주소를 primary memory의 주소로 바꾸는 `memory mapping` 작업이 필요하다. 이 과정에서 virtual memory가 secondary memory를 할당받은 경우라면 primary memory를 새롭게 할당받은 뒤 primary memory로 바꿔준다. memory mapping은 CPU와 메모리 중간에 `memory management unit (MMU)`라는 하드웨어 반도체 칩을 통해 이루어진다. MMU는 translation 속도를 높이기 위해  최근에 mapping된 정보들을 `translation lookaside buffer (TLB)`라는 연관 캐쉬에 저장하고 secondary memory에 저장되어 있어 새롭게 primary memory를 할당받는 경우를 최소화 하기 위한 기법이 적용되는 등 다양한 최적화가 되어있다. 이렇게 translation과 관련된 overhead를 줄임으로써 virtual memory가 physical memory와 같이 빠른 memory처럼 동작하게 한다.

그리고 primary memory에 불연속적으로 할당되어 있다고 하더라도 virtual memory 입장에서는 연속적으로 존재할 수 있다. 따라서 컴퓨터의 primary memory에 연속적으로 할당되어 존재할 필요가 없음으로 `불연속 메모리 할당(Noncontiguous Memory Allocation)`이 가능해진다. 이로써 memory 집약 과정 없이 연속적이지 않은 primary memory의 공간을 활용할 수 있기 때문에 효율적으로 external fragmentation 문제를 해결할 수 있다. 대신 프로그램 실행에 필요한 메모리가 블록 단위에 딱 맞지 않아 필요한 양보다 더 큰 메모리를 할당받아 메모리 공간이 낭비되는 internal fragmentation 문제가 발생할 수 있다. 이를 해결하기 위해 블록 단위를 작게하면 mapping 과정이 많아지므로 오히려 효율이 떨어질 수 있다.

---

현재 명렁어를 수행하는데 필요한 virtual memory만 primary memory에 할당하고 나머지는 secondary memeory의 `paging file` 혹은 `swap file`이라 불리는 파일에 할당하여 primary memory보다 큰 가상의 주소 공간을 갖고 있는 것과 같은 환상을 제공한다.

이 떄, 필요한 virtual memory만 primary memory에 할당해야 하기 때문에 virtual memory를 여러 개의 작은 블록 단위로 나누고 메모리 할당이 필요한 블록만 primary memory에 할당하고 나머지는 secondary memory에 할당한다. 

virtual memory가 primary memory를 할당받았을 때, 실제로 data에 접근하기 위해서는 virtual memory의 주소를 primary memory의 주소로 바꾸는 작업이 필요하다. 이를 `memory mapping`이라고 한다. memory mapping은 CPU와 메모리 중간에 `memory management unit (MMU)`라는 하드웨어 반도체 칩을 통해 이루어진다. MMU는 translation 속도를 높이기 위해  최근에 mapping된 정보들을 `translation lookaside buffer (TLB)`라는 연관 캐쉬에 저장하는등 다양한 최적화가 되어있다. 이렇게 translation과 관련된 overhead를 줄임으로써 virtual memory가 physical memory와 같이 빠른 memory처럼 동작하게 한다.

그리고 primary memory에 불연속적으로 할당되어 있다고 하더라도 virtual memory 입장에서는 연속적으로 존재할 수 있다. 따라서 컴퓨터의 primary memory에 연속적으로 할당되어 존재할 필요가 없음으로 `불연속 메모리 할당(Noncontiguous Memory Allocation)`이 가능해진다. 이로써 memory 집약 과정 없이 연속적이지 않은 primary memory의 공간을 활용할 수 있기 때문에 효율적으로 external fragmentation 문제를 해결할 수 있다. 대신 프로그램 실행에 필요한 메모리가 블록 단위에 딱 맞지 않아 필요한 양보다 더 큰 메모리를 할당받아 메모리 공간이 낭비되는 internal fragmentation 문제가 발생할 수 있다. 이를 해결하기 위해 블록 단위를 작게하면 mapping 과정이 많아지므로 오히려 효율이 떨어질 수 있다.

> Reference  
> {cite}`FundamentalC++`  
> [sorjfkrh5078 - 가상기억장치 할당 기법](https://sorjfkrh5078.tistory.com/50)  
> [mimimimamimimo - 가상 메모리](https://mimimimamimimo.tistory.com/29)   
> [bb-library - Paging](https://bb-library.tistory.com/65)   

## Paging System
virtual memory에 할당된 프로그램은 여러 개의 작은 단위로 나뉘게 되는데, 나누는 방법 중 하나인 `페이징(Paging)` 기법에 대해서 알아보자.

paging system에서는 virtual memory를 `페이지(page)` 단위로 나누고 각각의 페이지에는 `페이지 번호(virtual page number; VPN)`를 부여한다. 그리고 primary memory 또한 페이지 크기와 같은 크기로 나누고 그 단위를 `프레임(frame)`이라고 하며 각각의 프레임에도 `프레임 번호(physical frame number; PFN)`를 부여한다. 그리고 페이지와 프레임을 대응시키기 위해 `페이지 테이블(page table)`을 생성한다. 

```{figure} _image/0201.png
``` 

> Reference  
> [sorjfkrh5078 - 가상기억장치 할당 기법](https://sorjfkrh5078.tistory.com/50)  
> [ddongwon - 페이징(Paging)과 페이지 테이블(Page table)](https://ddongwon.tistory.com/49)  


### Page table entry
page table에는 페이지 번호가 몇번 프레임과 연결되어 있는지가 기록되어 있으며 개별 데이터는 `page table entry (PTE)`라고 한다. 32bit 기준으로 PTE는 대략 다음의 구조로 이루어져 있다.

```{figure} _image/0202.png
```

### Memory mapping process
Virtual Address를 Page Size로 나누어 몫 VPN과 나머지 offset을 구해낸다.

다음으로 VPN으로부터 PFN을 찾아내기 위해 TLB를 검색한다. `TLB 검색에 성공(TLB hit)`하면 PFN을 바로 찾아낸다. 하지만 `TLB에서 검색에 실패(TLB miss)`할 경우 `page table을 검색(page walk)`하여 PFN을 찾아낸다. 

이 때, page walk로 PFN을 찾기 못하게 되는 경우가 있는데 이는 두 가지 이유에서 비롯된다. 

첫번째는 virtual memory 주소 자체가 유효하지 않은 경우이다. 이 경우, 운영체제는 해당 가상 주소 접근을 요청한 프로세스에게 segmentation fault를 발생시키고 memory mapping 과정이 종료된다. 

두번째는 해당 페이지 primary memory에 존재하지 않는 `페이지 부재(page fault)`이다. page falut가 발생하면 secondary memory에 저장되어 있는 paging file에서 필요한 데이터를 찾아내어 새롭게 primary memory에 할당하여 PFN을 알아내고 page table과 TLB의 내용을 수정한다.

위 과정으로 찾아낸 PFN을 Frame Size크기만큼 곱한 값이 Physical Memory의 시작주소가 되며, offset 값을 더해주면 Physical Memory를 구할 수 있다.

요약하면 다음과 같다.
* Virtual Address  = VPN * Page Size + offset
* Physical Address = PFN * Frame Size + offset

```{figure} _image/0203.png
```

> Reference  
> {cite}`FundamentalC++`  
> [khj94811 - Paging - 1](https://m.blog.naver.com/khj94811/221019655505)  

#### 참고
page fault가 발생하고 primary memory에 빈 공간이 있을경우 paging file에 존재하는 데이터를 빈 공간에 할당한 뒤, 빈공간의 프레임 번호를 가지고 page table과 TLB를 업데이트 한다. 그러나 primary memory에 빈 공간이 없을경우 `페이지 교체(page replacement)`를 실시한다. 가장 먼저 `page replacement algorithm`으로 page-out 시킬 페이지를 먼저 결정한다. page out된 페이지의 PTE를 수정하고 이 페이지와 연결된 프레임에 저장된 데이터는 paging file에 저장한다. 이 후 원래 paging file에 존재했던 데이터를 이 프레임에 저장한다. 그리고 page table과 TLB를 업데이트 한다.

적절한 page replacement algorithm을 사용해서 최대한 page fault를 적게 발생하게 함으로써, secondary memory에서 primary memory로 data를 가져오는 횟수를 줄인다. 이로써 virtual memory는 secondary memory보다 훨씬 빠른 memory처럼 동작할 수 있게 된다.

#### Page Table vs TLB
page table은 memory에 있다. 접근하는데 오래걸림

TLB는 캐쉬이다. 접근이 매우 빠르다.

* https://about-myeong.tistory.com/35
* https://dkswnkk.tistory.com/443
  * 메인 메모리에 페이지 테이블을 저장하면 문맥 교환 속도가 빨라지지만 페이지 테이블을 확인하기 위해 메모리 액세스를 함으로써 시간이 더 느려질 수도 있습니다.
  * 이전 게시글에서 페이징 기법에 대해 알아보았습니다. 메인 메모리에 페이지 테이블을 저장하면 문맥 교환 속도가 빨라지지만 페이지 테이블을 확인하기 위해 메모리 액세스를 함으로써 시간이 더 느려질 수도 있습니다. 메모리 i에 액세스 하려고 한다고 가정해봅시다. 먼저 페이지 번호를 기준으로 페이지 테이블 기준 레지스터(PTBR) 오프셋의 값을 사용하여 페이지 테이블의 항목을 찾습니다. 이 작업에는 한 번의 메모리 액세스가 필요합니다. 이렇게 얻은 프레임 번호와 페이지 오프셋을 결합하여 실제 주소를 생성합니다. 그런 다음 메모리에서 원하는 위치에 액세스 할 수 있습니다. 이 기법을 사용하면 데이터에 액세스 하려면 두 번의 메모리 액세스가 필요합니다(한 번은 페이지 테이블 항목과 한 번은 실제 데이터). 따라서 메모리 접근 시간은 2배로 느려지고 이는 대부분의 상황에서 허용할 수 없는 지연시간입니다.
  * 이번 게시글에서는 이러한 문제를 해결하기 위한  TLB(translation look-aside buffers)라고 불리는 특수한 소형 하드웨어 캐시에 대해 알아보겠습니다

## Virtual Memory Size
x86은 32비트 시스템이다. 주소를 가리킬 수 있는 레지스터의 크기가 32비트라는 의미이다. 즉, 이론적으로는 $2^{32}$ 대략 42억 개의 주소가 나오게 된다. 각각의 주소가 1바이트씩 가리킬 수 있으므로 각각의 프로세스는 4GB의 가상 메모리를 부여 받게 된다. 

마찬가지로 x64는 64비트 시스템이며 레지스터의 크기가 64비트이므로 이론적으로 $2^{64}$ 개의 주소를 사용할 수 있기 때문에 대략 16EB$(2^4 \times 2^{60})$byte의 가상 메모리 공간이 부여될 수 있다. 사실상 무한한 메모리 공간이라고 할 수 있는데, 아직까지 이렇게 큰 메모리 공간이 필요한 프로그램이 존재하지 않기 때문에 운영체제별로 다르긴 하지만 윈도우의 경우 프로세스마다 대략 8TB의 공간만을 부여하도록 제한하고 있다. 

> Reference  
> {cite}`FundamentalC++`   
