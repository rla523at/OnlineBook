# enum D3D_DRIVER_TYPE
D3D_DRIVER_TYPE enum은 Direct3D 디바이스를 생성할 때 사용할 드라이버 유형을 정의하는 열거형이다.

이 열거형은 Direct3D 디바이스가 사용할 드라이버의 종류를 지정한다.

정의는 다음과 같다.

```cpp
typedef 
enum D3D_DRIVER_TYPE
    {
        D3D_DRIVER_TYPE_UNKNOWN     = 0,
        D3D_DRIVER_TYPE_HARDWARE    = 1,
        D3D_DRIVER_TYPE_REFERENCE   = 2,
        D3D_DRIVER_TYPE_NULL        = 3,
        D3D_DRIVER_TYPE_SOFTWARE    = 4,
        D3D_DRIVER_TYPE_WARP        = 5
    } 	D3D_DRIVER_TYPE;
```

각 enum의 의미는 다음과 같다.

* D3D_DRIVER_TYPE_UNKNOWN
  * 드라이버 유형이 정의되지 않았다.
* D3D_DRIVER_TYPE_HARDWARE
  * 하드웨어 가속을 사용하는 디바이스 드라이버를 지정한다.
* D3D_DRIVER_TYPE_REFERENCE
  * 소프트웨어 래스터라이저를 사용하는 디바이스 드라이버로, 정확한 결과를 제공하지만 매우 느리다.
* D3D_DRIVER_TYPE_NULL
  * 렌더링을 수행하지 않는 디바이스 드라이버로, 디버깅 목적으로 사용된다.
* D3D_DRIVER_TYPE_SOFTWARE
  * 사용자가 제공한 소프트웨어 드라이버를 지정한다.
* D3D_DRIVER_TYPE_WARP
  * Windows 고급 래스터라이저 플랫폼(WARP)을 사용하는 소프트웨어 드라이버를 지정한다. 이는 고성능 소프트웨어 래스터라이저이다.
