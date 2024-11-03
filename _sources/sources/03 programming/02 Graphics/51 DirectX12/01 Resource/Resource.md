# Resource

* 개념과 API 를 나누면 좋지 않을까 싶다.

* ID3D12Resource::Release
* D3D12_Descriptor_HEAP_DESC
* ID3D12DescriptorHeap
* ID3D12Device::CreateDescriptorHeap
* D3D12_CPU_DESCRIPTOR_HANDLE
* D3D12_GPU_DESCRIPTOR_HANDLE
* D3D12_DESCRIPTOR_HEAP_TYPE
* ID3D12Device::GetDescriptorHandleIncrementSize
* GetGPUVirtualAdress
* D3D12_CONSTANT_BUFFER_VIEW_DESC
* D3D12_CPU_DESCRIPTOR_HANDLE
* CreateConstantBufferView

## Commited Resource and Placed Resource
Committed Resource 는 resource 와 resource 를 저장하는데 필요한 독립적인 heap memory 공간이 동시에 할당되는 resource 를 의미한다.

Committed Resource 의 경우 독립적인 heap memory 공간이 할당되기 때문에, commited resource 를 생성할때는 heap memory 공간에 대한 고려를 할 필요가 없다.

이와 다르게 Placed Resource 의 경우 resource 를 생성할 때, resource 를 저장하는데 필요한 heap memory 공간을 함께 주어저야 하는 resource 를 의미한다.

따라서, placed resource 의 경우 heap memory 를 공유하게 할 수 있어 resource 간 메모리 낭비를 줄여 memory 를 보다 효율적으로 사용할 수 있게 만들 수 있다.

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

D3D12_HEAP_TYPE_UPLOAD 인 heap 에 저장된 resourece 로 copy resource 를 하면 오류가 발생하는지 확인해보기

## CPU -> GPU 데이터 전송

Map 하고 Unmap 안해도 문제 없는지 확인하기

## ID3D12Device::CreateCommittedResource
ID3D12Device::CreateCommittedResource 함수는 committed resource 를 생성하는 함수이다. 

함수의 시그니처는 다음과 같다.
```cpp
HRESULT CreateCommittedResource(
  [in]            const D3D12_HEAP_PROPERTIES *pHeapProperties,
  [in]            D3D12_HEAP_FLAGS            HeapFlags,
  [in]            const D3D12_RESOURCE_DESC   *pDesc,
  [in]            D3D12_RESOURCE_STATES       InitialResourceState,
  [in, optional]  const D3D12_CLEAR_VALUE     *pOptimizedClearValue,
  [in]            REFIID                      riidResource,
  [out, optional] void                        **ppvResource
);
```

인자는 다음과 같다.
* const D3D12_HEAP_PROPERTIES *pHeapProperties
  * 리소스가 할당될 메모리 힙의 속성을 지정하는 포인터다.

* D3D12_HEAP_FLAGS HeapFlags
  * 힙의 플래그를 지정하는 값이다.

* const D3D12_RESOURCE_DESC *pDesc
  * 생성할 리소스의 설명을 정의하는 구조체 포인터다. 

* D3D12_RESOURCE_STATES InitialResourceState
  * 생성된 리소스의 초기 상태를 정의한다. 

* const D3D12_CLEAR_VALUE *pOptimizedClearValue
  * 최적화된 클리어 값을 지정하는 포인터다. 리소스가 렌더 타겟 또는 깊이 스텐실 뷰인 경우 초기화에 사용할 클리어 값을 설정하며, 그렇지 않으면 `NULL`로 설정할 수 있다.

* REFIID riidResource
  * 요청하는 리소스 인터페이스의 식별자다. 일반적으로 `ID3D12Resource` 인터페이스를 요청한다.

* void **ppvResource
  * 성공 시, 생성된 리소스 객체의 포인터를 받을 포인터이다. 이 포인터는 `ID3D12Resource` 인터페이스를 가리킨다.

