# ID3D11DeviceContext
ID3D11DeviceContext는 Direct3D 11 API의 인터페이스 중 하나로, COM(Component Object Model) 기반의 객체다.

ID3D11DeviceContext는 그래픽 파이프라인의 다양한 단계를 설정하고 관리한다.

먼저, Input Assembler(IA) stage에서는 다음과 같은 일들을 하며, 관련된 멤버함수는 다음과 같다.
* 입력 레이아웃을 설정하여 정점 버퍼의 데이터 형식을 정의한다.
  * IASetInputLayout 멤버함수
* 입력 어셈블러 단계에서 사용할 정점 버퍼를 바인딩한다.
  * IASetVertexBuffers 멤버함수
* 입력 어셈블러 단계에서 사용할 인덱스 버퍼를 바인딩한다.
  * IASetIndexBuffer 멤버함수
* 입력 어셈블러 단계의 프리미티브 토폴로지를 설정한다.
  * IASetPrimitiveTopology 멤버함수

Vertex Shader(VS) stage에서는 다음과 같은 일들을 하며, 관련된 멤버함수는 다음과 같다.
* 정점 shader를 설정한다.
  * VSSetShader 멤버함수
* 정점 shader에서 사용할 상수 버퍼를 설정한다.
  * VSSetConstantBuffers 멤버함수
* 정점 shader에서 사용할 shader 리소스를 설정한다.
  * VSSetShaderResources 멤버함수
* 정점 shader에서 사용할 샘플러를 설정한다.
  * VSSetSamplers 멤버함수

Rasterizer(RS) stage에서는 다음과 같은 일들을 하며, 관련된 멤버함수는 다음과 같다.
* 래스터라이저 상태를 설정하여 폴리곤의 렌더링 방법을 정의한다.
  * RSSetState 멤버함수
* 뷰포트를 설정하여 렌더링할 화면 영역을 정의한다.
  * RSSetViewports 멤버함수
* 시저 영역을 설정하여 렌더링할 클리핑 영역을 정의한다.
  * RSSetScissorRects 멤버함수

Pixel Shader(PS) stage에서는 다음과 같은 일들을 하며, 관련된 멤버함수는 다음과 같다.
* 픽셀 shader를 설정한다.
  * PSSetShader 멤버함수
* 픽셀 shader에서 사용할 상수 버퍼를 설정한다.
  * PSSetConstantBuffers 멤버함수
* 픽셀 shader에서 사용할 shader 리소스를 설정한다.
  * PSSetShaderResources 멤버함수
* 픽셀 shader에서 사용할 샘플러를 설정한다.
  * PSSetSamplers 멤버함수

Output-Merger(OM) stage에서는 다음과 같은 일들을 하며, 관련된 멤버함수는 다음과 같다.
* 렌더 타겟 뷰와 깊이-스텐실 뷰를 설정하여 렌더링 결과가 기록될 위치를 지정한다.
  * OMSetRenderTargets 멤버함수
* 블렌드 상태를 설정하여 색상 블렌딩 방법을 정의한다.
  * OMSetBlendState 멤버함수
* 깊이-스텐실 상태를 설정하여 깊이 및 스텐실 테스트의 동작을 정의한다.
  * OMSetDepthStencilState 멤버함수

그리고 ID3D11DeviceContext는 실제 렌더링을 수행하는 다양한 멤버 함수를 제공한다.
* 인덱스 버퍼를 사용하지 않고, 바인딩된 정점 버퍼의 데이터를 순차적으로 읽어서 렌더링을 수행한다.
  * Draw 멤버함수
* 인덱스 버퍼를 사용하여 렌더링을 수행한다.
  * DrawIndexed 멤버함수
* 인스턴싱을 사용하여 동일한 메쉬를 여러 번 렌더링한다.
  * DrawInstanced 멤버함수
* 인덱스 버퍼와 인스턴싱을 조합하여 렌더링을 수행한다.
  * DrawIndexedInstanced 멤버함수


ID3D11DeviceContext는 주로 렌더링 파이프라인을 관리하고 GPU에 명령을 전달하는 역할을 담당한다. 이 인터페이스는 그래픽스 및 컴퓨팅 작업을 수행하기 위한 다양한 함수를 제공하며, 주로 드로우 호출, 리소스 관리, 파이프라인 상태 설정 등을 처리한다.



