# ID3D11Query
`ID3D11Query`는 Direct3D 11에서 쿼리 객체를 나타내는 인터페이스다. 

이 클래스는 GPU의 특정 상태나 성능 데이터를 질의하고, 이를 통해 렌더링 파이프라인의 성능을 분석하거나 디버깅하는 데 사용된다. 

쿼리 객체는 다양한 유형이 있으며, 각 쿼리는 특정한 목적을 가지고 있다.

이 클래스는 `ID3D11Device`를 사용하여 생성된다. 

이를 위해 `D3D11_QUERY_DESC` 구조체를 사용하여 쿼리의 속성을 정의하고, 해당 속성에 따라 쿼리 객체를 초기화한다. 그런 다음, `CreateQuery` 메서드를 호출하여 쿼리 객체를 생성한다.

`ID3D11Query`는 `ID3D11DeviceContext`를 통해 그래픽 파이프라인에 사용된다. 

쿼리를 시작하고 종료하는 명령을 내릴 수 있으며, 이를 통해 GPU에서 쿼리를 실행한다. 

예를 들어, `Begin` 메서드를 호출하여 쿼리를 시작하고, `End` 메서드를 호출하여 쿼리를 종료할 수 있다. 

쿼리의 결과는 `GetData` 메서드를 사용하여 비동기적으로 수집할 수 있다.

이 인터페이스는 다양한 쿼리 유형을 지원하여, 특정한 GPU 작업의 상태를 확인하거나 성능 데이터를 수집하는 데 유용하다. 예를 들어, `D3D11_QUERY_OCCLUSION` 쿼리는 특정 드로우 콜이 얼마나 많은 픽셀을 렌더링했는지 측정하고, `D3D11_QUERY_TIMESTAMP` 쿼리는 GPU에서 특정 이벤트가 발생한 시간을 측정할 수 있다. 이러한 데이터를 통해 렌더링 파이프라인의 성능을 분석하고, 최적화 기회를 식별할 수 있다.

`ID3D11Query`는 Direct3D 11 애플리케이션에서 성능 분석과 디버깅을 지원하는 중요한 도구로서, 개발자가 GPU의 상태와 성능을 이해하고 최적화할 수 있도록 돕는다.

## D3D11_QUERY_DESC 구조체
D3D11_QUERY_DESC 구조체는 Direct3D 11에서 쿼리 객체를 정의하기 위한 구조체이다. 

쿼리 객체는 GPU에서 특정 이벤트가 발생했는지 여부를 확인하거나, 통계 정보를 수집하는 데 사용된다.

정의는 다음과 같다.

```cpp
typedef struct D3D11_QUERY_DESC {
    D3D11_QUERY Query;
    UINT MiscFlags;
} D3D11_QUERY_DESC;
```

각각의 멤버변수는 다음과 같다.

* D3D11_QUERY Query
  * 쿼리의 유형을 지정한다.
  * 가능한 값
    * enum D3D11_QUERY 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT MiscFlags
  * 추가적인 옵션을 지정하는 플래그이다.
  * 가능한 값
    * 0: 특별한 옵션 없음
    * D3D11_QUERY_MISC_PREDICATEHINT: 쿼리 결과를 예측하는 힌트를 제공
  * 기본값은 0이다.

## TIMESTAMP_DISJOINT vs TIMESTAMP
D3D11_QUERY_TIMESTAMP_DISJOINT 쿼리는 GPU 클럭 주파수와 타임스탬프 값의 유효성을 확인하는 데 사용된다. 이 쿼리는 타이밍 범위 전체에 대해 유효성을 검사하기 때문에 Begin과 End 메서드 모두에 사용된다. 이를 통해 특정 구간 동안 GPU의 클럭 주파수가 일정하게 유지되었는지를 확인할 수 있다.

D3D11_QUERY_TIMESTAMP 쿼리는 특정 시점의 GPU 타임스탬프 값을 기록하는 데 사용된다. 이 쿼리는 단순히 특정 시점의 타임스탬프를 기록하기 때문에 End 메서드에만 사용된다. Begin 메서드는 필요하지 않다.

## D3D11_QUERY_DATA_TIMESTAMP_DISJOINT 구조체
D3D11_QUERY_DATA_TIMESTAMP_DISJOINT 구조체는 Direct3D 11에서 타임스탬프의 불연속 여부를 확인하기 위한 쿼리의 결과 데이터를 저장하는 구조체이다. 

이 구조체는 GPU 타이밍 정보의 유효성을 판단하는 데 사용된다.

정의는 다음과 같다.

```cpp
typedef struct D3D11_QUERY_DATA_TIMESTAMP_DISJOINT {
    UINT64 Frequency;
    BOOL Disjoint;
} D3D11_QUERY_DATA_TIMESTAMP_DISJOINT;
```

각각의 멤버변수는 다음과 같다.

* UINT64 Frequency
  * 타이밍 간격의 주파수를 나타낸다. GPU 타이머의 주파수를 Hz 단위로 지정한다.

* BOOL Disjoint
  * 타임스탬프가 불연속 상태인지 여부를 나타낸다. TRUE면 불연속 상태, FALSE면 연속 상태이다.
    * TRUE: 쿼리의 ID3D11DeviceContext::Begin 및 ID3D11DeviceContext::End 호출 사이에 문제가 발생하여 랩톱에서 AC 코드의 플러깅, 과열 또는 랩톱 절약 이벤트로 인한 업/다운 제한과 같이 타임스탬프 카운터가 불연속적이거나 연결되지 않음을 의미한다.
    * FALSE: 타임스탬프가 연속 상태임을 나타내며, 타임스탬프 쿼리에 대한 결과를 신뢰할 수 있음을 의미한다.

> Reference  
> [learn.microsoft - d3d11/ns-d3d11-d3d11_query_data_timestamp_disjoint](https://learn.microsoft.com/ko-kr/windows/win32/api/d3d11/ns-d3d11-d3d11_query_data_timestamp_disjoint)