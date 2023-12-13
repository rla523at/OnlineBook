# Paging system
paging system에서는 virtual memory를 페이지 단위로 나누고 각각의 페이지에는 `페이지 번호(virtual page number; VPN)`를 부여한다. 그리고 primary memory 또한 페이지 크기와 같은 크기로 나누고 그 단위를 `프레임(frame)`이라고 하며 각각의 프레임에도 `프레임 번호(physical frame number; PFN)`를 부여한다. 그리고 페이지와 프레임을 대응시키기 위해 `페이지 테이블(page table)`을 생성한다. 

```{figure} _image/0201.png
```

페이징 기법을 통해 연속적으로 존재하지 않는 물리적 메모리라도 페이징 기법을 통해 연속적으로 존재하는 것처럼 이용할 수 있다. 따라서 컴퓨터의 물리적 메모리는 연속적으로 할당되어 존재할 필요가 없음으로 `불연속 메모리 할당(Noncontiguous Memory Allocation)`이 가능해진다.

덕분에 memory 집약 과정 없이 연속적이지 않은 primary memory의 공간을 활용할 수 있기 때문에 효율적으로 external fragmentation 문제를 해결할 수 있다. 대신 프로그램 실행에 필요한 메모리가 페이지 단위에 딱 맞지 않아 필요한 양보다 더 큰 메모리를 할당받아 메모리 공간이 낭비되는 internal fragmentation 문제가 발생할 수 있다. 물론 페이지 단위를 작게하면 internal fragmentation 문제도 어느정도 해결할 수 있겠지만 대신 page mapping 과정이 많아지므로 오히려 효율이 떨어질 수 있다. 

## Page Table
page table에는 페이지 번호가 몇번 프레임과 연결되어 있는지가 기록되어 있으며 개별 데이터는 `page table entry (PTE)`라고 한다. 32bit 기준으로 PTE는 대략 다음의 구조로 이루어져 있다.

```{figure} _image/0202.png
```

page table은 프로세스마다 하나씩 존재하게 되며 primary memory에 상주하게 된다. 즉, 많은 프로세스가 구동될 수록 page table로 인한 primary memory 사용이 커진다.

프로세스가 특정 메모리에 접근하려 하면 page table을 참고하여 프로세스의 가상 주소를 시스템의 물리 메모리 주소로 변환해 주어야 한다. 이를 위해 CPU와 메모리 중간에 `memory management unit (MMU)`라는 하드웨어 반도체 칩이 있다. MMU는 최근에 맵핑된 page table의 정보들을 `translation lookaside buffer (TLB)`라는 연관 캐쉬에 저장한다. TLB는 자주 쓰는 page table의 주소를 저장해 둠으로써, translation의 속도를 향상시켜 주는 역할을 한다.

### 가상 메모리 주소의 변환 과정
첫번째로 TLB를 검색한다. `TLB 검색에 성공(TLB hit)`하면 primary memory 주소를 반환하고 과정을 끝낸다. 그러나 `TLB에서 검색에 실패(TLB miss)`할 경우 `page table을 검색(page walk)`하여 primary memory 주소를 찾아내고 TLB를 업데이트한 뒤 primary memory 주소를 반환한다. 

page table을 검색할 때 적절한 primary memory 주소를 찾기 못하게 되는 경우가 있는데 이는 두 가지 이유에서 비롯된다. 첫번째는 virtual memory 주소 자체가 유효하지 않은 경우이다. 이 경우, 운영체제는 해당 가상 주소 접근을 요청한 프로세스에게 segmentation fault를 발생시키게 된다. 두번째는 해당 페이지 primary memory에 존재하지 않는 `페이지 부재(page fault)`이다. 

페이지 부재가 발생하면 보조 기억장치에 저장되어 있는 `paging file` 혹은 `swap file`이라 불리는 파일을 primary memory에 저장하고 이 정보를 바탕으로 page table과 TLB의 내용을 수정한다.

만약 primary memory에 빈 공간이 있을경우 paging file에 존재하는 데이터를 빈 공간에 저장한 뒤, 빈공간의 프레임 번호를 가지고 page table과 TLB를 업데이트 한다.

만약 primary memory에 빈 공간이 없을경우 `페이지 교체(page replacement)`를 실시한다. 가장 먼저 `page replacement algorithm`으로 page-out 시킬 페이지를 먼저 결정한다. page out된 페이지의 PTE를 수정하고 이 페이지와 연결된 프레임에 저장된 데이터는 paging file에 저장한다. 이 후 원래 paging file에 존재했던 데이터를 이 프레임에 저장한다. 그리고 page table과 TLB를 업데이트 한다.

지금까지의 과정을 TLB와 page table 관점에서 정리하면, 아래 그림과 같다.

```{figure} _image/0203.png
```

### 가상 메모리 주소의 변환
Virtual Address를 Page Size로 나누어 몫은 VPN, 나머지는 offset이 된다. 다음으로, page table이나 TLB를 통해 PFN을 찾아낸다. 찾아낸 PFN을 Frame Size크기만큼 곱한 값이 Physical Memory의 시작주소가 되며, offset 값을 더해주면 Physical Memory를 구할 수 있다.

요약하면 다음과 같다.
* Virtual Address  = VPN * Page Size + offset
* PFN = page_talbe(VPN)
* Physical Address = PFN * Frame Size + offset

```{figure} _image/0204.png
```

> Reference  
> {cite}`FundamentalC++`  
> [sorjfkrh5078 - 가상기억장치 할당 기법](https://sorjfkrh5078.tistory.com/50)  
> [ddongwon - 페이징(Paging)과 페이지 테이블(Page table)](https://ddongwon.tistory.com/49)  
> [bb-library - Paging](https://bb-library.tistory.com/65)  
> [khj94811 - Paging - 1](https://m.blog.naver.com/khj94811/221019655505)