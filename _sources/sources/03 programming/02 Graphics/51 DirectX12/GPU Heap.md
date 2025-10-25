# GPU Heap

## ID3D12Device::CreateHeap 함수
placed resources 와 reserved resources 에 사용할 수 있는 GPU heap 을 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CreateHeap(
  [in]            const D3D12_HEAP_DESC *pDesc,
  [in]            REFIID                riid,
  [out, optional] void                  **ppvHeap
);
```

인자는 다음과 같다.

- const D3D12_HEAP_DESC *pDesc
  - 생성할 힙의 속성을 지정하는 D3D12_HEAP_DESC 구조체에 대한 포인터다. 
- REFIID riid
  - 요청하는 인터페이스의 식별자다.
  - 일반적으로 __uuidof(ID3D12Heap)를 사용하여 얻는다.
- void **ppvHeap
  - 성공 시, 생성된 ID3D12Heap 객체의 포인터를 받을 포인터다.

반환값은 다음과 같다.
- 성공 시
  - S_OK를 반환한다.
- 실패 시
  - HRESULT 오류 코드를 반환한다. 

- 힙을 생성한 후, ID3D12Device::CreateReservedResource 함수를 사용하여 예약된 리소스를 생성할 수 있다. 예약된 리소스는 가상 메모리 공간을 예약하지만 실제 물리 메모리는 할당되지 않는다. 이후 ID3D12CommandQueue::UpdateTileMappings 함수를 사용하여 실제 메모리를 매핑할 수 있다.  

> Reference  
> [learn.microsoft - id3d12device-createheap](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-createheap)

## D3D12_HEAP_DESC 구조체
GPU Heap 의 속성을 정의하는 데 사용된다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_HEAP_DESC {
  UINT64                SizeInBytes;
  D3D12_HEAP_PROPERTIES Properties;
  UINT64                Alignment;
  D3D12_HEAP_FLAGS      Flags;
} D3D12_HEAP_DESC;
```

각 멤버 변수는 다음과 같다.

* UINT64 SizeInBytes  
  * 힙의 크기를 바이트 단위로 지정한다.
  * 메모리 낭비를 피하기 위해 Alignment의 배수로 설정하는 것이 좋다.

* D3D12_HEAP_PROPERTIES Properties  
  * 힙의 속성을 지정하는 구조체이다.
  * 힙의 유형, CPU 페이지 속성, 메모리 풀 우선순위 등을 설정한다.

