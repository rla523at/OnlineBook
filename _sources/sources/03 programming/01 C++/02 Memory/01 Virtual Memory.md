# Momery Allocation
primary memory는 운영체제 영역과 사용자 영역으로 나뉘어져 있으며, 프로그램은 OS로부터 사용자 영역에 있는 memory를 할당받아 process가 된다.

## Contiguous Memory Allocation
프로그램에 메모리를 할당할 때, 하나의 커다란 공간에 연속적으로 할당하는 방식을 `연속 메모리 할당(Contiguous Memory Allocation)`이라고 한다.

초창기 컴퓨터에서는 사용자 영역을 한 프로그램에게 전부 할당하였다. 이를 `단일 분할 할당 기법`이라고 한다.

단일 분할 할당 기법은 하나의 프로그램 밖에 실행시킬 수 없다는 단점이 있다.

이를 개선하고자 해서 나온 기법이 바로 `다중 분할 할당 기법`이다. 다중 분할 할당 기법은 primary memory의 사용자 영역을 준비상태 큐에서 준비 중인 프로그램에게 적절하게 나누어 할당함으로써 여러 프로그램에 메모리가 할당 될 수 있도록 하는 방법이다. 이 때, primary memory의 사용자 영역을 나누는 방식에 따라 크게 고정 분할 할당 기법과 가변 분할 할당 기법으로 나뉜다.

`고정 분할 기법`은 primary memory를 고정된 크기의 파티션들로 분할한 뒤, 준비상태 큐에서 준비 중인 프로그램이 필요로 하는 메모리보다 같거나 더 큰 파티션을 할당하는 기법으로 `정적 할당(Static Allocation)` 기법이라고도 한다. 이 기법은 구현이 간단하고 운영체제에 오버헤드가 거의 없지만, 파티션 단위로 memory를 부여했기 때문에 프로그램에 부여된 memory가 실제 필요한 memory보다 더 커 memory가 낭비되는 `내부 단편화(internal fragmentation)`가 발생할 수 있으며 최대 활성 프로그램의 수가 고정된다는 단점이 있다.

```{figure} _image/0002.png
```

`동적 분할 기법`은 준비상태 큐에서 준비 중인 프로그램가 필요로 하는 메모리만큼 primary memory에서 파티션을 동적으로 생성하여 각 프로그램이 필요한만큼만 memory를 할당하는 기법이다. internal fragmentation이 없으며 primary memory들 보다 효율적으로 사용 가능하다. 하지만 primary memory에 필요한 memory 크기가 모두 제각각이므로 할당 해제되는 작업이 반복적으로 일어남에 따라 필연적으로 할당된 memory와 memory 사이에 사용하지 않는 작은 memory 공간이 생기는 `단편화(fragmentation)`가 발생하게 된다.

```{figure} _image/0003.png
```

이렇게 fragmentation이 발생하다 보면 결국 primary memory에 할당 가능한 memory의 크기가 프로그램이 필요로 하는 memory 크기보다 크지만 연속되어 있지 않아 memory 할당이 불가능한 `외부 단편화(external fragmentation)`가 발생할 수 있다. 이를 해결하기 위해 memory 집약이 요구되므로 처리나 효율이 좋지 않다는 단점이 있다.

```{figure} _image/0004.png
```

지금까지 살펴본 모든 경우에는 primary memory의 용량이 프로그램을 실행하는데 필요한 모든 메모리보다 항상 커야한다. 그렇지 않을 경우 메모리 부족 오류에 의해 해당 프로그램을 실행할 수 없게 된다. 따라서 primary memory의 용량보다 필요한 메모리가 더 큰 프로그램를 실행시키기 위해 `오버레이(overlay) 기법`이 나오게 되었다. overlay 기법은 프로그래머가 프로그램을 모듈로 나눈 후에 실행에 필요한 모듈만 primary memory에 올려 실행하는 방식이다. 

```{figure} _image/0001.png
```

