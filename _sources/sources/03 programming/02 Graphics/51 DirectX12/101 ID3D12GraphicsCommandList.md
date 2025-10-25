# ID3D12GraphicsCommandList
ID3D12GraphicsCommandList 는 Direct3D 12에서 GraphicsCommandList 을 관리하는 인터페이스다. 

ID3D12GraphicsCommandList 의 주요 역할은 GPU 가 실행할 명령들을 기록하고 관리하는 것이다. 

Direct3D 12 의 주요 특징 중 하나는 CommandList 을 미리 작성하고, 이를 GPU 에 보내서 실행하는 방식으로, ID3D12GraphicsCommandList 는 이러한 명령들을 기록하는 핵심 요소다.

이 CommandList 에는 드로우 호출, 리소스 상태 전환, 파이프라인 상태 설정, 텍스처 복사 및 기타 그래픽 작업이 포함된다. 

개발자는 먼저 이 클래스에 명령을 기록하고, 기록된 CommandList 을 ID3D12CommandQueue에 제출하여 GPU 가 실행할 수 있게 한다.

이 클래스는 ID3D12Device를 사용하여 생성된다. 

CommandList 을 생성할 때는 해당 CommandList 이 처리할 작업 유형을 지정해야 하며, 이는 주로 그래픽, 복사, 컴퓨트 작업을 구분한다. 

CommandList 은 GPU 의 여러 작업을 한꺼번에 기록할 수 있고, 이를 통해 성능을 최적화할 수 있다.

ID3D12GraphicsCommandList 는 다양한 그래픽 작업을 지원한다. 예를 들어, DrawInstanced나 DrawIndexedInstanced 메서드를 사용하여 3D 객체를 그릴 수 있으며, ResourceBarrier 메서드를 사용하여 리소스 상태 전환을 관리할 수 있다. 또한, SetGraphicsRootSignature와 같은 메서드를 사용하여 파이프라인 상태와 루트 서명을 설정할 수 있다. 이 외에도 복잡한 작업 흐름에서 필요한 명령들을 차례로 기록할 수 있다.

이 CommandList 은 GPU 에 명령을 제출하기 전에 Close 메서드를 통해 기록을 종료해야 한다. CommandList 이 종료되면, 이를 ID3D12CommandQueue를 통해 GPU 에 제출하여 실행할 수 있다. 이렇게 CommandList 을 미리 작성하고 여러 개의 CommandList 을 동시에 실행하는 방식은 CPU와 GPU  사이의 병렬 처리를 최적화할 수 있게 해준다.