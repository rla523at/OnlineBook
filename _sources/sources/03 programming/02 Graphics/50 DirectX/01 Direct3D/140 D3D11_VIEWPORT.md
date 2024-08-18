# D3D11_VIEWPORT
D3D11_VIEWPORT는 Direct3D 11에서 `뷰포트(Viewport)`를 정의하는 구조체다. 

뷰포트란 렌더링된 이미지가 화면에 그려지는 영역이며 3D 장면을 2D 화면 공간으로 변환할 때 사용된다. 

뷰포트는 렌더링 파이프라인의 래스터라이저(Rasterizer) 단계에서 중요한 역할을 한다.

구조체의 정의는 다음과 같다.
```cpp
typedef struct D3D11_VIEWPORT {
    FLOAT TopLeftX;
    FLOAT TopLeftY;
    FLOAT Width;
    FLOAT Height;
    FLOAT MinDepth;
    FLOAT MaxDepth;
} D3D11_VIEWPORT;
```

멤버변수는 다음과 같다.
* TopLeftX
  * 뷰포트의 왼쪽 상단 코너의 X 좌표를 정의한다. (픽셀 단위)
* TopLeftY
  * 뷰포트의 왼쪽 상단 코너의 Y 좌표를 정의한다. (픽셀 단위) 
* Width
  * 뷰포트의 너비를 정의한다. (픽셀 단위)  
* Height
  * 뷰포트의 높이를 정의한다. (픽셀 단위)  
* MinDepth
  * 뷰포트의 최소 깊이 값을 정의한다. 일반적으로 0.0f로 설정된다.  
* MaxDepth
  * 뷰포트의 최대 깊이 값을 정의한다. 일반적으로 1.0f로 설정된다.
  * MaxDepth를 0.0f로 설정하면 depth buffering이 동작하지 않는다.