## OMSetRenderTargets 멤버함수
OMSetRenderTargets 함수는 출력-병합(Output-Merger) 단계에서 그리기 작업이 이루어질 RenderTargetView와 Depth Test시 사용할 DepthStencilView를 설정하는 함수다. 

시그니처는 다음과 같다.
```cpp
void ID3D11DeviceContext::OMSetRenderTargets(
    UINT NumViews,
    ID3D11RenderTargetView * const *ppRenderTargetViews,
    ID3D11DepthStencilView *pDepthStencilView
);
```

매개변수는 다음과 같다.

* UINT NumViews
  * 설정할 렌더 타겟 뷰의 수
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* ID3D11RenderTargetView* const *ppRenderTargetViews
  * 렌더 타겟 뷰의 배열에 대한 포인터
  * 사용 가능한 값
    * NULL: 렌더 타겟 뷰를 설정하지 않는다.
    * 유효한 ID3D11RenderTargetView 객체의 포인터 배열
  * 기본값: NULL

* ID3D11DepthStencilView* pDepthStencilView
  * 깊이-스텐실 뷰에 대한 포인터
  * 사용 가능한 값
    * NULL: 깊이-스텐실 뷰를 설정하지 않는다.
    * 유효한 ID3D11DepthStencilView 객체
  * 기본값: NULL


## RSSetViewports 함수
RSSetViewports는 Direct3D 11에서 뷰포트를 설정하는 데 사용된다.

함수의 시그니쳐는 다음과 같다.
```cpp
void RSSetViewports(
  UINT NumViewports,
  const D3D11_VIEWPORT *pViewports
);
```

매개변수는 다음과 같다.
* NumViewports
  * 설정할 뷰포트의 개수를 지정한다. 
  * 이 값은 0에서 D3D11_VIEWPORT_AND_SCISSORRECT_OBJECT_COUNT_PER_PIPELINE (16) 사이의 값을 가질 수 있다.
  * 최대 16개의 뷰포트를 동시에 설정할 수 있다.
* pViewports
  * 뷰포트 배열에 대한 포인터다. 
  * 각 뷰포트는 D3D11_VIEWPORT 구조체로 정의된다.
  * 이 배열의 길이는 NumViewports와 같아야 한다.

## Map/Unmap 멤버 함수
Map 멤버 함수는 지정된 리소스의 데이터를 CPU에서 접근할 수 있도록 메모리에 매핑한다. 이 함수는 리소스가 GPU에서 사용 중일 때도 안전하게 호출할 수 있으며, 호출이 성공하면 D3D11_MAPPED_SUBRESOURCE 구조체를 통해 매핑된 데이터에 접근할 수 있다.

함수의 시그니쳐는 다음과 같다.
```cpp
HRESULT Map(
  ID3D11Resource *pResource,
  UINT Subresource,
  D3D11_MAP MapType,
  UINT MapFlags,
  D3D11_MAPPED_SUBRESOURCE *pMappedResource
);
```

각 매개변수는 다음과 같다.
* pResource
  * 매핑할 리소스에 대한 포인터이다. 예를 들어, 텍스처나 버퍼 객체일 수 있다.
* Subresource
  * 매핑할 서브 리소스의 인덱스를 지정한다. 일반적으로 텍스처의 MIP 수준이나 배열 요소를 나타낸다.
* MapType
  * 매핑의 유형을 지정하는 D3D11_MAP 열거형 값이다. 다음과 같은 값이 있다:
    * D3D11_MAP_READ: 리소스를 읽기 전용으로 매핑한다.
    * D3D11_MAP_WRITE: 리소스를 쓰기 전용으로 매핑한다.
    * D3D11_MAP_READ_WRITE: 리소스를 읽기/쓰기 가능으로 매핑한다.
    * D3D11_MAP_WRITE_DISCARD: 기존 리소스 데이터를 무시하고 새로운 데이터를 쓰기 위한 매핑이다.
    * D3D11_MAP_WRITE_NO_OVERWRITE: 기존 데이터의 일부를 덮어쓰지 않고 데이터를 쓰기 위한 매핑이다.
