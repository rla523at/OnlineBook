# Resource Binding

* GPU memory 에 resource 생성 및 할당
* Descriptor Heap 및 Descriptor 생성
  * GPU memory 에 생성한 resource 에 대한 descriptor 를 생성하여 descriptor heap 에 추가한다.
* Root Signature 생성
  * Shader 가 Resource 에 접근할 수 있는 구조를 정의하는 역할을 한다.
* Command List 에서 Resource Binding 설정
  * Command List 를 사용해 GPU 에 명령을 기록할 때, Root Signature 와 Descriptor Heap 을 설정하여 resource binding 을 완료한다.
  * SetGraphicsRootSignature 로 Root Signature 를 Command List 에 설정하고 SetGraphicsRootDescriptorTable 을 사용해 Root Signature 의 Descriptor Table 에 Descriptor Heap 의 특정 위치를 binding 한다.

## Specification
Shader resources (such as textures, constant tables, images, buffers and so on) are not bound directly to the shader pipeline; instead, they are referenced through a descriptor. A descriptor is a small object that contains information about one resource.

Descriptors are grouped together to form descriptor tables.

The graphics and compute pipelines gain access to resources by referencing into descriptor tables by index.

> Reference  
> [learn.microsoft - direct3d12/resource-binding-flow-of-control](https://learn.microsoft.com/en-us/windows/win32/direct3d12/resource-binding-flow-of-control)  



