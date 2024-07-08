# Semantic
## 임의의 Semantic
Semantic은 미리 정의된 세맨틱으로, 시스템에 의해 특정한 의미를 가지는 값들이다.

이런 Sementic의 경우 SV_로 시작하는 semantic을 갖는다.

그 외에는 개발자가 원하는 대로 semantic을 정의할 수 있다.

예를 들어, vertex shader에서 Input 구조체를 다음과 같이 정의할 수 있다.
```
struct VS_Input
{
    float3 pos : POSITION;
    float3 normal : NORMAL;
    bool is_end : IS_END;
};
```

그러면 이 semantic에 맞춰 C++코드상에서 D3D11_INPUT_ELEMENT_DESC를 만들어서 input layout을 만들면 된다.
```cpp
  D3D11_INPUT_ELEMENT_DESC pos_desc = {};
  pos_desc.SemanticName             = "POSITION";
  //...

  D3D11_INPUT_ELEMENT_DESC normal_desc = {};
  normal_desc.SemanticName             = "NORMAL";
  //...

  D3D11_INPUT_ELEMENT_DESC is_end_desc = {};
  is_end_desc.SemanticName             = "IS_END";
  //...

  std::vector<D3D11_INPUT_ELEMENT_DESC> input_element_descs = {pos_desc, normal_desc, is_end_desc};
  //...

  cptr_device->CreateInputLayout(//...
```

> Reference  
> [learn.microsoft - semantics](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics)

## SV_VertexID
SV_VertexID는 그래픽 파이프라인의 각 정점(Vertex)을 고유하게 식별하는 데 사용되는 `시스템 값 세맨틱(System Value Semantic)`이다. 

SV_VertexID는 주로 정점 셰이더(Vertex Shader)에서 사용되며, 각 정점의 고유한 인덱스를 제공한다.

vertex shader에서 input으로 받을 수 있다.

```
VS_Output main(VS_Input input, uint vertex_id : SV_VertexID)
{//...}
```

SV_VertexID를 다른 shader로 전달하기 위해서는 vertex shader output에 추가해줘야 한다.

단, semantics를 그대로 사용할 수는 없고 임의의 Semantic을 사용해야 한다.

왜냐하면 [learn.microsoft - semantics](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics)에 따르면 `Available as the input to the vertex shader only.`라고 되어 있기 때문이다.

```
struct VS_Output
{
  float4 pos : SV_Position;
  uint vertex_id : ID;
};

struct GS_Input
{
  float4 pos : SV_Position;
  uint vertex_id : ID;
};
```

## SV_PrimitiveID
SV_PrimitiveID는 HLSL(High-Level Shader Language)에서 주로 지오메트리 셰이더(Geometry Shader)에서 사용되는 시스템 값 세맨틱(System-Value Semantic)이다. 

이 값은 현재 처리 중인 기본 도형(primitive)의 인덱스를 나타낸다.

geometry shader나 pixel shader에서 input으로 받을 수 있다.

```
[maxvertexcount(3)]
void main(triangle GS_Input input[3], inout TriangleStream<GS_Output> outputStream, uint primID : SV_PrimitiveID)

float4 main(PS_Input input, uint primitive_id : SV_PrimitiveID) : SV_Target
```

그러나 geometry shader가 있는 경우 pixel shader에서 바로 input으로 받지는 못하고 SV_PrimitiveID를 geometry shader output에 추가해줘야 한다.

여기서는 SV_VertexID와 다르게 SV_PrimitiveID semantics를 바로 사용할 수 있으며 [learn.microsoft - semantics](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics)에 따르면 `Can be written to by the geometry or pixel shaders, and read by the geometry, pixel, hull or domain shaders.`라고 되어 있기 때문이다.

```
struct GS_Output
{
  float4 pos : SV_POSITION;
  uint primitive_id : SV_PrimitiveID;
};

struct PS_Input
{
  float4 pos : SV_POSITION;
  uint primitive_id : SV_PrimitiveID;
};
```

만약 아래와 같이 geometry shader output과 pixel shader input을 구성할 경우 오류가 발생한다.

```
struct GS_Output
{
  float4 pos : SV_POSITION;
};

struct PS_Input
{
  float4 pos : SV_POSITION;
};

float4 main(PS_Input input, uint primitive_id : SV_PrimitiveID) : SV_Target
{//...}

D3D11 ERROR: ID3D11DeviceContext::Draw: Geometry Shader - Pixel Shader linkage error: Signatures between stages are incompatible. The input stage requires Semantic/Index (SV_PrimitiveID,0) as input, but it is not provided by the output stage. [ EXECUTION ERROR #342: DEVICE_SHADER_LINKAGE_SEMANTICNAME_NOT_FOUND]
```

