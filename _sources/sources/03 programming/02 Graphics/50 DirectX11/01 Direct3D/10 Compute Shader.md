# Compute Shader
Compute Shader에서 그룹별 실행 순서는 특정한 규칙 없으며 GPU는 가능한 한 많은 스레드 그룹을 동시에 실행하려고 한다.

따라서, 스레드 그룹 간에는 동기화가 불가능하며 실행 순서가 정의되지 않는다. 

## System-Value Semantics

* SV_GroupID : 그룹의 3차원 고유 아이디
* SV_GroupThreadID : 그룹 내에서 Thread의 3차원 고유 아이디
* SV_GroupIndex : 그룹 내에서 Thread의 1차원 고유 아이디
* SV_DispatchThreadID : 전체에서 Thread의 3차원 고유 아이디

SV_GroupID는 (0,0,0)부터 시작한다.

> Reference  
> [learn.microsoft - semantics#system-value-semantics](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-semantics#system-value-semantics)  
> [learn.microsoft - dispatch](https://learn.microsoft.com/en-us/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-dispatch)  

## Groupshared Memory

Groupshared memory는 Group 내에서 share 하는 메모리로 그룹당 최대 16KB를 갖을 수 있다.

각 Thread는 Groupshared memory중 256byte에만 writing 할 수 있다.

> Reference  
> [learn.microsoft - direct3d-11-advanced-stages-compute-shader](https://learn.microsoft.com/en-us/windows/win32/direct3d11/direct3d-11-advanced-stages-compute-shader)