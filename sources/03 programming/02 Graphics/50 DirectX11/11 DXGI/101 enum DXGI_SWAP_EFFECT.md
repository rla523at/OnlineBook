# enum DXGI_SWAP_EFFECT
DXGI_SWAP_EFFECT enum은 스왑 체인에서 사용하는 스왑 효과를 정의하는 열거형이다.

정의는 다음과 같다.

```cpp
typedef 
enum DXGI_SWAP_EFFECT
    {
        DXGI_SWAP_EFFECT_DISCARD      = 0,
        DXGI_SWAP_EFFECT_SEQUENTIAL   = 1,
        DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL = 3,
        DXGI_SWAP_EFFECT_FLIP_DISCARD = 4
    } 	DXGI_SWAP_EFFECT;
```

각 enum의 의미는 다음과 같다.

* DXGI_SWAP_EFFECT_DISCARD
  * 각 프레임이 표시된 후, 백 버퍼의 내용이 폐기된다.
* DXGI_SWAP_EFFECT_SEQUENTIAL
  * 프레임이 순차적으로 표시되며, 각 프레임은 이전 프레임의 내용에 기반할 수 있다.
* DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL
  * 백 버퍼와 프론트 버퍼의 내용이 플립(교환)되며, 순차적으로 표시된다.
* DXGI_SWAP_EFFECT_FLIP_DISCARD
  * 백 버퍼와 프론트 버퍼의 내용이 플립되며, 표시된 후 백 버퍼의 내용이 폐기된다.

## DXGI_SWAP_EFFECT_DISCARD vs DXGI_SWAP_EFFECT_FLIP_DISCARD
**DXGI_SWAP_EFFECT_DISCARD**와 **DXGI_SWAP_EFFECT_FLIP_DISCARD**는 DirectX에서 스왑 체인을 설정할 때 사용하는 두 가지 스왑 효과다. 이 두 가지의 주요 차이점은 스왑 체인이 어떻게 작동하는지, 그리고 각기 다른 하드웨어와 소프트웨어 환경에서 어떻게 최적화되는지에 있다.

### DXGI_SWAP_EFFECT_DISCARD

- **설명**: 가장 기본적인 스왑 효과 중 하나로, DirectX 10과 이전 버전에서 주로 사용되었다.
- **작동 방식**: 
  - 프레임 버퍼의 내용을 다음 프레임에서 재사용하지 않는다.
  - 새로운 프레임이 그려질 때마다 이전 프레임의 데이터는 버려진다.
- **버퍼 관리**:
  - 기본적으로 두 개의 버퍼(프런트 버퍼와 백 버퍼)를 사용한다.
  - 백 버퍼의 내용은 프런트 버퍼로 복사되고, 그 후 백 버퍼의 내용은 버려진다.
- **성능**: 
  - 비교적 단순하지만, 효율적이지 않을 수 있다. 특히 최신 하드웨어와 최적화된 디스플레이 드라이버를 사용하는 경우 더 효율적인 방법이 필요하다.

### DXGI_SWAP_EFFECT_FLIP_DISCARD

- **설명**: DirectX 11.1에서 도입되었으며, 최신 디스플레이 기술과 더 잘 호환된다.
- **작동 방식**: 
  - 스왑 체인이 플립 모델(flip model)을 사용한다. 이는 실제로 프런트 버퍼와 백 버퍼를 전환(flip)하는 방식이다.
  - 프레임이 전환될 때 프런트 버퍼와 백 버퍼가 교체되며, 이전 프런트 버퍼의 내용은 버려진다.
- **버퍼 관리**: 
  - 최소한 두 개 이상의 버퍼가 필요하다. 일반적으로 더블 버퍼링 또는 트리플 버퍼링을 사용한다.
  - 백 버퍼와 프런트 버퍼의 직접 교체로 인해 메모리 복사 작업이 줄어들어 더 효율적이다.
- **성능**: 
  - 최신 하드웨어와 디스플레이 드라이버에서 더 나은 성능과 효율성을 제공한다.
  - 프레임 레이트와 대기 시간을 줄이고, 전반적인 렌더링 성능을 향상시킨다.
  - VSync와 더 잘 작동하며, 화면 찢김 현상을 줄이는 데 도움을 준다.

### 주요 차이점 요약

| 특성                      | DXGI_SWAP_EFFECT_DISCARD              | DXGI_SWAP_EFFECT_FLIP_DISCARD         |
|---------------------------|---------------------------------------|---------------------------------------|
| **도입 시기**             | DirectX 10 및 이전 버전               | DirectX 11.1 및 이후 버전             |
| **버퍼 관리 방식**        | 프레임 복사 후 버퍼 내용 버림         | 프런트 버퍼와 백 버퍼 교체 후 버림    |
| **최소 버퍼 수**          | 2 (프런트 버퍼, 백 버퍼)              | 2 이상 (더블 또는 트리플 버퍼링)     |
| **효율성**                | 최신 하드웨어에서 비효율적일 수 있음  | 최신 하드웨어에서 효율적              |
| **VSync 호환성**          | 상대적으로 덜 최적화됨                | VSync와 더 잘 호환됨                  |
| **일반적인 사용 사례**    | 구형 애플리케이션 및 하드웨어         | 최신 애플리케이션 및 하드웨어         |

DXGI_SWAP_EFFECT_DISCARD와 DXGI_SWAP_EFFECT_FLIP_DISCARD의 주요 차이점은 버퍼 관리 방식과 효율성에 있다. DXGI_SWAP_EFFECT_FLIP_DISCARD는 최신 하드웨어와 더 나은 성능을 제공하며, 최신 애플리케이션에서는 이 방식을 사용하는 것이 권장된다.