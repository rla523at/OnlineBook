# ID3D12ShaderReflection
ID3D12ShaderReflection 은 Direct3D 12 에서 셰이더 코드의 메타데이터를 반영(reflection)하는 인터페이스로, 셰이더 코드에 대한 정보를 추출하고 분석하는 데 사용된다. 

이를 통해 셰이더의 입력, 출력, 상수 버퍼, 자원 바인딩 정보 등을 확인할 수 있다.

셰이더를 개발하거나 디버깅할 때 유용한 정보를 제공하며, 런타임에 셰이더의 구조를 동적으로 탐색하고 이해하는 데 사용된다.


## GetDesc 멤버함수
D3D12_SHADER_DESC 을 가져오는 함수이다.

```cpp
HRESULT GetDesc(
  [out] D3D12_SHADER_DESC *pDesc
);
```

> Reference  
> [learn.microsoft - nf-d3d12shader-id3d12shaderreflection-getdesc](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12shader/nf-d3d12shader-id3d12shaderreflection-getdesc)  
