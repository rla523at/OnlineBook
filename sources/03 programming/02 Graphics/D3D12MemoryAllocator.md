# D3D12MemoryAllocator

> Reference  
> [github - D3D12MemoryAllocator](https://github.com/GPUOpen-LibrariesAndSDKs/D3D12MemoryAllocator)  
> [github.io/D3D12MemoryAllocator](https://gpuopen-librariesandsdks.github.io/D3D12MemoryAllocator/html/)


## D3D12MA 를 이용해서 할당한 메모리 사용량 알아내기

D3D12MA::Allocator 의 GetBudget 함수를 사용하면 된다.

함수의 시그니쳐는 다음과 같다.
```cpp
void D3D12MA::Allocator::GetBudget ( Budget* pLocalBudget, Budget*	pNonLocalBudget )
```

discrete graphics card 의 경우 pLocalBudget 변수는 video memory 에 대한 Budget 객체를 반환하고 pNonLocalBudget 변수는 D3D12 resource 가 사용가능한 system memory 에 대한 Budget 객체를 반환한다.

Budget 구조체는 Statistics of current memory usage and available budget for a specific memory segment group 이다.

구조체의 정의는 다음과 같다.
```cpp
struct Budget
{
    Statistics Stats;
    UINT64 UsageBytes;
    UINT64 BudgetBytes;
};
```

멤버변수는 다음과 같다.

* UINT64 UsageBytes
  * Estimated amount of memory available to the program 을 나타낸다.
* UsageBytes
  * Estimated current memory usage of the program 을 나타낸다.
  * 일반적으로 Stats.BlockBytes 보다 큰 값을 갖는데 이는 additional implicit objects also occupying the memory, like swapchain, pipeline state objects, descriptor heaps, command lists, or heaps and resources allocated outside of this library,

Statistics 구조체는 Calculated statistics of memory usage e.g. in a specific memory heap type, memory segment group, custom pool, or total 이다.

구조체의 정의는 다음과 같다.
```cpp
struct Statistics
{
    UINT BlockCount;
    UINT AllocationCount;
    UINT64 BlockBytes;
    UINT64 AllocationBytes;
};
```

* BlockBytes
  * Number of bytes allocated in memory blocks.
* AllocationBytes
  * Total number of bytes occupied by all D3D12MA::Allocation objects.

> Reference  
> [github.io/D3D12MemoryAllocator - GetBudget](https://gpuopen-librariesandsdks.github.io/D3D12MemoryAllocator/html/class_d3_d12_m_a_1_1_allocator.html#a1ac113daec5f6ef28ecb1786cf544144)    
> [D3D12MA::Budget Struct Reference](https://gpuopen-librariesandsdks.github.io/D3D12MemoryAllocator/html/struct_d3_d12_m_a_1_1_budget.html#a1255508930766db238cfb1312b15f1cf)  
> [D3D12MA::Statistics Struct Reference](https://gpuopen-librariesandsdks.github.io/D3D12MemoryAllocator/html/struct_d3_d12_m_a_1_1_statistics.html)  
