# Stencil Buffer
Stencil Buffer 는 3D 그래픽스에서 픽셀 단위로 특정 영역을 마스킹하거나 다양한 효과를 적용하기 위해 사용되는 buffer 이다.

## 동작 방식
픽셀을 렌더링하기 전에, 스텐실 테스트가 먼저 이루어진다. 

이 테스트는 픽셀이 그려질지 말지를 결정하는 조건으로, 현재 스텐실 값과 설정된 기준 값이 비교된다.

비교 연산이 통과되면 해당 픽셀은 화면에 그려지고, 그렇지 않으면 그 픽셀은 무시된다.

스텐실 테스트가 완료되면, 스텐실 값을 업데이트한다.