* UINT64 Alignment  
  * 힙의 정렬 값을 지정한다.
  * 사용가능한 값은 [여기](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_desc#members)를 참고하면 된다.
  * 0은 64KB 정렬을 의미한다.

* D3D12_HEAP_FLAGS Flags  
  * 힙의 옵션을 지정하는 플래그이다.

> Reference  
> [learn.microsoft - d3d12_heap_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_desc)  

## enum D3D12_HEAP_FLAGS
Heap 의 동작 방식을 제어하는 플래그를 정의한다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_HEAP_FLAGS
{
    D3D12_HEAP_FLAG_NONE                         = 0,
    D3D12_HEAP_FLAG_SHARED                       = 0x1,
    D3D12_HEAP_FLAG_DENY_BUFFERS                 = 0x4,
    D3D12_HEAP_FLAG_ALLOW_DISPLAY                = 0x8,
    D3D12_HEAP_FLAG_SHARED_CROSS_ADAPTER         = 0x20,
    D3D12_HEAP_FLAG_DENY_RT_DS_TEXTURES          = 0x40,
    D3D12_HEAP_FLAG_DENY_NON_RT_DS_TEXTURES      = 0x80,
    D3D12_HEAP_FLAG_HARDWARE_PROTECTED           = 0x100,
    D3D12_HEAP_FLAG_ALLOW_WRITE_WATCH            = 0x200,
    D3D12_HEAP_FLAG_ALLOW_SHADER_ATOMICS         = 0x400,
    D3D12_HEAP_FLAG_CREATE_NOT_RESIDENT          = 0x800,
    D3D12_HEAP_FLAG_CREATE_NOT_ZEROED            = 0x1000,
    D3D12_HEAP_FLAG_ALLOW_ALL_BUFFERS_AND_TEXTURES = 0,
    D3D12_HEAP_FLAG_ALLOW_ONLY_BUFFERS           = 0xC0,
    D3D12_HEAP_FLAG_ALLOW_ONLY_NON_RT_DS_TEXTURES = 0x44,
    D3D12_HEAP_FLAG_ALLOW_ONLY_RT_DS_TEXTURES    = 0x84
} D3D12_HEAP_FLAGS;
```

각 enum의 의미는 다음과 같다.

* D3D12_HEAP_FLAG_NONE
  * 힙에 특별한 동작을 지정하지 않는다.
* D3D12_HEAP_FLAG_SHARED
  * 힙을 여러 프로세스 간에 공유할 수 있도록 한다.
* D3D12_HEAP_FLAG_DENY_BUFFERS
  * 힙에 버퍼 리소스를 포함할 수 없도록 한다.
* D3D12_HEAP_FLAG_ALLOW_DISPLAY
  * 힙이 디스플레이 가능한 리소스를 포함할 수 있도록 한다. 이 플래그는 D3D12_HEAP_TYPE_DEFAULT와 함께 CreateCommittedResource에서만 사용할 수 있다.
* D3D12_HEAP_FLAG_SHARED_CROSS_ADAPTER
  * 힙을 여러 어댑터 간에 공유할 수 있도록 한다.
* D3D12_HEAP_FLAG_DENY_RT_DS_TEXTURES
  * 힙에 렌더 타겟 또는 깊이-스텐실 텍스처를 포함할 수 없도록 한다.
* D3D12_HEAP_FLAG_DENY_NON_RT_DS_TEXTURES
  * 힙에 렌더 타겟 또는 깊이-스텐실이 아닌 텍스처를 포함할 수 없도록 한다.
* D3D12_HEAP_FLAG_HARDWARE_PROTECTED
  * 힙을 하드웨어 보호 메모리에 할당한다.
* D3D12_HEAP_FLAG_ALLOW_WRITE_WATCH
  * 힙에 대한 쓰기 감시를 허용한다.
* D3D12_HEAP_FLAG_ALLOW_SHADER_ATOMICS
  * 힙에 포함된 리소스에서 셰이더 원자적 연산을 허용한다.
* D3D12_HEAP_FLAG_CREATE_NOT_RESIDENT
  * 힙을 생성할 때 메모리를 레지던트 상태로 만들지 않는다.
* D3D12_HEAP_FLAG_CREATE_NOT_ZEROED
  * 힙을 생성할 때 메모리를 0으로 초기화하지 않는다.
* D3D12_HEAP_FLAG_ALLOW_ALL_BUFFERS_AND_TEXTURES
  * 힙에 모든 버퍼와 텍스처를 포함할 수 있도록 한다.
* D3D12_HEAP_FLAG_ALLOW_ONLY_BUFFERS
  * 힙에 버퍼만 포함할 수 있도록 한다.
* D3D12_HEAP_FLAG_ALLOW_ONLY_NON_RT_DS_TEXTURES
  * 힙에 렌더 타겟 또는 깊이-스텐실이 아닌 텍스처만 포함할 수 있도록 한다.
* D3D12_HEAP_FLAG_ALLOW_ONLY_RT_DS_TEXTURES
  * 힙에 렌더 타겟 또는 깊이-스텐실 텍스처만 포함할 수 있도록 한다.

> Reference  
> [learn.microsoft - ne-d3d12-d3d12_heap_flags](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_heap_flags)  

## D3D12_HEAP_PROPERTIES 구조체  
Resource 할당을 위한 GPU Heap 의 속성을 정의하는 구조체이다. 

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_HEAP_PROPERTIES {
    D3D12_HEAP_TYPE Type;
    D3D12_CPU_PAGE_PROPERTY CPUPageProperty;
    D3D12_MEMORY_POOL MemoryPoolPreference;
    UINT CreationNodeMask;
    UINT VisibleNodeMask;
} D3D12_HEAP_PROPERTIES;
```

각 멤버 변수는 다음과 같다.

* D3D12_HEAP_TYPE Type  
  * 힙의 유형을 지정한다.
  * 힙 유형에 따라 메모리 접근 방식이 결정된다. 

* D3D12_CPU_PAGE_PROPERTY CPUPageProperty  
  * 힙의 CPU 페이지 속성을 지정한다.
  * 이 값은 D3D12_HEAP_TYPE이 CUSTOM으로 설정된 경우에만 유효하며, 다른 값으로 설정된 경우에는 D3D12_CPU_PAGE_PROPERTY_UNKNOWN이 기본적으로 사용된다. 

* D3D12_MEMORY_POOL MemoryPoolPreference  
  * 힙이 할당될 메모리 풀의 우선순위를 지정한다. 

* UINT CreationNodeMask  
  * 멀티 GPU 시스템에서 heap 이 생성될 노드를 지정하는 마스크 값이다.
  * 단일 GPU 환경에서는 1로 설정되며, 멀티 GPU 환경에서 특정 GPU에 할당할 수 있다.

* UINT VisibleNodeMask  
  * 힙을 볼 수 있는 노드를 지정하는 마스크 값이다.
  * 멀티 GPU 시스템에서 특정 GPU가 heap 에 접근할 수 있도록 설정한다.

> Reference  
> [learn.microsoft - ns-d3d12-d3d12_heap_properties](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_properties)  

### CD3DX12_HEAP_PROPERTIES 구조체
CD3DX12_HEAP_PROPERTIES 구조체는 D3D12_HEAP_PROPERTIES 구조체를 쉽게 초기화할 수 있는 helper 구조체다.

CD3DX12_HEAP_PROPERTIES 의 생성자에 enum D3D12_HEAP_TYPE 만 전달하면 다음 생성자가 호출된다.

```cpp
  explicit CD3DX12_HEAP_PROPERTIES(
      D3D12_HEAP_TYPE type,
      UINT creationNodeMask = 1,
      UINT nodeMask = 1 ) noexcept
  {
      Type = type;
      CPUPageProperty = D3D12_CPU_PAGE_PROPERTY_UNKNOWN;
      MemoryPoolPreference = D3D12_MEMORY_POOL_UNKNOWN;
      CreationNodeMask = creationNodeMask;
      VisibleNodeMask = nodeMask;
  }
```

> Reference  
> [learn.microsoft - cd3dx12-heap-properties](https://learn.microsoft.com/en-us/windows/win32/direct3d12/cd3dx12-heap-properties)  

## enum D3D12_HEAP_TYPE
GPU heap 의 유형을 정의하는 enum 이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_HEAP_TYPE
{
    D3D12_HEAP_TYPE_DEFAULT   = 1,
    D3D12_HEAP_TYPE_UPLOAD    = 2,
    D3D12_HEAP_TYPE_READBACK  = 3,
    D3D12_HEAP_TYPE_CUSTOM    = 4
} D3D12_HEAP_TYPE;
```

각 enum 의 의미는 다음과 같다.

* D3D12_HEAP_TYPE_DEFAULT
  * CPU 에서 접근이 제한된다.
  * GPU 에서 리소스를 사용하는 용도에 따라 데이터를 쓰는 상태와 읽는 상태를 바꾸는 작업을 하며 전환작업 중에 발생할 수 있는 데이터 충돌을 방지하기 위해 resource transition barrier 가 사용된다.
  * cpu 에 있는 resource 를 upload heap 에 있는 resource 에 복사하고 이를 default heap 에 있는 resource 에 복사하여 결론적으로 CPU 에 있는 resource 를 GPU 로 복사할 수 있다.
* D3D12_HEAP_TYPE_UPLOAD
  * CPU 에서 GPU 로 데이터를 전송할 떄 사용되는 GPU heap 이다.
  * CPU 접근 가능 memory 에 할당되며 최적화된 CPU 액세스를 제공한다.
  * CPU 에서 쓰기가 가능하며, GPU 에서 읽기 작업에 사용된다.
  * 이 힙에 생성되는 resource 는 D3D12_RESOURCE_STATE_GENERIC_READ 로 생성해야 하며, GPU 에서 읽기 작업만 함으로 이 heap 내에서 변경될 수 없다. 
* D3D12_HEAP_TYPE_READBACK
  * GPU에서 CPU로 데이터를 전송할 떄 사용되는 GPU heap 이다.
  * GPU에서 쓰기가 가능하며, CPU에서 읽기 작업에 사용된다.
* D3D12_HEAP_TYPE_CUSTOM
  * 사용자 정의 메모리 특성을 가진 힙이다.
  * 힙의 세부적인 동작을 사용자 정의할 수 있다.

> Reference  
> [learn.microsoft - ne-d3d12-d3d12_heap_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_heap_type)  

### Base Address Register
Base Adress Register (BAR) 는 GPU의 전용 메모리( VRAM )를 CPU 메모리 공간에 맵핑하는 데 사용된다.

VRAM 이 BAR 를 통해 CPU 의 가상 주소 공간에 나타나면, CPU 는 제한된 크기(보통 수백 MB에서 몇 GB)까지 GPU 메모리에 직접 접근할 수 있게 된다.

그리고 D3D12_HEAP_TYPE_UPLOAD, D3D12_HEAP_TYPE_READBACK 의 경우 BAR 를 사용하는 것 같다.

> Reference   
> [microsoft.github - D3D12GPUUploadHeaps.html](https://microsoft.github.io/DirectX-Specs/d3d/D3D12GPUUploadHeaps.html)  
> [therealmjp.github - gpu-memory-pool](https://therealmjp.github.io/posts/gpu-memory-pool/)  

## enum D3D12_CPU_PAGE_PROPERTY
CPU 메모리 페이지의 속성을 정의하는 enum 이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_CPU_PAGE_PROPERTY
{
    D3D12_CPU_PAGE_PROPERTY_UNKNOWN       = 0,
    D3D12_CPU_PAGE_PROPERTY_NOT_AVAILABLE = 1,
    D3D12_CPU_PAGE_PROPERTY_WRITE_COMBINE = 2,
    D3D12_CPU_PAGE_PROPERTY_WRITE_BACK    = 3
} D3D12_CPU_PAGE_PROPERTY;
```

각 enum의 의미는 다음과 같다.

* D3D12_CPU_PAGE_PROPERTY_UNKNOWN
  * 페이지 속성이 정의되지 않았다.
* D3D12_CPU_PAGE_PROPERTY_NOT_AVAILABLE
  * 페이지가 CPU에 의해 접근할 수 없다.
* D3D12_CPU_PAGE_PROPERTY_WRITE_COMBINE
  * 여러 쓰기 작업을 결합하여 성능을 향상시킨다.
* D3D12_CPU_PAGE_PROPERTY_WRITE_BACK
  * 쓰기 캐시가 가능한 메모리로, 쓰기 작업을 캐시에 저장한 후 백그라운드에서 메모리에 기록한다.

> Reference  
> [learn.microsoft - ne-d3d12-d3d12_cpu_page_property](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_cpu_page_property)  

### WRITE_COMBINE vs WRITE_BACK
Write-Combine 과 Write-Back 은 CPU가 메모리에 접근할 때의 캐시 정책에 따라 다르게 동작하는 메모리 유형이다. 

Write-Combine 메모리는 CPU 가 메모리에 데이터를 기록할 때 데이터를 임시 버퍼에 모았다가 한 번에 기록하는 방식으로 동작한다. 따라서, 캐시를 사용하지 않는다.

이 방식은 순차적인 쓰기 작업에 최적화되어 있으며, GPU에 데이터를 빠르게 전송해야 하는 경우에 주로 사용된다.

Write-Back 메모리는 CPU가 데이터를 메모리에 쓰기 전에 캐시에 저장하고, 이후에 캐시에서 메모리로 기록하는 방식으로 동작한다. 따라서, CPU가 동일 데이터를 여러 번 쓰거나 읽을 때 성능이 향상됩니다.

이는 일반적으로 읽기와 쓰기가 혼합된 작업에 최적화되어 있으며, 캐시 메모리를 통해 성능이 크게 향상됩니다.

## enum D3D12_MEMORY_POOL
Heap 의 메모리 풀이 할당될 물리적 메모리의 위치를 지정하는 열거형이다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_MEMORY_POOL
{
    D3D12_MEMORY_POOL_UNKNOWN = 0,
    D3D12_MEMORY_POOL_L0      = 1,
    D3D12_MEMORY_POOL_L1      = 2
} D3D12_MEMORY_POOL;
```

각 enum의 의미는 다음과 같다.

* D3D12_MEMORY_POOL_UNKNOWN
  * 메모리 풀이 unknown 이다.
* D3D12_MEMORY_POOL_L0
  * 물리적 시스템 메모리 풀을 나타낸다.
  * 이 풀은 CPU 에 대한 대역폭이 높고 GPU 에 대한 대역폭이 낮다.
  * 통합 메모리 아키텍처(UMA)에서는 이 풀이 유일하게 유효하다.
* D3D12_MEMORY_POOL_L1
  * 물리적 비디오 메모리 풀을 나타낸다.
  * 이 풀은 GPU 에 대한 대역폭이 높고 CPU 에서는 접근할 수 없다.
  * UMA 에서는 이 풀이 사용되지 않는다.

> Reference  
> [learn.microsoft - ne-d3d12-d3d12_memory_pool)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_memory_pool)  
