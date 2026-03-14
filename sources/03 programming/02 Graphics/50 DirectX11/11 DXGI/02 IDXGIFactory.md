# IDXGIFactory
DXGI 객체와 리소스를 생성하는 팩토리 인터페이스다. 스왑 체인, 어댑터, 디바이스 등을 생성할 수 있다. 이를 통해 그래픽 어댑터와 출력 장치를 관리하고, 스왑 체인을 생성할 수 있다.

IDXGIFactory는 CreateDXGIFactory 함수를 이용하여 생성할 수 있다.

```cpp
ComPtr<IDXGIFactory> dxgiFactory;
HRESULT hr = CreateDXGIFactory(__uuidof(IDXGIFactory), &dxgiFactory);
```

주요 역할은 다음과 같다.

## DXGI 객체 생성
IDXGIFactory는 다양한 DXGI 객체를 생성하는 기능을 제공한다. 이를 통해 그래픽 어댑터, 스왑 체인, 출력 장치 등을 생성하고 관리할 수 있다.

주요 생성 메서드로는 CreateSwapChain, CreateSoftwareAdapter등이 있다.

## 그래픽 어댑터 나열
시스템에 설치된 그래픽 어댑터를 나열하고, 각 어댑터의 속성을 쿼리할 수 있다. 이를 통해 애플리케이션은 사용 가능한 그래픽 하드웨어를 파악하고, 적절한 어댑터를 선택할 수 있다.

설치된 그래픽 어댑터는 `EnumAdapters` 메서드로 얻을 수 있으며, 인덱스를 통해 각 어댑터를 쿼리할 수 있다. `EnumAdapters`의 시그니쳐는 다음과 같다.
```cpp
HRESULT EnumAdapters(
    UINT Adapter,
    IDXGIAdapter **ppAdapter
);
```

## 디스플레이 모드 관리
어댑터에 연결된 출력 장치(모니터 등)의 디스플레이 모드를 쿼리하고 설정할 수 있다. 이를 통해 다양한 해상도와 리프레시율 설정을 관리할 수 있다.

## 스왑 체인 관리
스왑 체인을 생성하고 관리하는 기능을 제공한다. 스왑 체인은 화면에 렌더링된 프레임을 표시하는 데 사용된다.

스왑 체인은 CreateSwapChain 메서드로 생성하며 시그니처는 다음과 같다.
```cpp
HRESULT CreateSwapChain(
    IUnknown *pDevice,
    DXGI_SWAP_CHAIN_DESC *pDesc,
    IDXGISwapChain **ppSwapChain
);
```