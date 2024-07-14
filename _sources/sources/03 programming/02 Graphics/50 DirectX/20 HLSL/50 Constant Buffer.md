# Constant Buffer
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

하지만 cbuffer 같은 경우에는 C++에서 정의한 구조체의 메모리르 그대로 넘겨주기 때문에, C++에서 정의한 구조체에 이런 경계를 고려하여 패딩을 직접넣어줘야 한다.

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

> Reference  
> [learn.microsoft - hlsl-packing-rules](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-packing-rules)