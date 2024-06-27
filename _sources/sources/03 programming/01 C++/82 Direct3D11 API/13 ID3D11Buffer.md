# ID3D11Buffer

ID3D11Buffer는 Direct3D 11에서 다양한 데이터 버퍼를 나타내는 인터페이스다. 이 인터페이스는 그래픽 파이프라인에서 데이터를 효율적으로 관리하고 전송하기 위해 사용된다.

ID3D11Buffer의 주요 역할은 다양한 유형의 버퍼를 생성하고 관리하는 것이다. 이를 위해 ID3D11Device를 사용하여 버퍼를 생성하고, ID3D11DeviceContext를 통해 버퍼 데이터를 갱신하거나 바인딩한다.

또한, ID3D11Buffer는 그래픽 파이프라인에서 데이터 전송의 효율성을 극대화하는 책임을 진다. 이를 통해 그래픽 애플리케이션은 높은 성능을 유지하면서 복잡한 3D 씬을 렌더링할 수 있다. 예를 들어, 정점 데이터와 인덱스 데이터를 효율적으로 관리하여 다각형 모델을 빠르게 그릴 수 있으며, 상수 데이터를 신속하게 업데이트하여 실시간 렌더링의 동적인 변화를 지원한다.

ID3D11Buffer는 세 가지 주요 유형의 버퍼를 지원하며, 각각의 역할과 책임이 다르다.

첫 번째 유형은 버텍스 버퍼로, 3D 모델의 정점 데이터를 저장하는 데 사용된다. 버텍스 버퍼는 각 정점의 위치, 색상, 텍스처 좌표 등 다양한 속성을 포함하며, 그래픽 카드가 빠르게 접근하여 처리할 수 있도록 한다. 이를 통해 3D 모델이 화면에 렌더링된다.

두 번째 유형은 인덱스 버퍼로, 정점 데이터의 인덱스를 저장하여 그래픽 파이프라인이 동일한 정점을 여러 번 사용할 수 있게 한다. 인덱스 버퍼를 사용하면 정점 데이터를 중복으로 저장하지 않아도 되므로 메모리 사용 효율이 향상되고, 렌더링 성능이 개선된다.

세 번째 유형은 상수 버퍼로, 셰이더 프로그램에 전달되는 상수 데이터를 저장하는 데 사용된다. 상수 버퍼는 주로 매 프레임마다 업데이트되는 데이터(예: 변환 행렬, 조명 정보 등)를 포함하며, 셰이더가 이 데이터를 참조하여 그래픽 연산을 수행할 수 있도록 한다.

## D3D11_BUFFER_DESC 구조체
버퍼의 생성 시에는 용도, 크기, 접근 권한 등을 정의하는 D3D11_BUFFER_DESC 구조체를 사용한다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_BUFFER_DESC {
    UINT ByteWidth;
    D3D11_USAGE Usage;
    UINT BindFlags;
    UINT CPUAccessFlags;
    UINT MiscFlags;
    UINT StructureByteStride;
} D3D11_BUFFER_DESC;
```
각각의 멤버변수는 다음과 같다.

* UINT ByteWidth
  * 버퍼의 크기를 바이트 단위로 지정한다.
  * 가능한 값
    * 1 이상의 정수 값
  * 기본값은 없다. 사용자가 지정해야 한다.

* D3D11_USAGE Usage
  * 버퍼의 사용 방법을 지정한다.
  * 가능한 값
    * D3D11_USAGE_DEFAULT: 기본 사용 방법
    * D3D11_USAGE_IMMUTABLE: 변경 불가
    * D3D11_USAGE_DYNAMIC: 자주 업데이트
    * D3D11_USAGE_STAGING: CPU 접근 가능
  * 기본값은 D3D11_USAGE_DEFAULT이다.

* UINT BindFlags
  * 버퍼를 바인딩할 파이프라인 단계들을 지정한다.
  * 가능한 값
    * D3D11_BIND_VERTEX_BUFFER: 버텍스 버퍼
    * D3D11_BIND_INDEX_BUFFER: 인덱스 버퍼
    * D3D11_BIND_CONSTANT_BUFFER: 상수 버퍼
  * 기본값은 0이다.

* UINT CPUAccessFlags
  * CPU 접근 권한을 지정한다.
  * 가능한 값
    * D3D11_CPU_ACCESS_WRITE: 쓰기 접근
    * D3D11_CPU_ACCESS_READ: 읽기 접근
  * 기본값은 0이다.

* UINT MiscFlags
  * 추가적인 옵션을 지정한다.
  * 가능한 값
    * D3D11_RESOURCE_MISC_GENERATE_MIPS: MIP 맵 생성 지원
    * D3D11_RESOURCE_MISC_SHARED: 리소스 공유
    * D3D11_RESOURCE_MISC_TEXTURECUBE: 큐브 맵 텍스처
    * D3D11_RESOURCE_MISC_DRAWINDIRECT_ARGS: 간접 그리기 인수 버퍼
    * D3D11_RESOURCE_MISC_BUFFER_ALLOW_RAW_VIEWS: 버퍼의 원시 뷰 허용
    * D3D11_RESOURCE_MISC_BUFFER_STRUCTURED: 구조화된 버퍼
  * 기본값은 0이다.

* UINT StructureByteStride
  * 구조화된 버퍼의 각 요소의 크기를 바이트 단위로 지정한다. 이 값은 구조화된 버퍼에만 적용된다.
  * 가능한 값
    * 0 이상의 정수 값
  * 기본값은 0이다.

## D3D11_USAGE enum
D3D11_USAGE enum은 리소스의 사용 방법을 정의하는 열거형이다.

정의는 다음과 같다.
```cpp
typedef 
enum D3D11_USAGE
    {
        D3D11_USAGE_DEFAULT    = 0,
        D3D11_USAGE_IMMUTABLE  = 1,
        D3D11_USAGE_DYNAMIC    = 2,
        D3D11_USAGE_STAGING    = 3
    } 	D3D11_USAGE;
```
각 enum의 의미는 다음과 같다.

* D3D11_USAGE_DEFAULT
  * 리소스가 GPU에서만 접근 가능하며, 일반적인 용도로 사용된다.
* D3D11_USAGE_IMMUTABLE
  * 리소스가 생성된 후 변경할 수 없으며, 초기화 시 데이터가 설정된다.
* D3D11_USAGE_DYNAMIC
  * 리소스가 CPU에서 자주 갱신될 수 있으며, 주로 매 프레임마다 업데이트되는 데이터에 사용된다.
* D3D11_USAGE_STAGING
  * 리소스가 CPU와 GPU 간의 데이터 전송을 위해 사용되며, 읽기와 쓰기 모두 가능하다.
