# DirectX12
DirectX는 Microsoft에서 개발한 API(응용 프로그램 인터페이스) 집합으로, 멀티미디어 애플리케이션, 특히 게임 및 비디오 애플리케이션을 쉽게 개발할 수 있게 도와준다. DirectX API는 다양한 하위 API로 구성되어 있으며, 각 하위 API는 특정 멀티미디어 기능을 처리하는 데 사용된다. 

> Reference    
> [blog - ssinyoung.tistory](https://ssinyoung.tistory.com/category/PROGRAMMING/DirectX%2012)  
> [blog - dafher-diary.tistory](https://dafher-diary.tistory.com/category/DirectX12)   
> [3dgep](https://www.3dgep.com/learning-directx-12-1/)  

## Sample Code

ExecuteCommandLists 를 먼저 호출하여 준비된 command lists 를 GPU의 command queue 에 제출하여 GPU 가 command 를 실행하게 한다. 이로써 GPU가 그리기 작업을 수행하기 시작한다.

다음에 Present 를 호출하여 GPU 가 그리기 작업을 완료한 Frame 을 화면에 표시한다. 이 함수는 swap chain 의 back buffer 와 front buffer 를 교환하여 화면에 최신 프레임을 나타낸다.

Present 호출을 했을 떄, GPU 가 백 버퍼에서 렌더링 작업을 완료할 떄까지 실제 교환을 지연시킬 수 있는지 찾아보기.
* https://gamedev.stackexchange.com/questions/109345/if-idxgiswapchainpresent-blocks-does-that-mean-im-gpu-bound
* https://learn.microsoft.com/en-us/windows/win32/direct3ddxgi/dxgi-present
* https://developer.nvidia.com/blog/advanced-api-performance-swap-chains/
* https://stackoverflow.com/questions/71225739/direct3d-11-idxgiswapchainpresent-blocks-at-every-call-in-windows-10-windowe

### HelloWindow
D3D12HelloWindow.cpp 


https://github.com/microsoft/DirectX-Graphics-Samples/tree/master

https://directx.tistory.com/7

## Register Slot
Register Slot 을 명시하지 않으면 compiler 가 자동으로 slot 을 지정해준다.

아래와 같은 hlsl 파일을 컴파일해서 test.cso 파일을 만들었다고 하자.

```hlsl
struct VS_INPUT
{
float4 position : SV_POSITION;
float4 color : TEXCOORD0;
}

cbuffer MyVar1 : register(space1)
{
matrix projection1;
}
cbuffer MyVar2 : register(space1)
{
matrix projection2;
}
cbuffer MyVar3 : register(space1)
{
matrix projection3;
}

float4 mainVS(VS_INPUT input) : SV_POSITION
{
	matrix m = projection1 + projection2 + projection3;
	float4 out_pos = mul(m, input.position);
	return out_pos;
}
```

컴파일 된 결과를 ID3D12ShaderReflection 을 이용해서 읽어와보면 실제로 BindPoint(slot) 이 자동으로 0,1,2 로 지정되어 있음을 알 수 있다.

```cpp
#include <iostream>
#include <vector>
#include <dxcapi.h>
#include <d3d12shader.h>
#include <wrl.h>
#pragma comment(lib, "dxcompiler.lib")

int main(void) {
  	using Microsoft::WRL::ComPtr;

	ComPtr<IDxcUtils> utils;
	DxcCreateInstance(CLSID_DxcUtils, IID_PPV_ARGS(&utils));

	ComPtr<IDxBlobEncoding> sourceBlob;
	{
		const HRESULT result = utils->LoadFile(L"test.cso", nullptr, sourceBlob.GetAddressOf());
	}

	ComPtr<ID3D12ShaderReflection> shaderRefelction;
	{
		DxcBuffer sourceBuffer;
		sourceBuffer.Ptr = sourceBlob->GetBufferPointer();
		sourceBuffer.Size = sourceBlob->GetBufferSize();
		sourceBuffer.Encoding = DXC_CP_ACP;

		const HRESULT result = utils->CreateReflection(&sourceBuffer, IID_PPV_ARGS(&shaderReflection));
	}

	D3D12_SHADER_DESC shaderDesc{};
	shaderReflection->GetDesc(&shaderDesc);

	for(uint32_t i=0; i< shaderDesc.BoundResources; ++i)
	{
		D3D12_SHADER_INPUT_BIND_DESC bindingResourceDesc = {};
		shaderReflection->GetResourceBindingDesc(i, &bindingResourceDesc);
	}

	return 0;
}
```

### 참고
hlsl 에서 Cbuffer 를 코드에서 사용하지 않을 경우 compiler 가 최적화하는 과정에서 무시하게 되어 컴파일 된 결과를 읽어 들였을 때 BoundResource 개수에 포함되지 않을 수 있다.


## Register Spaces
HLSL register space 는 컴파일러가 모호성을 명확하게 하는 데 사용하는 추가 구문이다.

c++ 의 namespace 와 동일한 역할을 한다.

예를 들어, 동일한 register 에 두개의 변수를 정의하면 컴파일 오류가 발생한다.

```
cbuffer MyVar : register(b0)
{
	matrix projection;
};

cbuffer MyVar2 : register(b0)
{
	matrix projection2;
};
```

하지만 register space 를 사용해서 서로 다른 register space에 둔다면 컴파일 오류를 해결할 수 있다.

```
cbuffer MyVar : register(b0, space0)
{
	matrix projection;
};

cbuffer MyVar2 : register(b0, space1)
{
	matrix projection2;
};
```

> Reference
> [cml-a.com - register-spaces](https://cml-a.com/content/2022/09/20/register-spaces/)  


## Pipeline State Object (PSO)

PSO 는 graphics/compute pipeline 이 동작하기 위해 필요한 GPU 설정을 나타내는 객체이다.

### shader bytecode
shader code 는 컴파일 된 후 binary blob data 로 바뀐다.

컴파일 된 binary blob data 는 PSO 의 input 으로 사용된다.

> Reference  
> [logins.github - DX12PipelineStateObject](https://logins.github.io/graphics/2020/04/12/DX12PipelineStateObject.html)  


## Root Parameters

Root Parameter 는 root signature 에 한 요소이며, root parameter의 종류는 root constant, root descirtor, root table 가 있다.

종류에 따라 크기와 resource에 접근하기 위해 필요한 복잡도를 나타내는 간접 참조 수준(level of indirection) 이 다르다.

### D3D12_ROOT_PARAMETER 구조체

root signature version 1.0 에 사용되는 root parameter 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct D3D12_ROOT_PARAMETER {
    D3D12_ROOT_PARAMETER_TYPE ParameterType;
    union {
        D3D12_ROOT_DESCRIPTOR_TABLE DescriptorTable;
        D3D12_ROOT_CONSTANTS Constants;
        D3D12_ROOT_DESCRIPTOR Descriptor;
    };
    D3D12_SHADER_VISIBILITY ShaderVisibility;
} D3D12_ROOT_PARAMETER;
```

각각의 멤버변수는 다음과 같다.

* D3D12_ROOT_PARAMETER_TYPE ParameterType
  * Root Parameter 의 유형을 지정한다.
  * enum D3D12_ROOT_PARAMETER_TYPE 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* union
  * Root Parameter 의 구체적인 데이터를 지정하는 union 이다.
  * ParameterType 에 따라 사용되는 구조체가 달라진다.
  * 기본값은 없다. 사용자가 지정해야 한다.

* D3D12_SHADER_VISIBILITY ShaderVisibility
  * 이 루트 파라미터가 노출될 셰이더 스테이지를 지정한다.
  * 기본값은 D3D12_SHADER_VISIBILITY_ALL이다.

> Reference
> [learn.microsoft - root_parameter](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_root_parameter)

### D3D12_ROOT_PARAMETER1 구조체
root signature version 1.1 에 사용되는 root parameter 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct D3D12_ROOT_PARAMETER1 {
  D3D12_ROOT_PARAMETER_TYPE ParameterType;
  union {
    D3D12_ROOT_DESCRIPTOR_TABLE1 DescriptorTable;
    D3D12_ROOT_CONSTANTS         Constants;
    D3D12_ROOT_DESCRIPTOR1       Descriptor;
  };
  D3D12_SHADER_VISIBILITY   ShaderVisibility;
} D3D12_ROOT_PARAMETER1;
```

각각의 멤버변수는 다음과 같다.

* D3D12_ROOT_PARAMETER_TYPE ParameterType
  * Root Parameter 의 유형을 지정한다.
  * 기본값은 없다. 사용자가 지정해야 한다.

* union
  * Root Parameter 의 구체적인 데이터를 지정하는 union 이다.
  * ParameterType 에 따라 사용되는 구조체가 달라진다.
  * 기본값은 없다. 사용자가 지정해야 한다.

* D3D12_SHADER_VISIBILITY ShaderVisibility
  * root parameter 가 노출될 shader 를 지정한다.
  * 기본값은 D3D12_SHADER_VISIBILITY_ALL이다.

> Reference  
> [learn.microsoft - root_parameter1](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_root_parameter1)  

### CD3DX12_ROOT_PARAMETER1 구조체
D3D12_ROOT_PARAMETER1 구조체를 쉽게 초기화 하기 위한 helper 구조체이다.

```
CD3DX12_ROOT_PARAMETER1 rootParameters[1];

rootParameters[0].InitAsConstantBufferView( … );

rootParameters[0].InitAsShaderResourceView( … );  

rootParameters[0].InitAsUnorderedAccessView( … );
```

> Reference  
> [learn.microsoft - cd3dx12-root-parameter1](https://learn.microsoft.com/en-us/windows/win32/direct3d12/cd3dx12-root-parameter1)  


## Root Constant

Root Constant 는 CPU 에서 GPU 로 작은 크기의 상수 값을 빠르게 전달할 수 있는 방법이다.

정해진 개수의 상수 값들을 Root Signature 에 직접 포함하여 GPU 가 이 값을 shader 실행 시 사용하도록 한다.

> Reference  
> [microsoft.github - ResourceBinding.html#root-constants](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html#root-constants)  

### struct D3D12_ROOT_CONSTANTS 구조체
D3D12_ROOT_CONSTANTS 구조체는 Direct3D 12에서 Root Signature 에서 사용될 상수 데이터를 정의하기 위한 구조체이다. 

이 구조체는 shader 에서 사용할 32비트 상수를 루트 파라미터로 직접 전달하는 데 사용된다.

D3D12_ROOT_PARAMETER 구조체의 멤버 변수로 존재하는 구조체로 D3D12_ROOT_PARAMETER 구조체의 SlotType 멤버 변수가 D3D12_PARAMETER_32BIT_CONSTANTS 일 때 사용된다.

정의는 다음과 같다.

```cpp
typedef struct D3D12_ROOT_CONSTANTS {
    UINT ShaderRegister;
    UINT RegisterSpace;
    UINT Num32BitValues;
} D3D12_ROOT_CONSTANTS;
```

각각의 멤버변수는 다음과 같다.

* UINT ShaderRegister
  * 상수를 참조할 셰이더 레지스터를 지정한다. 셰이더 코드에서 사용할 레지스터 인덱스를 나타낸다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT RegisterSpace
  * 상수 데이터가 속한 레지스터 공간을 지정한다. 셰이더 레지스터가 서로 다른 공간에 배치될 수 있다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 0이다.

* UINT Num32BitValues
  * 루트 상수 데이터의 32비트 값 개수를 지정한다.
  * 가능한 값
    * 1 이상의 정수 값
  * 기본값은 없다. 사용자가 지정해야 한다.




## Shader Visibility
모든 root parameter는 어떤 shader type 에서 resource 에 접근할 수 있는지를 나타내는 shader visibility 항목을 가지고 있다.

shader visibility 는 D3D12_SHADER_VISIBILITY enum 으로 나타내어 진다.

shader visibility 는 프로그래머가 다양한 단계에서 필요한 리소스를 구분하기 위한 도구이다.

예를 들어, 동일한 slot 에 bind 되는 서로 다른 두개의 리소스를 가르키는 두개의 root parameter 를 정의할 수 있다.

이 때, 하나는 visibility 가 vertex shader 이고 다른 하나는 pixel shader 라고 하자.

그러면 어떤 shader 가 리소스를 호출하는지에 따라 다른 리소스가 shader register 에 bind 된다.

```
// VS.hlsl
Texture2D myVertexInputTexture : register(t0);

// PS.hlsl
Texture2D MyPixelInputTexture : register(t0);
```

예를 들어, 위와 같이 vertex shader 와 pixel shader 에서 동일한 register slot 에 접근하지만 각각이 실제로 접근하는 리소스는 다른 texture 이다.

따라서, 동일한 네임스페이스(shader binding set)의 동일한 shader register 를 참조하는 서로 다른 root parameter 의 가시성이 겹치지 않는지 확인해야 한다.

> Reference  
> [logins.github - DX12RootSignatureObject](https://logins.github.io/graphics/2020/06/26/DX12RootSignatureObject.html)  


## D3D12_DESCRIPTOR_RANGE1 구조체
D3D12_DESCRIPTOR_RANGE1 구조체는 Direct3D 12에서 루트 서명에서 사용되는 서술자 테이블 내의 서술자 범위를 정의하기 위한 확장된 구조체이다. 

D3D12_DESCRIPTOR_RANGE와 유사하지만, 추가적인 플래그와 옵션을 제공한다.

정의는 다음과 같다.

```cpp
typedef struct D3D12_DESCRIPTOR_RANGE1 {
    D3D12_DESCRIPTOR_RANGE_TYPE RangeType;
    UINT NumDescriptors;
    UINT BaseShaderRegister;
    UINT RegisterSpace;
    UINT Flags;
    UINT OffsetInDescriptorsFromTableStart;
} D3D12_DESCRIPTOR_RANGE1;
```

각각의 멤버변수는 다음과 같다.

* D3D12_DESCRIPTOR_RANGE_TYPE RangeType
  * 서술자 범위의 유형을 지정한다.
  * 가능한 값
    * D3D12_DESCRIPTOR_RANGE_TYPE_SRV: 셰이더 리소스 뷰(SRV) 범위
    * D3D12_DESCRIPTOR_RANGE_TYPE_UAV: 무순서 접근 뷰(UAV) 범위
    * D3D12_DESCRIPTOR_RANGE_TYPE_CBV: 상수 버퍼 뷰(CBV) 범위
    * D3D12_DESCRIPTOR_RANGE_TYPE_SAMPLER: 샘플러 범위
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT NumDescriptors
  * 서술자 범위에 포함된 서술자의 수를 지정한다.
  * 가능한 값
    * 1 이상의 정수 값
    * D3D12_DESCRIPTOR_RANGE_FLAG_DESCRIPTORS_VOLATILE 플래그가 설정된 경우, `NumDescriptors`를 `-1`로 지정할 수 있다.
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT BaseShaderRegister
  * 범위가 시작되는 첫 번째 셰이더 레지스터를 지정한다. 셰이더 코드에서 사용할 첫 번째 레지스터 인덱스를 의미한다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT RegisterSpace
  * 서술자 범위가 속한 레지스터 공간을 지정한다. 셰이더 레지스터가 서로 다른 공간에 배치될 수 있다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 0이다.

* UINT Flags
  * 서술자 범위에 대한 추가적인 동작을 지정하는 플래그를 설정한다.
  * 가능한 값
    * D3D12_DESCRIPTOR_RANGE_FLAG_NONE: 특별한 플래그 없음
    * D3D12_DESCRIPTOR_RANGE_FLAG_DESCRIPTORS_VOLATILE: 서술자가 실행 중에 변경될 수 있음을 나타냄
    * D3D12_DESCRIPTOR_RANGE_FLAG_DATA_VOLATILE: 서술자가 참조하는 데이터가 실행 중에 변경될 수 있음을 나타냄
    * D3D12_DESCRIPTOR_RANGE_FLAG_DATA_STATIC_WHILE_SET_AT_EXECUTE: 서술자가 설정될 때까지 데이터가 고정적임
    * D3D12_DESCRIPTOR_RANGE_FLAG_DATA_STATIC: 서술자가 생성된 이후로 데이터가 변경되지 않음을 나타냄
  * 기본값은 D3D12_DESCRIPTOR_RANGE_FLAG_NONE이다.

* UINT OffsetInDescriptorsFromTableStart
  * 서술자 테이블 시작으로부터의 오프셋을 지정한다. 테이블에서 이 범위가 시작되는 위치를 바이트 단위로 나타낸다.
  * 가능한 값
    * D3D12_DESCRIPTOR_RANGE_OFFSET_APPEND: 자동으로 범위가 이전 범위 뒤에 이어지도록 지정
  * 기본값은 D3D12_DESCRIPTOR_RANGE_OFFSET_APPEND이다.

## Descriptor Range
descriptor table 내에서 지정된 유형의 descriptor 범위를 정의한다.

> Reference  
> [microsoft.github - descriptor-range](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html#descriptor-range)  

## Descriptor Table Bind Types
descriptor table layout 정의의 일부로 참조할 수 있는 descriptor type 이다.

예를 들어, descriptor table 의 일부에 100개의 SRV가 있는 경우 해당 범위를 100개가 아닌 하나의 항목으로 선언할 수 있도록 하는 범위입니다. 

따라서 descriptor table  정의는 범위의 모음입니다.

```cpp
typedef enum D3D12_DESCRIPTOR_RANGE_TYPE
{
    D3D12_DESCRIPTOR_RANGE_SRV,
    D3D12_DESCRIPTOR_RANGE_UAV,
    D3D12_DESCRIPTOR_RANGE_CBV,
    D3D12_DESCRIPTOR_RANGE_SAMPLER
} D3D12_DESCRIPTOR_RANGE_TYPE;
```

> Reference  
> [microsoft.github - descriptor-table-bind-types](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html#descriptor-table-bind-types)  

## Root Descriptors

Root Descriptor 는 GPU 리소스를 직접 가리키는 GPU Virtual Address(GPUVA) 를 Root Signature 에 포함시켜 하나의 리소스를 GPU 에서 바로 접근하도록 한다.

GPUVA 는 64 bit 크기이기 때문에 Root Descriptor 에 크기는 2 DWORDs 이다.

Root Descriptor 는 root argument 에 inline 되어 있는 descriptor 를 의미한다.

지원되는 SRV/UAV format 은 32-bit FLOAT/UINT/SINT 이다.

Root Descriptor는 level 1 의 indirection 을 갖는다. 이는, 리소스에 접근하기 위해서는 하나의 추가적인 객체에게 query 를 해야 함을 의미한다. 이로 인해 root constants 에 비해 리소스 접근 속도가 늦게 된다.

참고로 Descriptor heaps 와 다르게 Root Descriptor 는 크기 검사를 하지 않는다.

> Reference  
> [logins.github - DX12RootSignatureObject](https://logins.github.io/graphics/2020/06/26/DX12RootSignatureObject.html)  

### D3D12_ROOT_DESCRIPTOR 구조체

root parameter 의 종류중 root descriptor 를 나타내는 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct D3D12_ROOT_DESCRIPTOR {
    UINT ShaderRegister;
    UINT RegisterSpace;
} D3D12_ROOT_DESCRIPTOR;
```

각각의 멤버변수는 다음과 같다.

* UINT ShaderRegister
  * 서술자를 참조할 셰이더 레지스터를 지정한다. 셰이더 코드에서 사용할 레지스터 인덱스를 나타낸다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT RegisterSpace
  * 서술자가 속한 레지스터 공간을 지정한다. 셰이더 레지스터가 서로 다른 공간에 배치될 수 있다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 0이다.

## Helper Classes
The d3dx12.h header provides a number of helper classes intended to simplify the usage of the API when working with C++. These are all in the global C++ namespace, and are documented on Microsoft Docs.

Note that d3dx12.h is not included in the Windows SDK, nor is it a part of the DirectX Tool Kit. It is included in the DirectX project templates built into Visual Studio and in the Direct3D Game VS Templates. You can find the latest version on GitHub.

For Xbox One development d3dx12_x.h is included in the Xbox One XDK / Microsoft GDKX; for Xbox Series X|S development you use d3dx12_xs.h.



> Reference  
> [github.com/microsoft - DirectXHelpers](https://github.com/microsoft/DirectXTK12/wiki/DirectXHelpers)  
> [github.com/microsoft - directx/d3dx12.h](https://github.com/microsoft/DirectX-Headers/blob/main/include/directx/d3dx12.h)  

