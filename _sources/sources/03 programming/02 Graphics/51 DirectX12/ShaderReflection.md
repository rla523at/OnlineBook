# Shader Reflection
shader reflection 은 shader code 의 메타데이터를 반영(reflection)한 정보로 주로 shader 의 입력 및 출력, 리소스 바인딩, 상수 버퍼, 텍스처 및 기타 정보를 명세하는 데 사용된다. 

Reflection 정보는 shader 를 컴파일할 때 생성된다.

> Reference    
> https://rtarun9.github.io/blogs/shader_reflection/  

## Shader Reflection 으로부터 Resource Binding 정보 읽어오기

```cpp
#include "wrl.h"
#include "d3d12shader.h"
#include "dxcapi.h"
#include <vector>

#pragma comment(lib, "dxcompiler.lib")

void read_refelction_file(void)
{
  using Microsoft::WRL::Comptr;

  ComPtr<ID3D12ShaderReflection> cptrShaderReflection = nullptr;
  {
    ComPtr<IDxcUtils> cptrDXCUtils = nullptr;
    {
      DxcCreateInstance(CLSID_DxcUtils, IID_PPV_ARGS(&cptrDXCUtils));
    }

    ComPtr<IDxcBlobEncoding> cptrRefelctionBlob = nullptr;
    {
      cptrDXCUtils->LoadFile(L"", nullptr, &cptrReflectionBlob);
    }

    DxcBuffer refelctionData;
    refelctionData.Ptr = cptrReflectionBlob->GetBufferPointer();
    reflectionData.Size = cptrReflectionBlob->GetBufferSize();
    reflectionData.Encoding = DXC_CP_ACP;

    cptrDXCUtils->CreateReflection(&reflectionData, IID_PPV_ARGS(&cptrShaderReflection));
  }

  int numBindResource = 0;
  {
    D3D12_Shader_DESC shaderDesc = {};
    cptrShaderReflection->GetDesc(&shaderDesc);

    numBindResource = shaderDesc.BoundResources;
  }

  std::vector<D3D12_SHADER_INPUT_BIND_DESC> bindingResourceDescs(numBindResource);
  {
    for(int i=0; i< numBindResource; ++i)
      cptrShaderReflection->GetResourceBindingDesc(i, &bindingResourceDescs[i]);
  }
}
```

```
struct VS_INPUT
{
  float4 position : SV_POSITION;
  float4 color : TEXCOORD0;
}

cbuffer MyVar : register(space1)
{
  matrix projection;
}

cbuffer MyVar2 : register(space1)
{
  matrix projection2;
}

static float sval =1.0;
float gval = 1.1;
float gval2 = 1.1;
matrix gval3;
float4 gval4;

float4 mainVS(VS_INPUT input) : SV_POSITION
{
  matrix m = gval * (projection + projection2);
  float4 out_pos = mul(m, input.position);
  return out_pos;
}
```

static 변수는 binding resource 로 취급되지 않는다.

만약, extern 변수가 있는 경우 "\$globlas" 라는 이름을 갖은 Constant Buffer 가 생기게 되며, extern 변수가 여러개 있는 경우에는 "\$globals" 라는 constant buffer 에 묶여서 0 번 binding resource 가 된다.

그리고 모든 extern 변수가 쓰이지 않으면 최적화에 의해 "\$globals" Constant Buffer 가 안만들어 질 수도 있으며, extern 변수중 하나라도 쓰인다면 나머지 변수들은 안쓰이더라도 "\$globals" constant buffer 가 생기게 된다.

\$globals 의 정보는 다음 코드로 확인해 볼 수 있다.

```cpp

D3D12_SHADER_BUFFER_DESC globalBufferDesc;
ID3D12ShaderReflectionConstantBuffer* bufferPtr = cptrShaderReflection->GetConstantBufferByIndex(0);
bufferPtr->GetDesc(&globalBufferDesc);
```

참고로, float gval 과 같이 별도의 storage class 가 없을 경우 extern 으로 취급된다. [learn.microsoft - variable-syntax](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-variable-syntax)  

