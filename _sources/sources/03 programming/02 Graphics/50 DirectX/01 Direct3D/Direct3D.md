# Direct3D
## Rendering Pipeline

### 입력 어셈블러 단계 (Input Assembler Stage)
이 단계에서는 정점 데이터와 인덱스 데이터를 GPU로 가져온다. 정점 데이터는 3D 모델의 점들에 대한 정보이며, 인덱스 데이터는 이러한 정점들이 어떻게 결합되어 폴리곤을 이루는지에 대한 정보를 포함한다. 이 단계에서 정점 버퍼와 인덱스 버퍼가 GPU에 바인딩되어 후속 단계에서 사용할 수 있도록 준비된다.

input assembler stage에서는 graphics pipe line에 input data인 vertex buffer를 설정하고 vertex buffer가 어떤 형태의 data인지 알려주는 input layout을 설정한다. 그리고 각 data의 고유한 index가 저장된 index buffer를 설정하고 index buffer를 어떻게 해석할지를 결정하는 primitive topology를 설정한다.

### 정점 셰이더 단계 (Vertex Shader Stage)
정점 셰이더는 각 정점에 대해 실행되며, 주로 정점의 변환과 조명을 계산한다. 이 단계에서는 3D 모델의 정점이 월드 공간, 뷰 공간, 클립 공간으로 변환된다. 또한 조명 계산을 통해 정점의 색상과 조명 효과를 결정할 수 있다. 정점 셰이더는 고유의 셰이더 프로그램으로 작성되며, HLSL을 사용하여 정의된다.

### 허황 셰이더 단계 (Hull Shader Stage) [선택적]
허황 셰이더는 테셀레이션 과정을 제어한다. 이 단계는 패치 데이터를 받아 세부적인 곡면을 생성하기 위해 테셀레이션 레벨을 계산한다. 고정밀 모델링에서 사용되며, 입력된 패치를 분할하여 더 많은 정점을 생성하는 과정의 일부이다. 허황 셰이더는 복잡한 기하학적 세부 사항을 렌더링할 때 유용하다.

### 테셀레이터 단계 (Tessellator Stage) [선택적]
테셀레이터는 허황 셰이더와 도메인 셰이더 사이에 위치하며, 입력 패치를 분할하여 더 많은 정점을 생성한다. 이 단계는 테셀레이션 레벨에 따라 입력 패치를 세분화하고, 각 패치를 작은 삼각형이나 다른 기본 프리미티브로 나누어 렌더링을 위한 추가 정점을 생성한다.

### 도메인 셰이더 단계 (Domain Shader Stage) [선택적]
도메인 셰이더는 테셀레이터에 의해 생성된 정점을 처리하여 최종 정점 위치를 계산한다. 이 단계에서는 테셀레이션된 정점들이 최종적으로 어떻게 배치될지를 결정하며, 월드 공간에서의 위치와 관련된 추가 계산을 수행한다. 도메인 셰이더는 주로 테셀레이션된 표면의 세부 사항을 추가적으로 조정하는 데 사용된다.

### 기하 셰이더 단계 (Geometry Shader Stage) [선택적]
기하 셰이더는 프리미티브(점, 선, 삼각형)를 입력으로 받아 새로운 프리미티브를 생성할 수 있다. 이 단계는 입력된 프리미티브를 변형하거나 확장하여 새로운 기하 구조를 생성할 수 있다. 이를 통해 점에서 선을 생성하거나, 선에서 삼각형을 생성하는 등 다양한 기하학적 변환이 가능하다.

### 래스터라이저 단계 (Rasterizer Stage)
래스터라이저는 3D 공간의 프리미티브를 2D 화면 공간으로 변환하고, 프래그먼트(픽셀)을 생성한다. 이 단계에서 프리미티브 클리핑, 뷰포트 변환, 백페이스 컬링 등이 이루어진다. 래스터라이저는 최종적으로 화면에 렌더링될 프래그먼트를 생성하고, 이를 픽셀 셰이더로 전달한다.

