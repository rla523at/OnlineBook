# Memory Allocation
프로그램은 OS로부터 primary memory를 할당받아 process가 된다.

이 때, primary memory를 할당하는 방식에 따라 `연속 메모리 할당(Contiguous Memory Allocation)`과 `불연속 메모리 할당(Noncontiguous Memory Allocation)` 방식이 존재한다.

> Reference  
> [sorjfkrh5078 - 주기억장치 할당 기법](https://sorjfkrh5078.tistory.com/49)  
> [kgr2626 - 메모리분할, 연속 메모리 관리 기법, 불연속 메모리 관리 기법](https://m.blog.naver.com/PostView.naver?blogId=kgr2626&logNo=222146006785&navType=by)   
> [letsmakemyselfprogrammer - 메모리관리](https://letsmakemyselfprogrammer.tistory.com/116)    

## Contiguous Memory Allocation
Contiguous memory allocation은 프로그램에 연속적인 primary memeory를 할당하는 방식이다.

이 떄, primary memory를 어떻게 분할하냐에 따라 `단일 분할 할당 기법`과 `다중 분할 할당 기법`으로 나뉜다.

단일 분할 할당 기법은 primary memory를 분할하지 않고 한 프로그램에게 전부 할당하는 방식으로 하나의 프로그램 밖에 실행시킬 수 없다는 단점이 있다.

다중 분할 할당 기법은 primary memory를 Job Queue에서 준비 중인 프로그램에게 적절하게 나누어 할당함으로써 여러 프로그램에 메모리가 할당 될 수 있도록 하는 방식입니다.

이 때, primary memory를 분할하는 방식에 따라 `정적 다중 분할 할당` 방식과 `동적 다중 분할 할당` 방식으로 나뉜다.

정적 분할 할당 방식은 primary memory를 고정된 크기의 블록들로 분할한 뒤, 프로그램이 필요로 하는 메모리보다 같거나 더 큰양의 블록들을 할당하는 방법이다. 이 방법은 구현이 간단하고 운영체제에 오버헤드가 거의 없다. 하지만 블록 단위로 memory를 부여했기 때문에 프로그램에 부여된 memory가 실제 필요한 memory보다 더 커 memory가 낭비되는 `내부 단편화(internal fragmentation)`가 발생할 수 있으며 최대 활성 프로그램의 수가 고정된다는 단점이 있다.

```{figure} _image/0101.png
```

동적 분할 할당 방식은 프로그램이 필요로 하는 메모리만큼 primary memory에서 블록을 동적으로 생성하여 각 프로그램이 필요한만큼만 memory를 할당하는 방법이다. 이 방법은 internal fragmentation이 없어 primary memory들 보다 효율적으로 사용 가능하다. 하지만 프로그램마다 필요한 memory 크기가 모두 제각각이므로 할당 해제되는 작업이 반복적되면 필연적으로 할당된 memory와 memory 사이에 사용하지 않는 작은 memory 공간이 생기는 `단편화(fragmentation)`가 발생하게 된다.

```{figure} _image/0102.png
```

이렇게 fragmentation이 발생하다 보면 결국 primary memory에 할당 가능한 빈 memory의 크기가 프로그램이 필요로 하는 memory 크기보다 크지만 연속되어 있지 않아 memory 할당이 불가능한 `외부 단편화(external fragmentation)`가 발생할 수 있다. 이를 해결하기 위해 memory 집약 과정을 수행하면 운영체제 오버헤드가 발생하는 단점이 있다.

```{figure} _image/0103.png
```

> Reference  
> [0715yk - 프로세스 스케줄링](https://velog.io/@0715yk/OS-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EC%8A%A4%EC%BC%80%EC%A4%84%EB%A7%81)  
> [sorjfkrh5078 - 주기억장치 할당 기법](https://sorjfkrh5078.tistory.com/49)  
> [iambeginnerdeveloper - 페이징과 세그멘테이션](https://iambeginnerdeveloper.tistory.com/158)  
> [threefivesix - 메모리 관리 기법](https://threefivesix.tistory.com/23)  

## Noncontiguous Memory Allocation
Noncotiguous memory allocation은 프로그램을 분할한 뒤 각 블록에 불연속적인 primary memory를 할당하는 방식입니다.

Noncotiguous memeory allocation은 program과 primary memory를 블락 단위로 분할하여 관리를 하는데 이 때, 분할하는 방식에 따라 `simple paging` 방법과 `simple segmentation` 방법으로 나뉜다.

simple paging 기법은 프로그램과 primary memory를 고정된 크기의 단위로 나누는 방식이다. 

이 때, 프로그램의 단위를 `페이지(page)`라고 하며, primary memory의 단위를 `프레임(frame)`이라고 한다.  그리고 프로그램에 primary memory를 할당하면 page마다 frame이 할당되며 이런 정보를 기록해둔 테이블을 `페이지 테이블(page table)`이라고 하며 기록된 정보를 `page table entry (PTE)`라고 한다.

simple paging에서는 고정된 크기의 단위로 나누기 때문에 external fragmentation 문제가 발생하지 않는다. 하지만 internal fragmentation 문제가 발생할 수 있다. 

simple segmentation 기법은 프로그램과 primary memory를 논리 단위로 나누는 방식이다. 이 때, 프로그램의 단위를 `세그먼트(segment)`라고 하며 segment별로 크기가 얼마인지 그리고 primary memory에 어디에 할당되었는지 정보를 저장하는 table을 `세그먼트 테이블(segment table)`이라고 한다.

segment table에는 segment 크기와 primary memory상의 시작주소가 저장되어 있다. segment 크기가 함께 저장되는 이유는 segment별로 크기가 다르기 때문에 유효한 주소들에만 접근하기 위해서는 segment 크기가 필요하다.

simple segmentation에서는 논리 단위로 나누기 때문에 internal fragmentation이 발생하지 않지만 external fragmentation 문제가 발생할 수 있다.

> Reference  
> [kgr2626 - 논리주소, 절대주소, 상대주소](https://m.blog.naver.com/kgr2626/222146089037)   
> [kgr2626 - 세그먼트, 세그먼테이션, 세그먼트 테이블](https://m.blog.naver.com/PostView.naver?blogId=kgr2626&logNo=222146539396&targetKeyword=&targetRecommendationCode=1)  

## Overlay 기법
지금까지 살펴본 모든 memory allocation 방법은 프로그램이 필요로 하는 모든 메모리를 primary memory에 할당하는 방식이기 때문에 primary memory보다 큰 프로그램은 메모리 부족으로 실행할 수 없다.  

이런 primary memory 크기의 한계를 극복하기 위해 `오버레이(overlay) 기법`이 나오게 되었다. 

overlay 기법은 프로그래머가 프로그램을 모듈로 나눈 후에 실행에 필요한 모듈만 primary memory에 올려 실행하는 방식이다. 

```{figure} _image/0104.png
```

예를 들어, primary memory는 100kb 크기이고 A 프로그램에서 필요한 메모리는 총 150kb이며 모듈1 크기가 70kb 모듈2 크기가 30kb 모듈3 크기가 50kb이고 모듈1,2,3 순서대로 A가 동작한다고 해보자. 

원래대로라면 모듈 1,2,3이 모두 올라올수 없어서 메모리 부족 오류가 발생해야 했다. 하지만 A 모듈 1,2,3 순서대로 동작함으로 먼저 모듈1,2를 올려서 실행한 다음에 모듈 1이 다 실행되면 그 자리에 모듈3를 `중첩(overlay)`하여 적재해서 모듈3을 실행하면 주어진 primary memory 100kb로 A 프로그램를 실행할 수 있다.

이로써 primary memory의 용량보다 memory가 더 많이 필요한 프로그램도 실행시킬 수 있다.

> Reference   
> [gateoverflow - what is difference between overlays and virtual memory?](https://gateoverflow.in/48306/what-difference-between-overlays-virtual-memory-transparent)  
