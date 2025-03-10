# Shader Compiler
shader compiler 는 HLSL shader code 를 binary shader object file 로 컴파일 한다.

이 때, legacy shader compiler 인 FXC 를 이용할 수도 있고 modern shader compiler 인 DXC 을 이용할 수도 있다.

이 두개의 compiler 는 서로 다른 binary format을 갖는다.

만약 FXC 를 사용해 compile 한다면 cso 파일에는 DirectX Byte Code(DXBC) 로 컴파일 된 결과가 포함되어 있다.

그리고 compiler 를 이용해서 컴파일 하는 방법에는 크게 두가지가 있다.

standalone excutable 을 사용해서 command line 을 활용하는 방법과 DirectX API 를 호출하는 방법이다.

> Reference  
> [learn.microsoft - Compiling shaders](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-part1)  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  

## FXC
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

## DXC
DXC 을 사용해 compile 한다면 결과로 나오는 binary shader object 파일은 DirectX Intermedia Language(DXIL) 로 표현되어 있다.

> Reference
> [github.com/microsoft - DirectXShaderCompiler/wiki](https://github.com/microsoft/DirectXShaderCompiler/wiki)  

### Standalone Excutable
DXC 의 경우 standalone excutable 로 dxc.exe 를 가지고 있으며 Windows SDK 에 포함되어 있다

```
c:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\dxc.exe

c:\Program Files (x86)\Windows Kits\10\redist\D3D\x64\dxc.exe
```

그리고 dxc.exe 를 가지고 compile 하기 위해서는 command line interface 를 사용하면 된다.

```
dxc.exe -T ps_6_0 -E main PS.hlsl -Fo PS.bin
```

참고로, dxc.exe 를 사용하려면 같은 경로에 반드시 dxcompiler.dll 이 있어야 한다. 왜냐하면 dxc.exe 는 커맨드라인 프로그램이고 실제 로직 구현은 dxcompiler.dll 에 되어 있어 dxc.exe 에서 compile 을 수행하기 위해 dxcompiler.dll 를 호출하기 때문이다. 이런 방식을 사용하게되면 변경이나 업데이트가 필요할 때, dxcompiler.dll만 교체하거나 업데이트함으로써 쉽게 대응할 수 있으며 다른 프로그램에서 dxcompiler.dll 의 기능이 필요할 때 DLL 을 로드만 하기 때문에 코드 재사용성이 높아진다.

그리고 dxc.exe 에서 제공하는 모든 command-line interface flag 는 `dxc.exe -?` 를 통해 알 수 있다.

> Reference  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)   

#### command line flag
-Qstrip_reflect
* 이 Flag 를 사용하면 binary shader object file 에 기본으로 저장되던 reflection data 를 제거 할 수 있다.

-Fre <file>
* reflection data 를 별도의 파일에 저장할 수 있다.

<details>
<summary> -Fd <file> </summary>

* debug information 을 별도의 파일에 저장할 수 있다.
  
* /Zi 나 /Zs flag 없이 이 flag 를 사용할 경우 debug info 가 없다는 오류가 발생한다.
  ```
  /Fd specified, but no Debug Info was found in the shader, please use the /Zi or /Zs switch to generate debug information compiling this shader.
  ```

</details>

> Reference  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#reflection](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#reflection)  

### API
dxcapi.h 에 있는 함수를 사용하기 위해서는 dxcompiler.lib 와 dxcompiler.dll, dxil.dll 를 로드시켜줘야한다.

dxcapi.h 와 dxcompiler.lib 그리고 dxcompiler.dll, dxil.dll 은 windows kits 에 포함되어 있다.
* dxcapi.h
  * C:\Program Files (x86)\Windows Kits\10\Include\10.0.26100.0\um
* dxcompiler.lib
  * C:\Program Files (x86)\Windows Kits\10\Lib\10.0.26100.0\um\x64
* dxcompiler.dll, dxil.dll
  * C:\Program Files (x86)\Windows Kits\10\bin\10.0.26100.0\x64

redist 에 있는 파일들은 최신 파일들이 아닌것 같다.

먼저, IDXCUtils 객체를 생성해준다.

```cpp
  ComPtr<IDxcUtils> utils_cptr = nullptr;
  {
    DxcCreateInstance( CLSID_DxcUtils, IID_PPV_ARGS( &utils_cptr ) );
  }
```