* MapFlags
  * 추가적인 매핑 옵션을 지정하는 플래그이다. 일반적으로 0 또는 D3D11_MAP_FLAG_DO_NOT_WAIT를 사용할 수 있다:
    * D3D11_MAP_FLAG_DO_NOT_WAIT: 리소스를 사용할 수 있을 때까지 대기하지 않고 바로 반환한다. 리소스가 아직 사용 중이면 DXGI_ERROR_WAS_STILL_DRAWING을 반환한다.
* pMappedResource
  * 매핑된 서브 리소스 데이터에 대한 정보를 저장하는 D3D11_MAPPED_SUBRESOURCE 구조체에 대한 포인터이다.

함수의 특징은 다음과 같다.
* D3D11_USAGE_DYNAMIC 사용 방식으로 생성된 리소스에 대해 사용할 수 있다.
* D3D11_USAGE_DEFAULT 사용 방식으로 생성된 리소스에 대해 사용할 수 없다.
* CPU에서 직접 리소스 데이터를 읽거나 쓸 수 있게 한다.
* CPU가 직접 메모리에 접근하여 데이터를 쓸 수 있으므로, 데이터 전송 오버헤드가 줄어든다.
* 기존 데이터를 무시하고 새로운 데이터를 쓰는 방식(D3D11_MAP_WRITE_DISCARD)은 메모리 할당/해제 오버헤드를 줄이고, 효율적으로 메모리를 사용할 수 있다.
* Map 메서드를 사용하면 GPU와의 동기화 없이 CPU에서 데이터를 쓸 수 있으므로, 동기화 오버헤드가 감소한다.
* GPU와의 동기화를 관리하여 리소스 충돌을 방지한다. 그러나 GPU와 CPU 간의 동기화가 필요한 경우 성능 저하가 발생할 수 있다.
* 리소스를 매핑하면 GPU가 해당 리소스를 사용하지 못하게 되므로, 작업이 끝난 후 반드시 Unmap 함수를 호출하여 매핑을 해제해야 한다.
* D3D11_MAPPED_SUBRESOURCE 구조체의 RowPitch
  * RowPitch는 기본적으로 Map 함수에 주어진 ID3D11Resource에 정의된 텍스처의 형식(DXGI_FORMAT)과 텍스처 너비로 정해진다.
  * 예를 들어, 텍스처의 형식이 DXGI_FORMAT_R32G32B32A32_FLOAT(16바이트)이고 텍스처의 너비가 100픽셀이라면, 기본적으로 1600 바이트가 된다.
  * 하지만, GPU의 메모리 정렬 요구 사항에 따라 행 간의 추가적인 패딩을 삽입할 수 있다. 
  * 이 패딩은 각 행의 시작 주소가 특정 정렬 경계를 맞추도록 하기 위함이다. 
  * 따라서 실제 RowPitch 값은 기본적인 행 크기보다 클 수 있다.

## Unmap 멤버 함수
Unmap 멤버 함수는 Map 멤버 함수를 사용하여 매핑된 리소스에 대한 CPU의 접근을 해제한다. 이 함수는 데이터 업데이트가 완료되었음을 Direct3D에 알리며, GPU가 리소스에 접근할 수 있도록 한다.

함수의 시그니쳐는 다음과 같다.
```cpp
void Unmap(
  ID3D11Resource *pResource,
  UINT Subresource
);
```

각 매개변수는 다음과 같다.
* pResource
  * 매핑을 해제할 리소스에 대한 포인터다.
* Subresource
  * 매핑을 해제할 서브 리소스의 인덱스를 지정한다. 일반적으로 텍스처의 MIP 수준이나 배열 요소를 나타낸다.


함수의 특징은 다음과 같다.
* 리소스가 GPU에서 다시 사용될 수 있도록 한다.
* Unmap 메서드를 호출할 때 동기화가 필요하다.

## UpdateSubresource 멤버 함수
UpdateSubresource 멤버 함수는 CPU 메모리에서 GPU 메모리로 데이터(리소스)를 전송하는 데 사용된다. 이 함수는 주로 텍스처 또는 버퍼 데이터를 업데이트할 때 사용된다.

