# Descriptors
Descriptor 는 GPU memory 에 저장되어 있는 resource 를 설명하는 작은 데이터 블록으로 D3D12 binding 의 기본 단위이다.

Descriptor 는 GPU memory 에 저장되어 있는 resource 의 설명이지만 Descriptor 자체는 CPU memory 에 저장된다.

Descriptor 의 종류에는 다음이 있다.
* render target views (RTVs)
* depth stencil views (DSVs)
* shader resource views (SRVs)
* unordered access views (UAVs)
* constant buffer views (CBVs)
* samplers

> Reference  
> [microsoft.github - ResourceBinding.html#descriptors](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html#descriptors)  
> [learn.microsoft - descriptors-overview](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptors-overview)  

<details> <summary> <h3 style="display:inline-block"> ID3D12DescriptorHeap 인터페이스 </h3></summary>
</details>


## Descriptor Heaps
Descriptor Heap 은 Descriptor 들을 저장하기 위한 CPU memory 풀(pool) 이다.

D3D12_DESCRIPTOR_HEAP_FLAG_SHADER_VISIBLE 플래그를 사용하여 생성할 경우 GPU 가 접근 가능한 메모리 영역에 배치(GPU 에 mapping)되어 shader 에서 descriptor 에 접근할 수 있다.

Descriptor heap 의 주요 목적은 GPU 가 사용할 리소스에 빠르게 접근할 수 있도록, 해당 리소스에 대한 정보를 한 곳에 모아 두는 것이다. 

> Reference      
> [learn.microsoft - descriptor-heaps-overview](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptor-heaps-overview)  


<details> <summary> <h3 style="display:inline-block"> ID3D12DescriptorHeap 인터페이스 </h3></summary>

Descriptor Heap 을 관리하는 객체로 [ID3D12Device::CreateDescriptorHeap](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createdescriptorheap) 함수로 생성할 수 있다.
  
이 때, ID3D12Device::CreateDescriptorHeap 함수의 인자로 사용되는 [D3D12_DESCRIPTOR_HEAP_DESC](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/ns-d3d12-d3d12_descriptor_heap_desc) 구조체는 ID3D12DescriptorHeap 객체의 특성을 정의하는 데 사용된다. 

ID3D12DescriptorHeap 의 NumDescriptors 멤버 변수의 최대 값은 시스템의 GPU 및 드라이버에 따라 달라질 수 있다.
* [learn.microsoft - hardware-support](https://learn.microsoft.com/en-us/windows/win32/direct3d12/hardware-support)

enum D3D12_DESCRIPTOR_HEAP_FLAG 의 D3D12_DESCRIPTOR_HEAP_FLAG_NONE 는 CPU 에서만 descriptor heap 에 접근하는 경우 사용한다.  D3D12_DESCRIPTOR_HEAP_FLAG_SHADER_VISIBLE flag 는 CPU 와 GPU 에서 descriptor heap 에 접근하는 경우 사용한다. 이런 Descriptor heap 이 내부적으로 어떻게 작동하는지는 구현 세부 사항이지만, CPU 에 보이는 GPU 메모리의 특수 영역인 Base Address Register (BAR) 에 저장하고 CreateConstantBufferView 호출은 PCI Express 버스를 통해 데이터를 쓰는 구현을 상상해 볼 수 있다.
* [learn.microsoft - D3D12_DESCRIPTOR_HEAP_FLAGS](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_descriptor_heap_flags)
* [asawicki.info - direct3d_12_long_way_to_access_data](https://asawicki.info/news_1754_direct3d_12_long_way_to_access_data) 

> Reference  
> [learn.microsoft - d3d12_descriptor_heap_type](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/ne-d3d12-d3d12_descriptor_heap_type)

</details>

descriptor heap 이 필요한 경우
* descriptor 를 만들 때, dest descirptor heap handle 이 필요하다
* setDescriptorHeap 을 할 때, 필요하다.
* SetGraphicsRootDescriptorTable 을 할 떄, 필요하다.

한번의 Draw Call 에 필요한 CBV, SRV, UAV Descriptor 는 반드시 동일한 Descirptor Heap 에 존재해야 한다. 왜냐하면 Command List 에 


## Descriptor handles
Descritpor handle 은 descriptor heap memory 의 고유한 주소를 나타낸다. 그리고 Descriptor handle 은 전체 descriptor heap 에서 고유하다. 다시 말해 여러 descriptor heap 에 존재하는 모든 memory 는 고유한 descriptor handle 을 갖는다.

Descritpor handle 은 Descriptor heap memory 의 고유한 주소를 나타내기 때문에 포인터와 유사한 역할을 하지만 실제 동작은 하드웨어에 따라 구현이 달라지기 때문에 코드레벨에서 직접 조작 및 참조 할 수 없다. 

Descriptor Heap 의 Flag 가 D3D12_DESCRIPTOR_HEAP_FLAG_NONE 인 경우 CPU 에서만 접근 가능한 memory 가 되지만  D3D12_DESCRIPTOR_HEAP_FLAG_SHADER_VISIBLE 인 경우 CPU, GPU 에서 접근 가능한 memory 가 된다.

따라서, Descriptor Heap 이 CPU 와 GPU 모두에서 접근 가능한 memory 가 된다면 descriptor handle 는 동일한 physical address (descriptor heap memory) 를 나타내지만 CPU 와 GPU 에서 서로 다른 virtual adress 를 갖을 수 있게 된다. 이를 위해 descriptor heap 은 CPU 상에 virtual address 를 나타내는 CPU descriptor handle 과 GPU 상에 virtual adrress 를 나타내는 GPU descriptor handle 이 존재한다.

Descriptor heap 의 시작 부분에 대한 handle 을 얻기 위해서는 각 각 ID3D12DescriptorHeap::GetCPUDescriptorHandleForHeapStart 와 ID3D12DescriptorHeap::GetGPUDescriptorHandleForHeapStart 함수를 사용하면 되며 임의의 위치의 descriptor 의 handle 을 얻기 위해서는 시작위치에서 특정 offset 을 더하여 handle 의 위치를 계산해야 한다. 이 때, descriptor 의 크기는 하드웨어에 따라 다르기 때문에, ID3D12Device::GetDescriptorHandleIncrementSize 를 호출해간격을 얻어내고 descriptor 개수만큼 곱해서 필요한 offset 을 구해낼 수 있다.

(예제코드 넣기)

참고로 CPU 에서 descriptor heap 에 접근할 때는 descriptor 생성과 같은 쓰기 작업을 위해 접근하는 반면 GPU 에서는 descriptor 의 정보를 참조하기 위한 읽기 작업을 위해 접근하게 된다. 따라서, descritptor 생성과 같은 함수를 호출할 때는 CPU descriptor handle 을 넘겨줘야하며 SetGraphicsRootDescriptorTable 과 같이 GPU 에서 참조하기 위한 함수를 호출할 때는 GPU descriptor handle 을 넘겨줘야 한다.

> Reference   
> [learn.microsoft - descriptors-overview#descriptor-handles](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptors-overview#descriptor-handles)   
> [learn.microsoft - creating-descriptor-heaps#descriptor-handles](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-descriptor-heaps#descriptor-handles)  
> [learn.microsoft - d3d12_cpu_descriptor_handle](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/ns-d3d12-d3d12_cpu_descriptor_handle)  
> [learn.microsoft - nf-d3d12-id3d12descriptorheap-getcpudescriptorhandleforheapstart)](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12descriptorheap-getcpudescriptorhandleforheapstart)  
> [gamedev - simple-question-about-heaps](https://www.gamedev.net/forums/topic/702866-simple-question-about-heaps/)  


<details> <summary> <h3 style="display:inline-block"> CD3DX12_CPU_DESCRIPTOR_HANDLE 구조체 </h3></summary>
D3D12_CPU_DESCRIPTOR_HANDLE 구조체를 쉽게 초기화 할 수 있도록 하는 Helper 구조체이다.

`Offset(INT offsetInDescriptors, UINT descriptorIncrementSize)` 함수는 현재 Handle 이 가르키고 있는 Descriptor 를 기준으로 offsetInDescriptors 개 뒤에 Descriptor 가 저장될 위치를 가르키는 Handle 이 된다. 예를 들어 다음 코드는 rtvHandle 이 다음번 render taget view descriptor 가 저장될 위치를 가르키는 handle 이 된다.
```cpp
rtvHandle.Offset(1, m_rtvDescriptorSize);
```

> Reference  
> [learn.microsoft - cd3dx12-cpu-descriptor-handle](https://learn.microsoft.com/ko-kr/windows/win32/direct3d12/cd3dx12-cpu-descriptor-handle)  
</details>

## GPU 에서 Descriptor Table 참조하기
1. ID3D12GraphicsCommandList::SetDescriptorHeap 함수를 통해 GPU 가 사용할 Descriptor Heap 을 설정한다.
2. ID3D12GraphicsCommandList::SetGraphicsRootDescriptorTable 함수를 통해서 Descriptor Table 의 시작주소에 해당하는 D3D12_GPU_DESCRIPTOR_HANDLE 을 등록한다. 



## Descriptor Tables
Descriptor Tables 은 descriptor heap에 있는 descriptor들의 subset 을 나타낸다.

Descriptor Tables 은 shader 에서 descriptor 가 나타내는 resource 중 특정 resource 집합을 참조하기 위해 사용된다.

각 Descriptor Table 은 하나 이상의 유형(SRV, UAV, CBV, 샘플러)의 Descriptor 를 저장하며 Descriptor Tables 은 메모리를 할당하는 것이 아니라 단순히 Descriptor Heap 에 오프셋과 길이를 지정하는 것이다.

따라서 Descriptor Table 은 API 수준에서 생성하거나 삭제할 필요가 없으며, 참조될 때마다 Descriptor heap 에서 오프셋과 크기로 식별된다.

그래픽 파이프라인은 root signature 을 통해 descriptor table 을 참조하여 resource 에 액세스한다.

Command List 에 Descriptor Table 을 설정한다는 것은, Root Signature 에 정의된 특정 Descriptor Table 에 실제로 사용할 자원의 위치를 바인딩하는 작업을 의미한다. 이 작업은 GPU가 실행 시점에 자원에 접근할 수 있도록 하는 중요한 과정이다.

D3D12 에서는 shader 가 resource 에 접근하려면, Root Signature 를 통해 구조를 정의하고, 이후 Command List 에서 실제 resource 의 위치를 연결해야 한다. Descriptor Table 을 Command List 에 설정하는 것은, shader 가 Root Signature 에 정의된 resource 에 실제로 접근할 수 있도록 resource 의 위치를 지정하는 과정입니다.

> Reference  
> [learn.microsoft - descriptor-tables](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptor-tables)  

## ID3D12GraphicsCommandList::SetGraphicsRootDescriptorTable 함수
graphics root signature 에 descriptor table 을 설정하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
void SetGraphicsRootDescriptorTable(
  [in] UINT                        RootParameterIndex,
  [in] D3D12_GPU_DESCRIPTOR_HANDLE BaseDescriptor
);
```

인자는 다음과 같다.

- UINT RootParameterIndex
  - 설정할 루트 파라미터의 인덱스다.
  - 이 인덱스는 root signature 에서 descriptor table 이 정의된 위치를 나타낸다.

- D3D12_GPU_DESCRIPTOR_HANDLE BaseDescriptor
  - descriptor table 의 시작 위치를 나타내는 GPU 디스크립터 핸들이다.
  - 이 핸들은 디스크립터 힙 내의 특정 오프셋을 가리킨다.

**주의사항:**

- 이 함수를 호출하기 전에 ID3D12GraphicsCommandList::SetDescriptorHeaps 함수를 사용하여 descriptor heap 을 설정해야 한다.
  - 이는 descriptor table 이 참조할 descriptor heap 을 지정하기 위함이다. 
- descriptor table 은 root signature 에서 정의된 descriptor 범위와 일치해야 한다. 즉, rootsignature 에서 정의된 descriptor 범위의 수와 순서가 descriptor table 과 일치해야 한다.

> Reference  
> [learn.microsoft - id3d12graphicscommandlist-setgraphicsrootdescriptortable](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-setgraphicsrootdescriptortable)  

## ID3D12Device::CreateConstantBufferView 함수
Constant buffer view 타입의 descriptor 를 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
void CreateConstantBufferView(
  [in, optional] const D3D12_CONSTANT_BUFFER_VIEW_DESC *pDesc,
  [in]           D3D12_CPU_DESCRIPTOR_HANDLE           DestDescriptor
);
```

인자는 다음과 같다.

- const D3D12_CONSTANT_BUFFER_VIEW_DESC *pDesc
  - 생성할 상수 버퍼 뷰의 속성을 지정하는 구조체에 대한 포인터다.
  - 이 구조체는 상수 버퍼의 GPU 가상 주소와 크기를 정의한다.
  - 만약 이 값을 nullptr로 설정하면, 상수 버퍼 뷰가 기본값으로 초기화된다.

- D3D12_CPU_DESCRIPTOR_HANDLE DestDescriptor
  - 생성된 constant buffer view 를 저장할 CPU descriptor heap 의 handle 이다.
  - 이 handle 은 descriptor heap 내의 특정 위치를 가리킨다.

반환값은 없다.

> Reference  
> [learn.microsoft - nf-d3d12-id3d12device-createconstantbufferview](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createconstantbufferview)


## D3D12_CONSTANT_BUFFER_VIEW_DESC 구조체
constant buffer view 를 설명하기 위해 사용된다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_CONSTANT_BUFFER_VIEW_DESC {
  D3D12_GPU_VIRTUAL_ADDRESS BufferLocation;
  UINT                      SizeInBytes;
} D3D12_CONSTANT_BUFFER_VIEW_DESC;
```

각 멤버 변수는 다음과 같다.

* D3D12_GPU_VIRTUAL_ADDRESS BufferLocation  
  * 상수 버퍼의 시작 위치를 지정하는 GPU 가상 주소이다.
  * D3D12_GPU_VIRTUAL_ADDRESS 는 UINT64 의 alias 이다.

* UINT SizeInBytes  
  * 상수 버퍼의 크기를 바이트 단위로 지정한다.
  * 256바이트의 배수로 지정해야 한다.
   * [여기](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-descriptors#constant-buffer-view) 의 예제코드를 보면 주석으로 256 바이트 alignment 가 필요함을 알 수 있다.
   * d3d12.h 에 보면 D3D12_CONSTANT_BUFFER_DATA_PLACEMENT_ALIGNMENT 가 256 으로 정의되어 있는걸 알 수 있다.
    * [참고 문서](https://learn.microsoft.com/ko-kr/windows/win32/direct3d12/constants)  
   * d3d12_xs.h 에는 D3D12_CONSTANT_BUFFER_DATA_PLACEMENT_ALIGNMENT 가 16 으로 정의되어 있는걸 알 수 있다.
 
> Reference  
> [learn.microsoft - ns-d3d12-d3d12_constant_buffer_view_desc](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/ns-d3d12-d3d12_constant_buffer_view_desc)  

## ID3D12Device::CreateShaderResourceView 함수
Shader resource view descripotr 를 생성하는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
void CreateShaderResourceView(
  [in, optional] ID3D12Resource                        *pResource,
  [in, optional] const D3D12_SHADER_RESOURCE_VIEW_DESC *pDesc,
  [in]           D3D12_CPU_DESCRIPTOR_HANDLE           DestDescriptor
);
```
인자는 다음과 같다.

- ID3D12Resource *pResource
  - 셰이더의 입력으로 사용할 리소스에 대한 포인터다.
  - 이 리소스는 D3D12_RESOURCE_FLAG_DENY_SHADER_RESOURCE 플래그가 설정되지 않아야 한다.
  - 만약 이 값을 nullptr로 설정하면, 기본값으로 초기화된 널(null) 디스크립터가 생성된다.

- const D3D12_SHADER_RESOURCE_VIEW_DESC *pDesc
  - 셰이더 리소스 뷰를 설명하는 D3D12_SHADER_RESOURCE_VIEW_DESC 구조체에 대한 포인터다.
  - 이 값을 nullptr로 설정하면, 리소스의 형식과 차원에 따라 기본값이 사용된다. 

- D3D12_CPU_DESCRIPTOR_HANDLE DestDescriptor
  - 생성된 셰이더 리소스 뷰를 저장할 CPU 디스크립터 힙의 핸들이다.
  - 이 핸들은 디스크립터 힙 내의 특정 위치를 가리킨다.

**주의사항:**

- 셰이더 리소스 뷰를 생성하기 전에, 해당 리소스를 위한 디스크립터 힙을 생성해야 한다.
  - 이는 셰이더 리소스 뷰가 디스크립터 힙 내에 저장되기 때문이다. 

- 셰이더 리소스 뷰를 생성한 후, 이를 루트 서명에 바인딩하여 셰이더에서 사용할 수 있도록 해야 한다.
  - 이는 셰이더가 리소스의 데이터를 올바르게 액세스하기 위함이다. 

- pResource 로 주어진 resource 의 형식과 pDesc 에 작성된 view 의 형식이 호환되어야 한다.
  - 예를 들어, 리소스가 DXGI_FORMAT_R32G32B32A32_TYPELESS 형식으로 생성되었다면, 뷰는 DXGI_FORMAT_R32G32B32A32_FLOAT 또는 DXGI_FORMAT_R32G32B32A32_UINT와 같은 구체적인 형식으로 지정해야 한다.

- D3D12 에서는 descriptor heap 이 CPU 전용과 GPU 가시성(Shader-Visible)으로 나뉜다.
  - shader 에서 resource 를 액세스하려면, shader visible descriptor heap 에 shader resource view 를 생성해야 한다.

> Reference  
> [learn.microsoft - id3d12device-createshaderresourceview](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-id3d12device-createshaderresourceview)  

## ID3D12Device::CreateConstantBufferView vs ID3D12Device::CreateShaderResourceView
ID3D12Device::CreateShaderResourceView 는 SRV 를 생성할 때 ID3D12Resource 인자를 필요로 하지만, ID3D12Device::CreateConstantBufferView는 CBV 를 생성할 때 ID3D12Resource 인자를 받지 않는다. 이는 SRV 와 CBV 의 구조적 차이와 사용 방식의 차이에서 비롯된다

SRV 는 텍스처, 버퍼, 구조화된 버퍼 등 다양한 형태의 자원을 가르킬 수 있다. 따라서 CreateShaderResourceView 는 resource 에 대한 세부 정보가 필요하기 때문에 SRV 가 참조할 자원을 정확하게 지정해줘야 한다.

반면 CBV는 상수 버퍼, 즉 256바이트 정렬 된 데이터를 가르킨다. 따라서 CreateConstantBufferView 는 resource 에 대한 시작 주소와 크기만 알면 되며, 추가적인 형식 정보나 차원 정보가 필요하지 않아 D3D12_CONSTANT_BUFFER_VIEW_DESC 구조체에서 상수 버퍼의 GPU 메모리 주소와 크기만을 요구하는 형태로 되어 있다.


## Null Descriptor

Null Descriptor가 필요한 이유
* 명시적 리소스 바인딩
  * D3D12에서 디스크립터 테이블은 ‘슬롯’ 수가 고정되어 있고, 이를 하드웨어에서 참조할 때 빈 상태로 두면 GPU 충돌이나 TDR(Timeout Detection & Recovery) 위험이 있다
  * 즉, 사용하지 않는다고 해서 디스크립터를 생략할 수 없고, 반드시 어떤 형태로든 ‘유효한 디스크립터’여야 합니다.
  * 이때 “유효하지만 실제 리소스는 없는” 상태를 나타내는 것이 바로 Null Descriptor입니다.
* 안전한 접근 보장
  * 셰이더 단계에서 어떤 리소스가 필요하지 않은 상황이라도, 잘못된 주소를 참조하거나 GPU 크래시를 일으키지 않도록 예방해야 합니다.
  * D3D12에서는 셰이더가 해당 슬롯을 읽더라도 의미 없는 값(일반적으로 0, 투명, 블랙 등)만 반환되도록 “Null Descriptor”를 바인딩해 둘 수 있습니다.
  * 이를 통해 잘못된 리소스를 바인딩하여 발생할 수 있는 GPU 액세스 위반, TDR(Timeout Detection & Recovery) 등을 예방하는 역할을 합니다

Null Descritpor 가 필요한 상황 예시
* 예를 들어, 게임 엔진에서 여러 종류의 머티리얼(매테리얼)들을 처리하기 위해 공통 루트 시그니처를 만들었다고 가정해 봅시다.
* 디자인 목표: 모든 머티리얼이 ‘최대 8장의 텍스처(SRV)’를 사용할 수 있도록 통일된 인터페이스 제공
* 구현 방법:
  * 루트 시그니처에 Descriptor Table을 하나 두고, 그 범위(Range)를 “SRV 8개”로 고정해둔다.
  * 셰이더 코드에서도 Texture2D gTexArray[8] : register(t0, space0); 처럼 선언해둔다.
  * 즉, 어떤 머티리얼이든 최대 8개까지 텍스처를 바인딩할 수 있는 구조로 만들어 둔 것입니다.
* 머티리얼 A가 베이스 컬러 텍스처(t0)와 노멀 맵(t1) 2개만 사용하는 경우
  * D3D12에서는 머티리얼별로 “SRV 디스크립터 힙에 실제 리소스를 작성”하여, 루트 시그니처의 Descriptor Table로 전달해야 합니다.
  * “8개의 슬롯”이라는 개수는 고정이므로, 머티리얼 A의 경우 실제로 2개만 필요하지만 나머지 6개 슬롯도 Null Descriptor로 채워 넣어야 합니다.
```cpp
// 1) 디스크립터 힙에 SRV를 만들 위치(8슬롯) 확보
D3D12_CPU_DESCRIPTOR_HANDLE baseCpuHandle = srvHeap->GetCPUDescriptorHandleForHeapStart();
UINT descriptorSize = device->GetDescriptorHandleIncrementSize(D3D12_DESCRIPTOR_HEAP_TYPE_CBV_SRV_UAV);

// 2) t0, t1용 실제 SRV 생성
D3D12_CPU_DESCRIPTOR_HANDLE slot0Handle = baseCpuHandle;
device->CreateShaderResourceView(gTexBaseColorResource, &srvDescBaseColor, slot0Handle);

D3D12_CPU_DESCRIPTOR_HANDLE slot1Handle = { slot0Handle.ptr + descriptorSize };
device->CreateShaderResourceView(gTexNormalMapResource, &srvDescNormal, slot1Handle);

// 3) t2 ~ t7 슬롯은 Null Descriptor 생성
D3D12_CPU_DESCRIPTOR_HANDLE nullSlotHandle = { slot1Handle.ptr + descriptorSize };

D3D12_SHADER_RESOURCE_VIEW_DESC nullSrvDesc = {};
nullSrvDesc.ViewDimension = D3D12_SRV_DIMENSION_TEXTURE2D;
nullSrvDesc.Format = DXGI_FORMAT_UNKNOWN;
// MipLevels, MostDetailedMip 등을 0으로 설정
for (int slotIndex = 2; slotIndex < 8; ++slotIndex)
{
    device->CreateShaderResourceView(nullptr, &nullSrvDesc, nullSlotHandle);
    nullSlotHandle.ptr += descriptorSize;
}
```



* In many cases, there is a defined behavior for accessing an unbound resource, such as SRVs which return default values. Those will be honored when accessing a NULL descriptor as long as the type of shader access is compatible with the descriptor type.
  * Shader 에서 Null Descriptor 로 접근하면 default value 가 return 된다.

> Reference  
> [learn.microsoft - descriptors-overview#null-descriptors](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptors-overview#null-descriptors)  


