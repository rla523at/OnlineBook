# Pixel Shader

## SV_POSITION
pixel shader의 input으로 변수중 SV_POSITION semantic을 갖는 변수는 화면 공간(Screen Space) 좌표이다.

변환 과정을 살펴보면, vertex shader에 model space의 좌표가 들어가면 MVP 변환을 통해 clip space 좌표로 변환되어 SV_POSITION semantic을 가지고 vertex shader output으로 나간다.

이후에 rasterizer 단계에서 view port 변환을 통해 screen space 좌표로 변환되어 pixel shader에 전달된다.