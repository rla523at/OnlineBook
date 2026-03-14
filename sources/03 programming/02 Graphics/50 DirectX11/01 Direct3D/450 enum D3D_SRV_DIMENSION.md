# enum D3D_SRV_DIMENSION
D3D_SRV_DIMENSION enum은 셰이더 리소스 뷰(Shader Resource View, SRV)의 차원을 정의하는 열거형이다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D_SRV_DIMENSION
    {
        D3D_SRV_DIMENSION_UNKNOWN            = 0,
        D3D_SRV_DIMENSION_BUFFER             = 1,
        D3D_SRV_DIMENSION_TEXTURE1D          = 2,
        D3D_SRV_DIMENSION_TEXTURE1DARRAY     = 3,
        D3D_SRV_DIMENSION_TEXTURE2D          = 4,
        D3D_SRV_DIMENSION_TEXTURE2DARRAY     = 5,
        D3D_SRV_DIMENSION_TEXTURE2DMS        = 6,
        D3D_SRV_DIMENSION_TEXTURE2DMSARRAY   = 7,
        D3D_SRV_DIMENSION_TEXTURE3D          = 8,
        D3D_SRV_DIMENSION_TEXTURECUBE        = 9,
        D3D_SRV_DIMENSION_TEXTURECUBEARRAY   = 10,
        D3D_SRV_DIMENSION_BUFFEREX           = 11
    } 	D3D_SRV_DIMENSION;
```

각 enum의 의미는 다음과 같다.

* D3D_SRV_DIMENSION_UNKNOWN
  * 차원이 정의되지 않았다.
* D3D_SRV_DIMENSION_BUFFER
  * SRV가 버퍼로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE1D
  * SRV가 1D 텍스처로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE1DARRAY
  * SRV가 1D 텍스처 배열로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE2D
  * SRV가 2D 텍스처로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE2DARRAY
  * SRV가 2D 텍스처 배열로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE2DMS
  * SRV가 멀티샘플링된 2D 텍스처로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE2DMSARRAY
  * SRV가 멀티샘플링된 2D 텍스처 배열로 구성된다.
* D3D_SRV_DIMENSION_TEXTURE3D
  * SRV가 3D 텍스처로 구성된다.
* D3D_SRV_DIMENSION_TEXTURECUBE
  * SRV가 큐브 맵 텍스처로 구성된다.
* D3D_SRV_DIMENSION_TEXTURECUBEARRAY
  * SRV가 큐브 맵 텍스처 배열로 구성된다.
* D3D_SRV_DIMENSION_BUFFEREX
  * 확장된 버퍼로 구성된다.