다음으로 compile 하고자 하는 shader file 을 memory 에 load 한다.

```cpp
ComPtr<IDxcBlobEncoding> shader_blob_cptr = nullptr;
{
  UINT32 encoding = CP_UTF8;
  auto   result   = utils_cptr->LoadFile( file_path, &encoding, &shader_blob_cptr );
  REQUIRE( SUCCEEDED( result ), L"{}를 Load 하는데 실패하였습니다. 오류코드({})", file_path, result );
}
```

이 때, shader_blob_cptr 객체가 memory 에 load 된 shader file 정보를 관리함으로 shader file 에 대한 사용이 끝날떄까지 shader_blob_cptr 객체가 소멸되지 않게 주의해야 한다. 만약 소멸될 경우, shader file 정보를 가르키던 포인터들은 전부 dangling pointer 가 되어 의도치 않은 동작을 할 수 있다.

다음으로 memory 에 load 된 shader file 을 가르키는 DxcBuffer 와 compile argument 를 정의한다. 

```cpp
DxcBuffer shader_blob_buffer = {};
{
  shader_blob_buffer.Ptr      = shader_blob_cptr->GetBufferPointer();
  shader_blob_buffer.Size     = shader_blob_cptr->GetBufferSize();
  shader_blob_buffer.Encoding = DXC_CP_ACP;
}

LPCWSTR argument_arr[] =
  {
    base_name.data(),               // Optional shader source file name for error reporting and for PIX shader source view.
    L"-E", L"main",                 // Entry point.
    L"-T", to_wchar_arr( profile ), // Target.
    L"-Zi",                         // Enable debug information (slim format)
    L"-Fo", bin_file_name.c_str(),  // Optional. Stored in the pdb.
    L"-Fd", pdb_file_name.c_str(),  // The file name of the pdb. This must either be supplied or the autogenerated file name must be used.
    L"-Qstrip_reflect",             // Strip reflection into a separate blob.
  };
```

다음으로 IDxcCompiler3 객체를 생성한다.

```cpp
ComPtr<IDxcCompiler3> compiler_cptr = nullptr;
{
  const auto result = DxcCreateInstance( CLSID_DxcCompiler, IID_PPV_ARGS( &compiler_cptr ) );
  REQUIRE( SUCCEEDED( result ), L"IDxcCompiler3 객체를 만드는데 실패하였습니다. 오류코드({})", result );
}
```

다음으로 IDxcCompiler3::Compile 함수를 호출한다.

```cpp
ComPtr<IDxcResult> result_cptr = nullptr;
{
  const auto result = compiler_cptr->Compile(
    &shader_blob_buffer,         // Source buffer.
    argument_arr,                // Array of pointers to arguments.
    _countof( argument_arr ),    // Number of arguments.
    nullptr,                     // User-provided interface to handle #include directives (optional).
    IID_PPV_ARGS( &result_cptr ) // Compiler output status, buffer, and errors.
  );

  REQUIRE( SUCCEEDED( result ), L"{} Compile 에 실패하였습니다. 오류코드({})", file_path, result );
}
```

IDxcResult::GetOutput 함수로 compile 된 shader 에 대한 다양한 정보를 얻을 수 있다.

* https://learn.microsoft.com/ko-kr/windows/win32/api/dxcapi/nf-dxcapi-idxcresult-getoutput
* https://learn.microsoft.com/ko-kr/windows/win32/api/dxcapi/ne-dxcapi-dxc_out_kind

