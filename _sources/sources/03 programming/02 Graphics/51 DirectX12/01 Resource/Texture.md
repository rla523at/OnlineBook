# Texture

## D3D12_SUBRESOURCE_DATA

RowPitch 멤버 변수는 각 row 가 시작되는 메모리 주소 간격을 바이트 단위로 나타낸 값이다. 

SlicePitch 멤버 변수는 3D Texture 를 사용할 경우, 하나의 슬라이스 (2D Texutre) 에서 다음 슬라이스가 시작되는 메모리 주소 간격을 바이트 단위로 나타내는 값이다. 2D Texture 의 경우에는 Texture 하나. 즉. 전체가 차지하는 바이트 수로 생각할 수 있다.

> Reference  
> [learn.microsoft - d3d12_subresource_data](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_subresource_data)  

## CD3DX12_RESOURCE_DESC::Tex2D

```
Tex2D(
        DXGI_FORMAT format,
        UINT64 width,
        UINT height,
        UINT16 arraySize = 1,
        UINT16 mipLevels = 0,
        UINT sampleCount = 1,
        UINT sampleQuality = 0,
        D3D12_RESOURCE_FLAGS flags = D3D12_RESOURCE_FLAG_NONE,
        D3D12_TEXTURE_LAYOUT layout = D3D12_TEXTURE_LAYOUT_UNKNOWN,
        UINT64 alignment = 0 )
    {
        return CD3DX12_RESOURCE_DESC( D3D12_RESOURCE_DIMENSION_TEXTURE2D, alignment, width, height, arraySize,
            mipLevels, format, sampleCount, sampleQuality, layout, flags );
    }

    CD3DX12_RESOURCE_DESC(
        D3D12_RESOURCE_DIMENSION dimension,
        UINT64 alignment,
        UINT64 width,
        UINT height,
        UINT16 depthOrArraySize,
        UINT16 mipLevels,
        DXGI_FORMAT format,
        UINT sampleCount,
        UINT sampleQuality,
        D3D12_TEXTURE_LAYOUT layout,
        D3D12_RESOURCE_FLAGS flags ) noexcept

```


## Shader Resource View 만들기

<details> <summary> <h3 style="display:inline-block"> D3D12_SHADER_RESOURCE_VIEW_DESC 의 Shader4ComponentMapping 멤버변수 </h3></summary>
Shader4ComponentMapping 은 Texture의 각 (R, G, B, A) 컴포넌트를 셰이더에서 어떤 식으로 매핑해서 사용할지를 지정하는 멤버입니다. 
즉, Texture의 실제 채널(R, G, B, A)을 셰이더에서 R에 가져올지, G에 가져올지, 혹은 1.0이나 0.0 같은 상수로 채울지 등을 지정할 수 있습니다.

예를 들어, R -> R, G -> G, B -> B, A -> A 와 같이 기본적인 Mapping 을 할 경우에는 D3D12_DEFAULT_SHADER_4_COMPONENT_MAPPING 매크로를 인자로 주면 된다.

만약 텍스쳐의 (R,G,B,A) 를 Shader 에서 (B,G,R,1.0) 으로 보게 하고 싶다면 D3D12_ENCODE_SHADER_4_COMPONENT_MAPPING 매크로로 다음과 같이 스위즐을 설정할 수 있다.
```cpp
D3D12_ENCODE_SHADER_4_COMPONENT_MAPPING(
    D3D12_SHADER_COMPONENT_MAPPING_FROM_MEMORY_COMPONENT_2, // R <- 텍스처 B
    D3D12_SHADER_COMPONENT_MAPPING_FROM_MEMORY_COMPONENT_1, // G <- 텍스처 G
    D3D12_SHADER_COMPONENT_MAPPING_FROM_MEMORY_COMPONENT_0, // B <- 텍스처 R
    D3D12_SHADER_COMPONENT_MAPPING_FORCE_VALUE_1            // A <- 1.0
);
```

> Reference   
> [learn.microsoft - d3d12_shader_component_mapping](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_shader_component_mapping)    
> [learn.microsoft - d3d12_shader_resource_view_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_shader_resource_view_desc)  
</details>
