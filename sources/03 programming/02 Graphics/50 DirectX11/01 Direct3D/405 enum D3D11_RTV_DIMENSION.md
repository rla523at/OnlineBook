# enum D3D11_RTV_DIMENSION
D3D11_RTV_DIMENSION enum은 Render Target View(RTV)의 차원을 정의하는 enum이다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D11_RTV_DIMENSION
    {
        D3D11_RTV_DIMENSION_UNKNOWN            = 0,
        D3D11_RTV_DIMENSION_BUFFER             = 1,
        D3D11_RTV_DIMENSION_TEXTURE1D          = 2,
        D3D11_RTV_DIMENSION_TEXTURE1DARRAY     = 3,
        D3D11_RTV_DIMENSION_TEXTURE2D          = 4,
        D3D11_RTV_DIMENSION_TEXTURE2DARRAY     = 5,
        D3D11_RTV_DIMENSION_TEXTURE2DMS        = 6,
        D3D11_RTV_DIMENSION_TEXTURE2DMSARRAY   = 7,
        D3D11_RTV_DIMENSION_TEXTURE3D          = 8
    } 	D3D11_RTV_DIMENSION;
```

각 enum의 의미는 다음과 같다.
* D3D11_RTV_DIMENSION_UNKNOWN
  * 차원이 정의되지 않았다.
* D3D11_RTV_DIMENSION_BUFFER
  * Render Target이 버퍼로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE1D
  * Render Target이 1D 텍스처로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE1DARRAY
  * Render Target이 1D 텍스처 배열로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2D
  * Render Target이 2D 텍스처로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2DARRAY
  * Render Target이 2D 텍스처 배열로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2DMS
  * Render Target이 멀티샘플링된 2D 텍스처로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE2DMSARRAY
  * Render Target이 멀티샘플링된 2D 텍스처 배열로 구성된다.
* D3D11_RTV_DIMENSION_TEXTURE3D
  * Render Target이 3D 텍스처로 구성된다.