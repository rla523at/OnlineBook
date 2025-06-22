# Root Signature 
Root Signature 는 그래픽 파이프라인에 바인딩되는 리소스를 Root Parameter 단위로 정의한다.

Root signature 은 API 함수 서명과 유사하게 shader 에 바인딩 될 리소스를 결정하지만 실제 메모리나 데이터를 정의하지는 않는다. 실제 Resource Binding 은 Command List 를 통해 Root Parameter 별로 적절한 바인딩 함수를 호출해 연결한다.

> Reference  
> [learn.microsoft - root-signatures](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures)  
> [learn.microsoft - root-signatures-overview](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview)  

## Root Parameter 
Root Parameter 의 종류는 다음과 같다.
* root constants (root argument 에 포함된 constants)
* root descriptors (root argument 에 포함된 descriptor)
* root descriptor tables (descriptor heap 에 일정 범위에 있는 descriptor 에 대한 포인터)

Root Signature Version 1.1 의 Root Parameter 는 [D3D12_ROOT_PARAMETER1](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_root_parameter1) 구조체로 나타내어진다.

<details> <summary> <h3 style="display:inline-block"> Root Argument  </h3></summary>

런타임에 설정 및 변경되는 root parameter 의 실제 값을 root argument 라고 한다. 즉, Command List 의 Binding 함수 호출을 통해 Binding 된 Resource 들을 Root Argument 라고 한다.

</details>


<details> <summary> <h3 style="display:inline-block"> Root Parameter 갯수 제한  </h3></summary>

