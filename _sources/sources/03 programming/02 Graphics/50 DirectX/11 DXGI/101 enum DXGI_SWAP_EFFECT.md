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