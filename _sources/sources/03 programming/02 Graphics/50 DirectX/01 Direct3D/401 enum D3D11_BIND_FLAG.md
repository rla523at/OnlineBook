# enum D3D11_BIND_FLAG
D3D11_BIND_FLAG enum은 Direct3D 11에서 리소스를 파이프라인에 바인딩할 때 사용되는 방법을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_BIND_FLAG
    {
        D3D11_BIND_VERTEX_BUFFER     = 0x1L,
        D3D11_BIND_INDEX_BUFFER      = 0x2L,
        D3D11_BIND_CONSTANT_BUFFER   = 0x4L,
        D3D11_BIND_SHADER_RESOURCE   = 0x8L,
        D3D11_BIND_STREAM_OUTPUT     = 0x10L,
        D3D11_BIND_RENDER_TARGET     = 0x20L,
        D3D11_BIND_DEPTH_STENCIL     = 0x40L,
        D3D11_BIND_UNORDERED_ACCESS  = 0x80L,
        D3D11_BIND_DECODER           = 0x200L,
        D3D11_BIND_VIDEO_ENCODER     = 0x400L
    } 	D3D11_BIND_FLAG;
```
각 enum의 의미는 다음과 같다.

* D3D11_BIND_VERTEX_BUFFER
  * 버퍼를 Vertex Buffer로 바인딩할 수 있다.
* D3D11_BIND_INDEX_BUFFER
  * 버퍼를 Index Buffer로 바인딩할 수 있다.
* D3D11_BIND_CONSTANT_BUFFER
  * 버퍼를 Constant Buffer로 바인딩할 수 있다.
* D3D11_BIND_SHADER_RESOURCE
  * 리소스를 셰이더 리소스로 바인딩할 수 있다.
* D3D11_BIND_STREAM_OUTPUT
  * 버퍼를 Stream Output Target으로 바인딩할 수 있다.
* D3D11_BIND_RENDER_TARGET
  * 리소스를 Render Target으로 바인딩할 수 있다.
* D3D11_BIND_DEPTH_STENCIL
  * 리소스를 Depth Stencil Buffer로 바인딩할 수 있다.
* D3D11_BIND_UNORDERED_ACCESS
  * 리소스를 Unordered Access View로 바인딩할 수 있다.
* D3D11_BIND_DECODER
  * 리소스를 비디오 디코더 출력으로 바인딩할 수 있다.
* D3D11_BIND_VIDEO_ENCODER
  * 리소스를 비디오 인코더 입력으로 바인딩할 수 있다.