# enum D3D11_TEXTURE_ADDRESS_MODE 
D3D11_TEXTURE_ADDRESS_MODE enum은 텍스처 좌표가 텍스처 범위를 벗어났을 때 텍스처를 샘플링하는 방법을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_TEXTURE_ADDRESS_MODE
    {
        D3D11_TEXTURE_ADDRESS_WRAP          = 1,
        D3D11_TEXTURE_ADDRESS_MIRROR        = 2,
        D3D11_TEXTURE_ADDRESS_CLAMP         = 3,
        D3D11_TEXTURE_ADDRESS_BORDER        = 4,
        D3D11_TEXTURE_ADDRESS_MIRROR_ONCE   = 5
    } 	D3D11_TEXTURE_ADDRESS_MODE;
```
각 enum의 의미는 다음과 같다.

* D3D11_TEXTURE_ADDRESS_WRAP
  * 텍스처 좌표가 범위를 벗어날 때, 좌표를 텍스처 크기로 나눈 나머지를 사용한다. 즉, 텍스처가 반복된다.
* D3D11_TEXTURE_ADDRESS_MIRROR
  * 텍스처 좌표가 범위를 벗어날 때, 좌표를 텍스처 크기로 나눈 몫의 홀짝 여부에 따라 좌표를 반사시킨다. 즉, 텍스처가 거울처럼 반복된다.
* D3D11_TEXTURE_ADDRESS_CLAMP
  * 텍스처 좌표가 범위를 벗어날 때, 가장자리 좌표를 사용한다. 즉, 텍스처가 고정된 가장자리 값을 사용한다.
* D3D11_TEXTURE_ADDRESS_BORDER
  * 텍스처 좌표가 범위를 벗어날 때, 지정된 테두리 색상을 사용한다.
* D3D11_TEXTURE_ADDRESS_MIRROR_ONCE
  * 텍스처 좌표가 범위를 처음 벗어날 때는 반사시키고, 그 이후에는 고정된 가장자리 좌표를 사용한다.