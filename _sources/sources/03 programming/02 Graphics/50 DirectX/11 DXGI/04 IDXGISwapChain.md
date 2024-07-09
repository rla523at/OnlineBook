# IDXGISwapChain
`IDXGISwapChain`은 DirectX Graphics Infrastructure(DXGI)에서 스왑 체인을 나타내는 인터페이스다. 이 클래스는 렌더링된 프레임을 화면에 표시하는 데 사용되며, 더블 버퍼링 또는 트리플 버퍼링을 구현하는 데 필수적이다.

`IDXGISwapChain`의 주요 역할은 백 버퍼와 프론트 버퍼 사이에서 프레임을 교환하는 것이다. 더블 버퍼링에서는 하나의 백 버퍼와 하나의 프론트 버퍼가 사용되며, 트리플 버퍼링에서는 두 개의 백 버퍼와 하나의 프론트 버퍼가 사용된다. 이러한 버퍼링 기법을 통해 화면 깜빡임을 최소화하고, 부드러운 화면 전환을 제공한다.

또한, `IDXGISwapChain`은 디스플레이 모드 설정을 관리한다. 이 클래스는 화면 해상도, 리프레시 레이트 등의 설정을 조정할 수 있으며, 이를 통해 다양한 디스플레이 환경에서 최적의 성능을 발휘할 수 있다. 예를 들어, 전체 화면 모드와 창 모드 간 전환도 `IDXGISwapChain`을 통해 쉽게 구현할 수 있다.

`IDXGISwapChain`은 프레젠테이션(화면에 프레임을 표시하는 과정)을 관리하는 기능도 제공한다. `Present` 메서드를 호출하여 백 버퍼의 내용을 프론트 버퍼로 전송하고, 이를 통해 렌더링된 프레임이 실제 화면에 표시된다. 이 과정에서 동기화 옵션을 설정하여 화면 찢어짐 현상을 방지할 수 있다.

이 클래스는 Direct3D 장치와 밀접하게 연동되며, `ID3D11Device`와 함께 사용하여 그래픽 파이프라인의 최종 출력을 관리한다. `IDXGISwapChain`을 사용하여 렌더 타겟 뷰를 생성하고, 이를 통해 렌더링 결과를 백 버퍼에 출력할 수 있다.

