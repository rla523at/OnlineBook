# Vertex Shader

## 변환 과정
vertex shader에서는 정의된 모델을 transformation을 한다.(model matrix)

다음으로 view csys로 좌표계 변환을 한다.(view matrix) 

그리고 clip 공간으로 변환한다.(proejction matrix)

### 참고
이후에 NDC로 변환하는 과정은 GPU에서 내부적으로 자동으로 처리한다. 

NDC 변환이 끝난 후, $x,y \in [-1.0, 1.0]$, $z \in [0.0, 1.0]$ 범위로 맵핑된다.

