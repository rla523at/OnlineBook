# Sampling

## Supersampling Anti Aliasing
Supersampling Anti Aliasing (SSAA) 는 화면을 고해상도로 렌더링한 다음, 결과를 다운샘플링하여 최종 이미지를 생성한다. 

예를 들어, 가로 세로의 원래 해상도의 두 배로 렌더링하기 위해 4배 큰 back buffer와 depth buffer 를 사용해 렌더링 한다.

그리고 4개로 이루어진 pixel block 의 color 를 평균내는 방식으로 다운샘플링하여 다시 원래 해상도로 축소한다. 

따라서 이 경우에 pixel processing 양과 memory 사용양이 4배로 증가하게 된다.


## Multisampling Anti Aliasing
Multisampling Anti Aliasing (MSAA) 는 SSAA 의 계산량을 줄이기 위한 방식으로 경계선에서만 다중 샘플링을 수행한다.

예를 들어, 각 픽셀을 4개의 subpixel 로 나눈다. 이를 위해 4배 큰 back buffer 와 depth buffer 를 사용해 렌더링 한다.

하지만 SSAA 와 다르게 각 subpixel 마다 color 를 계산하지 않는다. 

원래 pixel 의 center 에서 color 값을 계산한 다음에 coverage 따라서 color 를 업데이트 할지 말지 결정한다.

만약 subpixel 의 중심점이 polygon 안에 있는경우 subpixel 의 color 를 pixel center 의 color 로 업데이트 한다.

그렇지 않은 경우에는 subpixel 의 color 값을 업데이트 하지 않는다.

image 의 color 를 계산하는 일은 graphics pipeline 에서 가장 cost 가 많이 드는 단계 중 하나 임으로 super sampling 에 비해 cost 를 많이 줄일 수 있게 된다.