## DXGI_SWAP_CHAIN_DESC 구조체
DXGI_SWAP_CHAIN_DESC 구조체는 DirectX Graphics Infrastructure(DXGI)에서 스왑 체인의 특성을 정의하기 위한 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct DXGI_SWAP_CHAIN_DESC {
    DXGI_MODE_DESC BufferDesc;
    DXGI_SAMPLE_DESC SampleDesc;
    DXGI_USAGE BufferUsage;
    UINT BufferCount;
    HWND OutputWindow;
    BOOL Windowed;
    DXGI_SWAP_EFFECT SwapEffect;
    UINT Flags;
} DXGI_SWAP_CHAIN_DESC;
```

각각의 멤버변수는 다음과 같다.

* DXGI_MODE_DESC BufferDesc
  * 백 버퍼의 디스플레이 모드를 기술한다.
  * 가능한 값
      * DXGI_MODE_DESC 구조체를 사용하여 해상도, 리프레시 레이트, 포맷 등을 지정한다.
  * 기본값은 없다. 사용자가 지정해야 한다.

* DXGI_SAMPLE_DESC SampleDesc
  * 멀티샘플링의 파라미터를 기술한다.
  * 가능한 값
      * DXGI_SAMPLE_DESC 구조체를 사용하여 샘플 수와 품질 수준을 지정한다.
  * 기본값은 없다. 사용자가 지정해야 한다.

* DXGI_USAGE BufferUsage
  * 스왑 체인 버퍼의 용도를 지정한다.
  * 가능한 값
      * DXGI_USAGE_RENDER_TARGET_OUTPUT: 렌더 타겟 출력용
      * DXGI_USAGE_SHADER_INPUT: 셰이더 입력용
  * 기본값은 DXGI_USAGE_RENDER_TARGET_OUTPUT이다.

* UINT BufferCount
  * 스왑 체인에서 사용하는 버퍼의 수를 지정한다.
  * 가능한 값
      * 1 이상의 정수
  * 기본값은 1이다.

* HWND OutputWindow
  * 스왑 체인이 출력을 보낼 창의 핸들이다.
  * 가능한 값
      * 유효한 HWND 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* BOOL Windowed
  * 창 모드 또는 전체 화면 모드를 지정한다.
  * 가능한 값
      * TRUE: 창 모드
      * FALSE: 전체 화면 모드
  * 기본값은 TRUE이다.

* DXGI_SWAP_EFFECT SwapEffect
  * 스왑 체인이 플립하는 방식을 지정한다.
  * 가능한 값
      * DXGI_SWAP_EFFECT_DISCARD: 내용 폐기
      * DXGI_SWAP_EFFECT_SEQUENTIAL: 순차적 플립
  * 기본값은 DXGI_SWAP_EFFECT_DISCARD이다.

* UINT Flags
  * 스왑 체인의 옵션을 지정하는 플래그이다.
  * 가능한 값
      * DXGI_SWAP_CHAIN_FLAG_ALLOW_MODE_SWITCH: 모드 전환 허용
  * 기본값은 0이다.

### 주의사항
Direct3D 11에서 멀티샘플링을 사용할 때, back buffer와 depth-stencil buffer의 멀티샘플링 설정이 일치해야 한다.

따라서, 여기서 사용하는 SampleDesc와 depth stencil buffer를 만들 때 사용하는 SampleDesc가 동일해야 한다.

## DXGI_SAMPLE_DESC 구조체
DXGI_SAMPLE_DESC 구조체는 멀티샘플링에 대한 샘플 수와 품질 수준을 정의하기 위한 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct DXGI_SAMPLE_DESC {
    UINT Count;
    UINT Quality;
} DXGI_SAMPLE_DESC;
```

각각의 멤버변수는 다음과 같다.

* UINT Count
  * 멀티샘플링에 사용할 샘플 수를 지정한다.
  * 가능한 값
      * 1 이상의 정수 값
      * 1일 경우 멀티샘플링이 비활성화된다.
  * 기본값은 1이다.

* UINT Quality
  * 멀티샘플링의 품질 수준을 지정한다.
  * 가능한 값
      * 각 샘플 수에 대해 지원되는 품질 수준은 하드웨어에 따라 다르다. 일반적으로 0에서 지원하는 최대 품질 수준 사이의 값을 사용한다.
  * 기본값은 0이다.


## DXGI_MODE_DESC 구조체
DXGI_MODE_DESC 구조체는 디스플레이 모드의 특성을 정의하기 위한 구조체이다.

정의는 다음과 같다.

```cpp
typedef struct DXGI_MODE_DESC {
    UINT Width;
    UINT Height;
    DXGI_RATIONAL RefreshRate;
    DXGI_FORMAT Format;
    DXGI_MODE_SCANLINE_ORDER ScanlineOrdering;
    DXGI_MODE_SCALING Scaling;
} DXGI_MODE_DESC;
```

각각의 멤버변수는 다음과 같다.

* UINT Width
  * 디스플레이 모드의 가로 해상도를 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
  * 기본값은 0이다.

* UINT Height
  * 디스플레이 모드의 세로 해상도를 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
  * 기본값은 0이다.

* DXGI_RATIONAL RefreshRate
  * 디스플레이 모드의 리프레시 레이트를 지정한다.
  * 가능한 값
      * DXGI_RATIONAL 구조체를 사용하여 분자(Numerator)와 분모(Denominator)로 지정한다.
      * 예: 60Hz의 경우 Numerator = 60, Denominator = 1
  * 기본값은 Numerator = 0, Denominator = 1이다.
    * Numerator가 0일 경우 refresh rate를 자동으로 결정한다.

* DXGI_FORMAT Format
  * 디스플레이 모드의 픽셀 포맷을 지정한다.
  * 가능한 값
      * enum DXGI_FORMAT 값
  * 기본값은 DXGI_FORMAT_UNKNOWN이다.

