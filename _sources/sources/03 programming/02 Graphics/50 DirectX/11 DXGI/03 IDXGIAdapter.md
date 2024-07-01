# IDXGIAdapter
그래픽 어댑터를 나타내는 인터페이스로, 어댑터의 속성이나 모드를 쿼리하고 설정할 수 있다. 이를 통해 애플리케이션은 그래픽 카드의 메모리, 성능, 지원하는 디스플레이 모드 등의 정보를 얻을 수 있다.

주요 역할은 다음과 같다.

## 그래픽 어댑터 정보 제공
IDXGIAdapter는 시스템에 설치된 그래픽 어댑터의 정보를 제공한다. 이를 통해 애플리케이션은 사용 가능한 그래픽 하드웨어의 성능과 특성을 파악할 수 있다.

`GetDesc` 메서드를 이용하여 어댑터에 관한 정보를 담은 DXGI_ADAPTER_DESC 구조체를 얻을 수 있다.
```cpp
HRESULT GetDesc(DXGI_ADAPTER_DESC *pDesc);
```

DXGI_ADAPTER_DESC 구조체는 다음과 같이 정의되어 있다.
```cpp
typedef struct DXGI_ADAPTER_DESC {
    WCHAR Description[128];
    UINT VendorId;
    UINT DeviceId;
    UINT SubSysId;
    UINT Revision;
    SIZE_T DedicatedVideoMemory;
    SIZE_T DedicatedSystemMemory;
    SIZE_T SharedSystemMemory;
    LUID AdapterLuid;
} DXGI_ADAPTER_DESC;
```

각각의 멤버 변수는 다음과 같은 의미를 갖는다.

* Description
  * 어댑터의 이름을 나타낸다.
* VendorId
  * 벤더 ID를 나타낸다.
* DeviceId
  * 디바이스 ID를 나타낸다.
* SubSysId
  * 서브 시스템 ID를 나타낸다.
* Revision
  * 리비전 번호를 나타낸다.
* DedicatedVideoMemory
  * 전용 비디오 메모리 크기를 나타낸다.
* DedicatedSystemMemory
  * 전용 시스템 메모리 크기를 나타낸다.
* SharedSystemMemory
  * 공유 시스템 메모리 크기를 나타낸다.
* AdapterLuid
  * 어댑터의 로컬 고유 식별자(LUID)를 나타낸다.

## 출력 관리
어댑터에 연결된 디스플레이 출력 장치(모니터 등)에 대한 정보를 제공하고, 이를 관리할 수 있다.

`EnumOutputs` 메서드를 이용하여 어댑터에 연결된 출력 장치를 나열하고 이를 통해 여러 출력 장치(모니터 등)를 관리할 수 있다.

```cpp
HRESULT EnumOutputs(UINT Output, IDXGIOutput **ppOutput);
```