반환값은 다음과 같다.
* 성공 시
  * `S_OK`를 반환한다.

* 실패 시
  * `HRESULT` 오류 코드를 반환한다.
  * 오류가 발생한 경우, 코드 값에 따라 리소스 생성에 실패한 원인을 진단할 수 있다.

> Reference  
> [learn.microsoft - nf-d3d12-id3d12device-createcommittedresource)](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-createcommittedresource)

## D3D12_RESOURCE_DESC 구조체
resource 의 특성을 정의하는 데 사용되는 구조체이다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_RESOURCE_DESC {
  D3D12_RESOURCE_DIMENSION Dimension;
  UINT64                   Alignment;
  UINT64                   Width;
  UINT                     Height;
  UINT16                   DepthOrArraySize;
  UINT16                   MipLevels;
  DXGI_FORMAT              Format;
  DXGI_SAMPLE_DESC         SampleDesc;
  D3D12_TEXTURE_LAYOUT     Layout;
  D3D12_RESOURCE_FLAGS     Flags;
} D3D12_RESOURCE_DESC;
```

각 멤버 변수는 다음과 같다.

* D3D12_RESOURCE_DIMENSION Dimension  
  * 리소스의 차원을 지정한다. 

* UINT64 Alignment  
  * 리소스의 정렬(alignment) 요구 사항을 지정한다.
  * 정렬을 0으로 설정할 경우
    * MSAA 텍스처에 4MB를 사용하고 다른 모든 텍스처에 64KB를 사용한다.
    * 텍스처가 작은 경우 애플리케이션은 몇 가지 텍스처 유형에 대해 이 기본값보다 작은 정렬을 선택할 수 있다.
    * 알 수 없는 레이아웃 및 MSAA 텍스처는 64KB 정렬로 만들 수 있습니다

* UINT64 Width  
  * 리소스의 너비를 지정한다.
  * 버퍼의 경우 전체 크기를 바이트 단위로 나타내며, 텍스처의 경우 픽셀 단위의 너비를 나타낸다.

* UINT Height  
  * 텍스처 리소스의 높이를 지정한다.
  * 버퍼 리소스의 경우에는 1로 설정된다.

* UINT16 DepthOrArraySize  
  * 3D 텍스처의 경우 깊이(depth)를, 1D 또는 2D 텍스처 배열의 경우 배열의 크기를 지정한다.

* UINT16 MipLevels  
  * 리소스의 MIP 맵 레벨 수를 지정한다.
  * 0으로 설정하면 가능한 최대 MIP 레벨이 자동으로 결정된다.

* DXGI_FORMAT Format  
  * 리소스의 데이터 형식을 지정한다. 

* DXGI_SAMPLE_DESC SampleDesc  
  * 멀티샘플링에 대한 정보를 지정하는 구조체로, 샘플 수와 품질 수준을 설정한다.
  * 예를 들어, Count가 1이면 멀티샘플링이 적용되지 않는다.

* D3D12_TEXTURE_LAYOUT Layout  
  * 리소스의 메모리 레이아웃을 지정한다.

* D3D12_RESOURCE_FLAGS Flags  
  * 리소스의 특성을 지정하는 플래그다.

> Reference  
> [learn.microsoft - ns-d3d12-d3d12_resource_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_resource_desc)  

### CD3DX12_RESOURCE_DESC 구조체
D3D12_RESOURCE_DESC 구조체를 쉽게 생성하고 조작할 수 있도록 도와주는 헬퍼 클래스이다. 

구조체의 정의는 다음과 같다.
```cpp
struct CD3DX12_RESOURCE_DESC : public D3D12_RESOURCE_DESC {
    CD3DX12_RESOURCE_DESC();
    explicit CD3DX12_RESOURCE_DESC(const D3D12_RESOURCE_DESC& o);
    static CD3DX12_RESOURCE_DESC Buffer(UINT64 width, D3D12_RESOURCE_FLAGS flags = D3D12_RESOURCE_FLAG_NONE, UINT64 alignment = 0);
    static CD3DX12_RESOURCE_DESC Tex1D(DXGI_FORMAT format, UINT64 width, UINT16 arraySize = 1, UINT16 mipLevels = 0, D3D12_RESOURCE_FLAGS flags = D3D12_RESOURCE_FLAG_NONE, D3D12_TEXTURE_LAYOUT layout = D3D12_TEXTURE_LAYOUT_UNKNOWN, UINT64 alignment = 0);
    static CD3DX12_RESOURCE_DESC Tex2D(DXGI_FORMAT format, UINT64 width, UINT height, UINT16 arraySize = 1, UINT16 mipLevels = 0, UINT sampleCount = 1, UINT sampleQuality = 0, D3D12_RESOURCE_FLAGS flags = D3D12_RESOURCE_FLAG_NONE, D3D12_TEXTURE_LAYOUT layout = D3D12_TEXTURE_LAYOUT_UNKNOWN, UINT64 alignment = 0);
    static CD3DX12_RESOURCE_DESC Tex3D(DXGI_FORMAT format, UINT64 width, UINT height, UINT16 depth, UINT16 mipLevels = 0, D3D12_RESOURCE_FLAGS flags = D3D12_RESOURCE_FLAG_NONE, D3D12_TEXTURE_LAYOUT layout = D3D12_TEXTURE_LAYOUT_UNKNOWN, UINT64 alignment = 0);
};
```

Buffer 함수를 호출하게 되면 다음과 같은 대입이 발생하게 된다.
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

## enum D3D12_RESOURCE_STATES
resource 의 상태를 정의하는 플래그를 나타낸다.

정의는 다음과 같다.

```cppp
typedef enum D3D12_RESOURCE_STATES
{
    D3D12_RESOURCE_STATE_COMMON                     = 0,
    D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER = 0x1,
    D3D12_RESOURCE_STATE_INDEX_BUFFER               = 0x2,
    D3D12_RESOURCE_STATE_RENDER_TARGET              = 0x4,
    D3D12_RESOURCE_STATE_UNORDERED_ACCESS           = 0x8,
    D3D12_RESOURCE_STATE_DEPTH_WRITE                = 0x10,
    D3D12_RESOURCE_STATE_DEPTH_READ                 = 0x20,
    D3D12_RESOURCE_STATE_NON_PIXEL_SHADER_RESOURCE  = 0x40,
    D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE      = 0x80,
    D3D12_RESOURCE_STATE_STREAM_OUT                 = 0x100,
    D3D12_RESOURCE_STATE_INDIRECT_ARGUMENT          = 0x200,
    D3D12_RESOURCE_STATE_COPY_DEST                  = 0x400,
    D3D12_RESOURCE_STATE_COPY_SOURCE                = 0x800,
    D3D12_RESOURCE_STATE_RESOLVE_DEST               = 0x1000,
    D3D12_RESOURCE_STATE_RESOLVE_SOURCE             = 0x2000,
    D3D12_RESOURCE_STATE_RAYTRACING_ACCELERATION_STRUCTURE = 0x400000,
    D3D12_RESOURCE_STATE_SHADING_RATE_SOURCE        = 0x1000000,
    D3D12_RESOURCE_STATE_GENERIC_READ               = D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER |
                                                      D3D12_RESOURCE_STATE_INDEX_BUFFER |
                                                      D3D12_RESOURCE_STATE_NON_PIXEL_SHADER_RESOURCE |
                                                      D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE |
                                                      D3D12_RESOURCE_STATE_INDIRECT_ARGUMENT |
                                                      D3D12_RESOURCE_STATE_COPY_SOURCE,
    D3D12_RESOURCE_STATE_ALL_SHADER_RESOURCE        = D3D12_RESOURCE_STATE_NON_PIXEL_SHADER_RESOURCE |
                                                      D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE,
    D3D12_RESOURCE_STATE_PRESENT                    = 0,
    D3D12_RESOURCE_STATE_PREDICATION                = 0x200
} D3D12_RESOURCE_STATES;
```

각 enum의 의미는 다음과 같다.

* D3D12_RESOURCE_STATE_COMMON
  * 리소스가 특정 사용 상태에 있지 않으며, 일반적인 상태이다.
* D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER
  * 리소스가 버텍스 또는 상수 버퍼로 사용된다.
* D3D12_RESOURCE_STATE_INDEX_BUFFER
  * 리소스가 인덱스 버퍼로 사용된다.
* D3D12_RESOURCE_STATE_RENDER_TARGET
  * 리소스가 렌더 타겟으로 사용된다.
* D3D12_RESOURCE_STATE_UNORDERED_ACCESS
  * 리소스가 언오더드 액세스 뷰(UAV)로 사용된다.
* D3D12_RESOURCE_STATE_DEPTH_WRITE
  * 리소스가 깊이 버퍼로 쓰기 작업에 사용된다.
* D3D12_RESOURCE_STATE_DEPTH_READ
  * 리소스가 깊이 버퍼로 읽기 작업에 사용된다.
* D3D12_RESOURCE_STATE_NON_PIXEL_SHADER_RESOURCE
  * 리소스가 픽셀 셰이더 이외의 셰이더에서 읽기 전용으로 사용된다.
* D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE
  * 리소스가 픽셀 셰이더에서 읽기 전용으로 사용된다.
* D3D12_RESOURCE_STATE_STREAM_OUT
  * 리소스가 스트림 출력 타겟으로 사용된다.
* D3D12_RESOURCE_STATE_INDIRECT_ARGUMENT
  * 리소스가 간접 명령 인수로 사용된다.
* D3D12_RESOURCE_STATE_COPY_DEST
  * 리소스가 복사 작업의 대상으로 사용된다.
* D3D12_RESOURCE_STATE_COPY_SOURCE
  * 리소스가 복사 작업의 소스로 사용된다.
* D3D12_RESOURCE_STATE_RESOLVE_DEST
  * 리소스가 멀티샘플링 해상도 감소 작업의 대상으로 사용된다.
* D3D12_RESOURCE_STATE_RESOLVE_SOURCE
  * 리소스가 멀티샘플링 해상도 감소 작업의 소스로 사용된다.
* D3D12_RESOURCE_STATE_RAYTRACING_ACCELERATION_STRUCTURE
  * 리소스가 레이 트레이싱 가속 구조로 사용된다.
* D3D12_RESOURCE_STATE_SHADING_RATE_SOURCE
  * 리소스가 셰이딩 레이트 소스로 사용된다.
* D3D12_RESOURCE_STATE_GENERIC_READ
  * 일반적인 읽기 작업에 사용되는 상태로, 여러 읽기 전용 상태의 조합이다.
  * upload heap 에서 생성할 resource 의 필수 시작 상태다.
* D3D12_RESOURCE_STATE_ALL_SHADER_RESOURCE
  * 모든 셰이더에서 읽기 전용으로 사용되는 상태의 조합이다.
* D3D12_RESOURCE_STATE_PRESENT
  * 리소스가 표시(present) 작업에 사용된다.
* D3D12_RESOURCE_STATE_PREDICATION
  * 리소스가 조건부 렌더링(predication)에 사용된다.

> Reference
> [learn.microsoft - ne-d3d12-d3d12_resource_states](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_states)


## ID3D12Resource::Map
resource 를 CPU가 접근할 수 있도록 CPU memory 에 mapping 하는 함수이다. 

함수의 시그니처는 다음과 같다.
```cpp
HRESULT Map(
                  UINT              Subresource,
  [in, optional]  const D3D12_RANGE *pReadRange,
  [out, optional] void              **ppData
);
```

인자는 다음과 같다.
* UINT Subresource
  * 매핑할 하위 리소스의 인덱스이다.
  * 일반적으로 리소스가 단일 서브리소스를 가지는 경우 `0`을 전달한다.

* const D3D12_RANGE *pReadRange
  * CPU가 읽을 범위를 지정하는 D3D12_RANGE 구조체의 포인터이다.
  * 이 구조체는 읽기 작업의 시작과 끝을 정의한다.
  * nullptr 을 전달할 경우 CPU가 전체 하위 리소스를 읽을 수 있음을 나타낸다
  * CPU가 메모리를 읽지 않을 경우 Range 의 End 가 Begine 보다 작게 하면 된다.

* void **ppData
  * 성공 시, 매핑된 리소스의 메모리를 가리키는 포인터를 받는 포인터다.
  * 이 포인터를 통해 CPU는 리소스 데이터를 접근할 수 있게 된다.

반환값은 다음과 같다.
* 성공 시
  * `S_OK`를 반환한다.

* 실패 시
  * `HRESULT` 오류 코드를 반환한다.
  * 오류가 발생한 경우, 적절한 오류 코드를 통해 매핑 실패 원인을 진단할 수 있다.

> Reference  
> [learn.microsoft - nf-d3d12-id3d12resource-map](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12resource-map)  

### Caution with Read
Upload heap 에 있는 resource 를 mapping 하여 읽기를 시도하는 것을 피해야 한다. CPU 읽기는 정상적으로 수행되지만 많은 일반적인 GPU architecture 에서 엄청나게 느리다.

D3D12_CPU_PAGE_PROPERTY_WRITE_COMBINE 를 CPU page property 로 갖는 heap 에 있는 resource 를 mapping 하여 읽기를 시도하는 것을 피해야 한다. 이 또한 엄청나게 느리다.

> Reference  
> [learn.microsoft - nf-d3d12-id3d12resource-map#simple-usage-models](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12resource-map#simple-usage-models)

### Persistent Map
D3D11 에서는 resource 의 접근 권한과 상태를 API 가 자동으로 관리해주었다. 따라서 개발자가 명시적으로 관리할 필요가 없었지만 최적화를 하는데 한계점이 있었다.

예를 들어, D3D11 의 경우 CPU와 GPU가 동시에 같은 리소스를 접근하려고 하면 동기화 문제가 발생할 수 있기 때문에, Map 함수를 호출한 경우 Unmap 을 호출하기 전까지 GPU 에서 접근이 불가능하게 막았다.이로 인해 resource 를 매번 Map 하고 Unmap 해야 했다.

하지만 D3D12 에서는 리소스의 상태 전환과 접근 권한을 개발자가 직접 관리한다. 따라서 Map, Unmap 에 있떤 제약 조건은 사라졌고 CPU와 GPU가 서로 충돌 없이 resource 에 접근할 수 있도록 동기화 작업을 개발자가 제어하게 되었다.

따라서, D3D12 에서는 CPU 에서 접근가능한 heap 에 존재하는 resource 는 영구적 매핑인 persistent map 을 사용할 수 있다. persistent map 은 resource 생성 직후에 Map 을 한 번만 호출하고 Unmap 을 호출하지 않음으로써 구현할 수 있다. 

persistent map 을 사용하면 매 frame 마다 map, unmap 을 반복하지 않아도 되어 성능상에 이점이 있다.

persistent map 을 사용할 때, 애플리케이션은 GPU가 메모리를 읽거나 쓰는 command list 을 실행하기 전에 CPU 가 메모리에 데이터 쓰기를 완료해야 한다. 일반적인 시나리오에서는 애플리케이션이 ExecuteCommandLists 를 호출하기 전에 메모리에 쓰기만 하면 되지만, fence 를 사용하여 command list 실행을 지연시키는 방법도 작동한다.

단, resource 가 release 되고 난 후에는 Map 에서 반환된 주소를 더 이상 사용하지 않아야 한다. 

> Reference  
> [learn.microsoft - nf-d3d12-id3d12resource-map#advanced-usage-models)]([nf-d3d12-id3d12resource-map#simple-usage-models](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12resource-map#advanced-usage-models))  

