# Buffer

## Buffer Alignment
Buffer alignment restrictions:

* [learn.microsoft - buffer-alignment](https://learn.microsoft.com/en-us/windows/win32/direct3d12/upload-and-readback-of-texture-data#buffer-alignment)
  * Linear subresource copying must be aligned to 512 bytes (with the row pitch aligned to D3D12_TEXTURE_DATA_PITCH_ALIGNMENT bytes).
  * **Constant data** reads must be a multiple of 256 bytes from the beginning of the heap (i.e. only from addresses that are 256-byte aligned).
  * Index data reads must be a multiple of the index data type size (i.e. only from addresses that are naturally aligned for the data).
  * ID3D12GraphicsCommandList::ExecuteIndirect data must be from offsets that are multiples of 4 (i.e. only from addresses that are DWORD aligned).

  

## Shader Resource View 만들기

<details> <summary> <h3 style="display:inline-block"> D3D12_SHADER_RESOURCE_VIEW_DESC 의 Shader4ComponentMapping 멤버변수 </h3></summary>
Buffer resource 에 대한 SRV 를 생성할 때는 특별히 스위즐을 조작할 일이 없기 때문에 srvDesc.Shader4ComponentMapping = D3D12_DEFAULT_SHADER_4_COMPONENT_MAPPING 로 설정한다.

필수 필드이기 때문에(D3D12에서 SRV 생성 시 반드시 채워야 하는 값), “쓰지 않는다”거나 “무시”가 아니라 “그냥 기본 매핑”을 넣어주면 됩니다.

> Reference   
> [learn.microsoft - d3d12_shader_component_mapping](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_shader_component_mapping)    
> [learn.microsoft - d3d12_shader_resource_view_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_shader_resource_view_desc)  
</details>