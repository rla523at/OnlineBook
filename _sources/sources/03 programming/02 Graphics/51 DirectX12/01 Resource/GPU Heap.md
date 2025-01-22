# GPU Heap

ID3D12Device::CreateHeap 함수로 placed resources 와 reserved resources 에 사용할 수 있는 GPU heap 을 생성할 수 있다.

A placed resource object holds a reference on the heap it is created on; but a reserved resource doesn't hold a reference for each mapping made to a heap.  

> Reference  
> [learn.microsoft - id3d12device-createheap](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-createheap)  

D3D12_HEAP_DESC 구조체는 GPU Heap 을 만들기 위해 필요한 정보를 정의한다.

Alignment 멤버 변수로 사용가능한 값은 [명세](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_desc#members)에 정해져있다.

> Reference  
> [learn.microsoft - d3d12_heap_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_desc)  
> [learn.microsoft - ne-d3d12-d3d12_heap_flags](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_heap_flags)  

D3D12_HEAP_PROPERTIES 구조체는 GPU Heap 의 속성을 정의하는 구조체이다. 

CPUPageProperty 멤버 변수는 D3D12_HEAP_TYPE이 CUSTOM으로 설정된 경우에만 유효하며, 다른 값으로 설정된 경우에는 D3D12_CPU_PAGE_PROPERTY_UNKNOWN이 기본적으로 사용된다. 

> Reference  
> [learn.microsoft - ns-d3d12-d3d12_heap_properties](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_properties)   

CD3DX12_HEAP_PROPERTIES 구조체는 D3D12_HEAP_PROPERTIES 구조체를 쉽게 초기화할 수 있는 helper 구조체다. CD3DX12_HEAP_PROPERTIES 의 생성자에 enum D3D12_HEAP_TYPE 만 전달하면 다음과 같은 값을 갖는 D3D12_HEAP_PROPERTIES 구조체가 생성된다.

```cpp
Type = type;
CPUPageProperty = D3D12_CPU_PAGE_PROPERTY_UNKNOWN;
MemoryPoolPreference = D3D12_MEMORY_POOL_UNKNOWN;
CreationNodeMask = 1;
VisibleNodeMask = 1;
```

enum D3D12_MEMORY_POOL 은 Heap 의 메모리 풀이 할당될 물리적 메모리의 위치를 지정하는 enum 이다.

> Reference  
> [learn.microsoft - cd3dx12-heap-properties](https://learn.microsoft.com/en-us/windows/win32/direct3d12/cd3dx12-heap-properties)   
> [learn.microsoft - d3d12_heap_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_heap_type)   
> [learn.microsoft - d3d12_cpu_page_property](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_cpu_page_property)  
> [learn.microsoft - d3d12_memory_pool)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_memory_pool)  

### Base Address Register
Base Adress Register (BAR) 는 GPU의 전용 메모리( VRAM )를 CPU 메모리 공간에 맵핑하는 데 사용된다.

VRAM 이 BAR 를 통해 CPU 의 가상 주소 공간에 나타나면, CPU 는 제한된 크기(보통 수백 MB에서 몇 GB)까지 GPU 메모리에 직접 접근할 수 있게 된다.

그리고 D3D12_HEAP_TYPE_UPLOAD, D3D12_HEAP_TYPE_READBACK 의 경우 BAR 를 사용하는 것 같다.

> Reference   
> [microsoft.github - D3D12GPUUploadHeaps.html](https://microsoft.github.io/DirectX-Specs/d3d/D3D12GPUUploadHeaps.html)  
> [therealmjp.github - gpu-memory-pool](https://therealmjp.github.io/posts/gpu-memory-pool/)  

### WRITE_COMBINE vs WRITE_BACK
Write-Combine 과 Write-Back 은 CPU가 메모리에 접근할 때의 캐시 정책에 따라 다르게 동작하는 메모리 유형이다. 

Write-Combine 메모리는 CPU 가 메모리에 데이터를 기록할 때 데이터를 임시 버퍼에 모았다가 한 번에 기록하는 방식으로 동작한다. 따라서, 캐시를 사용하지 않는다.

이 방식은 순차적인 쓰기 작업에 최적화되어 있으며, GPU에 데이터를 빠르게 전송해야 하는 경우에 주로 사용된다.

Write-Back 메모리는 CPU가 데이터를 메모리에 쓰기 전에 캐시에 저장하고, 이후에 캐시에서 메모리로 기록하는 방식으로 동작한다. 따라서, CPU가 동일 데이터를 여러 번 쓰거나 읽을 때 성능이 향상됩니다.

이는 일반적으로 읽기와 쓰기가 혼합된 작업에 최적화되어 있으며, 캐시 메모리를 통해 성능이 크게 향상됩니다.
