# Root Signature 
Root Siganutre 는 command list 와 pipeline 에서 사용 되는 resource 간의 link 를 나타내는 객체이다.

Root Signature 객체는 static sampler descriptors 와 root parameter 로 구성된다.

root parameter 의 종류로는 root constant, root descriptor, root table 가 있으며 종류에 따라 크기가 다르며 크기는 DWORD(32bit, 4byte) 단위로 나타낸다.

* root constant   : 1 DWORD * NumConstants
* root descriptor : 2 DWORDs
* root table      : 1 DWORD

DirectX Spec 에 따르면 Root Signature 를 구성하는 root parameter 들의 최대 크기는 DWORD 64개 이다.

> Reference  
> [microsoft.github - DirectX-Specs](https://microsoft.github.io/DirectX-Specs/)  
> [microsoft.github - ResourceBinding](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html)  

## Version 1.1

Root Signature version 1.1 은 root descriptor 및 root descriptor 가 가르키는 데이터의 변경 가능성을 고려한 최적화가 도입되었다.

root descriptor 또는 root descriptor 가 가르키는 데이터가 command list 를 기록하는 설정 단계와 실행 단계에서 변하지 않는다면 static 설정을 할 수 있으며, 이는 driver가 root signature 를 최적화할 수 있도록 한다.

예를 들어, descriptor table 이 가리키는 resource 가 static 으로 되어 있다면, driver 는 해당 descriptor 를 table 에서 root signature 그 자체로 승격하여 root descriptor 로 만들 수 있다.

이를 통해, descriptor 에서 간접 참조 수준 (level of indirection) 을 제거할 수 있으며, 이는 더 빠르게 액세스 할 수 있게 해준다.

다음 concept 들이 root signature version 1.1 에 추가 되었다

* Static Descriptor
드라이버는 static descriptor 가 root signature에 binding 된 후에 변경되지 않을 것이라고 여긴다.

version 1.1 에서 CBV 와 SRV 의 기본 state 이다.

* Volatile Descriptor
드라이버는 이 descriptor 가 command list 에 recording(set up) 되는 중간에 바뀔 수 있음을 고려한다.

물론, command list 에서 실행 중인 명령이 있으면 여전히 변경할 수 없다.

version 1.1 에서 UAV 와 관련된 data 는 volatile 로 간주된다.

* Static Data
드라이버는 루트 서명 또는 설명자 테이블에서 설명자를 참조한 후에는 데이터가 변경되지 않는다고 가정한다.

기본적으로 relative descriptor는 static 으로 설정되며 이 상태는 드라이버에 최대 수준의 최적화를 부여합니다.

* Static Data While Set At Execute
드라이버는 descriptor 가 가리키는 데이터가 command list가 실행되기 시작할 때까지 변경될 수 있고 나머지 실행 기간 동안 변경되지 않는다고 가정한다.

위의 concept 들은 각각의 root parameter 에게 enum D3D12_ROOT_DESCRIPTOR_FLAGS 과 descriptor range 에게 enum D3D12_DESCRIPTOR_RANGE_FLAGS 로 설정이 되며, 이런 객체들은 D3D12_ROOT_SIGNATURE_DESC1 구조체의 일 부분이 된다.

위의 concept 과 관련된 추가적인 사항은 [learn.microsoft - root-signature-version-1-1](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signature-version-1-1)을 참고하면 된다.

> Reference  
> [logins.github - DX12RootSignatureObject](https://logins.github.io/graphics/2020/06/26/DX12RootSignatureObject.html)  

## Root Signature 과 HLSL 연결

Root Signature 는 HLSL 에 직접 정의할 수도 있으며, HLSL 에 직접 정의하지 않고 애플리케이션 코드에서 Direct3D 12 API 를 사용하여 명시적으로 설정할 수 있다.

### HLSL에 Root Signature를 작성하는 경우
HLSL 코드 내에서 RootSignature attribute 를 사용하여 직접 Root Signature 를 정의할 수 있다.

이 경우에 HLSL 에 Root Signature 와 관련된 내용이 문자열로 작성 되며 문자열은 root signature 의 구성 성분을 설명하는 comma-seperated 구문의 모음으로 구성되어 있다.

root signaute 는 하나의 PSO 에 대해서 전체 shader 에 동일해야 한다.

* 장점
셰이더 코드와 Root Signature 정의가 한 곳에 있기 때문에, 셰이더 개발 시 어떤 자원 레이아웃을 사용할지 쉽게 확인할 수 있다.

그리고 HLSL 코드 내에서 필요한 Root Signature를 바로 정의할 수 있으므로, 셰이더 변경 시 Root Signature를 함께 관리하기 편리하다.

또한 셰이더 바이트코드에 Root Signature가 포함되므로, 런타임에서 Root Signature를 별도로 설정하지 않아도 된다. 

즉, 파이프라인 상태 객체(PSO)를 생성할 때 자동으로 셰이더에 포함된 Root Signature를 사용할 수 있다.

* 단점
HLSL에 Root Signature를 직접 작성하면, 그 셰이더는 해당 Root Signature에 종속적이 되어 다른 루트 시그니처를 사용하려면 셰이더를 다시 컴파일해야 한다.

그리고 다양한 렌더링 상황에서 다른 Root Signature를 사용해야 할 경우, 코드를 통해 각 상황에 맞게 변경하기 어렵다.

> Reference  
> [learn.microsoft - specifying-root-signatures-in-hlsl](https://learn.microsoft.com/en-us/windows/win32/direct3d12/specifying-root-signatures-in-hlsl)  

### HLSL에 Root Signature를 작성하지 않는 경우
Root Signature를 HLSL에서 정의하지 않고, 애플리케이션 코드에서 Direct3D 12 API를 사용하여 명시적으로 설정한다.

HLSL 코드에는 Root Signature에 대한 정보가 없으며, 컴파일된 셰이더 바이트코드에도 Root Signature 정보가 포함되지 않는다.

장점
높은 유연성: 하나의 셰이더 바이트코드에 대해 다양한 Root Signature를 런타임에 설정할 수 있다. 이는 애플리케이션에서 동적으로 여러 Root Signature를 적용해야 하는 경우에 유리하다.
런타임 설정 제어: 애플리케이션에서 Root Signature를 설정하므로, 여러 셰이더에서 동일한 Root Signature를 공유하거나 동적으로 변경할 수 있다.

단점
추가적인 관리 필요: Root Signature를 셰이더와 별도로 관리해야 하기 때문에, 애플리케이션 코드에서 관리해야 할 내용이 늘어난다.
컴파일 시 확인 불가능: 셰이더 코드만으로 어떤 Root Signature가 필요한지 알 수 없으므로, 개발 시 혼동이 발생할 수 있다. 잘못된 Root Signature를 설정하면 런타임 오류가 발생할 수 있다.
별도의 PSO 설정: 파이프라인 상태 객체(PSO) 생성 시 항상 올바른 Root Signature를 설정해야 하므로, 코드가 복잡해질 수 있다.

Root Signature 객체의 생성은  D3D12_ROOT_SIGNATURE_DESC 구조체를 정의하는 것 부터 시작하며, D3D12_ROOT_SIGNATURE_DESC 구조체는 반드시 D3D12_ROOT_PARAMETER 객체의 배열을 포함하고 있어야 한다.

```
D3D12_ROOT_PARAMETER[]         rootParams;
D3D12_STATIC_SAMPLER_DESC[]    staticSamplers;

// Set root params here, more details later

D3D12_ROOT_SIGNATURE_DESC myRootSignatureDesc = {
    _countof(rootParams),
    rootParams,
    _countof(staticSamplers),
    staticSamplers,
    rootSignatureFlags
};
```

graphics command list는 graphics 와 compute root signature 를 둘 다 갖으며, 이 두 root signauture 는 다르고 독립적이다. 

root signature description 은 D3D12SerializeRootSignature 함수로 serialized 되어 해당하는 binary blob 을 생성한다.

생성된 binary blob 은 ID3D12Device::CreateRootSignature 함수의 인자로 사용되며 이 함수로  ID3D12RootSignature 객체가 생성된다.

binary blob 은 root signature 에 대한 정보를 shader cache 에 저장함으로 다음 응용 프로그램 실행시 root signature 에 대한 정보는  로드할 수 있다.
