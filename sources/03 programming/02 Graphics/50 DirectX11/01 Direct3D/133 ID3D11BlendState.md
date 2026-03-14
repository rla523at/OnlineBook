# ID3D11BlendState
`ID3D11BlendState`는 Direct3D 11에서 블렌딩 상태를 관리하는 인터페이스다. 이 클래스는 렌더링 파이프라인에서 픽셀 색상 결합 방식을 정의하며, 다양한 블렌딩 효과를 구현하는 데 사용된다. 블렌딩은 주로 투명도와 색상 결합을 처리하는 데 중요하며, 이를 통해 복잡한 시각 효과를 구현할 수 있다.

`ID3D11BlendState`의 주요 역할은 블렌딩 상태를 정의하고 관리하는 것이다. 블렌딩 상태는 소스 색상(현재 렌더링 중인 색상)과 대상 색상(이미 렌더 타겟에 존재하는 색상)을 결합하는 방식을 설정한다. 이를 통해 투명도, 색상 추가, 색상 곱셈 등의 다양한 효과를 적용할 수 있다. 예를 들어, 반투명한 물체를 렌더링할 때, 소스 색상의 알파 값에 따라 대상 색상과 결합하여 투명한 효과를 낼 수 있다.

이 클래스는 `ID3D11Device`를 사용하여 생성된다. 이를 위해 `D3D11_BLEND_DESC` 구조체를 사용하여 블렌딩 상태의 속성을 정의하고, 해당 속성에 따라 블렌드 상태 객체를 초기화한다. 이 구조체는 블렌드 활성화 여부, 블렌드 연산, 소스 및 대상 블렌드 팩터, 알파 블렌드 옵션 등을 포함한다. 그런 다음, `CreateBlendState` 메서드를 호출하여 블렌드 상태 객체를 생성한다.

`ID3D11BlendState`는 `ID3D11DeviceContext`를 통해 그래픽 파이프라인에 바인딩된다. 이를 통해 블렌딩 상태가 활성화되고, 렌더링 작업이 수행되는 동안 설정된 블렌딩 방식을 적용할 수 있다. 예를 들어, `OMSetBlendState` 메서드를 사용하여 현재 활성화된 블렌드 상태를 설정하고, 이후의 드로우 콜이 실행될 때 해당 블렌드 상태가 적용된다.

이 인터페이스는 다양한 그래픽 효과를 구현하는 데 필수적이다. 블렌딩 상태를 정확히 설정하고 관리함으로써, 투명도 처리, 다중 렌더 타겟에서의 색상 결합, 포스트 프로세싱 효과 등을 효율적으로 구현할 수 있다. `ID3D11BlendState`는 Direct3D 11 애플리케이션에서 고품질의 그래픽 렌더링을 지원하는 중요한 구성 요소다.

## Blending의 기본 개념
블렌딩은 두 색상을 혼합하여 최종 색상을 계산하는 과정이다. 

이는 주로 반투명 효과를 구현하거나 여러 레이어의 이미지를 합성할 때 사용된다. 

블렌딩 연산은 다음과 같은 일반적인 수식으로 표현된다

```
Result = (SourceColor * SrcBlend) op (DestinationColor * DestBlend)
```

이때, 어떤 op은 어떤 operator를 사용할지 D3D11_RENDER_TARGET_BLEND_DESC의 Blend Op 멤버변수로부터 결정된다.

