# Descriptors

Descriptors 는 GPU 전용 포맷으로 GPU 에 오브젝트를 완전히 설명하는 작은 데이터 블록으로 D3D12 binding 의 기본 단위이다.

Descriptors 에는 다음과 같은 다양한 type 이 있다.
* render target views (RTVs)
* depth stencil views (DSVs)
* shader resource views (SRVs)
* unordered access views (UAVs)
* constant buffer views (CBVs), and samplers

드라이버는 descriptor 에 대한 참조를 추적하거나 보유하지 않으며, 올바른 descriptor type 이 사용되고 있는지, 정보가 최신 상태인지 확인하는 것은 앱의 책임이다.

단, 한가지 예외로 드라이버는 swap chain 이 올바르게 작동하는지 확인하기 위해 RTV 의 바인딩을 검사한다.

descriptor 를 사용하는 기본 방법은 descriptor 를 위한 보조 메모리인 descriptor heap 에 descriptor 를 배치하는 것이다.

그리고 이 경우에는 반드시 descriptor table 을 정의해야 한다. descriptor table 은 파이프라인에 대한 descriptor heap 의 범위를 식별하여 사용할 descriptor 를 찾기 위해 어디를 찾아야 하는지 알 수 있도록 힌다.

> Reference  
> [microsoft.github - ResourceBinding.html#descriptors](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html#descriptors)  
> [learn.microsoft - descriptors-overview](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptors-overview)  

## Descriptor handles
Descritpor handle 은 Descriptor 고유의 주소이다.

포인터와 비슷하지만 하드웨어에 따라 구현이 달라지기 때문에 코드레벨에서 조작 및 참조 할 수 없다.

그리고 Descriptor handle 은 전체 descriptor heap 에서 고유하다. 다시 말해 여러 descriptor heap 에 정의된 모든 descriptor 는 고유한 descriptor handle 을 갖는다.

> Reference  
> [learn.microsoft - descriptors-overview#descriptor-handles](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptors-overview#descriptor-handles)   

## Descriptor Heaps
Descriptor Heap 은 Descriptor 들을 저장하기 위해 연속적으로 할당된 메모리 풀(pool) 이다.

Descriptor heap 의 주요 목적은 GPU 가 사용할 리소스에 빠르게 접근할 수 있도록, 해당 리소스에 대한 정보를 한 곳에 모아 두는 것이다. 

> Reference  
> [learn.microsoft - descriptor-heaps-overview](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptor-heaps-overview)  

## Descriptor Tables
Descriptor Tables 는 Descriptor 의 배열이다.

> Reference  
> [learn.microsoft - descriptor-tables](https://learn.microsoft.com/en-us/windows/win32/direct3d12/descriptor-tables)  