* [microsoft.github - root-argument-limits](https://microsoft.github.io/DirectX-Specs/d3d/ResourceBinding.html#root-argument-limits)
  * The maximum size of a root arguments is 64 DWORDs
  * Descriptor tables cost 1 DWORD each.
  * Root constants cost 1 DWORD * NumConstants
  * Raw/Structured Buffer SRVs/UAVs and CBVs cost 2 DWORDs.

</details>


<details> <summary> <h3 style="display:inline-block"> Root Parameter 별 Resource Binding 함수 </h3></summary>

| 루트 파라미터 타입                     | HLSL 레지스터 | 바인딩 함수                                         |
|-------------------------------------|-------------|--------------------------------------------------------|
| CBV (상수 버퍼 뷰)                   | bN          | SetGraphicsRootConstantBufferView(N, GPU_VA)           |
| SRV (셰이더 리소스 뷰)               | tN          | SetGraphicsRootShaderResourceView(N, GPU_VA)           |
| UAV (언오더드 액세스 뷰)             | uN          | SetGraphicsRootUnorderedAccessView(N, GPU_VA)          |
| Constants (32비트 상수)              | –           | SetGraphicsRoot32BitConstants(N, Num32, pData, Offset) |
| Descriptor Table (CBV/SRV/UAV 묶음) | bN/tN/uN    | SetGraphicsRootDescriptorTable(N, BaseDescriptor)       |

참고
* [SetGraphicsRootShaderResourceView](https://learn.microsoft.com/ms-my/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-setgraphicsrootshaderresourceview) 와 [SetGraphicsRootUnorderedAccessView](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-setgraphicsrootunorderedaccessview) 는 Buffer Resource 에만 사용 가능하고 Texutre Resource 에는 사용할 수 없다.

</details>


<details> <summary> <h3 style="display:inline-block"> Root Parameter 생성 예시 </h3></summary>

<<<<<<< HEAD
CD3DX12_VERSIONED_ROOT_SIGNATURE_DESC 구조체는 D3D12_VERSIONED_ROOT_SIGNATURE_DESC 를 쉽게 생성하기 위한 Helper class 이다.
* [learn.microsoft - cd3dx12-versioned-root-signature-desc](https://learn.microsoft.com/ko-kr/windows/win32/direct3d12/cd3dx12-versioned-root-signature-desc)  

Root signature 는 root parameter 들로 정의됨으로, Root Signature 를 생성하기 위해서는 먼저, Root Parameter 들을 생성해야 한다.

<details> <summary> <h3 style="display:inline-block"> Root Parameter 생성 </h3></summary>

=======
>>>>>>> 89c2d157aec645d33641a30cf5ca7a9c9c654e08
Root Parameter 는 HLSL 에 Resource 가 어떻게 Binding 되어 있는지를 나타낸다. 예를 들어 HLSL 에 다음과 같이 Resource 가 Binding 되어 있다고 하자.
```
Texture2D texture0 : register(t2);
Texture2D texture1 : register(t3);
Texture2D texture2 : register(t4);
```

Descriptor Range 와 Root Parameter 객체를 다음과 같이 만들면 된다.
```cpp
CD3DX12_DESCRIPTOR_RANGE1 range_arr[1] = {};
range_arr[0].Init( D3D12_DESCRIPTOR_RANGE_TYPE_SRV, 3, 2, 0, D3D12_DESCRIPTOR_RANGE_FLAG_DATA_STATIC );

CD3DX12_ROOT_PARAMETER1 root_parameter_arr[1] = {};
root_parameter_arr[0].InitAsDescriptorTable( 1, &range_arr[0], D3D12_SHADER_VISIBILITY_PIXEL );
```

Root Parameter 는 HLSL 에서 Resource 가 어떻게 Binding 되어 있는지를 나타낼 뿐이지 실제로 Resource 와 binding 해주지는 않는다. Resource 와 binding 은 ID3D12GraphicsCommandList::SetDescriptorHeaps 와 ID3D12GraphicsCommandList::SetGraphicsRootDescriptorTable 함수를 통해 이루어진다.

Root Parameter 는 D3D12_ROOT_PARAMETER1 구조체로 나타내어진다.
* [learn.microsoft - d3d12_root_parameter1](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_root_parameter1)

CD3DX12_ROOT_PARAMETER1 구조체는 D3D12_ROOT_PARAMETER1 구조체를 쉽게 생성하기 위한 Helper class이다.
* [learn.microsoft - cd3dx12-root-parameter1](https://learn.microsoft.com/ko-kr/windows/win32/direct3d12/cd3dx12-root-parameter1)  

CD3DX12_ROOT_PARAMETER1 구조체의 InitAsDescriptorTable 함수를 사용하면 간단하게 RootDescriptorTable 을 나타내는 D3D12_Root_PARAMETER1 구조체를 생성할 수 있다. 

D3D12_DESCRIPTOR_RANGE1 구조체의 BaseShaderRegister 변수는  base shader register 를 나타내는 변수이다. 예를 들어 RangeType 변수가 D3D12_DESCRIPTOR_RANGE_TYPE_SRV 이고 BaseShaderRegister 변수가 3이라면 HLSL 의 ":register(t3)" 와 Mapping 된다.
* [learn.microsoft - d3d12_descriptor_range1](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_descriptor_range1)  
* [learn.microsoft - cd3dx12-descriptor-range1](https://learn.microsoft.com/en-us/windows/win32/direct3d12/cd3dx12-descriptor-range1)
<<<<<<< HEAD
* [learn.micorosoft - d3d12_descriptor_range_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_descriptor_range_type) 
*  
=======
* [learn.micorosoft - d3d12_descriptor_range_type](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_descriptor_range_type)  

>>>>>>> 89c2d157aec645d33641a30cf5ca7a9c9c654e08
</details>


<details> <summary> <h3 style="display:inline-block"> Specification </h3></summary>

* [learn.microsoft - root-parameters-and-arguments](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview#root-parameters-and-arguments)  
  * **A root signature** is similar to an API function signature, it determines the types of data the shaders should expect, but does not define the actual memory or data.
  * **A root parameter** is one entry in the root signature.
  * The actual values of root parameters set and changed at runtime are called **root arguments**.

* [learn.microsoft - using-a-root-signature](https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-a-root-signature)
  * All shaders in a PSO must be compatible with the root layout specified with the PSO, or else the individual shaders must include embedded root layouts that match each other; otherwise, PSO creation will fail.

* [learn.microsoft - root-signatures-overview#root-constants-descriptors-and-tables](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signatures-overview#root-constants-descriptors-and-tables)
  * The inlined root descriptors should contain descriptors that are accessed most often, though is limited to CBVs, and raw or structured UAV or SRV buffers. 
  * A more complex type, such as a 2D texture SRV, cannot be used as a root descriptor.
  * Root descriptors do not include a size limit, so there can be no out-of-bounds checking, unlike descriptors in descriptor heaps, which do include a size.
  * Regardless of the level of hardware, applications should always try to make the root signature as small as needed for maximum efficiency.

</details>


## Root Signature 생성
다양한 버전의 Root Signature 는 구조체 D3D12_VERSIONED_ROOT_SIGNATURE_DESC 로 나타내어진다.
* [learn.microsoft - d3d12_versioned_root_signature_desc](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/ns-d3d12-d3d12_versioned_root_signature_desc)  

CD3DX12_VERSIONED_ROOT_SIGNATURE_DESC 구조체는 D3D12_VERSIONED_ROOT_SIGNATURE_DESC 를 쉽게 생성하기 위한 Helper class 이다.
* [learn.microsoft - cd3dx12-versioned-root-signature-desc](https://learn.microsoft.com/ko-kr/windows/win32/direct3d12/cd3dx12-versioned-root-signature-desc)  

Root signature 는 root parameter 들로 정의됨으로, Root Signature 를 생성하기 위해서는 먼저, Root Parameter 들을 생성해야 한다.

다음으로 D3D12SerializeRootSignature 함수로 serialization 된 root signature 의 blob 을 얻고 이를 ID3D12Device::CreateRootSignature 함수에 인자로 넣어주면 root signature 객체를 생성할 수 있다.

만약, shader 에 root signature 가 포함되어 작성된 경우 compiled shader 에는 이미 serialized root signature 가 포함되어 있다.

반대로, serialized root signature 를 가지고 D3D12_ROOT_SIGNATURE_DESC 데이터 구조를 얻을 수도 있다.

애플리케이션에 이미 serialized root signature 가 있거나 root signature 가 포함된 compiled shader가 있고 코드로 D3D12_ROOT_SIGNATURE_DESC 를 찾으려고 하는 경우 ( 이를 refelction 이라고 한다) D3D12CreateRootSignatureDeserializer 함수를 활용하면 된다.

D3D12CreateRootSignatureDeserializer 함수를 호출하면 역직렬화된 D3D12_ROOT_SIGNATURE_DESC 데이터 구조를 반환하는 메서드가 포함된 ID3D12RootSignatureDeserializer 인터페이스가 생성되며 이 인터페이스는 deserialized 된 데이터 구조의 수명을 소유한다.

> Reference  
> [learn.microsoft - creating-a-root-signature#root-signature-definition](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-a-root-signature#root-signature-definition)  
> [learn.microsoft - creating-a-root-signature#root-signature-data-structure-serialization--deserialization](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-a-root-signature#root-signature-data-structure-serialization--deserialization)
> [learn.microsoft - nf-d3d12-d3d12serializeversionedrootsignature](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-d3d12serializeversionedrootsignature)  


<details> <summary> <h3 style="display:inline-block"> serealized </h3></summary>

Root Signature 을 생성하는 API는 직렬화된(자체 포함, 포인터가 없는) 버전을 사용한다. C++ 데이터 구조에서 이 직렬화된 버전을 생성하는 방법이 제공되지만, 직렬화된 Root Signature 정의를 얻는 또 다른 방법은 Root Signature 를 포함해 컴파일된 셰이더에서 이를 검색하는 방식이다.

> Reference  
> [learn.microsoft - creating-a-root-signature](https://learn.microsoft.com/en-us/windows/win32/direct3d12/creating-a-root-signature)  

</details>

## Root Signautre 에 Descriptor 등록
descriptor table 은 ID3D12GraphicsCommandList::SetGraphicsRootDescriptorTable 함수를 통해 descritpor table 을 graphics root signautre 에 설정한다.

input 인자 RootParameterIndex 변수는 CD3DX12_ROOT_PARAMETER1 의 배열을 만들 떄, 몇번 째 root parameter 로 설정했는지를 나타낸다. 그리고 input 인자 BaseDescriptor 변수는 Shader 에서 Descirptor Table 을 참조할 때 사용하기 위해 Descriptor table 의 시작 주소를 나타내는 D3D12_GPU_DESCRIPTOR_HANDLE 이다.

이 때, ID3D12GraphicsCommandList::SetGraphicsRootDescriptorTable 함수를 호출하기 전에 반드시 ID3D12GraphicsCommandList::SetGraphicsRootSignature 함수가 호출이 되어 있어야 한다. 그렇지 않으면 다음과 같은 오류가 발생한다.

```
D3D12 ERROR: CGraphicsCommandList::SetGraphicsRootDescriptorTable: No root signature has been set, so setting a descriptor table doesn't make sense and is invalid. [ EXECUTION ERROR #708: SET_DESCRIPTOR_TABLE_INVALID]
```

> Reference  
> [learn.micorsoft - setgraphicsrootdescriptortable](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/nf-d3d12-id3d12graphicscommandlist-setgraphicsrootdescriptortable)   




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
