# Virtual Memeory
virtual memory 기법은 프로그램을 secondary memory의 swap 영역에 저장하고 프로그램 실행에 필요한 최소한의 메모리만 primary memory에 불연속적으로 할당하는 방식이다. 

따라서, virtual memory를 사용하면 secondary memory를 primary memory의 일부처럼 사용하기 때문에 primary memory 크기의 한계를 극복할 수 있다. 그리고 virtual memory을 사용하면 각각의 프로그램마다 virtual memory 공간이 독립적으로 부여되며 부여된 메모리 영역은 해당 프로세스만이 접근할 수 있도록 보호된다. 따라서 타 프로세스가 해당 메모리 영역을 침벌할 수 없음으로 virtual memory는 프로그램의 안정성을 크게 향상시킬 수 있다. 단, 프로세스에서 memory에 접근할 때는 virtual memory 주소를 primary memory의 주소로 바꾸는 `memory mapping` 과정이 필요하게 된다.

memory mapping은 CPU와 primary memory 중간에 `memory management unit (MMU)`라는 하드웨어 반도체 칩을 통해 이루어진다. MMU는 mapping 속도를 높이기 위해 최근에 mapping된 정보들을 `translation lookaside buffer (TLB)`라는 연관 캐쉬에 저장하고 secondary memory에 저장된 data를 primary memory에 할당하는 경우를 최소화 하는 알고리즘이 적용되는 등 다양한 최적화가 되어있다. MMU는 이렇게 mapping과 관련된 overhead를 줄임으로써 virtual memory가 primary memory와 같이 빠르게 동작할 수 있게 한다.

그리고 virtual memory 기법에서는 virtual memory와 primary memory를 블록들로 나누어서 관리하는데 이 때, 어떻게 블록을 나누는 지에 따라 `Paging virtual memory`와 `Segmentation virtual memory`으로 나뉜다.

