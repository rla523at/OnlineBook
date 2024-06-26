# ID3D11InputLayout
ID3D11InputLayout 클래스는 Input Assembler 단계에서 사용할 정점 데이터의 형식을 정의하는 데 사용되는 인터페이스다. 

입력 레이아웃은 정점 버퍼의 데이터를 셰이더로 전달하기 전에 각 정점의 구성 요소를 정의하고, 셰이더가 정점 데이터를 올바르게 해석할 수 있도록 한다.

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
      * D3D11_INPUT_PER_VERTEX_DATA: 각 버텍스마다 데이터
      * D3D11_INPUT_PER_INSTANCE_DATA: 각 인스턴스마다 데이터
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