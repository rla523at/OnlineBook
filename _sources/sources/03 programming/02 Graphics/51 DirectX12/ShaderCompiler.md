# Shader Compiler

Microsoft Visual Studio 는 빌드 과정의 일부로써 shader compiler 를 사용해 HLSL shader code 를 binary shader object file 로 컴파일 한다.

.cso 확장자는 HLSL compiler 의 output binary 를 위한 visual studio convention 이다.

> Reference  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  
> [learn.microsoft - Compiling shaders](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-part1)   
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll)  
> [stackoverflow - whats-the-relationship-between-cso-files-and-dxil](https://stackoverflow.com/questions/77252600/whats-the-relationship-between-cso-files-and-dxil)  

## Two Shader Compilers 
HLSL 로 작성한 shader code 를 PSO 객체를 생성하는 DirectX API 로 전달하기 위해 컴파일을 한다.

이 때, legacy shader compiler 인 FXC 를 이용할 수도 있고 modern shader compiler 인 DXC 을 이용할 수도 있다.

이 두개의 compiler 는 서로 다른 binary format을 갖는다.

만약 FXC 를 사용해 compile 한다면 cso 파일에는 DirectX Byte Code(DXBC) 로 컴파일 된 결과가 포함되어 있다.

만약 DXC 을 사용해 compile 한다면 cso 파일에는 DirectX Intermedia Language(DXIL) 로 컴파일 된 결과가 포함되어 있다.

compiler 를 이용해서 컴파일 하는 방법에는 크게 두가지가 있다.

standalone excutable 을 사용해서 command line 을 활용하는 방법과 C++ library 를 활용해 API 를 호출하는 방법이다.

> Reference  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  

### FXC

FXC 의 경우 standalone excutable 로 fxc.exe 를 가지고 있으며, 이는 Windows SDK 에 포함되어 있어 예를들면 다음과 같은 경로에 있다.

```
c:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\fxc.exe
```

그리고 fxc.exe 를 가지고 compile 하기 위해서는 command line interface 를 사용하면 된다.

```
fxc.exe /T ps_5_0 /E main PS.hlsl /Fo PS.bin
```

fxc.exe 의 경우 설정가능한 command-line interface 에서 제공하는 모든 flag 는 command line interface 에 `fxc.exe -?` 를 통해 알 수 있다

다음으로 FXC 를 API 로 호출하고 싶은 경우 C++ 프로젝트에 d3dcompiler.lib 를 링크 한 다음에 d3dcompiler.h 에 있는 D3DCompileFromFile 메서드를 호출하면 된다.

> Reference  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  

### DXC

DXC 의 경우 standalone excutable 로 dxc.exe 를 가지고 있으며 이 또한 Windows SDK 에 포함되어 있다

그리고 dxc.exe 를 가지고 compile 하기 위해서는 command line interface 를 사용하면 된다.

```
dxc.exe -T ps_6_0 -E main PS.hlsl -Fo PS.bin
```

dxc.exe 의 경우 설정가능한 command-line interface 에서 제공하는 모든 flag 는 `dxc.exe -?` 를 통해 알 수 있으며 [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll) 를 참고하면 된다.

다음으로 DXC 를 API 로 호출하고 싶은 경우 C++ 프로젝트에 dxcompiler.lib 를 링크 한 다음에 dxcapi.h 에 있는 Compile 메서드를 호출하면 된다.

이 때, 추가적으로 dxil.dll 과 dxcompiler.dll 를 추가적으로 링크 시켜줘야 한다.

> Reference  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  

#### command line flag
`-Qstrip_reflect`
* 이 Flag 를 사용하면 binary shader object file 에 기본으로 저장되던 reflection data 를 제거 할 수 있다.
* 대신 `-Fre` flag 를 사용하여 reflection data 를 별도의 파일에 저장할 수 있다.

> Reference  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#reflection](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#reflection)  

## DXIL
DirectX Intermediate Language(DXIL) 는 DirectX 12 에서 도입된 HLSL 의 중간 표현 (intermediate representation, IR) 이다.

## Reflection 

설정에 따라, compile 시에 shader 에 어떤 데이터가 사용되는지를 나타내는 알려주는 reflect data 를 만들 수 있으며 이를 shader reflection file 이라고 한다.

> Reference  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#reflection](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#reflection)
> [rtarun9.github - shader_reflection](https://rtarun9.github.io/blogs/shader_reflection/)  
> [kyuhwang.tistory - Shader Reflection](https://kyuhwang.tistory.com/10)  

## IDXCUtils

### LoadFile 함수
`IDxcUtils::LoadFile` 함수는 DXC(DXC Shader Compiler) 유틸리티 라이브러리의 멤버 함수로, 파일에서 데이터를 읽어 메모리에 로드하는 기능을 제공한다. 

이 함수는 주로 셰이더 파일을 로드하여 컴파일하기 전에 데이터를 메모리로 가져올 때 사용된다.

시그니처는 다음과 같다.

```
HRESULT IDxcUtils::LoadFile(
    _In_ LPCWSTR pFileName,
    _In_opt_ UINT32* pCodePage,
    _Outptr_result_maybenull_ IDxcBlob** ppBlob
);
```

매개변수는 다음과 같다.

* `LPCWSTR pFileName`
  * 로드할 파일의 경로와 이름을 나타내는 널 종료 문자열 (wide-string)

* `UINT32* pCodePage`
  * 파일을 읽을 때 사용할 코드 페이지 (선택 사항)
  * 사용 가능한 값
    * NULL: 기본 코드 페이지를 사용
    * 특정 코드 페이지 값 (예: CP_UTF8, CP_ACP 등)

* `IDxcBlob** ppBlob`
  * 로드된 파일 데이터를 저장할 `IDxcBlob` 객체의 포인터

반환값은 다음과 같다.

* `S_OK`: 파일이 성공적으로 로드됨
* `E_FAIL`: 파일 로드 실패 또는 잘못된 경로
* `E_INVALIDARG`: 잘못된 인수 (예: `pFileName`이 NULL이거나 `ppBlob`이 NULL인 경우)
* 기타 HRESULT 오류 코드

이 함수는 주로 셰이더 컴파일러와 함께 사용되며, 파일에서 데이터를 읽어 `IDxcBlob` 형태로 반환한다. 

로드된 데이터를 이용해 셰이더 컴파일러에서 셰이더 코드를 컴파일하거나 다른 처리를 수행할 수 있다.
