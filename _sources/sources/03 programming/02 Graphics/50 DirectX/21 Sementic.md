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

SV_VertexID는 주로 정점 셰이더(Vertex Shader)에서 사용되며, 각 정점의 인덱스를 제공한다.