### 픽셀 셰이더 단계 (Pixel Shader Stage)
픽셀 셰이더는 각 픽셀에 대해 실행되며, 최종 색상을 계산한다. 이 단계에서는 텍스처 샘플링, 조명 계산, 그림자 처리 등을 수행하여 최종적으로 디스플레이에 출력될 색상을 결정한다. 픽셀 셰이더는 복잡한 조명 모델과 텍스처 매핑 기술을 통해 현실감 있는 이미지를 생성하는 데 사용된다.

### 출력 병합 단계 (Output Merger Stage)
출력 병합 단계에서는 여러 렌더 타겟과 깊이-스텐실 버퍼를 결합하여 최종 렌더링된 이미지를 생성한다. 이 단계에서는 알파 블렌딩, 깊이 테스트, 스텐실 테스트 등이 이루어진다. 최종적으로 생성된 이미지가 렌더 타겟에 출력되며, 화면에 표시될 준비를 마친다.

> REFERENCE  
> [MSDN - input-assembler-stage](https://learn.microsoft.com/en-us/windows/win32/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)  

## D3D11CreateDevice 함수
D3D11CreateDevice 함수는 Direct3D 11 디바이스와 해당 디바이스의 즉시 컨텍스트를 생성하는 함수다. 이 함수는 Direct3D 애플리케이션의 기본 구성 요소로, 그래픽 하드웨어와 상호 작용할 수 있게 한다.

시그니처는 다음과 같다.
```cpp
HRESULT D3D11CreateDevice(
    IDXGIAdapter *pAdapter,
    D3D_DRIVER_TYPE DriverType,
    HMODULE Software,
    UINT Flags,
    const D3D_FEATURE_LEVEL *pFeatureLevels,
    UINT FeatureLevels,
    UINT SDKVersion,
    ID3D11Device **ppDevice,
    ID3D11DeviceContext **ppImmediateContext
);
```

매개변수는 다음과 같다.

* IDXGIAdapter* pAdapter
  * 디바이스가 연결될 어댑터를 지정하는 포인터
  * 사용 가능한 값
    * NULL: 기본 어댑터를 사용한다.
    * IDXGIAdapter: 특정 어댑터를 지정한다.
  * 기본값: NULL

* D3D_DRIVER_TYPE DriverType
  * 디바이스 드라이버의 유형을 지정하는 열거형
  * 사용 가능한 값
    * enum D3D_DRIVER_TYPE 값
  * 기본값: 없음

* HMODULE Software
  * 사용자 지정 소프트웨어 래스터라이저 모듈의 핸들
  * 사용 가능한 값
    * NULL: 소프트웨어 드라이버를 사용하지 않는다.
    * HMODULE: 특정 소프트웨어 드라이버 모듈을 지정한다.
  * 기본값: NULL

* UINT Flags
  * 디바이스 생성에 대한 추가 옵션을 지정하는 플래그
  * 사용 가능한 값
    * 0: 기본 설정을 사용한다.
    * D3D11_CREATE_DEVICE_FLAG enum 값
  * 기본값: 0

* const D3D_FEATURE_LEVEL* pFeatureLevels
  * 지원하려는 기능 수준의 배열에 대한 포인터
  * 사용 가능한 값
    * D3D_FEATURE_LEVEL_11_0: Direct3D 11.0 기능 수준
    * D3D_FEATURE_LEVEL_10_1: Direct3D 10.1 기능 수준
    * D3D_FEATURE_LEVEL_10_0: Direct3D 10.0 기능 수준
    * D3D_FEATURE_LEVEL_9_3: Direct3D 9.3 기능 수준
    * D3D_FEATURE_LEVEL_9_2: Direct3D 9.2 기능 수준
    * D3D_FEATURE_LEVEL_9_1: Direct3D 9.1 기능 수준
  * 기본값: NULL (Direct3D 11.0을 기본으로 설정)

* UINT FeatureLevels
  * pFeatureLevels 배열의 크기
  * 사용 가능한 값
    * 0: pFeatureLevels가 NULL일 때 사용한다.
    * 1 이상의 정수: pFeatureLevels 배열의 요소 수
  * 기본값: 0

* UINT SDKVersion
  * Direct3D SDK의 버전을 지정
  * 사용 가능한 값
    * D3D11_SDK_VERSION: 현재 SDK 버전을 사용한다.
  * 기본값: D3D11_SDK_VERSION

* ID3D11Device** ppDevice
  * 생성된 디바이스 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 디바이스 객체를 반환하지 않는다.
    * ID3D11Device: 생성된 디바이스 객체를 반환한다.
  * 기본값: 없음

* ID3D11DeviceContext** ppImmediateContext
  * 생성된 즉시 컨텍스트 객체에 대한 포인터의 주소
  * 사용 가능한 값
    * NULL: 유효한 즉시 컨텍스트 객체를 반환하지 않는다.
    * ID3D11DeviceContext: 생성된 즉시 컨텍스트 객체를 반환한다.
  * 기본값: 없음

## CheckMultisampleQualityLevels 함수
CheckMultisampleQualityLevels 함수는 지정된 포맷 및 샘플 수에 대해 사용할 수 있는 `멀티샘플링(Multi-Sample Anti-Aliasing; MSAA)` 품질 수준의 수를 확인하는 함수다.

멀티샘플링은 렌더링 품질을 향상시키기 위해 여러 샘플을 사용하여 픽셀의 가장자리 부드럽게 하는 기술이다.

함수의 시그니처는 다음과 같다.

```cpp
HRESULT CheckMultisampleQualityLevels(
    DXGI_FORMAT Format,
    UINT SampleCount,
    UINT *pNumQualityLevels
);
```

각각의 매개변수는 다음과 같다.

* Format
  * DXGI_FORMAT 열거형으로, 확인할 멀티샘플링을 지원하는 텍스처 포맷을 지정한다. 
  * 예를 들어, DXGI_FORMAT_R8G8B8A8_UNORM은 일반적인 32비트 RGBA 포맷이다.
* SampleCount
  * 멀티샘플링에 사용할 샘플의 수를 지정한다. 
  * 이 값은 보통 1, 2, 4, 8, 16 등의 값을 가지며, 특정 하드웨어가 지원하는 샘플 수에 따라 다를 수 있다.
* pNumQualityLevels
  * 지정된 포맷과 샘플 수에 대해 사용 가능한 품질 수준의 수를 반환하는 포인터다. 
  * 이 값은 0부터 시작하며, 값이 1 이상이면 멀티샘플링을 사용할 수 있는 품질 수준이 있음을 의미한다.

### 멀티 샘플링 안티앨리어싱(MSAA) vs 슈퍼 샘플링 안티앨리어싱(SSAA)
MSAA는 각 픽셀을 여러 개의 서브픽셀로 나누고, 경계선에서만 다중 샘플링을 수행한다. 이는 주로 삼각형의 가장자리에서 앨리어싱을 줄이는 데 집중된다. 따라서 경계선 내부의 픽셀은 다중 샘플링을 하지 않으므로, 전체 화면에서 필요한 샘플 수를 줄인다.

SSAA는 화면을 고해상도로 렌더링한 다음, 결과를 다운샘플링하여 최종 이미지를 생성한다. 예를 들어, 2x SSAA는 화면을 원래 해상도의 두 배로 렌더링하고 이를 다시 원래 해상도로 축소한다. 따라서 화면의 모든 픽셀에 대해 다중 샘플링을 수행하여 모든 부분에서 앨리어싱을 줄인다.

| 특성                  | SSAA                       | MSAA                         |
|-----------------------|----------------------------|------------------------------|
|   샘플링 방식         | 모든 픽셀에 대해 다중 샘플링 | 경계선에서만 다중 샘플링    |
|   이미지 품질         | 매우 높음                   | 높음                         |
|   성능 비용           | 매우 높음                   | 낮음                         |
|   메모리 사용량       | 높음                        | 낮음                         |
|   적용 범위           | 전체 이미지                 | 경계선 주로                  |
|   복잡성              | 간단                        | 복잡                         |

## D3D11CreateDeviceAndSwapChain
D3D11CreateDeviceAndSwapChain 함수는 Direct3D 11 디바이스와 디바이스 컨텍스트 그리고 스왑 체인을 동시에 생성하는 함수다.

이 함수의 시그니처는 다음과 같다.

```cpp
HRESULT D3D11CreateDeviceAndSwapChain(
    IDXGIAdapter* pAdapter,
    D3D_DRIVER_TYPE DriverType,
    HMODULE Software,
    UINT Flags,
    const D3D_FEATURE_LEVEL* pFeatureLevels,
    UINT FeatureLevels,
    UINT SDKVersion,
    const DXGI_SWAP_CHAIN_DESC* pSwapChainDesc,
    IDXGISwapChain** ppSwapChain,
    ID3D11Device** ppDevice,
    D3D_FEATURE_LEVEL* pFeatureLevel,
    ID3D11DeviceContext** ppImmediateContext
);
```
각각의 매개변수는 다음과 같다.

### IDXGIAdapter* pAdapter
사용할 DXGI 어댑터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 기본 어댑터가 사용되고 특정 GPU를 선택하려면 IDXGIAdapter 객체를 전달해야 한다.

### D3D_DRIVER_TYPE DriverType
디바이스의 드라이버 유형을 지정하기 위한 매개변수이다. 

사용 가능한 드라이버 유형은 다음과 같다:
* D3D_DRIVER_TYPE_HARDWARE
  * 하드웨어 가속을 사용하는 디바이스.
* D3D_DRIVER_TYPE_REFERENCE
  * 소프트웨어로 구현된 디바이스. 디버깅 및 테스트 목적으로 사용된다.
* D3D_DRIVER_TYPE_NULL
  * 출력이 없는 디바이스. 주로 성능 테스트에 사용된다.
* D3D_DRIVER_TYPE_SOFTWARE
  * 사용자 지정 소프트웨어 렌더러를 사용하는 디바이스.
* D3D_DRIVER_TYPE_WARP
  * WARP(WIndows Advanced Rasterization Platform) 소프트웨어 렌더러를 사용하는 디바이스.

### HMODULE Software
소프트웨어 드라이버의 경우 사용되는 DLL의 핸들을 지정하기 위한 매개변수이다.

DriverType이 D3D_DRIVER_TYPE_SOFTWARE인 경우 유효한 핸들이 필요하지만 그렇지 않으면 nullptr로 설정한다.

### UINT Flags
디바이스 생성 시 사용할 플래그를 지정하기 위한 매개변수이다. 

사용 가능한 주요 플래그는 다음과 같다:
* D3D11_CREATE_DEVICE_DEBUG
  * 디버그 디바이스를 생성하여 디버깅 정보를 활성화한다.
* D3D11_CREATE_DEVICE_SINGLETHREADED
  * 디바이스가 단일 스레드에서만 사용됨을 나타낸다.

만약 이 값에 0을 주는 경우 Direct3D 11 디바이스가 아무런 추가 디버그 기능이나 특수 모드를 사용하지 않고 기본 설정으로 생성된다.

### const D3D_FEATURE_LEVEL* pFeatureLevels
지원할 기능 수준을 나열한 배열을 지정하기 위한 매개변수이다. 

이 배열의 첫 번째 요소부터 적용이 시작됨으로 선호하는 기능 수준 순서로 배열을 만들어야 한다.

### UINT FeatureLevels
기능 수준 배열의 크기를 지정하기 위한 매개변수이다.

일반적으로 ARRAYSIZE(featureLevels)를 사용한다.

### UINT SDKVersion
SDK 버전을 지정하기 위한 매개변수이다. 

일반적으로 D3D11_SDK_VERSION을 사용한다.

### const DXGI_SWAP_CHAIN_DESC* pSwapChainDesc
스왑 체인을 생성하기 위해 필요한 DXGI_SWAP_CHAIN_DESC을 지정하기 위한 매개변수이다.

DXGI_SWAP_CHAIN_DESC 구조체는 DirectX Graphics Infrastructure (DXGI)에서 스왑 체인의 속성을 정의하며 스왑 체인 생성에 사용된다.

### ID3D11Device** ppDevice
생성된 디바이스 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.

### D3D_FEATURE_LEVEL* pFeatureLevel
생성된 디바이스의 기능 수준을 받을 포인터를 지정하기 위한 매개변수이다. 

nullptr로 설정하면 이 값을 무시한다.

### ID3D11DeviceContext** ppImmediateContext
생성된 디바이스 컨텍스트 객체를 받을 포인터의 주소를 지정하기 위한 매개변수이다.


## D3DCompileFromFile 함수
D3DCompileFromFile 함수는 HLSL(High-Level Shader Language) 파일로부터 셰이더 코드를 컴파일하는 함수다. 

이 함수는 파일에서 셰이더 코드를 읽어와서 지정된 프로필에 맞게 컴파일하고, 컴파일된 셰이더 바이트 코드를 반환한다.

시그니처는 다음과 같다.
```cpp
HRESULT D3DCompileFromFile(
    LPCWSTR pFileName,
    const D3D_SHADER_MACRO *pDefines,
    ID3DInclude *pInclude,
    LPCSTR pEntryPoint,
    LPCSTR pTarget,
    UINT Flags1,
    UINT Flags2,
    ID3DBlob **ppCode,
    ID3DBlob **ppErrorMsgs
);
```
매개변수는 다음과 같다.

* LPCWSTR pFileName
  * 셰이더 소스 코드가 포함된 파일의 이름
  * 사용 가능한 값
    * 유효한 파일 경로를 나타내는 문자열
  * 기본값: 없음

* const D3D_SHADER_MACRO* pDefines
  * 셰이더 컴파일에 사용할 매크로 정의
  * 사용 가능한 값
    * NULL: 매크로 정의를 사용하지 않는다.
    * 유효한 D3D_SHADER_MACRO 구조체 배열
  * 기본값: NULL

* ID3DInclude* pInclude
  * 셰이더 코드에서 포함 파일을 처리하는 인터페이스
  * 사용 가능한 값
    * NULL: 포함 파일을 사용하지 않는다.
    * 유효한 ID3DInclude 인터페이스 구현 객체
  * 기본값: NULL

* LPCSTR pEntryPoint
  * 셰이더의 진입점 함수 이름
  * 사용 가능한 값
    * 유효한 함수 이름 문자열
  * 기본값: 없음

* LPCSTR pTarget
  * 컴파일할 셰이더의 타겟 프로필
  * 사용 가능한 값
    * 유효한 셰이더 프로필 문자열 (예: "vs_5_0"은 버텍스 셰이더 모델 5.0을 의미)
  * 기본값: 없음

* UINT Flags1
  * 셰이더 컴파일 옵션 플래그
  * 사용 가능한 값
    * 0: 기본 설정
    * D3DCOMPILE_DEBUG: 디버그 정보를 생성한다.
    * D3DCOMPILE_SKIP_VALIDATION: 컴파일러가 셰이더의 유효성을 검사하지 않도록 한다.
    * D3DCOMPILE_SKIP_OPTIMIZATION: 셰이더 최적화를 건너뛴다.
    * D3DCOMPILE_PACK_MATRIX_ROW_MAJOR: 행 우선 순서로 행렬을 패킹한다.
    * D3DCOMPILE_PACK_MATRIX_COLUMN_MAJOR: 열 우선 순서로 행렬을 패킹한다.
  * 기본값: 0

* UINT Flags2
  * 셰이더 컴파일 이펙트 옵션 플래그
  * 사용 가능한 값
    * 0: 기본 설정
  * 기본값: 0

* ID3DBlob** ppCode
  * 컴파일된 셰이더 코드의 바이트 코드를 저장할 버퍼의 포인터
  * 사용 가능한 값
    * NULL: 컴파일된 셰이더 코드를 반환하지 않는다.
    * 유효한 ID3DBlob 객체 포인터
  * 기본값: 없음

* ID3DBlob** ppErrorMsgs
  * 컴파일 오류 메시지를 저장할 버퍼의 포인터
  * 사용 가능한 값
    * NULL: 오류 메시지를 반환하지 않는다.
    * 유효한 ID3DBlob 객체 포인터
  * 기본값: NULL

## Constant Buffer vs Shader Resource View
### constant buffer
상수 버퍼는 작은 양의 데이터를 셰이더에 전달할 때 매우 효율적이다. 이는 주로 매트릭스, 벡터, 스칼라 값과 같은 데이터를 전달하는 데 사용된다.

constant buffer는 효율성: 상수 버퍼는 작은 데이터 집합을 매우 빠르게 전달할 수 있도록 최적화되어 있다.
간단한 사용: 상수 버퍼는 설정이 비교적 간단하고, Direct3D에서 효율적으로 관리된다.
동기화: 상수 버퍼는 CPU와 GPU 간의 동기화가 필요 없으며, 빠르게 업데이트할 수 있다.

### shader resource
셰이더 리소스는 텍스처, 구조화된 버퍼, 버퍼 배열 등 더 큰 데이터 세트를 전달하는 데 사용된다. 이는 주로 텍스처 맵핑 또는 복잡한 데이터 구조를 전달하는 데 적합하다.

장점
유연성: 다양한 유형의 데이터(텍스처, 버퍼 등)를 전달할 수 있다.
큰 데이터 세트 처리: 더 큰 데이터 세트를 효율적으로 처리할 수 있다

### 정리

| 특성                    | 상수 버퍼 (Constant Buffer) | 셰이더 리소스 (Shader Resource) |
|------------------------|----------------------------|--------------------------------|
| **데이터 크기**         | 작은 데이터 (예: 매트릭스, 벡터) | 큰 데이터 (예: 텍스처, 구조화된 버퍼) |
| **사용 빈도**           | 매 프레임 업데이트 가능        | 자주 변경되지 않는 데이터          |
| **유형**                | 단순 데이터 구조               | 복잡한 데이터 구조                |
| **성능**                | 매우 효율적                    | 대규모 데이터 전송에 적합          |
| **동기화**              | 필요 없음                      | 일반적으로 필요 없음              |
| **유연성**              | 제한적                         | 매우 유연                         |


## XMMatrixLookAtLH 함수
XMMatrixLookAtLH 함수는 DirectXMath 라이브러리의 함수로, 좌측좌표계(Left-Handed Coordinate System)에서 뷰 행렬을 생성하는 함수다. 

이 함수는 카메라의 위치, 카메라가 바라보는 대상, 그리고 카메라의 상향 벡터를 기반으로 뷰 행렬을 계산한다.

시그니처는 다음과 같다.
```cpp
XMMATRIX XMMatrixLookAtLH(
    FXMVECTOR EyePosition,
    FXMVECTOR FocusPosition,
    FXMVECTOR UpDirection
);
```
매개변수는 다음과 같다.

* FXMVECTOR EyePosition
  * 카메라의 위치를 지정하는 3차원 벡터
  * 사용 가능한 값
    * 유효한 3차원 벡터 (예: XMVectorSet(x, y, z, w))
  * 기본값: 없음

* FXMVECTOR FocusPosition
  * 카메라가 바라보는 대상의 위치를 지정하는 3차원 벡터
  * 사용 가능한 값
    * 유효한 3차원 벡터 (예: XMVectorSet(x, y, z, w))
  * 기본값: 없음

* FXMVECTOR UpDirection
  * 카메라의 상향 벡터를 지정하는 3차원 벡터
  * 사용 가능한 값
    * 유효한 3차원 벡터 (예: XMVectorSet(x, y, z, w))
  * 기본값: 없음

## XMMatrixLookToLH 함수
XMMatrixLookToLH 함수는 DirectXMath 라이브러리의 함수로, 좌측좌표계(Left-Handed Coordinate System)에서 뷰 행렬을 생성하는 함수다. 

이 함수는 카메라의 위치, 카메라가 바라보는 방향, 그리고 카메라의 상향 벡터를 기반으로 뷰 행렬을 계산한다.

시그니처는 다음과 같다.
```cpp
XMMATRIX XMMatrixLookToLH(
    FXMVECTOR EyePosition,
    FXMVECTOR EyeDirection,
    FXMVECTOR UpDirection
);
```
매개변수는 다음과 같다.

* FXMVECTOR EyePosition
  * 카메라의 위치를 지정하는 3차원 벡터
  * 사용 가능한 값
    * 유효한 3차원 벡터 (예: XMVectorSet(x, y, z, w))
  * 기본값: 없음

* FXMVECTOR EyeDirection
  * 카메라가 바라보는 방향을 지정하는 3차원 벡터
  * 사용 가능한 값
    * 유효한 3차원 벡터 (예: XMVectorSet(x, y, z, w))
  * 기본값: 없음

* FXMVECTOR UpDirection
  * 카메라의 상향 벡터를 지정하는 3차원 벡터
  * 사용 가능한 값
    * 유효한 3차원 벡터 (예: XMVectorSet(x, y, z, w))
  * 기본값: 없음

## XMMatrixPerspectiveFovLH 함수
XMMatrixPerspectiveFovLH 함수는 DirectXMath 라이브러리의 함수로, 좌측좌표계(Left-Handed Coordinate System)에서 주어진 시야각(FOV), 종횡비, 그리고 근/원거리 평면을 기반으로 원근 투영 행렬을 생성하는 함수다.

시그니처는 다음과 같다.
```cpp
XMMATRIX XMMatrixPerspectiveFovLH(
    float FovAngleY,
    float AspectRatio,
    float NearZ,
    float FarZ
);
```
매개변수는 다음과 같다.

* float FovAngleY
  * Y축 방향의 시야각을 라디안 단위로 지정
  * 사용 가능한 값
    * 0보다 크고, π(파이)보다 작은 부동 소수점 값
  * 기본값: 없음

* float AspectRatio
  * 뷰포트의 종횡비를 지정 (너비 / 높이)
  * 사용 가능한 값
    * 0보다 큰 부동 소수점 값
  * 기본값: 없음

* float NearZ
  * 근거리 평면의 깊이를 지정
  * 사용 가능한 값
    * 0보다 크고, FarZ보다 작은 부동 소수점 값
  * 기본값: 없음

* float FarZ
  * 원거리 평면의 깊이를 지정
  * 사용 가능한 값
    * NearZ보다 큰 부동 소수점 값
  * 기본값: 없음

## XMMatrixOrthographicOffCenterLH함수
XMMatrixOrthographicOffCenterLH 함수는 DirectXMath 라이브러리의 함수로, 좌측좌표계(Left-Handed Coordinate System)에서 원근이 없는 직교 투영 행렬을 생성하는 함수다. 

이 함수는 사용자 지정 좌표로 정의된 평면을 기준으로 하는 직교 투영을 설정한다.

시그니처는 다음과 같다.
```cpp
XMMATRIX XMMatrixOrthographicOffCenterLH(
    float ViewLeft,
    float ViewRight,
    float ViewBottom,
    float ViewTop,
    float NearZ,
    float FarZ
);
```
매개변수는 다음과 같다.

* float ViewLeft
  * 투영 평면의 왼쪽 좌표
  * 사용 가능한 값
    * 임의의 부동 소수점 값
  * 기본값: 없음

* float ViewRight
  * 투영 평면의 오른쪽 좌표
  * 사용 가능한 값
    * 임의의 부동 소수점 값
  * 기본값: 없음

* float ViewBottom
  * 투영 평면의 아래쪽 좌표
  * 사용 가능한 값
    * 임의의 부동 소수점 값
  * 기본값: 없음

* float ViewTop
  * 투영 평면의 위쪽 좌표
  * 사용 가능한 값
    * 임의의 부동 소수점 값
  * 기본값: 없음

* float NearZ
  * 근거리 평면의 깊이
  * 사용 가능한 값
    * 임의의 부동 소수점 값 (일반적으로 0보다 큰 값)
  * 기본값: 없음

* float FarZ
  * 원거리 평면의 깊이
  * 사용 가능한 값
    * NearZ보다 큰 부동 소수점 값
  * 기본값: 없음

## Compute Shader
Compute Shader에서 그룹별 실행 순서는 특정한 규칙 없으며 GPU는 가능한 한 많은 스레드 그룹을 동시에 실행하려고 한다.

따라서, 스레드 그룹 간에는 동기화가 불가능하며 실행 순서가 정의되지 않는다. 