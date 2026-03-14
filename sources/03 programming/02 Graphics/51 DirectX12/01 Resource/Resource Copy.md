# Resource Copy

## D3D12_RESOURCE_STATE_GENERIC_READ  
D3D12_RESOURCE_STATE_GENERIC_READ 는 다음 read-only state 들의 bit or 연산자로 정의되어 있다.
* D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER
* D3D12_RESOURCE_STATE_INDEX_BUFFER
* D3D12_RESOURCE_STATE_NON_PIXEL_SHADER_RESOURCE
* D3D12_RESOURCE_STATE_PIXEL_SHADER_RESOURCE
* D3D12_RESOURCE_STATE_INDIRECT_ARGUMENT
* D3D12_RESOURCE_STATE_COPY_SOURCE

따라서, resource state 가 D3D12_RESOURCE_STATE_GENERIC_READ 인 resource 는 별도의 resource transition 없이 copy source 로 사용할 수 있다. 

> Reference  
> [learn.microsoft - d3d12-d3d12_resource_states](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_states)  
> [github.microsoft - directx/d3d12.h](https://github.com/microsoft/DirectX-Headers/blob/main/include/directx/d3d12.h)

## Copy Resource vs Copy Buffer Region

| 특성                          | CopyResource                  | CopyBufferRegion              |
|-------------------------------|-------------------------------|-------------------------------|
| **대상 리소스**               | 모든 리소스 유형              | 버퍼 리소스만                 |
| **복사 범위**                 | 전체 리소스                   | 특정 영역                     |
| **소스/대상 리소스 크기**     | 동일해야 함                   | 다를 수 있음                  |
| **유연성**                    | 낮음 (전체 복사)              | 높음 (부분 복사 가능)         |
| **주요 사용 사례**            | 리소스 전체 복사              | 버퍼의 일부 복사              |

> Reference  
> [learn.microsoft - id3d12graphicscommandlist-copyresource](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-copyresource)  
> [learn.microsoft - id3d12graphicscommandlist-copybufferregion](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-copybufferregion)

CopyResource can be used to initialize resources that alias the same heap memory. 

> Reference  
> [learn.microsoft - id3d12graphicscommandlist-copyresource](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-copyresource)  
> [learn.microsoft - id3d12device-createplacedresource](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12device-createplacedresource)