함수의 시그니처는 다음과 같다.
```cpp
void UpdateSubresource(
  ID3D11Resource *pDstResource,
  UINT DstSubresource,
  const D3D11_BOX *pDstBox,
  const void *pSrcData,
  UINT SrcRowPitch,
  UINT SrcDepthPitch
);
```

각 매개변수는 다음과 같다.
* pDstResource
  * 업데이트할 리소스에 대한 포인터다. 일반적으로 텍스처나 버퍼 객체를 가리킨다.
* DstSubresource
  * 업데이트할 서브 리소스의 인덱스를 지정한다. 
  * 일반적으로 텍스처의 MIP 수준이나 배열 요소를 나타낸다. 
  * 예를 들어, 텍스처의 첫 번째 MIP 레벨은 0, 두 번째 MIP 레벨은 1이다.
* pDstBox
  * 업데이트할 영역을 정의하는 D3D11_BOX 구조체에 대한 포인터이다. 
  * D3D11_BOX는 업데이트할 영역의 시작(X, Y, Z) 및 끝(X, Y, Z) 좌표를 정의한다.
  * 이 매개변수를 nullptr로 설정하면 리소스 전체를 업데이트한다.
* pSrcData
  * 소스 데이터에 대한 포인터이다. 업데이트할 데이터를 가리킨다.
* SrcRowPitch
  * 소스 데이터에서 한 행(row)의 바이트 수를 지정한다. 
  * 텍스처 데이터의 경우 사용된다.
* SrcDepthPitch
  * 소스 데이터에서 한 깊이 슬라이스(depth slice)의 바이트 수를 지정한다. 
  * 3D 텍스처 데이터의 경우 사용된다.

함수의 특징은 다음과 같다.
* D3D11_USAGE_DYNAMIC 사용 방식으로 생성된 리소스에 대해 사용할 수 없다.
* D3D11_USAGE_DEFAULT 또는 D3D11_USAGE_STAGING 사용 방식으로 생성된 리소스에 사용할 수 있다.
  * D3D11_USAGE_DEFAULT의 경우 CPU에서 GPU에 직접 접근이 불가능하게 된다. 
  * 하지만 이 함수는 CPU 메모리의 데이터를 GPU 메모리로 복사하는 것이기 때문에 직접 접근하는 것이 아니라 상관 없다.
* GPU에서 해당 리소스를 사용하는 동안에도 호출할 수 있다. 
  * GPU가 데이터를 사용 중일 때 호출되면, GPU의 작업을 중단시키고 데이터를 갱신해야 한다. 
  * 이로 인해 GPU 작업이 지연될 수 있으며, 예측할 수 없는 성능 문제를 야기할 수 있다.
* CPU에서 GPU로 데이터를 복사할 때 GPU와 동기화를 수행한다. 
  * 매 프레임마다 동기화를 수행하면 CPU와 GPU 사이의 버스 사용량이 증가하여 성능이 저하될 수 있다.
  * 많은 양의 데이터가 자주 전송되면 CPU와 GPU 간의 데이터 전송을 위한 버스가 병목 상태가 될 수 있다. 
* 내부적으로 메모리 할당과 해제를 수반할 수 있다. 매 프레임마다 이러한 작업이 반복되면 메모리 관리 오버헤드가 발생할 수 있다.

## VSSetConstantBuffers 멤버 함수
VSSetConstantBuffers 함수는 ID3D11DeviceContext 인터페이스의 멤버 함수로, 버텍스 shader 단계에서 사용할 상수 버퍼를 설정하는 함수다. 이 함수는 shader 프로그램에 상수 데이터를 전달하는 데 사용된다.

시그니처는 다음과 같다.
```cpp
void ID3D11DeviceContext::VSSetConstantBuffers(
    UINT StartSlot,
    UINT NumBuffers,
    ID3D11Buffer *const *ppConstantBuffers
);
```

매개변수는 다음과 같다.

* UINT StartSlot
  * 설정할 첫 번째 상수 버퍼 슬롯의 인덱스
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* UINT NumBuffers
  * 설정할 상수 버퍼의 수
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* ID3D11Buffer *const *ppConstantBuffers
  * 상수 버퍼 배열에 대한 포인터
  * 사용 가능한 값
    * NULL: 상수 버퍼를 설정하지 않는다.
    * 유효한 ID3D11Buffer 객체의 포인터 배열
  * 기본값: NULL