> Reference  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#using-the-compiler-interface)](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#using-the-compiler-interface)  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  
> [lifeisforu.tistory - DXC 필수 바이너리 및 기본 테스트](https://lifeisforu.tistory.com/?page=9)
> [Using-dxc.exe-and-dxcompiler.dll](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll)

#### Load File encoding
encoding 을 명시적으로 정하지 않고 nullptr 을 넘겨도 정상적으로 동작한다.

```cpp
ComPtr<IDxcBlobEncoding> shader_blob_cptr = nullptr;
{
  auto result = utils_cptr->LoadFile( file_path, nullptr, &shader_blob_cptr );
  REQUIRE( SUCCEEDED( result ), L"{}를 Load 하는데 실패하였습니다. 오류코드({})", file_path, result );
}
```

#### DLL 로드
DLL을 로드할 때 다음 순서로 경로를 검색한다. 

* 프로세스 실행 디렉터리
  * 현재 실행 중인 애플리케이션(예: MyApp.exe)이 위치한 디렉터리.
  * Visual Studio 에서 속성 >> 일반 >> 출력 디렉터리를 보면 실행 파일이 어디에 위치하는지 알 수 있다.
* 작업 디렉터리 (Current Working Directory)
  * 프로그램이 실행될 때의 작업 디렉터리.
  * Visual Studio에서 속성 > 디버깅 > 작업 디렉터리에 설정된 경로.
* 시스템 디렉터리
  * Windows 시스템 디렉터리(예: C:\Windows\System32).
  * 시스템 DLL은 여기서 로드된다.
* Windows 디렉터리
  * Windows 설치 디렉터리(예: C:\Windows).
* 환경 변수 PATH에 지정된 디렉터리
  * 시스템 또는 사용자 환경 변수에서 PATH에 설정된 디렉터리 목록.
  * Visual Studio에서는 디버깅 시 PATH 환경 변수에 추가된 경로도 고려된다.

## 참고
binary shader object file 의 확장자로 visual studio 에서는 .cso 를 쓴다.

> Reference  
> [learn.microsoft - dx-graphics-hlsl-part1#using-shader-code-file-extensions](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-part1#using-shader-code-file-extensions)  
> [stackoverflow - whats-the-relationship-between-cso-files-and-dxil](https://stackoverflow.com/questions/77252600/whats-the-relationship-between-cso-files-and-dxil)  

### DXC_XS
PC DXC compiler 는 Just in Time (JIT) 모드로 동작하는데 최적화가 되어있어 runtime 에 DXIL 을 Instruct Set Architecture(ISA) instructions 로 만드는데 부담이 없다. 하지만 Xbox DXC compiler (DXC_XS) 는 runtime 에 DXIL 을 ISA instructions 로 만드는데 부담이 되기 때문에 기본적으로 development PC 에서 precompilation 이 이루어지도록 한다.

별다른 매크로 옵션을 안줄 경우 기본적으로 /D__XBOX_STRIP_DXIL=1 이 정의된거와 같으며 Offline precompile stripped dxil 방식으로 이루어진다.
* DXC (offline) : HLSL -> DXIL -(RootSig)-> ISA
* XBOX (Runtime) : ISA -(RootSig)-> use ISA if valid

만약 /D__XBOX_STRIP_DXIL=0 매크로 옵션을 줄 경우 Offline precompile unstripped dxil 방식으로 이루어진다.
* DXC (offline) : HLSL -> DXIL -(RootSig)-> DXIL + ISA
* XBOX (Runtime) : DXIL + ISA -(RootSig)-> use ISA if valid, otherwise recompile from DXIL

만약 /D__XBOX_DISABLE_PRECOMPILE 매크로 옵션을 줄 경우 Offline no-precompile 방식으로 이루어진다.
* DXC (offline) : HLSL -> DXIL 
* XBOX (Runtime) : DXIL -(RootSig)-> compile to ISA

매크로로 조절하는 대신에 -noprecompile 이라는 컴파일 flag 옵션으로도 조절할 수 있다.

## DXIL
DirectX Intermediate Language(DXIL) 는 DirectX 12 에서 도입된 HLSL 의 중간 표현 (intermediate representation, IR) 이다.

## Reflection 
compile 시에 shader 에 어떤 데이터가 사용되는지를 알려주는 데이터이다.

기본적으로 compile 된 shader 코드에 

reflect data 를 만들 수 있으며 이를 shader reflection file 이라고 한다.

DXC C++ API를 사용하면 셰이더를 컴파일하고 셰이더의 데이터를 '반영'할 수 있습니다(즉, 셰이더가 어떤 리소스를 사용하는지에 대한 정보를 얻을 수 있습니다).

> Reference   
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#reflection](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#reflection)  
> [rtarun9.github - shader_reflection](https://rtarun9.github.io/blogs/shader_reflection/)     
> [kyuhwang.tistory - Shader Reflection](https://kyuhwang.tistory.com/10)  
