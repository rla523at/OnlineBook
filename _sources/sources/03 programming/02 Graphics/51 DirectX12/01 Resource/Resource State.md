# Resource State
GPU 의 관점에서 현재 Resource 가 어떤 상태인지를 나타내는 enum 이다.

> Reference  
> [learn.microsoft - d3d12_resource_states](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_resource_states)  

## D3D12_RESOURCE_STATE_RENDER_TARGET
ID3D12GraphicsCommandList::ClearRenderTargetView 함수를 호출하기 전에 Resource State 가 D3D12_RESOURCE_STATE_RENDER_TARGET 여야 한다.
* [learn.microsoft - id3d12graphicscommandlist-clearrendertargetview](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-clearrendertargetview#debug-layer)  
  * The debug layer will issue an error if the subresources referenced by the view are not in the appropriate state. For ClearRenderTargetView, the state must be D3D12_RESOURCE_STATE_RENDER_TARGET.

## D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER vs D3D12_RESOURCE_STATE_GENERIC_READ
단순히 Vertex, Constant Buffer 를 위한 Read Only resource 를 만들기 위해서라면 어느 것을 사용해도 기능적으로 큰 차이는 없다.


<details> <summary> <h3 style="display:inline-block"> 두 상태의 의미 </h3></summary>
1. **D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER**  
   말 그대로 “정점 버퍼(vertex buffer) 또는 상수 버퍼(constant buffer)로 GPU가 읽을 준비가 된 상태”를 의미합니다.  
   - 이 상태로 설정해두면, 파이프라인에서 이 리소스를 VSSetConstantBuffers나 IASetVertexBuffers 같은 명령으로 그대로 사용할 수 있습니다.  
   - 다른 읽기 용도(Index Buffer, Shader Resource, Copy Source 등)로 사용하려면 리소스 배리어(transition)를 거쳐 다른 상태로 전환해야 합니다.

2. **D3D12_RESOURCE_STATE_GENERIC_READ**  
   여러 “읽기 전용” 상태를 묶어놓은 상위(슈퍼셋) 상태입니다.  
   - VERTEX_AND_CONSTANT_BUFFER, INDEX_BUFFER, NON_PIXEL_SHADER_RESOURCE, PIXEL_SHADER_RESOURCE, INDIRECT_ARGUMENT, COPY_SOURCE 등의 읽기 용도를 전부 포괄합니다.  
   - 즉, “GPU 쪽에서 읽기 전용으로 사용”하기 위한 대부분의 상황을 한 번에 커버합니다.
</details>


<details> <summary> <h3 style="display:inline-block"> 어떤 힙(Heap)에서 주로 쓰이는가? </h3></summary>
- **업로드 힙(Upload Heap)**:  
  CPU가 데이터를 자주 갱신하는(업로드하는) 용도로 주로 사용합니다. 업로드 힙은 항상 D3D12_HEAP_TYPE_UPLOAD 속성을 갖고, 리소스 상태로 D3D12_RESOURCE_STATE_GENERIC_READ를 사용합니다.  
  - Vertex/Constant 용도로만 쓰더라도 굳이 상태를 VERTEX_AND_CONSTANT_BUFFER로 바꿀 필요가 없습니다.

- **디폴트 힙(Default Heap)**:  
  GPU가 주로 접근(읽고 쓰는)을 담당하는 힙입니다. 여기서는 리소스를 만들 때 초기 상태를 지정한 후, 파이프라인 사용 시점에 따라 적절히 D3D12_RESOURCE_BARRIER로 상태 전환을 해줘야 합니다.  
  - 만약 “오직 정점/상수 버퍼”로만 사용할 예정이라면, 미리 D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER로 세팅하거나, 혹은 필요 시점에 그 상태로 전환해서 사용합니다.  
  - 여러 가지 용도(복사, 쉐이더 리소스, 인덱스 버퍼 등)로 쓰인다면, GENERIC_READ 같은 상위 상태를 사용하거나, 필요에 따라 상태를 바꿔가며 쓸 수 있습니다.
</details>


<details> <summary> <h3 style="display:inline-block"> 실제로는 어떤 방식을 많이 쓰나요? </h3></summary>
- **업로드 힙으로 정점/상수 데이터를 올리는 경우**  
  보통 리소스를 생성할 때 `D3D12_RESOURCE_STATE_GENERIC_READ`로 두고(혹은 생성 시 `D3D12_HEAP_TYPE_UPLOAD`이면 드라이버에서 자동으로 세팅), CPU에서 데이터를 업데이트한 뒤에 GPU에 넘깁니다.  
  - 이 때 GPU에서 쓸 때도 별도 상태 전환 없이 그대로 `VERTEX/CONSTANT BUFFER`로 읽습니다(“GENERIC_READ ⊇ VERTEX_AND_CONSTANT_BUFFER” 관계).  

- **디폴트 힙에 두고, 초기에 업로드 → 이후에 정점/상수만 쓰는 경우**  
  1. 업로드 힙에서 디폴트 힙으로 복사(Copy) 작업을 진행할 때는 `COPY_DEST` 상태로 전환해둡니다.  
  2. 복사가 끝나면, 실제 렌더링에서 필요한 상태(`VERTEX_AND_CONSTANT_BUFFER`)로 전환합니다.  
  3. 이후 계속 정점/상수 버퍼 용도로만 쓸 예정이라면 굳이 다른 상태로 전환할 일은 많지 않습니다.
</details>