예를 들어, primary memory는 100kb 크기이고 A 프로그램에서 필요한 메모리는 총 150kb이며 모듈1 크기가 70kb 모듈2 크기가 30kb 모듈3 크기가 50kb이고 모듈1,2,3 순서대로 A가 동작한다고 해보자. 

원래대로라면 모듈 1,2,3이 모두 올라올수 없어서 메모리 부족 오류가 발생해야 했다. 하지만 A 모듈 1,2,3 순서대로 동작함으로 먼저 모듈1,2를 올려서 실행한 다음에 모듈 1이 다 실행되면 그 자리에 모듈3를 `중첩(overlay)`하여 적재해서 모듈3을 실행하면 주어진 primary memory 100kb로 A 프로그램를 실행할 수 있다.

이로써 primary memory의 용량보다 memory가 더 많이 필요한 프로그램도 실행시킬 수 있다. 이처럼 "프로그램을 실행하는데 할당받아야 될 최소한의 메모리가 얼마인가?"에 집중해서 overlay 기법을 한층 더 발전시킨 기법을 `가상 메모리(virtual memory) 기법`이라고 한다.

> Reference  
> [sorjfkrh5078 - 주기억장치 할당 기법](https://sorjfkrh5078.tistory.com/49) 
> [devdebin - 물리메모리 관리](https://devdebin.tistory.com/m/35)  
> [iambeginnerdeveloper - 페이징과 세그멘테이션](https://iambeginnerdeveloper.tistory.com/158)
> [threefivesix - 메모리 관리 기법](https://threefivesix.tistory.com/23)  
> [letsmakemyselfprogrammer - 메모리관리](https://letsmakemyselfprogrammer.tistory.com/116)  
> [ahnanne - 가상 메모리](https://ahnanne.tistory.com/15)   
> [gateoverflow - what is difference between overlays and virtual memory?](https://gateoverflow.in/48306/what-difference-between-overlays-virtual-memory-transparent)  

## Vritual Memory
virtual memory는 "프로그램이 실행되기 위해서 꼭 필요한 모든 메모리를 할당 받을 필요는 없다."는 점을 활용한 메모리 관리 기법이다.

메모리 접근은 순차적이고 지역적이기 때문에 명령어 수행에 필요한 메모리는 프로그램에 필요한 모든 메모리 용량에 비해 훨씬 작다. 단독 기계 명령어를 실행하기 위해서는 다음 3가지의 메모리 접근이 필요하다.

* 메모리에서 명령어 읽기
* 메모리에서 명령어를 수행하는데 필요한 데이터 읽기
* 명령어 수행 후  결과를 메모리에 기록

매번 메모리에 접근하는데 필요한 실제 바이트 수는 CPU 아키텍쳐 및 실제 명령어와 데이터 유형에 따라 달라진다. 그러나 예를 들어 각 메모리 접근 유형마다 100 바이트의 메모리가 필요하다고 가정하면 위의 경우 300 바이트만 메모리에 올라와 있어도 명령어를 수행할 수 있다.

이런 순차적이고 지역적인 특성을 활용하면 전체 프로그램에 필요한 용량이 아닌 명령어 수행에 필요한 최소 메모리만 primary memory에 있고 나머지는 secondary memory에 위치해도 프로그램을 실행할 수 있다. 결국 빠르고 작은 primary memory를 크고 느린 secondary memory와 병합하여 하나의 크고 빠른 기억장치(가상 메모리)처럼 동작하게 한다.

이를 위해서 virtual memory에 저장되어 있는 프로그램을 여러 개의 작은 블록 단위로 나눈 뒤, 메모리 할당이 필요한 블록만 primary memory에 불연속적으로 할당하여 처리해야 한다. 

이 떄, 프로그램을 어떻게 나누느냐에 따라서 `페이징(Paging)` 기법과 `세그먼테이션(Segmentation)` 기법으로 나뉜다.

> Reference  
> {cite}`FundamentalC++`  
> [sorjfkrh5078 - 가상기억장치 할당 기법](https://sorjfkrh5078.tistory.com/50)
> [mimimimamimimo - 가상 메모리](https://mimimimamimimo.tistory.com/29)   