> Reference  
> [greencloud33 - Swap Memory 개념 및 swap partition, file 생성 방법](https://greencloud33.tistory.com/47)   

## Paging Virtual Memory
paging virtual memory란 virtual memory와 primary memory를 고정된 크기의 블록으로 나누는 방식이다.

고정된 크기로 나뉘어진 virtual memory의 단위를 `페이지(page)`라고 하며, primary memory의 단위를 `프레임(frame)`이라고 한다. 그리고 각각의 페이지에는 `페이지 번호(virtual page number; VPN)`를 각각의 프레임에는 `프레임 번호(physical frame number; PFN)`를 부여한다. 그리고 virtual memory가 primary memory를 할당받는 경우 VPN과 그에 해당하는 PFN이 존재하게 되는데 이런 정보를 기록해둔 테이블을 `페이지 테이블(page table)`이라고 하며 기록된 정보를 `page table entry (PTE)`라고 한다.

```{figure} _image/0201.png
``` 

```{figure} _image/0202.png
```

paging virtual memory에서는 고정된 크기의 단위로 나누기 때문에 external fragmentation 문제가 발생하지 않는다. 하지만 internal fragmentation 문제가 발생할 수 있다. 이를 개선하기 위해 page단위를 작게할 수는 있지만 이럴 경우 mapping overhead가 증가하는 문제가 생길 수 있다.



> Reference  
> [ddongwon - 페이징(Paging)과 페이지 테이블(Page table)](https://ddongwon.tistory.com/49)  

### Memory mapping Process
paging system에서 memory mapping이 어떻게 이루어지는지 자세히 살펴보자.

먼저, Virtual Address를 Page Size로 나누어 몫 VPN과 나머지 offset을 구해낸다.

다음으로 VPN으로부터 PFN을 찾아내기 위해 TLB를 검색한다. `TLB 검색에 성공(TLB hit)`하면 PFN을 바로 찾아낸다. 하지만 `TLB에서 검색에 실패(TLB miss)`할 경우 `page table을 검색(page walk)`한다.

`page table에서 검색에 성공(Page Hit)`하면 PFN을 찾을 수 있다. 하지만 page table에서 검색을 실패할 수 있는데 이는 두 가지 이유에서 비롯된다. 

첫번째는 virtual memory 주소 자체가 유효하지 않은 경우이다. 이 경우, 운영체제는 해당 가상 주소 접근을 요청한 프로세스에게 segmentation fault를 발생시키고 memory mapping 과정이 종료된다. 

두번째는 해당 page가 frame을 할당 받지 않아 page table에 없는 `페이지 부재(page fault)`이다. page falut가 발생하면 secondary memory의 swap 영역에서 저장된 데이터를 찾아낸 다음 primary memory에 할당하고 page table과 TLB의 내용을 수정한다. 

만약 primary memory에 빈 공간이 없어 primary memory에 할당할 수 없는 경우  `page replacement algorithm`으로 page-out 시킬 page를 결정한 뒤 page out된 page의 PTE를 수정하고 연결된 frame에 저장된 데이터는 swap 영역에 저장하고, 찾은 데이터를 이 frame에 저장한다.

이 때, 중요한점은 적절한 page replacement algorithm을 사용함으로써 앞으로 최대한 page fault를 적게 발생하게 하는 것이다. 이로써 secondary memory에 접근하는 횟수를 줄여 virtual memory가 secondary memory보다 훨씬 빠른 memory처럼 동작할 수 있게 한다.

위 과정으로 찾아낸 PFN을 Frame Size크기만큼 곱한 값이 primary memory의 시작주소가 되며, offset 값을 더해주면 primary memory를 구할 수 있다.

요약하면 다음과 같다.
* Virtual Memory Address  = VPN * Page Size + offset
* Primary Memory Address = PFN * Frame Size + offset

```{figure} _image/0203.png
```

> Reference  
> [dkswnkk - TLB](https://dkswnkk.tistory.com/456)  
> [khj94811 - Paging - 1](https://m.blog.naver.com/khj94811/221019655505)  

### Page Table 매핑 방식
#### 직접 매핑
Page Table이 primary memory에 있는 경우 이를 직접 매핑방식이라고 한다. 

이 경우 page table이 primary memory에 있기 때문에 다른 매핑 방식에 비해 문맥 교환속도가 빨라지지만 page table의 크기가 결코 작지 않기 때문에 메모리 부족을 야기할 수 있다.

또한, 직접 매핑방식이여도 TLB는 유의미한 역할을 한다. 왜냐하면 비록 page table이 primary memory에 있지만 cache에 접근하는 속도가 primary memory에 접근하는 속도보다 빠르기 때문이다.

> Reference  
> [dkswnkk - TLB](https://dkswnkk.tistory.com/456)  
> [threefivesix - 3) 페이지 테이블 매핑 방식](https://threefivesix.tistory.com/24)  

#### 연관 매핑
Page Table이 secondary memory에 있는 경우 이를 연관 매핑방식이라고 한다.

이 경우 page table이 secondary memory에 있기 때문에 page table에 의한 메모리 부족 문제를 해결할 수 있지만 TLB miss가 빈번히 발생하면 memory mapping overhead가 크게 증가할 수 있는 문제가 있다.

> Reference  
> [threefivesix - 3) 페이지 테이블 매핑 방식](https://threefivesix.tistory.com/24) 

## 명령어 수행에 필요한 최소한의 메모리
virtual memory 기법의 동작원리는 virtual memory 중 명령어 수행에 필요한 메모리만 primary memory에 할당하는 것이다.

단독 기계 명령어를 실행하기 위해서는 다음 3가지의 메모리 접근이 필요하다.
* 메모리에서 명령어 읽기
* 메모리에서 명령어를 수행하는데 필요한 데이터 읽기
* 명령어 수행 후 결과를 메모리에 기록하기

즉, 이를 위해 필요한 메모리만 primary memory에 할당되고 나머지는 secondary memory에 할당 되도 명령어 수행이 가능함으로 프로그램을 실행할 수 있다. 

> Reference  
> {cite}`FundamentalC++`

## Private Bytes(= Commit Size)와 Working Set
### 1. Private Bytes / Commit Size
* **정의**: 프로세스가 운영체제로부터 “내가 이만큼은 꼭 쓸 거니까 보장해줘” 하고 **커밋(Commit)** 한 가상 메모리 양.
* 이 값은 **물리 메모리 + 스왑 파일(pagefile.sys)** 을 합친 공간에서 운영체제가 보장한다.
* 즉, 실제로 RAM에 올라가 있지 않아도 프로세스는 이만큼을 언제든 사용할 수 있다고 믿고 동작할 수 있다.
* 흔히 **프로세스의 전체 메모리 footprint**를 표현할 때 사용한다.
* Task Manager의 **Commit Size** 와 Performance Counter의 **Private Bytes** 가 사실상 같은 의미다.

### 2. Working Set
* **정의**: 현재 시점에 **실제로 RAM(물리 메모리)에 올라와 있는 페이지들의 크기 합**.
* 즉, 지금 프로세스가 접근 가능한 “따끈따끈한” 메모리.
* 자주 쓰이는 페이지는 계속 Working Set 안에 남아 있고, 오래 안 쓰면 OS가 페이지를 다른 프로세스나 캐시에 주기 위해 디스크로 스왑 내릴 수 있다.
* Working Set은 시간에 따라 커졌다가 줄어들 수 있다(동적으로 변함).

### 3. 차이를 쉽게 비유하면
* **Private Bytes / Commit Size** = "내가 쓰겠다고 예약해둔 방의 전체 크기"
* **Working Set** = "그 방에서 지금 불 켜놓고 실제로 쓰고 있는 부분"

예를 들어,

* 프로세스가 200MB를 Commit 했다면 Private Bytes/Commit Size는 200MB이다.
* 하지만 당장 CPU가 접근하려고 RAM에 올라와 있는 것은 50MB뿐이라면 Working Set은 50MB이다.
* 나머지 150MB는 예약만 되어 있거나, 스왑으로 내려갔을 수 있다.