* DXGI_MODE_SCANLINE_ORDER ScanlineOrdering
  * 디스플레이 모드의 스캔라인 순서를 지정한다.
  * 가능한 값
      * DXGI_MODE_SCANLINE_ORDER_UNSPECIFIED: 지정되지 않음
      * DXGI_MODE_SCANLINE_ORDER_PROGRESSIVE: 순차적 스캔
      * DXGI_MODE_SCANLINE_ORDER_UPPER_FIELD_FIRST: 상위 필드 우선
      * DXGI_MODE_SCANLINE_ORDER_LOWER_FIELD_FIRST: 하위 필드 우선
  * 기본값은 DXGI_MODE_SCANLINE_ORDER_UNSPECIFIED이다.

* DXGI_MODE_SCALING Scaling
  * 디스플레이 모드의 스케일링을 지정한다.
  * 가능한 값
      * DXGI_MODE_SCALING_UNSPECIFIED: 지정되지 않음
      * DXGI_MODE_SCALING_CENTERED: 중심에 맞춤
      * DXGI_MODE_SCALING_STRETCHED: 화면에 맞춤
  * 기본값은 DXGI_MODE_SCALING_UNSPECIFIED이다.

### DXGI_RATIONAL RefreshRate
디스플레이의 재생 빈도(리프레시 레이트)를 정의하는 변수이다.

디스플레이가 화면을 갱신하는 빈도를 나타내며 단위는 Hz(헤르츠)이다.
예를 들어 60Hz는 디스플레이가 초당 60번 화면을 갱신함을 의미하며 모니터의 물리적인 특성에 따라 결정된다. 일반적으로 60Hz, 75Hz, 120Hz, 144Hz 등이 있다.

DXGI_MODE_DESC 구조체에서 RefreshRate.Numerator가 0이면, DirectX는 디스플레이의 재생 빈도를 자동으로 결정한다. 일반적으로 이는 운영 체제나 드라이버가 기본 재생 빈도로 설정된 값을 사용하게 된다. 이러한 설정은 일반적으로 최적의 재생 빈도를 의미하며, 주로 모니터의 기본값이나 최적의 재생 빈도로 설정된다.

만약 RefreshRate.Numerator를 10, RefreshRate.Denominator를 1로 설정하면 전체 화면 모드에서 모니터가 10Hz로 동작하도록 시도할 수 있다. 그러나, 대부분의 현대 모니터는 60Hz, 120Hz, 144Hz 등의 고정 재생 빈도를 지원하며, 10Hz와 같은 매우 낮은 재생 빈도는 지원하지 않는다. 따라서, 이런 설정이 실제로 적용되지 않을 수 있다.

참고로 Frame Rate는 GPU가 새로운 프레임을 생성하는 빈도로, 단위는 FPS(Frames Per Second)이며, 예를 들어 60FPS는 GPU가 초당 60개의 프레임을 생성함을 의미한다.

## DXGI_USAGE Flag
DXGI_USAGE는 DirectX Graphics Infrastructure (DXGI)에서 리소스의 사용 용도를 정의하는 플래그다. 

이 플래그는 리소스가 어떻게 사용될지를 지정하여, 그래픽 파이프라인에서 효율적으로 리소스를 관리하고 접근할 수 있도록 한다. 

여러 플래그를 비트 OR 연산으로 결합하여 하나의 리소스가 여러 용도로 사용될 수 있도록 지정할 수 있다.

플래그는 다음과 같다.

* DXGI_USAGE_SHADER_INPUT
  * 리소스가 셰이더에서 입력으로 사용될 수 있음을 나타낸다.
  * 주로 텍스처나 버퍼가 셰이더에서 샘플링되거나 읽히는 경우에 사용된다.

* DXGI_USAGE_RENDER_TARGET_OUTPUT
  * 리소스가 렌더 타겟으로 사용될 수 있음을 나타낸다.
  * 이는 렌더링 출력 결과를 저장하는 용도로 사용되며, 주로 렌더 타겟 뷰(RTV)로 설정된다.

* DXGI_USAGE_BACK_BUFFER
  * 리소스가 스왑 체인의 백 버퍼로 사용될 수 있음을 나타낸다.
  * 백 버퍼는 화면에 렌더링된 이미지를 저장하고, 프레젠트 작업을 통해 화면에 표시된다.

