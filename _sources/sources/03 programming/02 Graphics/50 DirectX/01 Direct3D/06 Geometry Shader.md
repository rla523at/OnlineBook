# Geometry Shader
Geometry Shader는 vertex shader에서 출력된 primitive type(점, 선, 삼각형)을 구성하고 있는 vertex가 가지고 있는 data의 array를 입력으로 받아 새로운 primitive type을 생성할 수 있다. 

이를 통해 복잡한 기하학적 구조를 간단한 입력으로부터 동적으로 생성할 수 있다. 또한, 입력된 기하 도형의 정점 데이터를 수정할 수 있다. 예를 들어, 정점의 위치를 변형하거나, 색상 및 텍스처 좌표를 변경할 수 있다.

## Geometry shader object
geometry shader object는 다음과 같은 형태를 갖는다.

```
[maxvertexcount(NumVerts)] 
void ShaderName ( PrimitiveType DataType Name [ NumElements ], inout StreamOutputObject)
{ //... }
```

`[maxvertexcount(NumVerts)]`는 새롭게 생성될 vertex수의 최대값을 선언하는 부분이다.

`ShaderName`는 geometry shader 함수의 이름이다.

`PrimitiveType`은 shader가 처리할 기하 도형의 기본 단위로 다음과 같은 값들이 가능하다.


| Primitive Type | Description                                              |
|----------------|----------------------------------------------------------|
| point          | Point list                                                |
| line           | Line list or line strip                                   |
| triangle       | Triangle list or triangle strip                           |
| lineadj        | Line list with adjacency or line strip with adjacency     |
| triangleadj    | Triangle list with adjacency or triangle strip with adjacency |

`DataType`은 geometry shader의 input으로 주어지는 data의 type이다.

`Name`은 geometry shader의 input으로 주어지는 data의 이름이다.

`NumElements`는 input으로 주어지는 data array의 크기를 나타내며, PrimitiveType에 따라 그 수가 결정된다.

| Primitive Type | NumElements | 설명                                    |
|----------------|-------------|-----------------------------------------|
| point          | 1           | 하나의 점을 처리한다.                    |
| line           | 2           | 선은 두 개의 정점을 필요로 한다.        |
| triangle       | 3           | 삼각형은 세 개의 정점을 필요로 한다.    |
| lineadj        | 4           | 선의 양 끝을 포함하여 네 개의 정점을 필요로 한다. |
| triangleadj    | 6           | 삼각형의 경계를 이루는 세 개의 삼각형을 포함하여 여섯 개의 정점을 필요로 한다. |

`StreamOutputObject`는 templated object로 Geometry Shader의 출력을 캡처하여 버퍼에 저장할 수 있는 기능을 제공하는 객체이다. 이를 통해 GPU에서 처리된 기하 도형 데이터를 다시 GPU에서 재사용하거나, CPU로 전송하여 추가 처리를 할 수 있다.

StreamOutputObject는 다음과 같은 형태를 갖는다.
```
inout StreamOutputObject<DataType> Name;
```




> Reference  
> [learn.microsoft - hlsl-geometry-shader](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-geometry-shader)  
> [learn.microsoft - hlsl-so-type](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-so-type)