# Resource Creation

## Commited Resource and Placed Resource
Committed Resource 는 resource 와 resource 를 저장하는데 필요한 독립적인 GPU 의 heap memory 공간이 동시에 할당되는 resource 를 의미한다.

Committed Resource 의 경우 독립적인 heap memory 공간이 할당되기 때문에, commited resource 를 생성할때는 heap memory 공간에 대한 고려를 할 필요가 없다.

이와 다르게 Placed Resource 의 경우 resource 를 생성할 때, resource 를 저장하는데 필요한 heap memory 공간을 함께 주어저야 하는 resource 를 의미한다.

따라서, placed resource 의 경우 heap memory 를 공유하게 할 수 있어 resource 간 메모리 낭비를 줄여 memory 를 보다 효율적으로 사용할 수 있게 만들 수 있다.

<details> <summary> <h3 style="display:inline-block"> CreateCommittedResource </h3></summary>
ID3D12Device::CreateCommittedResource 함수로 생성할 수 있다.

> Reference   
> [learn.microsoft - nf-d3d12-id3d12device-createcommittedresource](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommittedresource)  

D3D12_HEAP_PROPERTIES 구조체의 enum D3D12_HEAP_TYPE 이 D3D12_HEAP_TYPE_UPLOAD 일 경우에는 반드시 enum D3D12_RESOURCE_STATES 는 D3D12_RESOURCE_STATE_GENERIC_READ 여야 한다. 만약 그렇지 않을 경우 Debug Layer 에서 다음과 같은 오류 메세지가 출력된다.
```
D3D12 ERROR: ID3D12Device::CreateCommittedResource: Certain resources are restricted to certain D3D12_RESOURCE_STATES states, and cannot be changed. Resources on D3D12_HEAP_TYPE_UPLOAD heaps requires D3D12_RESOURCE_STATE_GENERIC_READ. Reserved buffers used exclusively for texture placement requires D3D12_RESOURCE_STATE_COMMON. [ RESOURCE_MANIPULATION ERROR #741: RESOURCE_BARRIER_INVALID_HEAP]
```

> Reference   
> [learn.microsoft - d3d12_heap_properties](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_heap_properties)  
> [learn.microsoft - d3d12_heap_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_heap_type)  
> [learn.microsoft - d3d12_resource_states](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_states)  

CD3DX12_RESOURCE_DESC 구조체는 D3D12_RESOURCE_DESC 구조체를 쉽게 생성하고 조작할 수 있도록 도와주는 헬퍼 클래스이다. 

Buffer(UINT64 width, D3D12_RESOURCE_FLAGS flags = D3D12_RESOURCE_FLAG_NONE, UINT64 alignment = 0); 함수를 호출하게 되면 다음과 같은 대입이 발생하게 된다.
```
Dimension         = D3D12_RESOURCE_DIMENSION_BUFFER
Alignment         = alignment
Width             = width
Height            = 1
DepthOrArraySize  = 1
MipLevels         = 1
Format            = DXGI_FORMAT_UNKNOWN
SampleDesc.Count  = 1
SampleDesc.Quality= 0
Layout            = D3D12_TEXTURE_LAYOUT_ROW_MAJOR
Flags             = flags
```

