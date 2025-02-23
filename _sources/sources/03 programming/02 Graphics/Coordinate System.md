# Coordinate System

## Model Space
객체가 자체적으로 정의되는 좌표계입니다. 예를 들어, 메시 데이터는 보통 객체의 중심이나 기준점에 맞춰 로컬 좌표계에서 생성됩니다.

## World Space
여러 객체가 존재하는 전역 좌표계입니다. 

모델 행렬 (World Matrix) 을 사용하여 모델 좌표계의 데이터를 월드 좌표계로 변환합니다. 이 행렬에는 객체의 이동(translation), 회전(rotation), 스케일(scale) 등의 변환 정보가 포함됩니다. 이를 통해 개별 객체들을 전체 씬 내에서 올바른 위치와 방향으로 배치합니다.

## View/Camera Space
카메라(관찰자)를 기준으로 한 좌표계입니다.

뷰 행렬 (View Matrix) 을 사용하여 월드 좌표계의 데이터를 카메라 좌표계로 변환합니다. 이때, 카메라의 위치, 타겟(시선이 향하는 방향) 및 업 벡터(up vector)를 이용하여 행렬을 구성합니다. 이를 통해 Scene 의 객체들을 카메라 기준으로 재배열하여, 관찰자가 보는 방향에 맞게 좌표를 재정의합니다.

## Clip Space
투영 행렬 (Projection Matrix) 을 적용하여 얻은 좌표계로, 클리핑(화면에 보이지 않는 부분을 잘라내기) 작업이 이루어집니다. 

뷰 좌표계의 정점에 투영 행렬을 곱해 클립 좌표를 얻습니다.  클립 좌표는 동차 좌표(homogeneous coordinates)로 표현되며, 일반적으로 $\( x, y \)$ 성분은 $\([-w, w]\)$ 범위, $\( z \)$ 성분은 $\([0, w]\)$ 범위를 갖습니다. 시야(Frustum) 내부에 있는지 여부를 결정하여, 렌더링 시 클리핑 처리를 할 수 있도록 합니다.

Vertex Shader 에서 넘겨주는 Position 좌표가 Clip Space 다. 따라서, Position.x / Position.w, Position.y / Position.w 가 NDC 범위 내에 있으면 Screen Space 내에 존재하게 된다.

<details> <summary> <h3 style="display:inline-block"> Perspective Projection Matrix </h3></summary>
원근 투영은 멀리 있는 객체가 작게, 가까운 객체가 크게 보이는 효과를 주어 원근감(Perspective)을 표현합니다.

* clipping window = near plane
* https://www.songho.ca/opengl/gl_projectionmatrix.html#google_vignette
* View Space 의 좌표를 Clip Space 로 옮기기 위해서 필요한 행렬을 역으로 계산하는 듯 하다.
</details>

## Normalized Device Coordinates, NDC
클립 좌표에서 원근 분할(perspective division) 을 수행하여 \(w\)로 각 좌표를 나눈 결과 얻어지는 좌표계입니다.

$\( x \)$와 $\( y \)$는 $\([-1, 1]\)$ 범위를 가집니다. Direct3D의 경우 $\( z \)$는 $\([0, 1]\)$ 범위를 갖습니다. 좌표들을 표준화하여, 뷰포트 변환에서 화면 좌표로 매핑하기 용이하게 만듭니다.

## Screen/Window Space
NDC 좌표를 실제 렌더 타겟(예: 모니터 화면)의 픽셀 좌표로 변환한 결과의 좌표계입니다.

뷰포트(Viewport) 변환을 통해, NDC의 \([-1, 1]\) 범위를 렌더 타겟의 해상도(픽셀 좌표)로 변환합니다. 이때, 화면의 좌측 상단이나 하단을 기준으로 하는 좌표계가 될 수 있으며, 이는 API나 설정에 따라 달라질 수 있습니다.

최종적으로 사용자에게 보여질 화면상의 위치를 결정합니다.
