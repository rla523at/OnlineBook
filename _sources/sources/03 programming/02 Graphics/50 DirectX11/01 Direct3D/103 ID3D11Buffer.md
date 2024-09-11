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
    * enum D3D11_USAGE 값
  * 기본값은 D3D11_USAGE_DEFAULT이다.

* UINT BindFlags
  * 버퍼를 바인딩할 파이프라인 단계들을 지정한다.
  * 가능한 값
    * enum D3D11_BIND_FLAG
  * 기본값은 0이다.

* UINT CPUAccessFlags
  * CPU 접근 권한을 지정한다.
  * 가능한 값
    * 0 : 접근 권한 없음
    * enum D3D11_CPU_ACCESS_FLAG 값
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

### USAGE_DYNAMIC & CPU_ACESS_READ
D3D11_BUFFER_DESC의 Usage에 D3D11_USAGE_DYNAMIC을 할당하고 CPUAccessFlags에 D3D11_CPU_ACCESS_READ를 할당하면 CreateBuffer함수에서 다음 오류가 발생한다.

```
A D3D11_USAGE_DYNAMIC Resource may only have the D3D11_CPU_ACCESS_WRITE CPUAccessFlags set.
```

이는 DirectX에서 동시에 사용할 수 없게 막아놓은 조합이다.

왜냐하면 동적으로 업데이트할 수 있는 버퍼를 만들어 놓고 그 데이터를 읽기 전용으로 설정하려는 시도는 모순적인 행동이기 때문이다.

D3D11_USAGE_DYNAMIC은 CPU에서 버퍼의 데이터를 자주 수정하고자 할 때 사용하며 CPU가 버퍼에 대한 읽기와 쓰기 액세스를 가능하게 한다. 이 옵션을 사용하면 GPU와 CPU 간의 데이터 전송이 빠르게 이루어질 수 있어서 동적인 데이터 업데이트가 필요한 경우 유용하다

반면, D3D11_CPU_ACCESS_READ는 데이터를 읽기 위한 목적으로 사용된다. 

> Reference   
> [stackoverflow - error-when-creating-2d-texture-with-dynamic-format](https://stackoverflow.com/questions/56017150/error-when-creating-2d-texture-with-dynamic-format)  

### USAGE_DYNAMIC & UNORDERED_ACCESS
D3D11_BUFFER_DESC의 Usage에 D3D11_USAGE_DYNAMIC을 할당하여 생성한 Buffer의 UAV를 생성하려고 하면 CreateUnorderedAccessView 함수에서 다음과 같은 오류가 발생한다.

```
 A D3D11_USAGE_DYNAMIC Resource cannot be bound to certain parts of the graphics pipeline, but must have at least one BindFlags bit set. The BindFlags bits (0x80) have the following settings: D3D11_BIND_STREAM_OUTPUT (0), D3D11_BIND_RENDER_TARGET (0), D3D11_BIND_DEPTH_STENCIL (0), D3D11_BIND_UNORDERED_ACCESS 
```

오류 내용만 봐서는 무슨 말인지 이해하기 어렵지만, DYNAMIC과 UNORDERED_ACCESS의 의미를 생각해보면 왜 오류가 발생하는지 이해할 수 있다.

D3D11_USAGE_DYNAMIC으로 생성된 버퍼는 CPU에서도 접근 가능하며, CPU에서 데이터를 변경한 후 GPU에서 사용할 수 있도록 설계되어 있다.

반면, CreateUnorderedAccessView 메서드는 GPU에서 데이터를 읽고 쓸 수 있는 Unordered Access View (UAV)를 생성하기 위한 메서드이다.

즉, CPU에서도 GPU에서도 데이터 변경이 가능한 상태가 되며 이런 상태는 비정상적이기 때문에 오류가 발생하는 것이다.

> Reference  
> [gamedev - dynamic-buffers-and-compute-shaders](https://gamedev.net/forums/topic/554092-dynamic-buffers-and-compute-shaders/4561317/)  

### USAGE_STAGING & BindFlags
D3D11_BUFFER_DESC의 Usage에 D3D11_USAGE_STAGING을 할당하고 BindFlags에 무엇인가를 할당하면 CreateBuffer 함수에서 다음 오류가 발생한다.

```
A D3D11_USAGE_STAGING Resource cannot be bound to any parts of the graphics pipeline, so therefore cannot have any BindFlags bits set.
```

STAGING buffer는 GPU에 있는 데이터를 CPU로 옮기기 위한 buffer임으로 Graphics Pipeline에 binding되는 것은 오류이다.

### MISC DRAWINDIRECT_ARGS & BUFFER_STRUCTURED
MiscFlags의 인자로 `D3D11_RESOURCE_MISC_DRAWINDIRECT_ARGS | D3D11_RESOURCE_MISC_BUFFER_STRUCTURED`가 주어지면 다음과 같은 오류가 발생한다.

```
D3D11 ERROR: ID3D11Device::CreateBuffer: A resource cannot created with both D3D11_RESOURCE_MISC_DRAWINDIRECT_ARGS and D3D11_RESOURCE_MISC_BUFFER_STRUCTURED. [ STATE_CREATION ERROR #68: CREATEBUFFER_INVALIDMISCFLAGS]
```


## StructuredBuffer
Buffer를 사용할 수 있는 상황에서도 StructuredBuffer를 사용하는 것이 편하다.

왜냐하면, Buffer의 경우에는 UAV를 생성할 때, D3D11_UNORDERED_ACCESS_VIEW_DESC를 일일이 작성해줘야 하지만, StructuredBuffer의 경우에는 nullptr을 넘겨줘도 자동으로 생성해주기 때문이다.

