# DxcBuffer

`DXCBuffer`는 `dxcapi.h`에 정의된 구조체로, DXC 컴파일러에서 데이터를 처리하기 위한 버퍼를 나타낸다. 

이 구조체는 HLSL 코드나 기타 바이너리 데이터를 담고, 이를 컴파일하거나 처리하는 과정에서 사용된다. 

DXCBuffer 구조체 정의는 다음과 같다.

```cpp
typedef struct DXCBuffer {
    LPCVOID Ptr;   // 버퍼의 시작 지점을 가리키는 포인터
    SIZE_T Size;   // 버퍼의 크기(바이트 단위)
    UINT32 Encoding; // 버퍼의 인코딩 방식 (일반적으로 0 또는 CP_UTF8 사용)
} DXCBuffer;
```

각 멤버변수는 다음과 같다.
* LPCVOID Ptr
  * 버퍼의 데이터를 가리키는 포인터이다. 이 포인터는 일반적으로 HLSL 코드나 바이너리 데이터가 위치한 메모리를 가리킨다.

* SIZE_T Size
  * 버퍼에 저장된 데이터의 크기(바이트 단위)이다. 이 값은 컴파일러가 데이터를 처리하는 데 필요한 크기를 알려준다.

* UINT32 Encoding
  * 버퍼에 저장된 데이터의 인코딩 방식을 나타낸다.
  * Binary 나 ANsi Text 또는 Autodetect UTF with BOM 인경우 DXC_CP_ACP 값을 사용한다.
  * UTF8 : DXC_CP_UTF8
  * UTF16 : DXC_CP_UTF16