* DXGI_USAGE_SHARED
  * 리소스가 여러 Direct3D 디바이스 간에 공유될 수 있음을 나타낸다.
  * 주로 여러 디바이스에서 동일한 리소스를 사용할 필요가 있을 때 설정된다.

* DXGI_USAGE_READ_ONLY
  * 리소스가 읽기 전용으로 사용될 수 있음을 나타낸다.
  * 이는 주로 데이터를 읽기만 하고 쓰지 않는 경우에 사용되며, 읽기 작업의 성능 최적화를 위해 사용될 수 있다.

* DXGI_USAGE_DISCARD_ON_PRESENT
  * 프레젠트 작업 시 리소스가 폐기될 수 있음을 나타낸다.
  * 이는 새로운 데이터를 렌더링할 때 이전 데이터를 유지할 필요가 없는 경우에 사용된다.

* DXGI_USAGE_UNORDERED_ACCESS
  * 리소스가 무순서 접근이 가능함을 나타낸다.
  * 이는 주로 컴퓨트 셰이더 등에서 무순서 접근으로 읽기/쓰기가 필요한 경우에 사용된다.

## 프레임 버퍼 관리
스왑 체인은 하나 이상의 백 버퍼와 하나의 프론트 버퍼로 구성된다. 프론트 버퍼는 현재 화면에 표시되는 버퍼이고, 백 버퍼는 다음 프레임을 렌더링할 버퍼다. 렌더링이 완료되면 백 버퍼와 프론트 버퍼를 교환(swap)하여 화면에 새로운 프레임을 표시한다.

이 때, 하나의 프론트 버퍼와 하나의 백 버퍼를 사용하여 렌더링이 완료되면 두 버퍼를 교환하는 방식을 더블 버퍼링이라고 한다.
그리고 프론트 버퍼와 두 개의 백 버퍼를 사용하여 더 부드러운 애니메이션과 입력 지연을 줄이는 방식을 트리플 버퍼링이라고 한다.

이처럼, 스왑 체인은 두 개 이상의 프레임 버퍼를 관리하여, 하나의 프레임을 디스플레이에 표시하는 동안 다른 프레임을 준비할 수 있게 한다. 이를 통해 화면의 깜빡임을 줄이고 부드러운 애니메이션을 제공할 수 있다.

프레임 버퍼 관리와 관련해 주요 메서드들은 다음과 같다.

### ResizeBuffers
스왑 체인의 버퍼 크기, 개수 또는 포맷을 변경할 수 있다.
```cpp
HRESULT ResizeBuffers(
    UINT BufferCount,
    UINT Width,
    UINT Height,
    DXGI_FORMAT NewFormat,
    UINT SwapChainFlags
);
```

이 때, 각 매개 변수는 다음과 같다.
* BufferCount
  * 새로 생성할 버퍼의 개수.
* Width
  * 버퍼의 새로운 너비.
* Height
  * 버퍼의 새로운 높이.
* NewFormat
  * 버퍼의 새로운 포맷.
* SwapChainFlags
  * 추가적인 스왑 체인 옵션.

### ResizeTarget
스왑 체인의 출력 창 크기를 변경한다.
```cpp
HRESULT ResizeTarget(
    const DXGI_MODE_DESC *pNewTargetParameters
);
```

이 때, 각 매개 변수는 다음과 같다.
* pNewTargetParameters
  * 새로운 출력 창 크기를 지정하는 DXGI_MODE_DESC 구조체.

### GetBuffer
스왑 체인의 백 버퍼에 대한 포인터를 얻는다.

```cpp
HRESULT GetBuffer(
    UINT Buffer,
    REFIID riid,
    void **ppSurface
);
```

이 떄, 각 매게 변수는 다음과 같다.
* Buffer
  * 가져올 버퍼의 인덱스.
* riid
  * 요청할 인터페이스의 식별자.
* ppSurface
  * 요청된 인터페이스 포인터를 받는 변수.

