# D3D11_MAPPED_SUBRESOURCE
D3D11_MAPPED_SUBRESOURCE는 Direct3D 11에서 리소스의 서브 리소스를 매핑할 때 사용되는 구조체다. 이 구조체는 매핑된 리소스 데이터에 접근하기 위한 정보를 제공한다. 매핑은 주로 CPU가 GPU 리소스에 접근하여 데이터를 읽거나 쓸 수 있도록 하는데 사용된다.

구조체의 정의는 다음과 같다.
```cpp
typedef struct D3D11_MAPPED_SUBRESOURCE {
    void *pData;
    UINT RowPitch;
    UINT DepthPitch;
} D3D11_MAPPED_SUBRESOURCE;
```

각 멤버변수는 다음과 같다.
* pData
  * 매핑된 서브 리소스의 데이터에 대한 포인터다.
  * 이 포인터를 통해 리소스의 데이터에 접근할 수 있다.
* RowPitch
  * 텍스처 데이터의 행(row) 간의 바이트 수를 나타낸다.
  * 2D 텍스처의 경우, 한 행의 시작 지점과 다음 행의 시작 지점 간의 바이트 단위 오프셋을 나타낸다.
* DepthPitch
  * 3D 텍스처 데이터의 깊이 슬라이스 간의 바이트 수를 나타낸다.
  * 한 깊이 슬라이스의 시작 지점과 다음 깊이 슬라이스의 시작 지점 간의 바이트 단위 오프셋을 나타낸다.

주의사항은 다음과 같다.
* RowPitch
  * RowPitch 값은 Map 메소드 호출 시 Direct3D가 자동으로 설정하는 값이며, 이는 매핑된 리소스의 실제 메모리 레이아웃을 반영한다. 
  * RowPitch는 GPU 메모리 정렬 요구사항을 포함한 각 행(row)의 바이트 수를 나타내며, 이 값을 수동으로 수정한다고 해서 Direct3D가 사용하는 메모리 레이아웃이 변경되는 것은 아니다.