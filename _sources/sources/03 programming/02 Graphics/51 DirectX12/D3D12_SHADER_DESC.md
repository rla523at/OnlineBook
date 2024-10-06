# D3D12_SHADER_DESC

D3D12_SHADER_DESC 구조체는 Direct3D 12 에서 셰이더에 대한 다양한 정보를 제공하는 구조체이다. 

이 구조체는 셰이더의 전반적인 특성, 입력과 출력 변수, 상수 버퍼, 바인딩된 리소스 등에 관한 정보를 나타낸다.

ID3D12ShaderReflection 인터페이스를 사용하여 셰이더의 정보를 가져올 때, GetDesc 메서드를 통해 D3D12_SHADER_DESC 구조체에 접근할 수 있다.

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
