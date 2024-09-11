# enum D3D11_RESOURCE_MISC_FLAG
D3D11_RESOURCE_MISC_FLAG enum은 Direct3D 11에서 리소스의 추가적인 특성을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_RESOURCE_MISC_FLAG
    {
        D3D11_RESOURCE_MISC_GENERATE_MIPS              = 0x1L,
        D3D11_RESOURCE_MISC_SHARED                     = 0x2L,
        D3D11_RESOURCE_MISC_TEXTURECUBE                = 0x4L,
        D3D11_RESOURCE_MISC_DRAWINDIRECT_ARGS          = 0x10L,
        D3D11_RESOURCE_MISC_BUFFER_ALLOW_RAW_VIEWS     = 0x20L,
        D3D11_RESOURCE_MISC_BUFFER_STRUCTURED          = 0x40L,
        D3D11_RESOURCE_MISC_RESOURCE_CLAMP             = 0x80L,
        D3D11_RESOURCE_MISC_SHARED_KEYEDMUTEX          = 0x100L,
        D3D11_RESOURCE_MISC_GDI_COMPATIBLE             = 0x200L,
        D3D11_RESOURCE_MISC_SHARED_NTHANDLE            = 0x800L,
        D3D11_RESOURCE_MISC_RESTRICTED_CONTENT         = 0x1000L,
        D3D11_RESOURCE_MISC_RESTRICT_SHARED_RESOURCE   = 0x2000L,
        D3D11_RESOURCE_MISC_RESTRICT_SHARED_RESOURCE_DRIVER = 0x4000L,
        D3D11_RESOURCE_MISC_GUARDED                    = 0x8000L,
        D3D11_RESOURCE_MISC_TILE_POOL                  = 0x20000L,
        D3D11_RESOURCE_MISC_TILED                      = 0x40000L,
        D3D11_RESOURCE_MISC_HW_PROTECTED               = 0x80000L
    } 	D3D11_RESOURCE_MISC_FLAG;
```
각 enum의 의미는 다음과 같다.

* D3D11_RESOURCE_MISC_GENERATE_MIPS
  * 리소스가 MIP 맵 생성을 지원하도록 한다.
* D3D11_RESOURCE_MISC_SHARED
  * 리소스를 여러 장치 간에 공유할 수 있다.
* D3D11_RESOURCE_MISC_TEXTURECUBE
  * 리소스가 큐브 맵 텍스처임을 나타낸다.
* D3D11_RESOURCE_MISC_DRAWINDIRECT_ARGS
  * 리소스를 간접 그리기 명령의 인수 버퍼로 사용할 수 있다.
* D3D11_RESOURCE_MISC_BUFFER_ALLOW_RAW_VIEWS
  * 버퍼 리소스가 raw 뷰를 허용하도록 한다.
* D3D11_RESOURCE_MISC_BUFFER_STRUCTURED
  * 버퍼 리소스가 구조화된 버퍼임을 나타낸다.
* D3D11_RESOURCE_MISC_RESOURCE_CLAMP
  * 리소스 클램핑을 지원하도록 한다.
* D3D11_RESOURCE_MISC_SHARED_KEYEDMUTEX
  * 키드 뮤텍스를 사용하여 리소스를 공유할 수 있다.
* D3D11_RESOURCE_MISC_GDI_COMPATIBLE
  * 리소스가 GDI와 호환되도록 한다.
* D3D11_RESOURCE_MISC_SHARED_NTHANDLE
  * 리소스를 NT 핸들을 사용하여 공유할 수 있다.
* D3D11_RESOURCE_MISC_RESTRICTED_CONTENT
  * 리소스가 제한된 콘텐츠를 포함할 수 있음을 나타낸다.
* D3D11_RESOURCE_MISC_RESTRICT_SHARED_RESOURCE
  * 공유 리소스에 대한 제한을 적용한다.
* D3D11_RESOURCE_MISC_RESTRICT_SHARED_RESOURCE_DRIVER
  * 드라이버가 공유 리소스에 대한 제한을 적용할 수 있도록 한다.
* D3D11_RESOURCE_MISC_GUARDED
  * 리소스가 보호되어 있음을 나타낸다.
* D3D11_RESOURCE_MISC_TILE_POOL
  * 리소스가 타일 풀임을 나타낸다.
* D3D11_RESOURCE_MISC_TILED
  * 리소스가 타일드 리소스임을 나타낸다.
* D3D11_RESOURCE_MISC_HW_PROTECTED
  * 리소스가 하드웨어 보호되어 있음을 나타낸다.