> Reference  
> [learn.microsoft - cd3dx12-resource-desc](https://learn.microsoft.com/en-us/windows/win32/direct3d12/cd3dx12-resource-desc)  
</details>

## Map
Map 은 GPU memory 중 CPU 접근이 가능한 영역과 CPU 에서 접근 가능한 virtual memory adrress 를 mapping 하고 mapping 된 virtual memory adrress 를 반환하는 과정이다.


<details> <summary> <h3 style="display:inline-block"> ID3D12Resource::Map </h3></summary>
Map 은 ID3D12Resource::Map 함수를 통해서 이루어진다. 

인자중 UINT Subresource 는 subresource 의 index 를 나타내는 값이다.

인자중 const D3D12_RANGE *pReadRange 는 CPU가 읽을 범위를 지정하는 D3D12_RANGE 구조체의 포인터이다. 만약 nullptr 을 전달할 경우 CPU가 전체 하위 리소스를 읽을 수 있음을 나타낸다. 그리고 CPU가 메모리를 읽지 않을 경우 Range 의 End 가 Begine 보다 작거나 같게 하면 된다.

만약, Resource 가 존재하는 GPU Heap 의 Heap Type 이 D3D12_HEAP_TYPE_UPLOAD, D3D12_HEAP_TYPE_READBACK 이 아닌데 Map 함수를 호출하면 다음과 같은 오류가 발생한다.
```
D3D12 ERROR: ID3D12Resource2::ID3D12Resource::Map: Map and Unmap can not be called on a resource associated with a heap that has the CPU page properties of D3D12_CPU_PAGE_PROPERTY_NOT_AVAILABLE. Heaps of the type D3D12_HEAP_TYPE_DEFAULT should be assumed to have these properties. [ EXECUTION ERROR #822: MAP_INVALIDHEAP]
```

> Reference  
> [learn.microsoft - id3d12resource-map](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12resource-map)  
</details>


<details> <summary> <h3 style="display:inline-block"> Persistenet Map </h3></summary>
D3D11 에서는 resource 의 접근 권한과 상태를 API 가 자동으로 관리해주었다. 따라서 개발자가 명시적으로 관리할 필요가 없었지만 최적화를 하는데 한계점이 있었다. 예를 들어, D3D11 의 경우 CPU와 GPU가 동시에 같은 리소스를 접근하려고 하면 동기화 문제가 발생할 수 있기 때문에, Map 함수를 호출한 경우 Unmap 을 호출하기 전까지 GPU 에서 접근이 불가능하게 막았다.이로 인해 resource 를 매번 Map 하고 Unmap 해야 했다.

하지만 D3D12 에서는 리소스의 상태 전환과 접근 권한을 개발자가 직접 관리한다. 따라서 Map, Unmap 에 있떤 제약 조건은 사라졌고 CPU와 GPU가 서로 충돌 없이 resource 에 접근할 수 있도록 동기화 작업을 개발자가 제어하게 되었다. 따라서, D3D12 에서는 CPU 에서 접근가능한 heap 에 존재하는 resource 는 영구적 매핑인 persistent map 을 사용할 수 있다. persistent map 은 resource 생성 직후에 Map 을 한 번만 호출하고 Unmap 을 호출하지 않음으로써 구현할 수 있다. 

persistent map 을 사용하면 매 frame 마다 map, unmap 을 반복하지 않아도 되어 성능상에 이점이 있다.

persistent map 을 사용할 때, 애플리케이션은 GPU가 메모리를 읽거나 쓰는 command list 을 실행하기 전에 CPU 가 메모리에 데이터 쓰기를 완료해야 한다. 일반적인 시나리오에서는 애플리케이션이 ExecuteCommandLists 를 호출하기 전에 메모리에 쓰기만 하면 되지만, fence 를 사용하여 command list 실행을 지연시키는 방법도 작동한다.

단, resource 가 release 되고 난 후에는 Map 에서 반환된 주소를 더 이상 사용하지 않아야 한다. 

> Reference  
> [learn.microsoft - id3d12resource-map#advanced-usage-models](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12resource-map#advanced-usage-models)  
</details>




## GPU -> CPU 데이터 전송
* READBACK 리소스 생성
  * D3D12_HEAP_TYPE_READBACK 힙에 있는 리소스를 생성하여, GPU에서 데이터를 복사할 공간을 마련한다.
* 데이터 복사
  *  CopyResource 명령을 통해 GPU에서 사용 중인 리소스 데이터를 READBACK 리소스로 복사한다.
* 동기화
  * 복사 작업이 완료될 때까지 Fence를 사용하여 CPU 를 대기시킨다.
* Map으로 데이터 읽기
  * Map을 통해 CPU가 READBACK 리소스의 데이터에 접근하여 데이터를 읽어온다.
* Unmap으로 메모리 접근을 해제한다.  

### Caution
Upload heap 에 있는 resource 를 mapping 하여 읽기를 시도하는 것을 피해야 한다. CPU 읽기는 정상적으로 수행되지만 많은 일반적인 GPU architecture 에서 엄청나게 느리다.

D3D12_CPU_PAGE_PROPERTY_WRITE_COMBINE 를 CPU page property 로 갖는 heap 에 있는 resource 를 mapping 하여 읽기를 시도하는 것을 피해야 한다. 이 또한 엄청나게 느리다.

> Reference  
> [learn.microsoft - nf-d3d12-id3d12resource-map#simple-usage-models](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12resource-map#simple-usage-models)

## Resource 를 만드는데 필요한 GPU memory 알아내기
ID3D12Device::GetResourceAllocationInfo

> Reference  
> [learn.microsoft - getresourceallocationinfo](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-getresourceallocationinfo(uint_uint_constd3d12_resource_desc))  
