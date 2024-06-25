# ID3D11DeviceContext
ID3D11DeviceContext는 Direct3D 11 API의 인터페이스 중 하나로, COM(Component Object Model) 기반의 객체다.

ID3D11DeviceContext는 주로 렌더링 파이프라인을 관리하고 GPU에 명령을 전달하는 역할을 담당한다. 이 인터페이스는 그래픽스 및 컴퓨팅 작업을 수행하기 위한 다양한 함수를 제공하며, 주로 드로우 호출, 리소스 관리, 파이프라인 상태 설정 등을 처리한다.

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