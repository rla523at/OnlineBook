# Vertex Shader
Vertex Shader의 출력이 Geometry Shader의 입력으로 사용된다. 따라서, Vertex Shader에서 출력으로 정의한 구조체가 Geometry Shader의 입력으로 사용된다.

즉, Vertex Shader에서 VS_OUTPUT으로 사용하는 구조체와 Geometry Shader에서 GS_INPUT으로 사용하는 구조체가 같아야 한다.

## Ouput
### SV_POSITION
Vertex Shader의 출력에서 SV_POSITION 세맨틱은 반드시 포함되어야 한다.

왜냐하면 SV_POSITION 세멘틱이 없을 경우 그래픽 파이프라인은 정점의 위치를 올바르게 처리할 수 없으므로 정상적인 렌더링이 불가능하다. SV_POSITION은 정점 셰이더에서 계산된 정점의 위치를 나타내며, 래스터화 단계에서 반드시 필요하다.

### 순서
Vertex Shader의 출력의 순서와 Pixel Shader의 입력의 순서가 반드시 같아야 한다.

예를 들어 다음과 같은 상황을 가정하자
```
struct VS_Output {    
    float4 pos : SV_POSITION;
    float3 color : COLOR;
};

struct PS_Input {
    float3 color : COLOR;
};
```

Pixel Shader에서 pos가 필요 없을 경우, 위와 같이 Input을 작성할 수 있다.

하지만, 이 경우에는 VS_Output과 PS_Input의 순서가 다르기 때문에 그래픽스 파이프라인상에서 데이터가 정상적으로 전달되지 않고 렌더링이 제대로 되지 않는다.

이를 해결하기 위해서는 다음과 같이 순서를 맞춰줘야한다.
```
struct VS_Output {    
    float3 color : COLOR;
    float4 pos : SV_POSITION;
};

struct PS_Input {
    float3 color : COLOR;
};
```

참고로, Pixel Shader의 Input 변수가 하나임으로 입력 구조체를 사용하지 않고 pixel shader를 작성할 수 있다.

```
float4 main(float3 color : COLOR) : SV_TARGET {
    return float4(color, 1.0);
}
```