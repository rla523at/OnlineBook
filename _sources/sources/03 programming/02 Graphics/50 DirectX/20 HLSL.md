# HLSL
`HLSL(High-Level Shader Language)`은 Microsoft가 개발한 Shader 프로그래밍 언어로, 주로 DirectX 그래픽 API와 함께 사용된다. 

HLSL은 GPU(Graphics Processing Unit)에서 실행되는 Shader를 작성하기 위한 언어로, 그래픽 렌더링 파이프라인에서 다양한 그래픽 효과를 구현하는 데 사용된다. 이 언어는 C 언어와 유사한 문법을 사용하여 개발자들이 쉽게 배울 수 있으며, 고수준 언어로서 Shader 코드의 작성 및 유지보수를 용이하게 한다.

HLSL은 그래픽 파이프라인의 다양한 단계에서 실행되는 여러 Shader 타입을 작성할 수 있다. Vertex Shader는 각 정점의 위치와 속성을 처리하며, 정점 변환, 조명 계산, 텍스처 좌표 변환 등의 작업을 수행한다. Pixel Shader는 각 픽셀의 색상을 계산하며, 텍스처 매핑, 조명 효과, 색상 조정 등의 작업을 수행한다. Geometry Shader는 프리미티브(점, 선, 삼각형)를 입력으로 받아 새로운 프리미티브를 생성하고, 정점 추가, 삭제, 변형 등의 작업을 수행할 수 있다. Compute Shader는 그래픽 렌더링과 직접적으로 관련되지 않은 일반적인 병렬 계산 작업을 수행하며, 물리 시뮬레이션, 이미지 처리 등의 작업에 사용된다.

## 데이터 형식

### Buffer
Buffer 변수는 다음과 같은 형태를 가지고 있다.

```
Buffer<Type> Name;
```

`Type`은 HLSL에서 지원하는 scalar, vector, 그리고 4개의 32비트 크기를 넘지 않는 matrix 타입이여야 한다.

예를 들어, Buffer<float2x2>를 쓸 수 있지만, Buffer<float4x4>는 너무 커서 컴파일러 오류가 발생한다.

> Reference  
> [learn.microsoft - hlsl-buffer](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/dx-graphics-hlsl-buffer)

 

## Register
셰이더 모델 5 리소스 구문은 register 키워드(keyword) 사용하여 리소스를 HLSL 컴파일러에 바인딩합니다

register 키워드는 특정 리소스를 GPU 레지스터에 수동으로 할당하는 데 사용된다. 이 키워드를 사용하면 상수 버퍼, 텍스처, 샘플러 등을 특정 슬롯에 매핑할 수 있다. 이를 통해 셰이더와 Direct3D API 간의 데이터 바인딩을 명시적으로 관리할 수 있습니다.

사용가능한 레지스터 종류는 다음과 같다
* b#: 상수 버퍼(Constant Buffer)
* t#: 텍스처(Texture)
* s#: 샘플러(Sampler)
* u#: UAV(Unordered Access View)