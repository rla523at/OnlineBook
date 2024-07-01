# D3D11_SUBRESOURCE_DATA 구조체
D3D11_SUBRESOURCE_DATA 구조체는 서브 리소스 데이터를 초기화하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_SUBRESOURCE_DATA {
    const void *pSysMem;
    UINT SysMemPitch;
    UINT SysMemSlicePitch;
} D3D11_SUBRESOURCE_DATA;
```
각각의 멤버변수는 다음과 같다.

* const void *pSysMem
  * 서브 리소스의 초기화 데이터를 가리키는 포인터이다.
  * 가능한 값
      * 초기화할 데이터의 메모리 주소
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT SysMemPitch
  * 텍스처 데이터의 한 행(row)을 바이트 단위로 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
  * 기본값은 0이다. 텍스처가 아닌 경우 0으로 설정할 수 있다.

* UINT SysMemSlicePitch
  * 3D 텍스처 데이터의 한 슬라이스(slice)를 바이트 단위로 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
  * 기본값은 0이다. 2D 텍스처 또는 1D 텍스처인 경우 0으로 설정할 수 있다.