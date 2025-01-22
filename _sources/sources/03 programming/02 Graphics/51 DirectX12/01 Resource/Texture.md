# Texture

## D3D12_SUBRESOURCE_DATA

RowPitch 멤버 변수는 각 row 가 시작되는 메모리 주소 간격을 바이트 단위로 나타낸 값이다. 

SlicePitch 멤버 변수는 3D Texture 를 사용할 경우, 하나의 슬라이스 (2D Texutre) 에서 다음 슬라이스가 시작되는 메모리 주소 간격을 바이트 단위로 나타내는 값이다. 2D Texture 의 경우에는 Texture 하나. 즉. 전체가 차지하는 바이트 수로 생각할 수 있다.

> Reference  
> [learn.microsoft - d3d12_subresource_data](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ns-d3d12-d3d12_subresource_data)  