# ID3D11InputLayout
`ID3D11InputLayout`는 Direct3D 11에서 입력 레이아웃을 나타내는 인터페이스다. 

이 클래스는 그래픽 파이프라인에서 셰이더로 전달되는 정점 데이터의 구조를 정의하는 역할을 한다. `ID3D11InputLayout`은 주로 정점 버퍼의 데이터가 셰이더에서 어떻게 해석되는지를 지정한다.

`ID3D11InputLayout`의 주요 역할은 정점 데이터의 형식을 정의하고, 이를 그래픽 파이프라인에 적용하는 것이다. 정점 데이터는 위치, 색상, 텍스처 좌표, 법선 벡터 등 다양한 속성을 포함할 수 있다. 이 클래스는 이러한 속성들이 셰이더 프로그램에서 올바르게 해석될 수 있도록 입력 레이아웃을 설정한다. 예를 들어, 정점 버퍼가 위치와 색상 데이터를 포함하고 있다면, 입력 레이아웃은 위치와 색상 데이터가 각각 어떤 형식이며, 정점 구조체에서 어떤 오프셋에 위치하는지를 정의한다.

`ID3D11InputLayout`은 `ID3D11Device`를 사용하여 생성된다. 이를 위해 `D3D11_INPUT_ELEMENT_DESC` 구조체 배열을 사용하여 각 입력 요소의 형식, 슬롯, 오프셋 등을 정의한다. 이 구조체는 입력 요소의 데이터 형식(DXGI_FORMAT), 입력 슬롯, 정점 버퍼 내의 오프셋, 그리고 인스턴싱 데이터 여부 등을 포함한다. 그런 다음, `CreateInputLayout` 메서드를 호출하여 입력 레이아웃 객체를 생성한다.

또한, `ID3D11InputLayout`은 `ID3D11DeviceContext`를 통해 그래픽 파이프라인에 바인딩된다. 이를 통해 정점 셰이더가 실행될 때 해당 입력 레이아웃이 활성화되고, 정점 데이터가 올바르게 해석된다. 예를 들어, `IASetInputLayout` 메서드를 사용하여 현재 활성화된 입력 레이아웃을 설정할 수 있다.

이 클래스는 다양한 정점 데이터를 처리하고, 셰이더 프로그램과의 호환성을 보장하는 데 중요한 역할을 한다. 이를 통해 복잡한 3D 모델의 정점 데이터를 효율적으로 관리하고, 정확한 그래픽 렌더링을 수행할 수 있다.

## D3D11_INPUT_ELEMENT_DESC 구조체
D3D11_INPUT_ELEMENT_DESC 구조체는 입력 레이아웃을 정의하기 위한 구조체이다.

정의는 다음과 같다.
```cpp
typedef struct D3D11_INPUT_ELEMENT_DESC {
    LPCSTR SemanticName;
    UINT SemanticIndex;
    DXGI_FORMAT Format;
    UINT InputSlot;
    UINT AlignedByteOffset;
    D3D11_INPUT_CLASSIFICATION InputSlotClass;
    UINT InstanceDataStepRate;
} D3D11_INPUT_ELEMENT_DESC;
```
각각의 멤버변수는 다음과 같다.

* LPCSTR SemanticName
  * 입력 요소의 의미 이름을 지정한다.
  * 가능한 값
      * "POSITION", "COLOR", "TEXCOORD" 등 셰이더 코드에서 사용되는 의미 이름
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT SemanticIndex
  * 의미 이름의 인덱스를 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
  * 기본값은 0이다.

* DXGI_FORMAT Format
  * 입력 요소의 데이터 형식을 지정한다.
  * 가능한 값
      * DXGI_FORMAT_R32G32B32_FLOAT: 3D 벡터 데이터 형식
      * DXGI_FORMAT_R8G8B8A8_UNORM: 4채널 8비트 정규화된 정수
  * 기본값은 없다. 사용자가 지정해야 한다.

* UINT InputSlot
  * 입력 슬롯의 인덱스를 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
  * 기본값은 0이다.

* UINT AlignedByteOffset
  * 입력 요소의 바이트 단위 오프셋을 지정한다.
  * 가능한 값
      * 0 이상의 정수 값 또는 D3D11_APPEND_ALIGNED_ELEMENT (자동 정렬)
  * 기본값은 D3D11_APPEND_ALIGNED_ELEMENT이다.

* D3D11_INPUT_CLASSIFICATION InputSlotClass
  * 입력 데이터의 클래스(상수 또는 인스턴스 데이터)를 지정한다.
  * 가능한 값
      * enum D3D11_INPUT_CLASSIFICATION 값
  * 기본값은 D3D11_INPUT_PER_VERTEX_DATA이다.

