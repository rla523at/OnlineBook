# Shader Compiler

shader compiler 는 HLSL shader code 를 binary shader object file 로 컴파일 한다.

이 때, legacy shader compiler 인 FXC 를 이용할 수도 있고 modern shader compiler 인 DXC 을 이용할 수도 있다.

이 두개의 compiler 는 서로 다른 binary format을 갖는다.

만약 FXC 를 사용해 compile 한다면 cso 파일에는 DirectX Byte Code(DXBC) 로 컴파일 된 결과가 포함되어 있다.

만약 DXC 을 사용해 compile 한다면 cso 파일에는 DirectX Intermedia Language(DXIL) 로 컴파일 된 결과가 포함되어 있다.

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
DXC 의 경우 standalone excutable 로 dxc.exe 를 가지고 있으며 이 또한 Windows SDK 에 포함되어 있다

```
c:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\dxc.exe

c:\Program Files (x86)\Windows Kits\10\redist\D3D\x64\dxc.exe
```

그리고 dxc.exe 를 가지고 compile 하기 위해서는 command line interface 를 사용하면 된다.

```
dxc.exe -T ps_6_0 -E main PS.hlsl -Fo PS.bin
```

참고로, dxc.exe 를 사용하려면 같은 경로에 반드시 dxcompiler.dll 이 있어야 한다.

왜냐하면 dxc.exe 는 커맨드라인 프로그램이고 실제 로직 구현은 dxcompiler.dll 에 되어 있어 dxc.exe 에서 compile 을 수행하기 위해 dxcompiler.dll 를 호출하기 때문이다

이런 방식을 채택하게 되면 유지보수와 확장성을 높일 수 있다. 변경이나 업데이트가 필요할 때, dxcompiler.dll만 교체하거나 업데이트함으로써 쉽게 대응할 수 있으며 다른 프로그램에서 dxcompiler.dll 의 기능이 필요할 때 DLL 을 로드만 하기 때문에 코드 재사용성이 높아진다.

그리고 dxc.exe 에서 제공하는 모든 command-line interface flag 는 `dxc.exe -?` 를 통해 알 수 있다.

다음으로 DXC 를 API 로 호출하고 싶은 경우 dxcapi.h 에 있는 Compile 메서드를 호출하면 된다.

이 때, dxcompiler.lib 와 dxcompiler.dll 를 링크 시켜줘야 한다.

> Reference  
> [sawicki - two_shader_compilers_of_direct3d_12](https://asawicki.info/news_1719_two_shader_compilers_of_direct3d_12)  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#using-the-compiler)](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#using-the-compiler)  
> [lifeisforu.tistory - DXC 필수 바이너리 및 기본 테스트](https://lifeisforu.tistory.com/?page=9)  

### command line flag
-Qstrip_reflect
* 이 Flag 를 사용하면 binary shader object file 에 기본으로 저장되던 reflection data 를 제거 할 수 있다.

-Fre
* reflection data 를 별도의 파일에 저장할 수 있다.

> Reference  
> [github.com/microsoft - Using-dxc.exe-and-dxcompiler.dll#reflection](https://github.com/microsoft/DirectXShaderCompiler/wiki/Using-dxc.exe-and-dxcompiler.dll#reflection)  

## 참고
binary shader object file 의 확장자로 visual studio 에서는 .cso 를 쓴다.

> Reference  
> [learn.microsoft - dx-graphics-hlsl-part1#using-shader-code-file-extensions](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-part1#using-shader-code-file-extensions)  
> [stackoverflow - whats-the-relationship-between-cso-files-and-dxil](https://stackoverflow.com/questions/77252600/whats-the-relationship-between-cso-files-and-dxil)  

## DXC_XS
GDK 문서의 SHader Compiler 항목을 보면 Xbox 용 DXC compiler 는 Instruct Set Architecture(ISA) 를 만들어 낼 때, 비용이 많이 드는 compile step 이 xbox console 에서 runtime 에 발생하는 상황을 피하기 위해 기본적으로 development PC 에서 precompilation 이 이루어지도록 하며 이를 조절하는 매크로들을 정의해 두었다.

별다른 매크로 옵션을 안줄 경우 기본적으로 Offline precompile stripped dxil 방식으로 이루어진다.
* DXC (offline) : HLSL -> DXIL -(RootSig)-> ISA
* XBOX (Runtime) : ISA -(RootSig)-> use ISA if valid

만약 /D__XBOX_STRIP_DXIL=0 매크로 옵션을 줄 경우 Offline precompile unstripped dxil 방식으로 이루어진다.
* DXC (offline) : HLSL -> DXIL -(RootSig)-> DXIL + ISA
* XBOX (Runtime) : DXIL + ISA -(RootSig)-> use ISA if valid

만약 /D__XBOX_DISABLE_PRECOMPILE 매크로 옵션을 줄 경우 Offline no-precompile 방식으로 이루어진다.
* DXC (offline) : HLSL -> DXIL 
* XBOX (Runtime) : DXIL -(RootSig)-> compile to ISA

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
