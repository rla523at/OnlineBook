# enum D3D11_QUERY
D3D11_QUERY enum은 Direct3D 11에서 GPU 작업의 상태나 성능을 질의하는 데 사용되는 쿼리 타입을 정의하는 열거형이다.

이 열거형은 다양한 GPU 상태 및 성능 데이터를 검색하는 데 사용된다.

정의는 다음과 같다.

```cpp
typedef enum D3D11_QUERY
{
    D3D11_QUERY_EVENT                        = 0,
    D3D11_QUERY_OCCLUSION                    = 1,
    D3D11_QUERY_TIMESTAMP                    = 2,
    D3D11_QUERY_TIMESTAMP_DISJOINT           = 3,
    D3D11_QUERY_PIPELINE_STATISTICS          = 4,
    D3D11_QUERY_OCCLUSION_PREDICATE          = 5,
    D3D11_QUERY_SO_STATISTICS                = 6,
    D3D11_QUERY_SO_OVERFLOW_PREDICATE        = 7,
    D3D11_QUERY_SO_STATISTICS_STREAM0        = 8,
    D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM0 = 9,
    D3D11_QUERY_SO_STATISTICS_STREAM1        = 10,
    D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM1 = 11,
    D3D11_QUERY_SO_STATISTICS_STREAM2        = 12,
    D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM2 = 13,
    D3D11_QUERY_SO_STATISTICS_STREAM3        = 14,
    D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM3 = 15
} D3D11_QUERY;
```

각 enum의 의미는 다음과 같다.

* D3D11_QUERY_EVENT
  * GPU에서 이벤트가 완료되었는지 여부를 확인한다.
* D3D11_QUERY_OCCLUSION
  * 렌더링된 픽셀이 화면에 표시되었는지 여부를 확인한다.
* D3D11_QUERY_TIMESTAMP
  * GPU의 현재 타임스탬프를 쿼리한다.
* D3D11_QUERY_TIMESTAMP_DISJOINT
  * 타임스탬프가 연속적이지 않음을 나타내는 정보를 쿼리한다.
* D3D11_QUERY_PIPELINE_STATISTICS
  * 파이프라인의 다양한 통계 정보를 쿼리한다.
* D3D11_QUERY_OCCLUSION_PREDICATE
  * 조건부 렌더링을 위한 오클루전 결과를 쿼리한다.
* D3D11_QUERY_SO_STATISTICS
  * 스트림 출력의 통계 정보를 쿼리한다.
* D3D11_QUERY_SO_OVERFLOW_PREDICATE
  * 스트림 출력 버퍼 오버플로 여부를 쿼리한다.
* D3D11_QUERY_SO_STATISTICS_STREAM0
  * 스트림 0의 스트림 출력 통계 정보를 쿼리한다.
* D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM0
  * 스트림 0의 스트림 출력 버퍼 오버플로 여부를 쿼리한다.
* D3D11_QUERY_SO_STATISTICS_STREAM1
  * 스트림 1의 스트림 출력 통계 정보를 쿼리한다.
* D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM1
  * 스트림 1의 스트림 출력 버퍼 오버플로 여부를 쿼리한다.
* D3D11_QUERY_SO_STATISTICS_STREAM2
  * 스트림 2의 스트림 출력 통계 정보를 쿼리한다.
* D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM2
  * 스트림 2의 스트림 출력 버퍼 오버플로 여부를 쿼리한다.
* D3D11_QUERY_SO_STATISTICS_STREAM3
  * 스트림 3의 스트림 출력 통계 정보를 쿼리한다.
* D3D11_QUERY_SO_OVERFLOW_PREDICATE_STREAM3
  * 스트림 3의 스트림 출력 버퍼 오버플로 여부를 쿼리한다.


> Reference  
> [learn.microsoft - d3d11/ne-d3d11-d3d11_query](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d11/ne-d3d11-d3d11_query)