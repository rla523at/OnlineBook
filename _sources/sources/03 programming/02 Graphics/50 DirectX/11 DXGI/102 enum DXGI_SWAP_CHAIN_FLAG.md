# enum DXGI_SWAP_CHAIN_FLAG
DXGI_SWAP_CHAIN_FLAG enum은 DXGI 스왑 체인의 특정 동작을 제어하는 플래그를 정의하는 열거형이다.

정의는 다음과 같다.

```cpp
typedef enum DXGI_SWAP_CHAIN_FLAG
{
    DXGI_SWAP_CHAIN_FLAG_NONPREROTATED         = 1 << 0,
    DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH     = 1 << 1,
    DXGI_SWAP_CHAIN_FLAG_GDI_COMPATIBLE        = 1 << 2,
    DXGI_SWAP_CHAIN_FLAG_RESTRICTED_CONTENT    = 1 << 3,
    DXGI_SWAP_CHAIN_FLAG_RESTRICT_SHARED_RESOURCE_DRIVER = 1 << 4,
    DXGI_SWAP_CHAIN_FLAG_DISPLAY_ONLY          = 1 << 5,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT = 1 << 6,
    DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER      = 1 << 7,
    DXGI_SWAP_CHAIN_FLAG_FULLSCREEN_VIDEO      = 1 << 8,
    DXGI_SWAP_CHAIN_FLAG_YUV_VIDEO             = 1 << 9,
    DXGI_SWAP_CHAIN_FLAG_HW_PROTECTED          = 1 << 10,
    DXGI_SWAP_CHAIN_FLAG_ALLOW_TEARING         = 1 << 11,
    DXGI_SWAP_CHAIN_FLAG_RESTRICTED_TO_ALL_HOLOGRAPHIC_DISPLAYS = 1 << 12
} DXGI_SWAP_CHAIN_FLAG;
```

각 enum의 의미는 다음과 같다.

* DXGI_SWAP_CHAIN_FLAG_NONPREROTATED
  * 스왑 체인에서 사전 회전을 허용하지 않는다.
* DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH
  * 스왑 체인이 모드 전환을 허용한다.
* DXGI_SWAP_CHAIN_FLAG_GDI_COMPATIBLE
  * 스왑 체인이 GDI 호환성을 가진다.
* DXGI_SWAP_CHAIN_FLAG_RESTRICTED_CONTENT
  * 제한된 콘텐츠가 있는 경우 스왑 체인을 제한한다.
* DXGI_SWAP_CHAIN_FLAG_RESTRICT_SHARED_RESOURCE_DRIVER
  * 공유 리소스에 대한 드라이버 제한이 있는 경우 스왑 체인을 제한한다.
* DXGI_SWAP_CHAIN_FLAG_DISPLAY_ONLY
  * 표시 전용 스왑 체인을 나타낸다.
* DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
  * 프레임 대기 가능 개체를 지원하는 스왑 체인이다.
* DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER
  * 전경 레이어에 스왑 체인이 있다.
* DXGI_SWAP_CHAIN_FLAG_FULLSCREEN_VIDEO
  * 전체 화면 비디오 스왑 체인을 나타낸다.
* DXGI_SWAP_CHAIN_FLAG_YUV_VIDEO
  * YUV 비디오 스왑 체인을 나타낸다.
* DXGI_SWAP_CHAIN_FLAG_HW_PROTECTED
  * 하드웨어로 보호된 스왑 체인이다.
* DXGI_SWAP_CHAIN_FLAG_ALLOW_TEARING
  * 티어링을 허용하는 스왑 체인이다.
* DXGI_SWAP_CHAIN_FLAG_RESTRICTED_TO_ALL_HOLOGRAPHIC_DISPLAYS
  * 모든 홀로그래픽 디스플레이에 제한된 스왑 체인을 나타낸다.

## DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH
이 플래그는 DirectX 그래픽 애플리케이션에서 전체 화면 모드와 창 모드 간의 전환을 허용하는 데 사용된다. 

일반적으로 DirectX 애플리케이션은 전체 화면 모드(Fullscreen Mode)에서 실행될 때 더 높은 성능을 제공하지만, 사용자는 일반적으로 창 모드(Windowed Mode)에서 더 많은 편의성을 제공받기를 원한다. 

이 플래그를 설정하면 사용자가 애플리케이션의 창 모드에서 전체 화면 모드로 전환할 수 있습니다.

## DXGI_SWAP_CHAIN_FLAG_ALLOW_TEARING
이 플래그는 화면의 수직 동기화(V-Sync)를 무시하고 그래픽 카드가 화면을 업데이트할 때마다 그림을 그리는 방식을 지원한다.