### StartSlot 인자
StartSlot 인자는 상수 버퍼를 바인딩할 첫 번째 슬롯을 지정하며, 그 뒤의 슬롯에 순차적으로 상수 버퍼를 바인딩한다.

예를 들어 다음과 같은 C++ 코드가 있다고 하자.
```cpp
// 상수 버퍼 배열 준비
ID3D11Buffer* constantBuffers[2] = { pConstantBuffer0, pConstantBuffer1 };

// Vertex Shader에 상수 버퍼 설정 (슬롯 0과 슬롯 1에 바인딩)
deviceContext->VSSetConstantBuffers(0, 2, constantBuffers);
```

상수 버퍼는 슬롯 0과 슬롯 1에 바인딩된다. 그리고 shader 코드에서 아래와 같이 슬롯을 참조하여 상수 버퍼에 접근할 수 있다.
```
cbuffer ConstantBuffer0 : register(b0) // StartSlot = 0
{
    //pConstantBuffer0의 내용
};

cbuffer ConstantBuffer1 : register(b1) // StartSlot = 1
{
    //pConstantBuffer1의 내용
};
```




## Immediate Context와 Deferred Context

### Immediate Context
Immediate Context는 Direct3D 11에서 제공하는 기본적인 렌더링 컨텍스트로, 렌더링 명령을 즉시 GPU에 전달하고 실행하는 데 사용된다. 이는 주로 메인 스레드에서 사용되며, Direct3D 11 디바이스를 생성할 때 기본적으로 함께 생성된다. Immediate Context는 Direct3D 11의 모든 디바이스가 하나씩 갖고 있으며, 실시간으로 그래픽 명령을 처리하여 GPU가 곧바로 실행할 수 있도록 한다.

### Deferred Context
Deferred Context는 Direct3D 11에서 멀티스레딩 환경을 지원하기 위해 도입된 기능으로, 렌더링 명령을 비동기적으로 기록하고 나중에 실행할 수 있도록 한다. 여러 스레드가 동시에 렌더링 명령을 생성하고, 이를 메인 스레드에서 병합하여 실행함으로써 성능을 향상시킬 수 있다. Deferred Context는 명령을 기록할 때 즉시 GPU에 전달하지 않고, 명령 리스트(Command List)에 저장하며, 나중에 Immediate Context를 통해 한꺼번에 실행될 수 있다. Deferred Context는 ID3D11Device::CreateDeferredContext 메서드를 통해 생성된다.

### Immediate Context와 Deferred Context의 차이점
Immediate Context와 Deferred Context는 각각 다른 용도로 사용되며, 다음과 같은 차이점이 있다:

* Immediate Context:
  * D3D11CreateDevice 함수에 의해 기본적으로 생성된다.
  * 렌더링 명령을 즉시 GPU에 전달하여 실행한다.
  * 주로 메인 스레드에서 사용된다.
  * Direct3D 11의 모든 디바이스는 하나의 Immediate Context를 갖는다.

* Deferred Context:
  * 멀티스레딩 환경에서 렌더링 명령을 비동기적으로 기록하고 나중에 Immediate Context를 통해 실행할 수 있도록 한다.
  * 명령을 즉시 실행하지 않고, 명령 리스트(Command List)에 기록한다.
  * 주로 추가적인 스레드에서 사용되어 명령을 기록하고, 메인 스레드에서 명령 리스트를 실행한다.
  * ID3D11Device::CreateDeferredContext 메서드를 통해 생성된다.

### Immediate Context의 장단점

* 장점:
  * 실시간 처리: 렌더링 명령이 즉시 실행되므로 실시간 응답이 필요할 때 유용하다.
  * 간단한 코드 구조: 명령이 즉시 실행되므로 코드가 상대적으로 단순하다.

* 단점:
  * 성능 제한: 멀티스레드 환경에서 효율적으로 작동하지 않으며, 복잡한 렌더링 작업에서는 성능이 제한될 수 있다.
  * CPU 사용량: 모든 렌더링 명령이 메인 스레드에서 처리되므로, CPU 사용량이 높아질 수 있다.

### Deferred Context의 장단점