* UINT InstanceDataStepRate
  * 인스턴스 데이터의 진행 속도를 지정한다.
  * 가능한 값
      * 0 이상의 정수 값
      * 0일 경우 당 입력 요소가 인스턴싱되지 않음을 의미하며 각 버텍스마다 데이터가 변경된다.
  * 기본값은 0이다.

### InstanceDataStepRate

```
D3D11_INPUT_ELEMENT_DESC inputElementDesc[] = {
    {"POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0},
    {"COLOR", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0},
    {"TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0}
};
```
여기서 모든 입력 요소의 InstanceDataStepRate가 0이므로, 모든 정점 데이터가 각 정점마다 사용된다. 인스턴싱이 적용되지 않는다.

```
D3D11_INPUT_ELEMENT_DESC inputElementDesc[] = {
    {"POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0},
    {"COLOR", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0},
    {"INSTANCE_TRANSFORM", 0, DXGI_FORMAT_R32G32B32A32_FLOAT, 1, 0, D3D11_INPUT_PER_INSTANCE_DATA, 1},
    {"INSTANCE_COLOR", 0, DXGI_FORMAT_R32G32B32_FLOAT, 1, 16, D3D11_INPUT_PER_INSTANCE_DATA, 1}
};
```
여기서 POSITION과 COLOR는 정점마다 사용되고, INSTANCE_TRANSFORM과 INSTANCE_COLOR는 인스턴스마다 한 번씩 사용된다. 예를 들어, 100개의 정점을 가진 메쉬가 10개의 인스턴스로 렌더링되는 경우, 각 인스턴스마다 고유한 변환과 색상을 적용할 수 있다.

```
D3D11_INPUT_ELEMENT_DESC inputElementDesc[] = {
    {"POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0},
    {"COLOR", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0},
    {"INSTANCE_TRANSFORM", 0, DXGI_FORMAT_R32G32B32A32_FLOAT, 1, 0, D3D11_INPUT_PER_INSTANCE_DATA, 2},
    {"INSTANCE_COLOR", 0, DXGI_FORMAT_R32G32B32_FLOAT, 1, 16, D3D11_INPUT_PER_INSTANCE_DATA, 2}
};
```
여기서 INSTANCE_TRANSFORM과 INSTANCE_COLOR의 InstanceDataStepRate가 2로 설정되었다. 이는 2개의 인스턴스마다 한 번씩 이 데이터를 사용한다는 의미다. 예를 들어, 10개의 인스턴스가 렌더링될 때, 동일한 INSTANCE_TRANSFORM과 INSTANCE_COLOR 데이터가 2개의 인스턴스에 반복적으로 적용된다.

### InputSlot
D3D11_INPUT_ELEMENT_DESC의 InputSlot 값은 입력 어셈블러 단계에서 여러 개의 입력 버퍼를 사용하는 경우를 정의한다. 각 InputSlot은 정점 버퍼의 위치를 나타내며, 하나의 정점 셰이더 입력 레이아웃에 여러 정점 버퍼를 사용할 수 있다.

InputSlot 값이 0인 경우, 모든 입력 요소가 동일한 정점 버퍼에서 제공된다는 의미다. 이는 가장 일반적인 사용 방식으로, 대부분의 간단한 예제나 기본적인 렌더링에서는 모든 정점 데이터가 단일 버퍼에 저장된다.
```cpp
D3D11_INPUT_ELEMENT_DESC layout[] = {
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR", 0, DXGI_FORMAT_R32G32B32A32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

위의 예제에서는 위치와 색상 데이터 모두 InputSlot 0에서 가져오며, 이는 동일한 버퍼에서 제공된다는 의미다.

InputSlot 값이 0 외에 다른 값을 갖는 경우는 여러 개의 정점 버퍼를 사용하는 경우다. 이는 주로 복잡한 데이터 구조를 효율적으로 관리하기 위해 사용된다. 예를 들어, 위치 데이터와 색상 데이터를 별도의 버퍼로 관리할 수 있다.
```cpp
D3D11_INPUT_ELEMENT_DESC layout[] = {
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR", 0, DXGI_FORMAT_R32G32B32A32_FLOAT, 1, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

위의 예제에서는 위치 데이터는 InputSlot 0에서, 색상 데이터는 InputSlot 1에서 가져온다. 이는 각 데이터가 별도의 버퍼에 저장되어 있다는 것을 의미한다.

여러 InputSlot을 사용하는 이유는 버퍼 업데이트의 유연성 때문이다. 특정 데이터만 자주 업데이트해야 하는 경우, 해당 데이터만 포함하는 버퍼를 업데이트하면 된다. 예를 들어, 위치 데이터는 고정되어 있고 색상 데이터만 변경될 때, 색상 데이터 버퍼만 업데이트하면 된다.

