# Resource State

## D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER vs D3D12_RESOURCE_STATE_GENERIC_READ
먼저 결론부터 말씀드리면, **동일한 용도로만(즉 단순히 정점/상수 버퍼로만 읽기) 사용한다면 `D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER`나 `D3D12_RESOURCE_STATE_GENERIC_READ` 중 어느 것을 사용해도 기능적으로 큰 차이는 없습니다.**  
다만 상황(힙 종류나 다른 용도로 함께 쓸 가능성 등)에 따라 권장되는 설정이 달라질 수 있습니다.

---

## 두 상태의 의미

1. **`D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER`**  
   말 그대로 “정점 버퍼(vertex buffer) 또는 상수 버퍼(constant buffer)로 GPU가 읽을 준비가 된 상태”를 의미합니다.  
   - 이 상태로 설정해두면, 파이프라인에서 이 리소스를 `VSSetConstantBuffers`나 `IASetVertexBuffers` 같은 명령으로 그대로 사용할 수 있습니다.  
   - 다른 읽기 용도(Index Buffer, Shader Resource, Copy Source 등)로 사용하려면 리소스 배리어(transition)를 거쳐 다른 상태로 전환해야 합니다.

2. **`D3D12_RESOURCE_STATE_GENERIC_READ`**  
   여러 “읽기 전용” 상태를 묶어놓은 상위(슈퍼셋) 상태입니다.  
   - `VERTEX_AND_CONSTANT_BUFFER`, `INDEX_BUFFER`, `NON_PIXEL_SHADER_RESOURCE`, `PIXEL_SHADER_RESOURCE`, `INDIRECT_ARGUMENT`, `COPY_SOURCE` 등의 읽기 용도를 전부 포괄합니다.  
   - 즉, “GPU 쪽에서 읽기 전용으로 사용”하기 위한 대부분의 상황을 한 번에 커버합니다.

---

## 어떤 힙(Heap)에서 주로 쓰이는가?

- **업로드 힙(Upload Heap)**:  
  CPU가 데이터를 자주 갱신하는(업로드하는) 용도로 주로 사용합니다. 업로드 힙은 항상 `D3D12_HEAP_TYPE_UPLOAD` 속성을 갖고, 리소스 상태로 보통 `D3D12_RESOURCE_STATE_GENERIC_READ`를 사용합니다.  
  - 업로드 힙에 올라간 버퍼는 GPU 입장에서는 읽기 전용으로 사용되고, CPU가 수시로 쓸 수 있도록 맵핑이 되어 있기 때문에, 보통 `GENERIC_READ` 상태로 고정한 뒤 별도의 상태 전환 없이 사용합니다.  
  - Vertex/Constant 용도로만 쓰더라도 업로드 힙에서는 굳이 상태를 `VERTEX_AND_CONSTANT_BUFFER`로 바꿀 필요가 없습니다.

- **디폴트 힙(Default Heap)**:  
  GPU가 주로 접근(읽고 쓰는)을 담당하는 힙입니다. 여기서는 리소스를 만들 때 초기 상태를 지정한 후, 파이프라인 사용 시점에 따라 적절히 `D3D12_RESOURCE_BARRIER`로 상태 전환을 해줘야 합니다.  
  - 만약 “오직 정점/상수 버퍼”로만 사용할 예정이라면, 미리 `D3D12_RESOURCE_STATE_VERTEX_AND_CONSTANT_BUFFER`로 세팅하거나, 혹은 필요 시점에 그 상태로 전환해서 사용합니다.  
  - 여러 가지 용도(복사, 쉐이더 리소스, 인덱스 버퍼 등)로 쓰인다면, `GENERIC_READ` 같은 상위 상태를 사용하거나, 필요에 따라 상태를 바꿔가며 쓸 수 있습니다.

---

## 실제로는 어떤 방식을 많이 쓰나요?

- **업로드 힙으로 정점/상수 데이터를 올리는 경우**  
  보통 리소스를 생성할 때 `D3D12_RESOURCE_STATE_GENERIC_READ`로 두고(혹은 생성 시 `D3D12_HEAP_TYPE_UPLOAD`이면 드라이버에서 자동으로 세팅), CPU에서 데이터를 업데이트한 뒤에 GPU에 넘깁니다.  
  - 이 때 GPU에서 쓸 때도 별도 상태 전환 없이 그대로 `VERTEX/CONSTANT BUFFER`로 읽습니다(“GENERIC_READ ⊇ VERTEX_AND_CONSTANT_BUFFER” 관계).  

- **디폴트 힙에 두고, 초기에 업로드 → 이후에 정점/상수만 쓰는 경우**  
  1. 업로드 힙에서 디폴트 힙으로 복사(Copy) 작업을 진행할 때는 `COPY_DEST` 상태로 전환해둡니다.  
  2. 복사가 끝나면, 실제 렌더링에서 필요한 상태(`VERTEX_AND_CONSTANT_BUFFER`)로 전환합니다.  
  3. 이후 계속 정점/상수 버퍼 용도로만 쓸 예정이라면 굳이 다른 상태로 전환할 일은 많지 않습니다.

- **여러 용도로 함께 사용해야 하는 경우**  
  가령 동일한 버퍼를 **Index Buffer**나 **Shader Resource(StructuredBuffer, ByteAddressBuffer 등)**로도 사용해야 한다면, `D3D12_RESOURCE_STATE_GENERIC_READ` 상태 하나로 여러 용도를 커버할 수 있어 편리합니다(필요 시점에 상태 전환을 자주 하지 않아도 됨).

---

## 정리

1. **사용할 힙**(Upload인지 Default인지)에 따라 기본적으로 쓰이는 상태가 달라집니다.  
2. 버퍼를 어느 용도로만 한정해서 쓰는지, 아니면 다른 용도로도 겸해서 쓰는지에 따라 달라질 수 있습니다.
3. **기능적인 차이**(GPU에서 읽기에 문제 되는지 여부)는 거의 없지만, **최적화(不要한 상태 전환을 줄이는 것)**나 **사용 편의성** 면에서 선택이 달라집니다.

- **업로드 힙(Upload Heap)에 있는 버퍼라면 보통 `GENERIC_READ`로 충분**합니다.  
- **디폴트 힙(Default Heap)에 있는 버퍼를 전용으로 쓴다면 `VERTEX_AND_CONSTANT_BUFFER`**로 두는 것이 명시적이며 상태 전환도 단순합니다.  
- **다른 용도까지 함께 쓸 계획이 있다면** `GENERIC_READ`로 묶어서 한 번에 쓰거나, 필요 시점에 배리어를 통해 정확한 상태로 전환해가며 사용할 수 있습니다.

즉, “딱히 다른 용도로 안 쓰고, 오직 정점/상수 버퍼만 쓴다”면 **둘 중 어느 걸 써도 큰 문제는 없지만**,  
- **Upload Heap**이라면 -> `GENERIC_READ`가 일반적  
- **Default Heap**이라면 -> 상황에 따라 `VERTEX_AND_CONSTANT_BUFFER`로 두고 쓰는 것이 조금 더 명시적  

정도로 이해하시면 됩니다.