* 장점:
  * 성능 향상: 멀티스레딩 환경에서 렌더링 명령을 동시에 생성할 수 있으므로, 복잡한 렌더링 작업의 성능을 향상시킬 수 있다.
  * CPU 활용 최적화: 여러 CPU 코어를 활용하여 렌더링 명령을 생성하고 기록할 수 있으므로, CPU 자원을 효율적으로 사용할 수 있다.
  * 유연성 증가: 렌더링 명령을 미리 기록하고 나중에 실행할 수 있으므로, 렌더링 파이프라인을 보다 유연하게 구성할 수 있다.

* 단점:
  * 복잡성 증가: 멀티스레드 환경에서 명령을 기록하고 관리해야 하므로, 코드의 복잡성이 증가할 수 있다.
  * 동기화 문제: 여러 스레드에서 명령을 기록할 때 동기화 문제를 잘 관리해야 한다. 그렇지 않으면 예상치 못한 동작이 발생할 수 있다.
  * 추가 메모리 사용: 명령 리스트를 저장하기 위한 추가 메모리가 필요하다.


> REFERENCE  
> [MSDN - deferred-context](https://learn.microsoft.com/en-us/windows/win32/direct3d11/overviews-direct3d-11-devices-intro#deferred-context)


## PSSetShaderResources 멤버 함수
PSSetShaderResources 멤버 함수는 ID3D11DeviceContext 인터페이스의 멤버 함수로, pixel shader 단계에서 사용할 shader resource view를 설정하는 함수다. 이 함수는 shader 프로그램에 텍스처나 버퍼와 같은 리소스를 전달하는 데 사용된다.

시그니처는 다음과 같다.
```cpp
void ID3D11DeviceContext::PSSetShaderResources(
    UINT StartSlot,
    UINT NumViews,
    ID3D11ShaderResourceView *const *ppShaderResourceViews
);
```
매개변수는 다음과 같다.

* UINT StartSlot
  * 설정할 첫 번째 shader 리소스 뷰 슬롯의 인덱스
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* UINT NumViews
  * 설정할 shader 리소스 뷰의 수
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* ID3D11ShaderResourceView *const *ppShaderResourceViews
  * shader 리소스 뷰 배열에 대한 포인터
  * 사용 가능한 값
    * NULL: shader 리소스 뷰를 설정하지 않는다.
    * 유효한 ID3D11ShaderResourceView 객체의 포인터 배열
  * 기본값: NULL

### StartSlot
pixel shader에서 리소스를 참조하는 코드와 일치해야 한다.

예를 들어 pixel shader 코드가 다음과 같다고 하자.
```
Texture2D texture0 : register(t0); // t0 슬롯에 바인딩
Texture2D texture1 : register(t1); // t1 슬롯에 바인딩
```

그리고 C++ 코드가 다음과 같다고 하자.
```cpp
ID3D11ShaderResourceView* shaderResourceViews[] = { srv0, srv1 };
deviceContext->PSSetShaderResources(0, 2, shaderResourceViews);
```

그러면 srv0은 t0 슬롯에, srv1은 t1 슬롯에 바인딩된다.

## DrawIndexed 멤버 함수
DrawIndexed 함수는 ID3D11DeviceContext 인터페이스의 멤버 함수로, 인덱스 버퍼를 사용하여 기하 도형을 그리는 함수다. 

DrawIndexed 함수가 호출되면, 그 시점에서 Direct3D가 현재 설정된 모든 상태를 포함한 그리기 명령을 GPU 커맨드 큐에 추가한다.

이 함수는 정점 데이터를 효율적으로 재사용할 수 있게 해준다.

시그니처는 다음과 같다.
```cpp
void ID3D11DeviceContext::DrawIndexed(
    UINT IndexCount,
    UINT StartIndexLocation,
    INT BaseVertexLocation
);
```
매개변수는 다음과 같다.

* UINT IndexCount
  * 그릴 인덱스의 수
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* UINT StartIndexLocation
  * 인덱스 버퍼 내에서 시작할 위치의 인덱스
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* INT BaseVertexLocation
  * 각 인덱스에 더할 정점 위치의 오프셋 값
  * 사용 가능한 값
    * 음수 또는 0 이상의 정수 값
  * 기본값: 없음
