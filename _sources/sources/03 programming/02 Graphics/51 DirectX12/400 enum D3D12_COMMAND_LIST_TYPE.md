# enum D3D12_COMMAND_LIST_TYPE
enum D3D12_COMMAND_LIST_TYPE 은 Direct3D 12에서 CommandList 의 유형을 정의하는 열거형이다.

이 열거형은 CommandList 가 수행할 작업의 종류를 지정한다.

정의는 다음과 같다.

```cpp
typedef enum D3D12_COMMAND_LIST_TYPE
{
    D3D12_COMMAND_LIST_TYPE_DIRECT   = 0,
    D3D12_COMMAND_LIST_TYPE_BUNDLE   = 1,
    D3D12_COMMAND_LIST_TYPE_COMPUTE  = 2,
    D3D12_COMMAND_LIST_TYPE_COPY     = 3,
    D3D12_COMMAND_LIST_TYPE_VIDEO_DECODE = 4,
    D3D12_COMMAND_LIST_TYPE_VIDEO_PROCESS = 5,
    D3D12_COMMAND_LIST_TYPE_VIDEO_ENCODE  = 6
} D3D12_COMMAND_LIST_TYPE;
```

각 enum의 의미는 다음과 같다.

* D3D12_COMMAND_LIST_TYPE_DIRECT
  * 모든 그래픽 및 복사 명령을 실행할 수 있는 디렉트 CommandList이다.
* D3D12_COMMAND_LIST_TYPE_BUNDLE
  * 다른 CommandList에 의해 호출될 수 있는 CommandList으로, 재사용이 가능하다.
* D3D12_COMMAND_LIST_TYPE_COMPUTE
  * 컴퓨팅 셰이더 명령만 실행할 수 있는 컴퓨트 CommandList이다.
* D3D12_COMMAND_LIST_TYPE_COPY
  * 복사 작업만 실행할 수 있는 CommandList이다.
* D3D12_COMMAND_LIST_TYPE_VIDEO_DECODE
  * 비디오 디코딩 작업을 실행할 수 있는 CommandList이다.
* D3D12_COMMAND_LIST_TYPE_VIDEO_PROCESS
  * 비디오 처리 작업을 실행할 수 있는 CommandList이다.
* D3D12_COMMAND_LIST_TYPE_VIDEO_ENCODE
  * 비디오 인코딩 작업을 실행할 수 있는 CommandList이다.
