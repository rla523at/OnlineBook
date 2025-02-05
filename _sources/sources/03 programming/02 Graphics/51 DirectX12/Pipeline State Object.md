# Pipeline State Object (PSO)
PSO 는 graphics/compute pipeline 이 동작하기 위해 필요한 GPU 설정을 나타내는 객체이다.

## Graphics pipeline states set outside of the pipeline state object
Graphics Pipeline State 중 PSO 가 아닌 ID3D12GraphicsCommandList interface 의 함수를 호출해서 설정해야 되는 State 들이 있다.

| State | Method |
| --- | --- |
| Resource bindings | IASetIndexBuffer </br> IASetVertexBuffers </br> SOSetTargets </br> OMSetRenderTargets </br> SetDescriptorHeaps </br> all SetGraphicsRoot... methods </br> all SetComputeRoot... methods |
| Viewports | RSSetViewports |
| Scissor rects | RSSetScissorRects |
| Blend factor | OMSetBlendFactor |
| The depth stencil reference value | OMSetStencilRef |
| The input-assembler primitive topology order and adjacency type | IASetPrimitiveTopology |


 표는 [Managing Graphics Pipeline State in Direct3D 12](https://learn.microsoft.com/en-us/windows/win32/direct3d12/managing-graphics-pipeline-state-in-direct3d-12#graphics-pipeline-states-set-outside-of-the-pipeline-state-object) 문서의 "Graphics pipeline states set outside of the pipeline state object" 섹션에 있는 내용을 기반으로 작성되었습니다.


> Reference  
> [learn.microsoft - graphics-pipeline-states-set-outside-of-the-pipeline-state-object](https://learn.microsoft.com/en-us/windows/win32/direct3d12/managing-graphics-pipeline-state-in-direct3d-12#graphics-pipeline-states-set-outside-of-the-pipeline-state-object)  

## Specification
[learn.microsoft - managing-graphics-pipeline-state-in-direct3d-12](https://learn.microsoft.com/en-us/windows/win32/direct3d12/managing-graphics-pipeline-state-in-direct3d-12)
* When geometry is submitted to the graphics processing unit (GPU) to be drawn, **there are a wide range of hardware settings that determine how the input data is interpreted and rendered.** Collectively, these settings are called the graphics pipeline state
*  In Microsoft Direct3D 12, **most graphics pipeline state is set by using pipeline state objects (PSO).**
*  Then, at render time, command lists can quickly switch multiple settings of the pipeline state by calling ID3D12GraphicsCommandList::SetPipelineState in a direct command list or bundle to set the active PSO.
*  PSOs in Direct3D 12 were designed to allow the GPU to pre-process all of the dependent settings in each pipeline state, typically during initialization, to make switching between states at render time as efficient as possible.
*  Note that while most of the pipeline state settings are set using PSOs, **there are some state settings which are set separately using APIs provided by ID3D12GraphicsCommandList.**