> Reference  
> [asawicki.info - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  

## D3D12_SHADER_BUFFER_DESC 구조체

> Reference  
> [learn.microsoft - d3d12_shader_buffer_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12shader/ns-d3d12shader-d3d12_shader_buffer_desc)  

## ID3D12FunctionReflection::GetConstantBufferByIndex 함수

> Reference  
> [learn.microsoft - id3d12functionreflection-getconstantbufferbyindex](https://learn.microsoft.com/en-us/windows/win32/api/d3d12shader/nf-d3d12shader-id3d12functionreflection-getconstantbufferbyindex)  

## ID3D12ShaderReflectionConstantBuffer 인터페이스

> Reference  
> [learn.microsoft - id3d12shaderreflectionconstantbuffer](https://learn.microsoft.com/en-us/windows/win32/api/d3d12shader/nn-d3d12shader-id3d12shaderreflectionconstantbuffer)  

## ID3D12ShaderReflection 인터페이스
ID3D12ShaderReflection 은 shader reflection 을 나타내는 인터페이스다. 

## ID3D12ShaderReflection::GetDesc 함수
D3D12_SHADER_DESC 을 가져오는 함수이다.

```cpp
HRESULT GetDesc(
  [out] D3D12_SHADER_DESC *pDesc
);
```

> Reference   
> [learn.microsoft - id3d12shaderreflection-getdesc](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12shader/nf-d3d12shader-id3d12shaderreflection-getdesc)  

## D3D12_SHADER_DESC 구조체
shader 에 대한 다양한 정보를 제공하는 구조체이다. 

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_SHADER_DESC {
    UINT Version;
    LPCSTR Creator;
    UINT Flags;
    UINT ConstantBuffers;
    UINT BoundResources;
    UINT InputParameters;
    UINT OutputParameters;
    UINT InstructionCount;
    UINT TempRegisterCount;
    UINT TempArrayCount;
    UINT DefCount;
    UINT DclCount;
    UINT TextureNormalInstructions;
    UINT TextureLoadInstructions;
    UINT TextureCompInstructions;
    UINT TextureBiasInstructions;
    UINT TextureGradientInstructions;
    UINT FloatInstructionCount;
    UINT IntInstructionCount;
    UINT UintInstructionCount;
    UINT StaticFlowControlCount;
    UINT DynamicFlowControlCount;
    UINT MacroInstructionCount;
    UINT ArrayInstructionCount;
    UINT CutInstructionCount;
    UINT EmitInstructionCount;
    D3D_PRIMITIVE_TOPOLOGY GSOutputTopology;
    UINT GSMaxOutputVertexCount;
    D3D_PRIMITIVE InputPrimitive;
    UINT PatchConstantParameters;
    UINT cGSInstanceCount;
    UINT cControlPoints;
    UINT HSOutputPrimitive;
    D3D_TESSELLATOR_OUTPUT_PRIMITIVE HSPartitioning;
    D3D_TESSELLATOR_DOMAIN TessellatorDomain;
    UINT cBarrierInstructions;
    UINT cInterlockedInstructions;
    UINT cTextureStoreInstructions;
} D3D12_SHADER_DESC;
```

각 멤버 변수는 다음과 같다.

* Version (UINT):
  * 셰이더의 버전 번호를 나타낸다. 상위 16비트는 주요 버전, 하위 16비트는 부 버전으로 구성된다.

* Creator (LPCSTR):
  * 이 셰이더를 생성한 도구 또는 API의 이름을 나타내는 문자열이다.

* Flags (UINT):
  * 셰이더의 특성을 나타내는 플래그 값이다.

* ConstantBuffers (UINT):
  * 셰이더에서 사용되는 상수 버퍼의 수를 나타낸다.

* BoundResources (UINT):
  * 셰이더에 바인딩된 리소스(텍스처, 샘플러, 상수 버퍼 등)의 수를 나타낸다.

* InputParameters (UINT):
  * 셰이더에 대한 입력 파라미터의 수를 나타낸다.

* OutputParameters (UINT):
  * 셰이더에 대한 출력 파라미터의 수를 나타낸다.

* InstructionCount (UINT):
  * 셰이더에 포함된 명령어의 총 수를 나타낸다.

* TempRegisterCount (UINT):
  * 셰이더가 사용하는 임시 레지스터의 수를 나타낸다.

* TempArrayCount (UINT):
  * 셰이더에서 사용되는 임시 배열의 수를 나타낸다.

* DefCount (UINT):
  * 셰이더에서 정의된 명령어의 수를 나타낸다.

* DclCount (UINT):
  * 셰이더에 선언된 명령어의 수를 나타낸다.

* TextureNormalInstructions (UINT):
  * 셰이더에서 텍스처의 일반 명령어 수를 나타낸다.

* TextureLoadInstructions (UINT):
  * 셰이더에서 텍스처의 로드 명령어 수를 나타낸다.

* TextureCompInstructions (UINT):
  * 셰이더에서 텍스처의 비교 명령어 수를 나타낸다.

* TextureBiasInstructions (UINT):
  * 셰이더에서 텍스처의 바이어스 명령어 수를 나타낸다.

* TextureGradientInstructions (UINT):
  * 셰이더에서 텍스처의 그라디언트 명령어 수를 나타낸다.

* FloatInstructionCount (UINT):
  * 부동 소수점 명령어의 수를 나타낸다.

* IntInstructionCount (UINT):
  * 정수 명령어의 수를 나타낸다.

* UintInstructionCount (UINT):
  * 부호 없는 정수 명령어의 수를 나타낸다.

* StaticFlowControlCount (UINT):
  * 셰이더에서 정적 흐름 제어 명령어의 수를 나타낸다.

* DynamicFlowControlCount (UINT):
  * 셰이더에서 동적 흐름 제어 명령어의 수를 나타낸다.

* MacroInstructionCount (UINT):
  * 매크로 명령어의 수를 나타낸다.

* ArrayInstructionCount (UINT):
  * 배열과 관련된 명령어의 수를 나타낸다.

* CutInstructionCount (UINT):
  * 컷 명령어의 수를 나타낸다 (주로 기하 셰이더에서 사용).

* EmitInstructionCount (UINT):
  * 방출 명령어의 수를 나타낸다 (주로 기하 셰이더에서 사용).

* GSOutputTopology (D3D_PRIMITIVE_TOPOLOGY):
  * 기하 셰이더의 출력 토폴로지를 나타낸다.

* GSMaxOutputVertexCount (UINT):
  * 기하 셰이더에서 생성될 수 있는 최대 출력 정점 수를 나타낸다.

* InputPrimitive (D3D_PRIMITIVE):
  * 셰이더에 전달되는 입력 프리미티브 유형을 나타낸다.

* PatchConstantParameters (UINT):
  * 패치 상수 파라미터의 수를 나타낸다.

* cGSInstanceCount (UINT):
  * 기하 셰이더 인스턴스의 수를 나타낸다.

* cControlPoints (UINT):
  * 제어점을 위한 개수이다.

* HSOutputPrimitive (UINT):
  * Hull 셰이더에서 출력 프리미티브의 개수를 나타낸다.

* HSPartitioning (D3D_TESSELLATOR_OUTPUT_PRIMITIVE):
  * Hull 셰이더의 파티셔닝을 나타낸다.

* HSOutputPrimitive (D3D_TESSELLATOR_DOMAIN):
  * Tessellation 도메인을 나타낸다.

* cBarrierInstructions (UINT):
  * 셰이더의 배리어 명령어 수를 나타낸다.

* cInterlockedInstructions (UINT):
  * 상호 배제 명령어의 수를 나타낸다.

* cTextureStoreInstructions (UINT):
  * 텍스처 저장 명령어의 수를 나타낸다.
 
## ID3D12ShaderReflection::GetResourceBindingDesc 함수
shader 의 리소스 바인딩 정보를 가져오는 함수다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT GetResourceBindingDesc(
  [in]  UINT                         ResourceIndex,
  [out] D3D12_SHADER_INPUT_BIND_DESC *pDesc
);
```

인자는 다음과 같다.

- UINT ResourceIndex
  - 가져올 리소스 바인딩의 인덱스다.
  - 이 인덱스는 리소스 바인딩이 정의된 순서에 따라 결정된다.

- D3D12_RESOURCE_BINDING_DESC *pDesc
  - 리소스 바인딩의 정보를 저장할 구조체에 대한 포인터다. 

> Reference  
> [learn.microsoft - id3d12functionreflection-getresourcebindingdesc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12shader/nf-d3d12shader-id3d12functionreflection-getresourcebindingdesc)

## D3D12_SHADER_INPUT_BIND_DESC 구조체
shader 가 사용하는 리소스의 바인딩 정보를 설명하는 데 사용되는 구조체이다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_SHADER_INPUT_BIND_DESC {
    LPCSTR Name;
    D3D_SHADER_INPUT_TYPE Type;
    UINT BindPoint;
    UINT BindCount;
    UINT uFlags;
    D3D_RESOURCE_RETURN_TYPE ReturnType;
    D3D_SRV_DIMENSION Dimension;
    UINT NumSamples;
    UINT Space;
    UINT uID;
} D3D12_SHADER_INPUT_BIND_DESC;
```

각 멤버 변수는 다음과 같다.

* Name (LPCSTR):
  * 셰이더에서 리소스를 선언할 때 사용된 이름을 나타내는 문자열이다.
  * 예를 들어 HLSL 코드에서 Texture2D MyTexture;로 선언했다면, Name은 "MyTexture"가 된다.
  * 만약 HLSL 코드에서 extern 으로 간주되는 변수의 경우 Name 은 "$Globals" 가 된다.
      * HLSL 에 float globalVariable; 으로 되어 있으면 기본적으로 extern 으로 간주된다.

* Type (D3D_SHADER_INPUT_TYPE):
  * 셰이더에서 사용되는 리소스의 종류를 나타낸다.
  * D3D_SHADER_INPUT_TYPE은 이 열거형 값이며, 예를 들어 D3D_SIT_CBUFFER는 상수 버퍼(Constant Buffer), D3D_SIT_TBUFFER는 텍스처 버퍼(Texture Buffer), D3D_SIT_TEXTURE는 일반 텍스처(Texture), D3D_SIT_SAMPLER는 샘플러(Sampler)를 의미한다.

* BindPoint (UINT):
  * 셰이더 코드에서 해당 리소스가 바인딩되는 레지스터 슬롯의 시작 위치를 나타낸다.
  * 예를 들어, HLSL 코드에서 Texture2D가 t3 레지스터에 바인딩된다면, BindPoint 값은 3이 된다.
  * 이는 해당 리소스가 어느 레지스터에 바인딩되는지를 명확하게 알려준다.

* BindCount (UINT):
  * 리소스가 사용 가능한 레지스터의 개수를 나타낸다.
  * 단일 리소스인 경우 BindCount는 1이 되지만, 배열로 선언된 리소스의 경우 배열의 크기가 된다.
  * 예를 들어, Texture2D MyTextures[4];로 선언되었다면, BindCount는 4이다.

* uFlags (UINT):
  * 리소스에 적용된 플래그를 나타내며, 비트 필드 형식으로 저장된다.
  * 대부분의 경우에는 0으로 설정되지만, 특정 상황에서 리소스에 추가 정보를 표시하는 데 사용될 수 있다.
  * 이는 주로 Direct3D 12 내부에서 사용되는 정보이다.

* ReturnType (D3D_RESOURCE_RETURN_TYPE):
  * 리소스가 반환하는 데이터 타입을 나타낸다.
  * D3D_RESOURCE_RETURN_TYPE은 텍스처나 다른 리소스에서 반환되는 값의 타입을 지정하는 열거형으로, 예를 들어 D3D_RETURN_TYPE_FLOAT는 float형 데이터를 반환함을 의미하며, D3D_RETURN_TYPE_INT나 D3D_RETURN_TYPE_UINT 등도 포함된다.
  * 주로 텍스처 리소스에서 사용된다.

* Dimension (D3D_SRV_DIMENSION):
  * 리소스의 차원(Dimension)을 나타내는 열거형 값으로, 해당 리소스가 1D, 2D, 3D 텍스처인지 또는 큐브 맵인지 등을 나타낸다. 

* NumSamples (UINT):
  * 리소스가 멀티샘플링을 지원하는 경우, 해당 리소스가 가지고 있는 샘플의 개수를 나타낸다.
  * 예를 들어, 멀티샘플링이 적용된 2D 텍스처의 경우 샘플 수를 나타내지만, 일반 텍스처인 경우 이 값은 1이 된다.

* Space (UINT):
  * HLSL에서 지정된 리소스의 "Register Space"를 나타낸다.
  * 이는 DirectX 12의 "Root Signature"와 관련된 개념으로, 레지스터 바인딩 시 서로 다른 공간을 분리하여 지정할 수 있게 한다.
  * 예를 들어, space0, space1 등의 형태로 HLSL 코드에서 명시될 수 있다.
   
* uID (UINT):
  * 이 구조체에 대한 Direct3D 내부 고유 식별자(ID)로 사용된다.
  * Direct3D 12가 내부적으로 리소스를 식별하고 관리할 때 활용되며, 개발자가 직접 사용하는 경우는 거의 없다.
