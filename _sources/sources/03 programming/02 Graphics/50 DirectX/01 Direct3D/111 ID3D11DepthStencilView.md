# ID3D11DepthStencilView 
ID3D11DepthStencilView는 depthStencilBuffer를 관리하고 이를 렌더링 파이프라인에 바인딩하는 데 사용되는 인터페이스다. 

## Depth Buffer and Stenctil Buffer
`깊이 버퍼(Depth Buffer)`는 각 픽셀의 깊이 값을 저장하는 버퍼다. 깊이 값은 카메라에서 해당 픽셀까지의 거리로 표현된다. 깊이 버퍼를 사용하면 화면에 가까운 객체가 먼 객체를 가릴 수 있도록 깊이 테스트를 수행할 수 있다.

`스텐실 버퍼(Stenctil Buffer)`는 각 픽셀의 스텐실 값을 저장하는 버퍼다. 스텐실 테스트는 특정 픽셀을 포함하거나 제외시키는 다양한 효과를 구현하는 데 사용된다.

## D3D11_DEPTH_STENCIL_VIEW_DESC 구조체
D3D11_DEPTH_STENCIL_VIEW_DESC 구조체는 Direct3D 11에서 깊이-스텐실 뷰를 정의하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_DEPTH_STENCIL_VIEW_DESC {
    DXGI_FORMAT Format;
    D3D11_DSV_DIMENSION ViewDimension;
    UINT Flags;
    union {
        D3D11_TEX1D_DSV Texture1D;
        D3D11_TEX1D_ARRAY_DSV Texture1DArray;
        D3D11_TEX2D_DSV Texture2D;
        D3D11_TEX2D_ARRAY_DSV Texture2DArray;
        D3D11_TEX2DMS_DSV Texture2DMS;
        D3D11_TEX2DMS_ARRAY_DSV Texture2DMSArray;
    };
} D3D11_DEPTH_STENCIL_VIEW_DESC;
```
각각의 멤버변수는 다음과 같다.

* DXGI_FORMAT Format
  * 깊이-스텐실 뷰의 포맷을 지정한다.
  * 가능한 값
    * DXGI_FORMAT_D24_UNORM_S8_UINT: 24비트 깊이와 8비트 스텐실
    * DXGI_FORMAT_D32_FLOAT: 32비트 부동 소수점 깊이
  * 기본값은 DXGI_FORMAT_UNKNOWN이다.

* D3D11_DSV_DIMENSION ViewDimension
  * 깊이-스텐실 뷰의 차원을 지정한다.
  * 가능한 값
    * enum D3D11_DSV_DIMENSION 값
  * 기본값은 D3D11_DSV_DIMENSION_UNKNOWN이다.

* UINT Flags
  * 깊이-스텐실 뷰의 플래그를 지정한다.
  * 가능한 값
    * enum D3D11_DSV_FLAG 값
  * 기본값은 0이다.

* union
  * 깊이-스텐실 뷰의 상세 설정을 위한 유니온이다. 각 타입에 따라 구조체를 사용한다.
  * 가능한 값과 각 값이 갖는 의미
    * D3D11_TEX1D_DSV Texture1D: 1D 텍스처에 대한 설정
    * D3D11_TEX1D_ARRAY_DSV Texture1DArray: 1D 텍스처 배열에 대한 설정
    * D3D11_TEX2D_DSV Texture2D: 2D 텍스처에 대한 설정
    * D3D11_TEX2D_ARRAY_DSV Texture2DArray: 2D 텍스처 배열에 대한 설정
    * D3D11_TEX2DMS_DSV Texture2DMS: 멀티샘플 2D 텍스처에 대한 설정
    * D3D11_TEX2DMS_ARRAY_DSV Texture2DMSArray: 멀티샘플 2D 텍스처 배열에 대한 설정
  * 기본값은 없다. 각 ViewDimension에 따라 적절한 값을 설정해야 한다.

### D3D11_TEX1D_DSV (1D 텍스처)
```cpp
typedef struct D3D11_TEX1D_DSV {
    UINT MipSlice;
} D3D11_TEX1D_DSV;
```
* MipSlice: 사용할 MIP 슬라이스를 지정한다.

### D3D11_TEX1D_ARRAY_DSV (1D 텍스처 배열)
```cpp
typedef struct D3D11_TEX1D_ARRAY_DSV {
    UINT MipSlice;
    UINT FirstArraySlice;
    UINT ArraySize;
} D3D11_TEX1D_ARRAY_DSV;
```
* MipSlice: 사용할 MIP 슬라이스를 지정한다.
* FirstArraySlice: 첫 번째 배열 슬라이스를 지정한다.
* ArraySize: 배열의 크기를 지정한다.

### D3D11_TEX2D_DSV (2D 텍스처)
```cpp
typedef struct D3D11_TEX2D_DSV {
    UINT MipSlice;
} D3D11_TEX2D_DSV;
```
* MipSlice: 사용할 MIP 슬라이스를 지정한다.

### D3D11_TEX2D_ARRAY_DSV (2D 텍스처 배열)
```cpp
typedef struct D3D11_TEX2D_ARRAY_DSV {
    UINT MipSlice;
    UINT FirstArraySlice;
    UINT ArraySize;
} D3D11_TEX2D_ARRAY_DSV;
```
* MipSlice: 사용할 MIP 슬라이스를 지정한다.
* FirstArraySlice: 첫 번째 배열 슬라이스를 지정한다.
* ArraySize: 배열의 크기를 지정한다.

### D3D11_TEX2DMS_DSV (멀티샘플링된 2D 텍스처)
``` cpp
typedef struct D3D11_TEX2DMS_DSV {
    // No members
} D3D11_TEX2DMS_DSV;
```

### D3D11_TEX2DMS_ARRAY_DSV (멀티샘플링된 2D 텍스처 배열)
```cpp
typedef struct D3D11_TEX2DMS_ARRAY_DSV {
    UINT FirstArraySlice;
    UINT ArraySize;
} D3D11_TEX2DMS_ARRAY_DSV;
```
* FirstArraySlice: 첫 번째 배열 슬라이스를 지정한다.
* ArraySize: 배열의 크기를 지정한다.


### D3D11_DEPTH_STENCIL_VIEW_DESC의 기본값
ID3D11Device의 CreateDepthStencilView함수로 ID3D11DepthStencilView을 생성할 때, D3D11_DEPTH_STENCIL_VIEW_DESC를 NULL로 주게 되면 D3D11_DEPTH_STENCIL_VIEW_DESC의 기본값으로 ID3D11DepthStencilView가 생성이된다.

이 때, D3D11_DEPTH_STENCIL_VIEW_DESC의 기본값은 다음과 같이 정해진다.

* Format
  * 텍스처 리소스의 형식과 일치
* ViewDimension
  * 텍스처 리소스의 차원에 따라 자동으로 설정
* Flags
  * 0
* Union
  * 텍스처 리소스의 차원에 따라 자동으로 설정 및 초기화
  * D3D11_TEX1D_DSV (1D 텍스처)
    * UINT MipSlice;  // 기본값: 0
  * D3D11_TEX1D_ARRAY_DSV (1D 텍스처 배열)
    * UINT MipSlice;        // 기본값: 0
    * UINT FirstArraySlice; // 기본값: 0
    * UINT ArraySize;       // 기본값: 1 (배열 크기 1)
  * D3D11_TEX2D_DSV (2D 텍스처)
    * UINT MipSlice;  // 기본값: 0
  * D3D11_TEX2D_ARRAY_DSV (2D 텍스처 배열)
    * UINT MipSlice;        // 기본값: 0
    * UINT FirstArraySlice; // 기본값: 0
    * UINT ArraySize;       // 기본값: 1 (배열 크기 1)
  * D3D11_TEX2DMS_DSV (멀티샘플링된 2D 텍스처)
    * 멀티샘플링된 2D 텍스처는 추가 멤버가 없다
  * D3D11_TEX2DMS_ARRAY_DSV (멀티샘플링된 2D 텍스처 배열)
    * UINT FirstArraySlice; // 기본값: 0
    * UINT ArraySize;       // 기본값: 1 (배열 크기 1)