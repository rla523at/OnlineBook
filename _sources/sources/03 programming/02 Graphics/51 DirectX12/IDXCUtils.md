# IDXCUtils
IDXCUtils는 DirectX 12의 DXC 컴파일러에서 제공하는 유틸리티 인터페이스로, dxcapi.h 에 포함되어 있으며 주로 HLSL(High-Level Shading Language) 컴파일러의 작업을 도와주는 다양한 도구를 포함하고 있다. 

이를 사용하여 HLSL 코드 컴파일, Blob 관리, 리플렉션 등의 다양한 기능을 수행할 수 있다.

> Reference
> [learn.microsoft - ns-dxcapi-idxcutils](https://learn.microsoft.com/en-us/windows/win32/api/dxcapi/ns-dxcapi-idxcutils)

## Globally Unique Identifier
Globally Unique Identifier(GUID) 는 전역에서 유일한 식별자를 나타내며, COM 객체의 인터페이스를 식별하는 데 사용된다. 

각 인터페이스는 고유한 GUID를 가지고 있어, 프로그램이 인터페이스에 접근할 수 있게 한다. DXC의 여러 인터페이스들 역시 dxcapi.h에 정의된 GUID를 통해 접근 가능하다.

## DxcCreateInstance 함수
DxcCreateInstance 함수는 지정된 CLSID와 연결된 DirectX 컴파일러(DXC)의 인터페이스 클래스의 초기화되지 않은 단일 개체를 만드는 함수이다.

함수의 시그니처는 다음과 같다.
```cpp
DXC_API_IMPORT HRESULT DxcCreateInstance(
  REFCLSID rclsid,
  REFIID   riid,
  LPVOID   *ppv
);
```

인자는 다음과 같다.
* REFCLSID rclsid
  * 생성할 COM 객체의 클래스 식별자다.
  * REFCLSID 의 정의를 살펴보면 const GUID& 임을 알 수 있다.
  
* REFIID riid
  * 요청한 인터페이스의 식별자다.

* LPVOID *ppv
  * 요청한 인터페이스의 인스턴스를 받을 포인터다. 이 포인터는 함수가 성공적으로 완료되면, 요청한 인터페이스의 주소를 가지게 된다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * 이 오류 코드는 다양한 원인에 의해 발생할 수 있으며, 적절한 진단을 위해 해당 코드를 분석해야 한다.

## LoadFile 멤버함수
`IDxcUtils::LoadFile` 함수는 DXC(DXC Shader Compiler) 유틸리티 라이브러리의 멤버 함수로, 파일에서 데이터를 읽어 Blob을 만드는(메모리에 로드하는) 기능을 제공한다. 

함수의 signature 는 다음과 같다

```
HRESULT LoadFile(
  LPCWSTR          pFileName,
  UINT32           *pCodePage,
  IDxcBlobEncoding **ppBlobEncoding
);
```

매개변수는 다음과 같다.

* `LPCWSTR pFileName`
  * 로드할 파일의 경로와 이름을 나타내는 널 종료 문자열 (wide-string)

* `UINT32* pCodePage`
  * 파일을 읽을 때 사용할 코드 페이지 (선택 사항)
  * 바이너리 파일일 경우 NULL 을 사용한다.

* `IDxcBlobEncoding** ppBlob`
  * 로드된 파일 데이터를 저장할 `IDxcBlobEncoding` 객체의 포인터

> Reference
> [learn.microsoft - nf-dxcapi-idxcutils-loadfile](https://learn.microsoft.com/ko-kr/windows/win32/api/dxcapi/nf-dxcapi-idxcutils-loadfile)

## CreateReflection 멤버함수
CreateReflection 함수는 DirectX 12에서 셰이더의 메타데이터를 추출할 수 있도록 `ID3D12ShaderReflection` 인터페이스를 생성하는 함수다.

이를 통해 셰이더의 입력, 출력, 상수 버퍼, 자원 바인딩 정보 등을 얻을 수 있습니다. 

이 함수는 셰이더 코드의 구조를 런타임에서 동적으로 분석하고 탐색할 수 있게 해줍니다.

함수의 signature는 다음과 같다.

```cpp
HRESULT D3DReflect(
  LPCVOID pSrcData,
  SIZE_T SrcDataSize,
  REFIID pInterface,
  void **ppReflector
);
```

인자는 다음과 같다.
* pSrcData
  * 셰이더 바이너리 데이터에 대한 포인터다.
* SrcDataSize
  * 셰이더 데이터의 크기를 나타낸다.
* pInterface
  * 요청하는 리플렉션 인터페이스의 GUID로, `IID_ID3D12ShaderReflection`을 사용한다.
* ppReflector
  * 리플렉션 인터페이스를 받을 포인터의 포인터다. 함수가 성공하면 이 포인터에 `ID3D12ShaderReflection` 인스턴스를 반환한다.

반환값은 다음과 같다.
* 성공 시  
  * S_OK를 반환한다.

* 실패 시  
  * E_FAIL을 반환하며, 셰이더 데이터가 잘못되었거나 다른 오류가 발생한 경우이다.
  * 오류 처리를 통해 문제를 해결할 수 있다.

> Reference   
> [learn.microsoft](https://learn.microsoft.com/en-us/windows/win32/api/dxcapi/nf-dxcapi-idxcutils-createreflection)  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#reflection](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#reflection)  
