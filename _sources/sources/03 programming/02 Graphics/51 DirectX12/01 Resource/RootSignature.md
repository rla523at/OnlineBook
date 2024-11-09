# Root Signature 
Root Signature 는 그래픽 파이프라인에 바인딩되는 리소스를 정의한다.

이를 위해 Root Siganutre 는 command list 와 pipeline 에서 사용 되는 resource 을 연결한다.

Root signature 은 API 함수 서명과 유사하게 shader 에 바인딩 될 리소스를 결정하지만 실제 메모리나 데이터를 정의하지는 않는다.

> Reference  
> [learn.microsoft - root-signatures](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures)  
> [learn.microsoft - root-signatures-overview](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview)  

## Root Parameter & Root Argument
Root signature 는 root parameter 들로 정의되며, 런타임에 설정 및 변경되는 root parameter 의 실제 값을 root argument 라고 한다. 따라서, root argument 를 변경하면 shader 가 읽는 데이터가 변경된다.

> Reference  
> [learn.microsoft - root-signatures-overview#root-parameters-and-arguments](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview#root-parameters-and-arguments)  

## Root constants, descriptors and descriptor tables
Root Signature 에는 다음 세 가지 유형의 parameter 가 포함될 수 있다
* root constants (root argument 에 포함된 constants)
* root descriptors (root argument 에 포함된 descriptor)
* descriptor tables (descriptor heap 에 일정 범위에 있는 descriptor 에 대한 포인터)

### Root Constants
Root Constants 는 셰이더에 Constant Buffer 로 표시되는 인라인 32비트 값이다.

### Root Descriptors
Root descriptors 는 CBV 와 raw buffer 또는 structured buffer 의 SRV 나 UAV 로 제한된다.

이 외에 2D 텍스처의 SRV 와 같이 더 복잡한 유형은 root descriptor로 사용할 수 없다. 

root descriptors 는 크기 제한이 없으므로 out-of-bounds 검사를 진행하지 않는다. 단, descriptor heap 에 존재하는 descriptor 의 경우 크기 정보를 포함하기 때문에 out-of-bounds 검사가 진행된다.

> Reference  
> [learn.microsoft - root-signatures-overview#root-constants-descriptors-and-tables](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview#root-constants-descriptors-and-tables)  

## Creating a Root Signature
D3D12_ROOT_SIGNATURE_DESC 구조체를 생성 및 초기화 하고 D3D12SerializeRootSignature 함수로 serialization 된 root signature 의 blob 을 얻고 이를 ID3D12Device::CreateRootSignature 함수에 인자로 넣어주면 root signature 객체를 생성할 수 있다.

만약, shader 에 root signature 가 포함되어 작성된 경우 compiled shader 에는 이미 serialized root signature 가 포함되어 있다.

반대로, serialized root signature 를 가지고 D3D12_ROOT_SIGNATURE_DESC 데이터 구조를 얻을 수도 있다.

애플리케이션에 이미 serialized root signature 가 있거나 root signature 가 포함된 compiled shader가 있고 코드로 D3D12_ROOT_SIGNATURE_DESC 를 찾으려고 하는 경우 ( 이를 refelction 이라고 한다) D3D12CreateRootSignatureDeserializer 함수를 활용하면 된다.

D3D12CreateRootSignatureDeserializer 함수를 호출하면 역직렬화된 D3D12_ROOT_SIGNATURE_DESC 데이터 구조를 반환하는 메서드가 포함된 ID3D12RootSignatureDeserializer 인터페이스가 생성되며 이 인터페이스는 deserialized 된 데이터 구조의 수명을 소유한다.

> Reference  
> [learn.microsoft - creating-a-root-signature#root-signature-definition](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-a-root-signature#root-signature-definition)  
> [learn.microsoft - creating-a-root-signature#root-signature-data-structure-serialization--deserialization](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-a-root-signature#root-signature-data-structure-serialization--deserialization)  

### D3D12_VERSIONED_ROOT_SIGNATURE_DESC 구조체
D3D12_VERSIONED_ROOT_SIGNATURE_DESC 구조체는 Direct3D 12에서 버전에 따라 달라진 루트 서명(Root Signature)을 정의하기 위해 사용되는 구조체이다. 

이 구조체는 여러 버전의 루트 서명을 지원하며, Direct3D 12 API의 호환성을 유지하면서도 다양한 기능을 제공한다. 

이를 통해 개발자는 D3D12_ROOT_SIGNATURE_DESC 또는 D3D12_ROOT_SIGNATURE_DESC1 버전을 선택적으로 사용할 수 있다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_VERSIONED_ROOT_SIGNATURE_DESC {
    D3D_ROOT_SIGNATURE_VERSION Version;
    union {
        D3D12_ROOT_SIGNATURE_DESC Desc_1_0;
        D3D12_ROOT_SIGNATURE_DESC1 Desc_1_1;
    };
} D3D12_VERSIONED_ROOT_SIGNATURE_DESC;
```

각 멤버 변수는 다음과 같다.

* Version (D3D_ROOT_SIGNATURE_VERSION):
  * 루트 서명의 버전을 나타낸다.
  * 이 값에 따라 루트 서명에서 사용할 구조체가 결정된다.

* Desc_1_0 (D3D12_ROOT_SIGNATURE_DESC):
  * 루트 서명 버전 1.0에 해당하는 루트 서명 구조체를 나타낸다. 

* Desc_1_1 (D3D12_ROOT_SIGNATURE_DESC1):
  * 루트 서명 버전 1.1에 해당하는 루트 서명 구조체를 나타낸다. 

### D3D12_ROOT_SIGNATURE_DESC 구조체
root signature version 1.0. 을 나타내는 구조체이다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_ROOT_SIGNATURE_DESC {
    UINT NumParameters;
    const D3D12_ROOT_PARAMETER* pParameters;
    UINT NumStaticSamplers;
    const D3D12_STATIC_SAMPLER_DESC* pStaticSamplers;
    D3D12_ROOT_SIGNATURE_FLAGS Flags;
} D3D12_ROOT_SIGNATURE_DESC;
```

각 멤버 변수는 다음과 같다.

* NumParameters (UINT):
  * 루트 서명에 포함된 루트 파라미터의 수를 나타낸다.
  * 루트 파라미터는 셰이더에 바인딩되는 상수, 테이블, 디스크립터 등을 정의하는 항목이다.

* pParameters (const D3D12_ROOT_PARAMETER*):
  * 루트 서명에 정의된 루트 파라미터 배열에 대한 포인터이다.
  * 각 파라미터는 `D3D12_ROOT_PARAMETER` 구조체로 정의되며, 상수 버퍼, 디스크립터 테이블, 셰이더 리소스 등을 설정할 수 있다.

* NumStaticSamplers (UINT):
  * 루트 서명에 정의된 정적 샘플러의 수를 나타낸다.
  * 정적 샘플러는 런타임 시 변경되지 않는 샘플러 상태를 의미한다.

* pStaticSamplers (const D3D12_STATIC_SAMPLER_DESC*):
  * 루트 서명에 정의된 정적 샘플러 배열에 대한 포인터이다.
  * 각 정적 샘플러는 `D3D12_STATIC_SAMPLER_DESC` 구조체로 정의되며, 텍스처 필터링, 주소 지정 모드 등의 샘플러 상태를 포함한다.

* Flags (D3D12_ROOT_SIGNATURE_FLAGS):
  * 루트 서명에 적용할 플래그를 나타낸다.
  * 이 플래그는 루트 서명의 특정 동작을 제어하는 데 사용되며, 다양한 최적화 옵션이나 리소스 바인딩 방법을 설정할 수 있다.

> Reference  
> [learn.microsoft - ns-d3d12-d3d12_root_signature_desc](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_root_signature_desc)  

### D3D12_ROOT_SIGNATURE_DESC1 구조체
root signature version 1.1. 을 나타내는 구조체이다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct D3D12_ROOT_SIGNATURE_DESC1 {
    UINT NumParameters;
    const D3D12_ROOT_PARAMETER1* pParameters;
    UINT NumStaticSamplers;
    const D3D12_STATIC_SAMPLER_DESC* pStaticSamplers;
    D3D12_ROOT_SIGNATURE_FLAGS Flags;
} D3D12_ROOT_SIGNATURE_DESC1;
```

각 멤버 변수는 다음과 같다.

* **NumParameters (UINT)**:
  * 루트 서명에 포함된 루트 파라미터의 수를 나타낸다.
  * 루트 파라미터는 셰이더와 상호작용하는 리소스, 상수 버퍼, 디스크립터 테이블 등의 정보를 정의한다.

* pParameters (const D3D12_ROOT_PARAMETER1*):
  * 루트 서명에 포함된 루트 파라미터 배열에 대한 포인터이다.
  * `D3D12_ROOT_PARAMETER1` 구조체는 `D3D12_ROOT_PARAMETER`의 개선된 버전으로, 더 세밀한 리소스 바인딩과 관리 기능을 제공한다.

* **NumStaticSamplers (UINT)**:
  * 루트 서명에 포함된 정적 샘플러의 수를 나타낸다.
  * 정적 샘플러는 런타임 동안 변경되지 않는 샘플러 상태를 의미하며, 주로 텍스처 필터링과 주소 지정 모드를 설정한다.

* pStaticSamplers (const D3D12_STATIC_SAMPLER_DESC*):
  * 루트 서명에 정의된 정적 샘플러 배열에 대한 포인터이다.
  * 각 샘플러는 `D3D12_STATIC_SAMPLER_DESC` 구조체로 정의되며, 특정 텍스처 샘플링 상태를 고정적으로 설정할 수 있다.

* Flags (D3D12_ROOT_SIGNATURE_FLAGS):
  * 루트 서명에 적용할 플래그를 나타낸다.
  * 이 플래그는 루트 서명 동작을 제어하며, 리소스 바인딩 및 파이프라인 상태에 영향을 미친다. 

> Reference  
> [learn.microsoft - ns-d3d12-d3d12_root_signature_desc1](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_root_signature_desc1)  

### D3D12SerializeVersionedRootSignature 함수
D3D12SerializeVersionedRootSignature 함수는 D3D12_VERSIONED_ROOT_SIGNATURE_DESC 구조체를 serialize 하여 blob 으로 변환하는 함수이다. 

함수의 시그니처는 다음과 같다.
```cpp
HRESULT D3D12SerializeVersionedRootSignature(
  const D3D12_VERSIONED_ROOT_SIGNATURE_DESC *pRootSignature,
  ID3DBlob                                  **ppBlob,
  ID3DBlob                                  **ppErrorBlob
);
```

인자는 다음과 같다.
* const D3D12_VERSIONED_ROOT_SIGNATURE_DESC *pRootSignature
  * 직렬화할 버전이 지정된 루트 시그니처의 설명이다.
  * D3D12_VERSIONED_ROOT_SIGNATURE_DESC 구조체는 루트 시그니처의 다양한 버전을 지원한다.

* ID3DBlob **ppBlob
  * 직렬화된 루트 시그니처 데이터가 저장될 블롭이다.
  * 함수가 성공적으로 완료되면, 이 포인터는 직렬화된 루트 시그니처 데이터를 가리킨다.

* ID3DBlob **ppErrorBlob
  * 오류 정보가 저장될 블롭이다.
  * 직렬화 과정에서 문제가 발생하면 이 블롭에 오류 메시지가 저장된다.
  * 필요하지 않을 경우 nullptr을 전달할 수 있다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * ppErrorBlob에 오류 메시지가 저장되므로 이를 통해 문제를 진단할 수 있다.

> Reference  
> [learn.microsoft - nf-d3d12-d3d12serializeversionedrootsignature](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-d3d12serializeversionedrootsignature)  

### serealized
Root Signature 을 생성하는 API는 직렬화된(자체 포함, 포인터가 없는) 버전을 사용한다. 

C++ 데이터 구조에서 이 직렬화된 버전을 생성하는 방법이 제공되지만, 직렬화된 Root Signature 정의를 얻는 또 다른 방법은 Root Signature 를 포함해 컴파일된 셰이더에서 이를 검색하는 방식이다.

> Reference
> [learn.microsoft - creating-a-root-signature](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-a-root-signature)

## ??

Root Signature 객체는 static sampler descriptors 와 root parameter 로 구성된다.

root parameter 의 종류로는 root constant, root descriptor, root table 가 있으며 종류에 따라 크기가 다르며 크기는 DWORD(32bit, 4byte) 단위로 나타낸다.

* root constant   : 1 DWORD * NumConstants
* root descriptor : 2 DWORDs
* root table      : 1 DWORD

DirectX Spec 에 따르면 Root Signature 를 구성하는 root parameter 들의 최대 크기는 DWORD 64개 이다.

> Reference
> [microsoft.github - DirectX-Specs](https://microsoft.github.io/DirectX-Specs/)  
> [microsoft.github - ResourceBinding](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html)  

## ??
하드웨어 수준에 관계없이 애플리케이션은 효율성을 극대화하기 위해 항상 Root Signature 을 가능한 작게 만들어야 한다.

> Reference  
> [learn.microsoft - root-signatures-overview#root-constants-descriptors-and-tables](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview#root-constants-descriptors-and-tables)  

## ??
PSO 에 지정된 모든 shader 는 PSO 에 지정된 Root signature 와 호환되어야 한다.

만약 PSO 에 root signature 를 따로 지정되어 있지 않다면 개별 shader 에 shader 와 호환되는 내장된 root signature 가 포함되어 있어야 한다.

그렇지 않을 경우 PSO 생성에 실패하게 된다.

> Reference  
> [learn.microsoft - using-a-root-signature](https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-a-root-signature)   

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
아래 예시와 같이 HLSL 코드에서 Entry function 위에 RootSignature attribute 를 사용하여 직접 Root Signature 를 정의할 수 있다.

```
[RootSignature(MyRS1)]
float4 main(float4 coord : COORD) : SV_Target
```

이 경우에 MyRS1 매크로는 root signature 의 구성 성분을 설명하는 comma-seperated 문자열로 정의되어 있어야 한다.

그러면 컴파일러는 컴파일할 때 shader 의 root signature blob 을 생성한다. 그리고 shader blob 에 shader byte code와 함께 삽입한다.

참고로, root signaute 는 하나의 PSO 에 대해서 전체 shader 에 동일해야 한다. 따라서 하나의 pipe line 에 속한 모든 shader 의 entry function 위에 root signaute attribute 가 존재해야 하며, 모든 shader blob 에는 root signature blob 정보가 포함되어 있다.

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
> [learn.microsoft - specifying-root-signatures-in-hlsl#compiling-an-hlsl-root-signature](https://learn.microsoft.com/en-us/windows/win32/direct3d12/specifying-root-signatures-in-hlsl#compiling-an-hlsl-root-signature)  

### HLSL 에 Root Signature 를 작성하지 않는 경우
Root Signature를 HLSL에서 정의하지 않고, 애플리케이션 코드에서 Direct3D 12 API 를 사용하여 명시적으로 설정한다.

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
