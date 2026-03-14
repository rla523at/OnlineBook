# Buffer
## Buffer
Buffer 변수는 다음과 같은 형태를 가지고 있다.

```
Buffer<Type> Name;
```

`Type`은 HLSL에서 지원하는 scalar, vector, 그리고 4개의 32비트(4바이트) 크기를 넘지 않는 matrix 타입이여야 한다.

예를 들어, Buffer\<float2x2>를 쓸 수 있지만, Buffer\<float4x4>는 너무 커서 컴파일러 오류가 발생한다.

Buffer에서 값을 가져올 때에는 `Load`함수를 사용하며 index는 0부터 시작한다.

```
Buffer<uint> index_buffer;

uint index = index_buffer.Load(1); // 2번째 index를 가져온다.
```

> Reference  
> [learn.microsoft - hlsl-buffer](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/dx-graphics-hlsl-buffer)

## StructuredBuffer
HLSL에서 StructuredBuffer는 GPU에서 구조화된 데이터를 다루기 위해 사용하는 특별한 버퍼 타입이다. 일반적인 버퍼와 달리 StructuredBuffer는 데이터 요소가 구조체로 구성되어 있으며, 이를 통해 복잡한 데이터 구조를 효율적으로 처리할 수 있다. 

StructuredBuffer는 다음과 같은 형태를 가지고 있다.
```
StructuredBuffer<Type> Name;
```

이 때, Type은 HLSL에서 지원하는 기본타입에 추가로 사용자 정의 구조체가 들어갈 수 있다.

> Reference  
> [learn.microsoft - sm5-object-structuredbuffer](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/sm5-object-structuredbuffer)  
> [learn.microsoft - windows/win32/direct3d11/direct3d-11-advanced-stages-cs-resources](https://learn.microsoft.com/en-us/windows/win32/direct3d11/direct3d-11-advanced-stages-cs-resources)  

### 크기제한
StructuredBuffer에 Type으로 주어지는 구조체는 최대 크기가 2048Byte까지이다.

만약 이를 초과할 경우 다음과 같은 오류가 발생한다.
```
error X4599: structured buffer elements cannot be larger than 2048 bytes in cs_5_0 
```

아래 Reference의 Buffer structure size를 보면 2048Byte 까지 되어 있음을 알 수 있다.

> Reference  
> [learn.microsoft - windows/win32/direct3d11/overviews-direct3d-11-resources-limits](https://learn.microsoft.com/en-us/windows/win32/direct3d11/overviews-direct3d-11-resources-limits)  

### GetDimensions 멤버함수
리소스 차원을 가져오는 함수로 시그니쳐는 다음과 같다.

```
void GetDimensions(
  out uint numStructs,
  out uint stride
);
```

numStructs는 데이터의 개수, stride는 데이터의 크기(Byte)를 나타낸다.

사용 예시는 다음과 같다.

```
    uint num_data, size;
    output_buffer.GetDimensions(num_data, size);
```

> Reference  
> [learn.microsoft - getdimensions](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/sm5-object-structuredbuffer-getdimensions)  


## Constant Buffer Padding
HLSL에서는 컴파일러에 의해 VS Input 구조체를 제외한 구조체가 16byte 경계에 맞춰진다.
(IA stage에서 data를 unpack 할 수 없기 때문에, VS input은 패킹 되지 않는다.)

```
//  2 x 16byte elements
cbuffer IE
{
    float4 Val1;
    float2 Val2;  // starts a new vector
    float2 Val3;
};

//  3 x 16byte elements
cbuffer IE
{
    float2 Val1;
    float4 Val2;  // starts a new vector
    float2 Val3;  // starts a new vector
};

//  1 x 16byte elements
cbuffer IE
{
    float1 Val1;
    float1 Val2;
    float2 Val3;
};
```

일반적으로, HLSL 컴파일러가 이를 자동으로 처리해주기 때문에 개발자가 직접 데이터 경계를 고려할 필요가 없다.

하지만 cbuffer 같은 경우에는 C++에서 정의한 구조체의 메모리를 그대로 넘겨주기 때문에, C++에서 정의한 구조체에 이런 경계를 고려하여 패딩을 직접넣어줘야 한다.

예를 들어 다음과 같은 cbuffer가 필요하다고 하자.
```
cbuffer Cbuffer : register(b0)
{
  float   radius;
  float3  v_cam_pos;
  float3  v_cam_up;
  matrix  m_view;
  matrix  m_proj;
};
```

그러면 16바이트 경계를 맞추기 위해 C++의 구조체를 다음과 같이 만들어줘야 한다.
```cpp
struct Cbuffer_Data
{
  float   radius;
  Vector3 v_cam_pos;
  Vector3 v_cam_up;
  float   padding;
  Matrix  m_view;
  Matrix  m_proj;
};
```

참고로, constant buffer로 넘기는 구조체의 크기가 16byte 단위에 안맞을 경우 CreateBuffer 함수가 실패한다.

따라서, 각 변수가 16Byte 단위에 맞았는지, 그리고 전체 구조체의 크기가 16byte 단위에 맞는지 둘 다 신경써야 한다.

> Reference  
> [learn.microsoft - hlsl-packing-rules](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-packing-rules)


## ConsumeStructuredBuffer

### Consume
buffer의 마지막 값을 리턴후 제거하는 함수이다.

함수의 시그니쳐는 다음과 같다.
```
T Consume(void);
```

Consume 함수는 buffer에 더이상 원소가 없는 경우, 예외를 발생시키지 않고 0으로 초기화된 값을 return 한다.

