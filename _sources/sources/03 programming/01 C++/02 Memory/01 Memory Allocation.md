# Memory Allocation
프로그램은 OS로부터 primary memory를 할당받아 process가 된다.

## Contiguous Memory Allocation
프로그램에 메모리를 할당할 때, primary memeory에 연속적으로 할당하는 방식을 `연속 메모리 할당(Contiguous Memory Allocation)`이라고 한다.

Contiguous memory allocation 방법으로는 `단일 분할 할당 기법`과 `다중 분할 할당 기법`이 있다.

단일 분할 할당 기법은 primary memory를 한 프로그램에게 전부 할당하는 방식으로 하나의 프로그램 밖에 실행시킬 수 없다는 단점이 있다.

다중 분할 할당 기법은 이를 개선하고자 해서 나온 기법으로 primary memory를 준비상태 큐에서 준비 중인 프로그램에게 적절하게 나누어 할당함으로써 여러 프로그램에 메모리가 할당 될 수 있도록 하는 방법이다. 

다중 분할 할당 기법은 primary memory를 나누는 방식에 따라 크게 `정적 할당(Static Allocation)`과 `동적 할당(Dynamic Allocation)`으로 나뉜다.

static allocation은 primary memory를 고정된 크기의 파티션들로 분할한 뒤, 준비상태 큐에서 준비 중인 프로그램이 필요로 하는 메모리보다 같거나 더 큰 파티션을 할당하는 기법이다. 이 기법은 구현이 간단하고 운영체제에 오버헤드가 거의 없지만, 파티션 단위로 memory를 부여했기 때문에 프로그램에 부여된 memory가 실제 필요한 memory보다 더 커 memory가 낭비되는 `내부 단편화(internal fragmentation)`가 발생할 수 있으며 최대 활성 프로그램의 수가 고정된다는 단점이 있다.

```{figure} _image/0101.png
```

dynamic allocation은 준비상태 큐에서 준비 중인 프로그램이 필요로 하는 메모리만큼 primary memory에서 파티션을 동적으로 생성하여 각 프로그램이 필요한만큼만 memory를 할당하는 기법이다. internal fragmentation이 없으며 primary memory들 보다 효율적으로 사용 가능하다. 하지만 primary memory에 필요한 memory 크기가 모두 제각각이므로 할당 해제되는 작업이 반복적으로 일어남에 따라 필연적으로 할당된 memory와 memory 사이에 사용하지 않는 작은 memory 공간이 생기는 `단편화(fragmentation)`가 발생하게 된다.

```{figure} _image/0102.png
```

이렇게 fragmentation이 발생하다 보면 결국 primary memory에 할당 가능한 memory의 크기가 프로그램이 필요로 하는 memory 크기보다 크지만 연속되어 있지 않아 memory 할당이 불가능한 `외부 단편화(external fragmentation)`가 발생할 수 있다. 이를 해결하기 위해 memory 집약 과정이 요구되어 운영체제 오버헤드가 발생하는 단점이 있다.

```{figure} _image/0103.png
```

지금까지 살펴본 모든 경우에는 primary memory의 용량이 프로그램을 실행하는데 필요한 모든 메모리보다 항상 커야한다. 그렇지 않을 경우 메모리 부족으로 해당 프로그램을 실행할 수 없다. 따라서 primary memory의 용량보다 필요한 메모리가 더 큰 프로그램를 실행시키기 위해 `오버레이(overlay) 기법`이 나오게 되었다. overlay 기법은 프로그래머가 프로그램을 모듈로 나눈 후에 실행에 필요한 모듈만 primary memory에 올려 실행하는 방식이다. 

```{figure} _image/0104.png
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


