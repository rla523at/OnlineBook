# ID3D12Device

ID3D12Device 는 Direc3D 12 에서 device 를 나타내는 interface 이다.

여기서 device 란 display adapter 를 나타내는 개념으로 graphics card 와 같은 hardware 일 수도 있고 WARP adapter 와 같은 software 일 수도 있다.

ID3D12Device 는 feature support 를 확인하고 다른 Direct3D interface 객체들을 생성하는 역할을 한다.

## Display adapter
Display adapter는 컴퓨터와 화면 표시 장치(모니터)를 연결하는 장치로써 화면 표시 장치에 정보를 표현할 수 있도록 그래픽을 처리하고 출력하는데 사용되는 장치를 말한다.

## Windows Advanced Rasterization Platform adapter
Windows Advanced Rasterization Platform(WARP) adapter 는 Direct3D 10, 11, 12에서 제공되는 소프트웨어 기반의 그래픽 어댑터이다.

하드웨어 GPU가 없거나, GPU 기능이 제한된 시스템에서 고성능 그래픽을 제공하기 위한 기술로 WARP는 CPU에서 그래픽 처리를 수행하며, 하드웨어 가속이 불가능할 때에도 Direct3D 기능을 사용할 수 있게 해준다.

## D3D12CreateDevice 함수
D3D12CreateDevice 함수는 ID3D12Device를 생성하는 함수이다. 이 함수는 Direct3D 12 기반 그래픽 애플리케이션에서 GPU와 상호작용할 수 있는 디바이스 객체를 초기화하는 데 사용된다.

함수의 시그니처는 다음과 같다.
```cpp
HRESULT D3D12CreateDevice(
  IUnknown          *pAdapter,
  D3D_FEATURE_LEVEL MinimumFeatureLevel,
  REFIID            riid,
  void              **ppDevice
);
```

인자는 다음과 같다.
* IUnknown *pAdapter
  * 사용할 그래픽 어댑터를 지정하는 포인터이다. 
  * nullptr을 전달하면 기본 어댑터가 선택된다.

* D3D_FEATURE_LEVEL MinimumFeatureLevel
  * 생성하려는 디바이스가 지원해야 하는 최소 기능 수준을 나타낸다. 
  * 예를 들어, D3D_FEATURE_LEVEL_11_0 등을 설정할 수 있다.

* REFIID riid
  * 요청하는 인터페이스의 식별자이다. 
  * 주로 ID3D12Device 인터페이스가 요청된다.

* void **ppDevice
  * 성공 시, 생성된 디바이스 객체를 받는 포인터다. 
  * 이 포인터는 함수가 성공적으로 완료되면, ID3D12Device와 같은 인터페이스를 가리키게 된다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * 실패 원인은 호환되지 않는 어댑터나 기능 수준 부족 등이 될 수 있으며, 적절한 진단을 위해 오류 코드를 확인해야 한다.

> Reference  
> [learn.microsoft - nf-d3d12-d3d12createdevice](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d12/nf-d3d12-d3d12createdevice)  


## CreateCommandAllocator 멤버함수
CreateCommandAllocator 함수는 ID3D12Device 인터페이스의 멤버 함수로, DirectX 12의 커맨드 리스트를 기록하기 위한 커맨드 할당기를 생성하는 함수다. 이 함수는 GPU 커맨드의 실행을 위해 메모리를 할당하는 객체를 만든다.

시그니처는 다음과 같다.

```cpp
HRESULT ID3D12Device::CreateCommandAllocator(
    D3D12_COMMAND_LIST_TYPE type,
    REFIID riid,
    void **ppvObject
);
```

매개변수는 다음과 같다.

* D3D12_COMMAND_LIST_TYPE type
  * 생성할 커맨드 할당기의 타입을 지정
  * 사용 가능한 값
    * D3D12_COMMAND_LIST_TYPE_DIRECT: 직접 커맨드 리스트
    * D3D12_COMMAND_LIST_TYPE_BUNDLE: 번들 커맨드 리스트
    * D3D12_COMMAND_LIST_TYPE_COMPUTE: 컴퓨트 커맨드 리스트
    * D3D12_COMMAND_LIST_TYPE_COPY: 복사 커맨드 리스트
  * 기본값: 없음

* REFIID riid
  * 요청하는 인터페이스의 고유 식별자 (GUID)
  * 사용 가능한 값
    * IID_ID3D12CommandAllocator: ID3D12CommandAllocator 인터페이스
  * 기본값: 없음

* void** ppvObject
  * 생성된 커맨드 할당기의 인터페이스 포인터를 받을 변수
  * 사용 가능한 값
    * 유효한 포인터
    * NULL: 포인터가 필요함
  * 기본값: 없음

반환값은 다음과 같다.

* S_OK: 커맨드 할당기가 성공적으로 생성됨
* E_OUTOFMEMORY: 메모리 부족
* E_INVALIDARG: 잘못된 인수
* 기타 HRESULT 오류 코드

이 함수는 커맨드 리스트의 기록을 위해 메모리를 할당하는 `ID3D12CommandAllocator` 객체를 생성하며, 다양한 커맨드 리스트 유형을 지원한다. `type` 매개변수는 커맨드 할당기의 용도를 결정하며, `riid`와 `ppvObject`를 통해 요청된 인터페이스를 반환받는다. 이 함수는 주로 GPU 명령의 효율적인 관리를 위해 사용된다.