## D3D11_BLEND_DESC 구조체
D3D11_BLEND_DESC 구조체는 Direct3D 11에서 블렌딩 상태를 정의하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_BLEND_DESC {
    BOOL AlphaToCoverageEnable;
    BOOL IndependentBlendEnable;
    D3D11_RENDER_TARGET_BLEND_DESC RenderTarget[8];
} D3D11_BLEND_DESC;
```
각각의 멤버변수는 다음과 같다.

* BOOL AlphaToCoverageEnable
  * 알파-투-커버리지(Alpha-to-Coverage)를 사용할지 여부를 지정한다.
  * 가능한 값
    * TRUE: 알파-투-커버리지 활성화
    * FALSE: 알파-투-커버리지 비활성화
  * 기본값은 FALSE이다.

* BOOL IndependentBlendEnable
  * 렌더 타겟 별로 독립적인 블렌딩을 사용할지 여부를 지정한다.
  * 가능한 값
    * TRUE: 각 렌더 타겟에서 독립적인 블렌딩 활성화
    * FALSE: 동일한 블렌딩 상태 사용
  * 기본값은 FALSE이다.

* D3D11_RENDER_TARGET_BLEND_DESC RenderTarget[8]
  * 각 렌더 타겟에 대한 블렌딩 상태를 지정한다.
  * 가능한 값
    * D3D11_RENDER_TARGET_BLEND_DESC 구조체를 사용하여 각 렌더 타겟의 블렌딩 설정을 지정
  * 기본값은 모든 요소가 기본 D3D11_RENDER_TARGET_BLEND_DESC 값이다.

### AlphaToCoverageEnable
AlphaToCoverageEnable은 Direct3D 11에서 멀티샘플링 안티앨리어싱(MSAA)과 관련된 옵션이다. 이 옵션은 알파 값에 따라 커버리지 마스크를 생성하여 픽셀의 부분적 투명성을 보다 정확하게 표현하는 데 사용된다.

* TRUE: 알파-투-커버리지 활성화
  * 알파 값에 기반하여 MSAA 샘플이 부분적으로 커버되도록 하여, 투명한 물체의 가장자리에서 부드러운 트랜지션을 제공한다.
* FALSE: 알파-투-커버리지 비활성화
  * 표준 블렌딩만 사용하며, MSAA 샘플이 픽셀 전체에 대해 동일하게 적용된다.

### D3D11_RENDER_TARGET_BLEND_DESC 구조체
D3D11_RENDER_TARGET_BLEND_DESC 구조체는 Direct3D 11에서 각 렌더 타겟에 대한 블렌딩 상태를 정의하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_RENDER_TARGET_BLEND_DESC {
    BOOL BlendEnable;
    D3D11_BLEND SrcBlend;
    D3D11_BLEND DestBlend;
    D3D11_BLEND_OP BlendOp;
    D3D11_BLEND SrcBlendAlpha;
    D3D11_BLEND DestBlendAlpha;
    D3D11_BLEND_OP BlendOpAlpha;
    UINT8 RenderTargetWriteMask;
} D3D11_RENDER_TARGET_BLEND_DESC;
```

각각의 멤버변수는 다음과 같다.

* BOOL BlendEnable
  * 블렌딩을 활성화할지 여부를 지정한다.
  * 가능한 값
    * TRUE: 블렌딩 활성화
    * FALSE: 블렌딩 비활성화
  * 기본값은 FALSE이다.

* D3D11_BLEND SrcBlend
  * 색상 블렌딩에 사용할 소스 블렌드 팩터를 지정한다.
  * 가능한 값
    * D3D11_BLEND_ZERO: (0, 0, 0, 0)
    * D3D11_BLEND_ONE: (1, 1, 1, 1)
    * D3D11_BLEND_SRC_COLOR: (Rs, Gs, Bs, As)
    * D3D11_BLEND_INV_SRC_COLOR: (1-Rs, 1-Gs, 1-Bs, 1-As)
    * 그 외 다양한 블렌드 팩터들
  * 기본값은 D3D11_BLEND_ONE이다.

* D3D11_BLEND DestBlend
  * 색상 블렌딩에 사용할 대상 블렌드 팩터를 지정한다.
  * 가능한 값
    * D3D11_BLEND_ZERO: (0, 0, 0, 0)
    * D3D11_BLEND_ONE: (1, 1, 1, 1)
    * D3D11_BLEND_SRC_COLOR: (Rs, Gs, Bs, As)
    * D3D11_BLEND_INV_SRC_COLOR: (1-Rs, 1-Gs, 1-Bs, 1-As)
    * 그 외 다양한 블렌드 팩터들
  * 기본값은 D3D11_BLEND_ZERO이다.

