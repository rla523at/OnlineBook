# Depth Buffer
3D 그래픽스에서 카메라에 가까운 물체가 앞에 그려지게 하기 위해 사용되는 Buffer 이다.

## 동작 방식
화면의 모든 픽셀에 대응될 수 있게 화면의 크기와 동일한 크기의 2D depth buffer 를 만든다.

처음에는 모든 값이 카메라로부터 최대 거리를 나타내는 값( 보통 1.0 )으로 초기화한다.

객체 1 번을 렌더링 할 때, 객체 1의 각각의 픽셀 깊이 값과 그에 해당하는 depth buffer 에 저장된 값을 비교해서 만약 작다면( 카메라에 더 가까이 있다면) 다음 작업을 수행한다.
* pixel color update : 객체 1의 해당 픽셀에 색상을 color buffer 에 저장한다.
* update depth buffer : 객체 1의 해당 픽셀에 깊이 값으로 depth buffer 를 업데이트 한다.

이 과정을 모든 객체에 대해서 반복하게 되면, depth buffer 에는 각 픽셀마다 가장 가까운 depth 값이 저장되게 되고 color buffer 에는 가장 가까운 객체의 색상이 나타게 된다.