## Present 멤버함수
Present 함수는 IDXGISwapChain 인터페이스의 멤버 함수로, 백 버퍼의 내용을 화면에 표시하는 함수다. 이 함수는 더블 버퍼링을 통해 렌더링된 프레임을 스왑 체인에 연결된 디스플레이 장치에 출력한다.

시그니처는 다음과 같다.
```cpp
HRESULT IDXGISwapChain::Present(
    UINT SyncInterval,
    UINT Flags
);
```
매개변수는 다음과 같다.

* UINT SyncInterval
  * 수직 동기화 간격을 지정
  * 사용 가능한 값
    * 0: 수직 동기화를 하지 않는다.
    * 1 이상: 수직 동기화 간격 (예: 1은 매 프레임 수직 동기화, 2는 두 번째 프레임마다 수직 동기화)
  * 기본값: 1

* UINT Flags
  * 표시 옵션을 지정
  * 사용 가능한 값
    * 0: 기본 설정
    * DXGI_PRESENT_DO_NOT_WAIT: 스왑 체인이 차단되지 않도록 한다.
    * DXGI_PRESENT_RESTART: 프레젠트 작업을 다시 시작한다.
    * DXGI_PRESENT_TEST: 화면 표시 없이 오류 테스트를 수행한다.
  * 기본값: 0

### SyncInterval
SyncInterval 인자는 수직 동기화 간격을 지정하며, 각 값에 따라 프레임 레이트와 V-Sync의 동작이 달라진다.

0은 V-Sync를 비활성화한다. 프레임은 가능한 한 빨리 렌더링되고 표시된다. 이 설정은 화면 잘림(screen tearing) 현상을 유발할 수 있지만, 최대 성능을 추구할 때 사용된다.

1은 V-Sync를 활성화하고, 프레임을 모니터의 수직 동기화 주기(V-Blank)에 맞춘다. 모니터가 60Hz인 경우, 프레임 레이트가 60FPS로 제한된다.

2는 프레임을 두 번째 수직 동기화 주기마다 표시한다. 모니터가 60Hz인 경우, 프레임 레이트가 30FPS로 제한된다.

3은 프레임을 세 번째 수직 동기화 주기마다 표시한다. 모니터가 60Hz인 경우, 프레임 레이트가 20FPS로 제한된다.

4는 프레임을 네 번째 수직 동기화 주기마다 표시한다. 모니터가 60Hz인 경우, 프레임 레이트가 15FPS로 제한된다.

....

### 수직동기화
수직 동기화(V-Sync)는 그래픽 렌더링 기술 중 하나로, 그래픽 카드(GPU)가 생성하는 프레임을 모니터의 재생 빈도에 맞추어 출력하는 기술이다. 이를 통해 화면 잘림(screen tearing) 현상을 방지하고 더 부드러운 시각적 경험을 제공한다.

화면 잘림은 GPU가 프레임을 렌더링하는 속도와 모니터가 화면을 갱신하는 속도가 일치하지 않을 때 발생한다. 이로 인해 모니터가 한 번의 화면 갱신 주기 내에 두 개 이상의 다른 프레임을 받게 되어, 화면에 두 개 이상의 프레임이 겹쳐 보이게 된다. 이는 특히 빠르게 움직이는 장면에서 잘 드러난다.

## 전체 화면 모드 전환
`SetFullscreenState` 메서드를 통해 전체 화면 모드와 창 모드 간의 전환을 관리할 수 있다. 이를 통해 게임이나 멀티미디어 애플리케이션에서 유연하게 화면 모드를 전환할 수 있다.
```cpp
HRESULT SetFullscreenState(
    BOOL Fullscreen,
    IDXGIOutput *pTarget
);
```

이 때, 각 매개 변수는 다음과 같다.
* Fullscreen
  * 전체 화면 모드로 전환할지 여부를 지정한다.
* pTarget
  * 전체 화면 모드에서 사용할 출력 장치.


### IDXGIOutput
디스플레이 출력을 나타내는 인터페이스로, 모니터와 같은 출력 장치에 대한 정보와 제어를 제공한다. 연결된 모니터의 해상도, 리프레시율 등의 정보를 얻고 설정할 수 있다.