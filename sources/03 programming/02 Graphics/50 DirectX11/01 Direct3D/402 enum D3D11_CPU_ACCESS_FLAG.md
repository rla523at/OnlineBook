# enum D3D11_CPU_ACCESS_FLAG
정의는 다음과 같다.

```cpp
typedef 
enum D3D11_CPU_ACCESS_FLAG
    {
        D3D11_CPU_ACCESS_WRITE = 0x10000L,
        D3D11_CPU_ACCESS_READ  = 0x20000L
    } 	D3D11_CPU_ACCESS_FLAG;
```

각 enum의 의미는 다음과 같다.
* D3D11_CPU_ACCESS_WRITE
  * 리소스를 CPU에서 쓸 수 있다.
* D3D11_CPU_ACCESS_READ
  * 리소스를 CPU에서 읽을 수 있다.