* D3D11_BLEND_OP BlendOp
  * 색상 블렌딩에 사용할 블렌드 연산을 지정한다.
  * 가능한 값
    * D3D11_BLEND_OP_ADD: (SrcBlend * SrcBlendFactor) + (DestBlend * DestBlendFactor)
    * D3D11_BLEND_OP_SUBTRACT: (SrcBlend * SrcBlendFactor) - (DestBlend * DestBlendFactor)
    * D3D11_BLEND_OP_REV_SUBTRACT: (DestBlend * DestBlendFactor) - (SrcBlend * SrcBlendFactor)
    * D3D11_BLEND_OP_MIN: 최소값 선택
    * D3D11_BLEND_OP_MAX: 최대값 선택
  * 기본값은 D3D11_BLEND_OP_ADD이다.

* D3D11_BLEND SrcBlendAlpha
  * 알파 블렌딩에 사용할 소스 블렌드 팩터를 지정한다.
  * 가능한 값
    * D3D11_BLEND_ZERO: (0, 0, 0, 0)
    * D3D11_BLEND_ONE: (1, 1, 1, 1)
    * D3D11_BLEND_SRC_ALPHA: (As, As, As, As)
    * D3D11_BLEND_INV_SRC_ALPHA: (1-As, 1-As, 1-As, 1-As)
    * 그 외 다양한 블렌드 팩터들
  * 기본값은 D3D11_BLEND_ONE이다.

* D3D11_BLEND DestBlendAlpha
  * 알파 블렌딩에 사용할 대상 블렌드 팩터를 지정한다.
  * 가능한 값
    * D3D11_BLEND_ZERO: (0, 0, 0, 0)
    * D3D11_BLEND_ONE: (1, 1, 1, 1)
    * D3D11_BLEND_SRC_ALPHA: (As, As, As, As)
    * D3D11_BLEND_INV_SRC_ALPHA: (1-As, 1-As, 1-As, 1-As)
    * 그 외 다양한 블렌드 팩터들
  * 기본값은 D3D11_BLEND_ZERO이다.

* D3D11_BLEND_OP BlendOpAlpha
  * 알파 블렌딩에 사용할 블렌드 연산을 지정한다.
  * 가능한 값
    * D3D11_BLEND_OP_ADD: (SrcBlendAlpha * SrcBlendFactorAlpha) + (DestBlendAlpha * DestBlendFactorAlpha)
    * D3D11_BLEND_OP_SUBTRACT: (SrcBlendAlpha * SrcBlendFactorAlpha) - (DestBlendAlpha * DestBlendFactorAlpha)
    * D3D11_BLEND_OP_REV_SUBTRACT: (DestBlendAlpha * DestBlendFactorAlpha) - (SrcBlendAlpha * SrcBlendFactorAlpha)
    * D3D11_BLEND_OP_MIN: 최소값 선택
    * D3D11_BLEND_OP_MAX: 최대값 선택
  * 기본값은 D3D11_BLEND_OP_ADD이다.

* UINT8 RenderTargetWriteMask
  * 렌더 타겟의 각 색상 채널의 쓰기

#### SrcBlend와 SrcBlendAlpha
* SrcBlend는 색상 채널(R, G, B)에 적용되는 블렌드 팩터를 지정한다.
  * 예: D3D11_BLEND_SRC_COLOR는 원본 색상의 R, G, B 값을 사용한다.
* SrcBlendAlpha는 알파 채널(A)에 적용되는 블렌드 팩터를 지정한다.
  * 예: D3D11_BLEND_SRC_ALPHA는 원본 알파 값을 사용한다.