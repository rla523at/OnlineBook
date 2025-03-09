# DirectX12

> Reference    
> [blog - ssinyoung.tistory](https://ssinyoung.tistory.com/category/PROGRAMMING/DirectX%2012)   
> [blog - dafher-diary.tistory](https://dafher-diary.tistory.com/category/DirectX12)    
> [3dgep](https://www.3dgep.com/learning-directx-12-1/)  

## Globally Unique Identifier
Globally Unique Identifier(GUID) 는 전역에서 유일한 식별자를 나타내며, COM 객체의 인터페이스를 식별하는 데 사용된다. 

각 인터페이스는 고유한 GUID를 가지고 있어, 프로그램이 인터페이스에 접근할 수 있게 한다. DXC의 여러 인터페이스들 역시 dxcapi.h에 정의된 GUID를 통해 접근 가능하다.


<details> <summary> <h2 style="display:inline-block"> IDxcUtils::CreateBlob VS IDxcUtils::CreateBlobFromPinned </h2></summary>

IDxcUtils::CreateBlob 함수는 독립적인 메모리 공간을 할당하고 기존의 데이터를 복사하여 IDxcBlobEncoding 객체를 생성한다.

반면에 IDxcUtils::CreateBlobFromPinned 함수는 별도의 메모리 공간 할당 및 복사를 수행하지 않고 인자로 주어진 pData 를 참고하는 IDxcBlobEncoding 객체를 생성한다.

> Reference  
> [learn.microsoft - idxcutils-createblob](https://learn.microsoft.com/ko-kr/windows/win32/api/dxcapi/nf-dxcapi-idxcutils-createblob)  
> [learn.microsoft - idxcutils-createblobfrompinned)](https://learn.microsoft.com/ko-kr/windows/win32/api/dxcapi/nf-dxcapi-idxcutils-createblobfrompinned)  

</details>

## Execute Command List 후 바로 Presnet 호출
Microsoft Sample code 를 보면 다음과 같이 되어 있다.
```cpp
void D3D12HelloTriangle::OnRender()
{
// Record all the commands we need to render the scene into the command list.
PopulateCommandList();

// Execute the command list.
ID3D12CommandList* ppCommandLists[] = { m_commandList.Get() };
m_commandQueue->ExecuteCommandLists(_countof(ppCommandLists), ppCommandLists);

// Present the frame.
ThrowIfFailed(m_swapChain->Present(1, 0));

WaitForPreviousFrame();
}
```

맨 처음 실행단계에서 ExecuteCommandLists 는 비동기 수행임으로 만약 GPU 작업이 완료되지 못한채로 바로 Present 가 호출이 된다면 어떻게 될까?

Present 는 Display 에게 back buffer 가 준비가 완료되면 front buffer 로 두고 scan out 을 시작하라고 비동기적으로 명령을 내려 놓는다. 따라서, GPU 작업이 완료되지 않았다면 기다렸다가 Display 가 scan out 을 하며, 이 과정은 비동기적으로 이루어지기 때문에 Present 함수에서 CPU wait 은 발생하지 않는것으로 유추된다. 하지만 실제로 이렇다는 문서를 찾을 수가 없다.

> Reference  
> [stackoverflow - directx12-executecommandlists-and-present-function](https://stackoverflow.com/questions/33416715/directx12-executecommandlists-and-present-function)  

## Direct Memory Acess ( DMA )

> Reference  
> [wiki - Direct_memory_access](https://en.wikipedia.org/wiki/Direct_memory_access)  

## Deffered Rendering
각 오브젝트를 그릴 때 조명을 계산하는 Forward Rendering 과는 다르게 모든 오브젝트를 먼저 화면 버퍼에 그리고, 조명 계산은 그 후에 한꺼번에 수행하는 방식이다.

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

### shader bytecode
shader code 는 컴파일 된 후 binary blob data 로 바뀐다.

컴파일 된 binary blob data 는 PSO 의 input 으로 사용된다.

> Reference  
> [logins.github - DX12PipelineStateObject](https://logins.github.io/graphics/2020/04/12/DX12PipelineStateObject.html)  

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

