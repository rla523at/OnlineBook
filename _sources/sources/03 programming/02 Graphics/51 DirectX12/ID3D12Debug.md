# ID3D12Debug
ID3D12Debug는 DirectX 12에서 디버깅 및 오류 탐지를 돕기 위한 인터페이스로, 개발자가 DirectX 12 애플리케이션을 작성할 때 코드의 정확성과 성능을 개선하는 데 유용한 기능을 제공한다. 

이 인터페이스는 디버그 레이어(Debug Layer)를 활성화하고, 잘못된 API 호출이나 리소스 사용, 메모리 누수 등을 감지할 수 있게 해준다.

## Debug Layer
Direct3D 12에서의 Debug Layer는 Direct3D 애플리케이션을 개발할 때 버그를 찾고, 문제를 해결하며, 성능을 최적화하는 데 도움을 주는 디버깅 도구이다. 

debug layer 가 활성화가 되면 Direct3D는 추가적인 debugging 을 수행하여 런타임 시 API 호출의 유효성을 검사하고 경고 메시지를 출력하여 개발자가 코드에서 발생하는 문제를 파악할 수 있도록 돕는다.

```
D3D12 ERROR: ID3D12CommandList::Reset: Reset fails because the command 
list was not closed.
```

> Reference   
> [learn.microsoft - using-d3d12-debug-layer-gpu-based-validation](https://learn.microsoft.com/en-us/windows/win32/direct3d12/using-d3d12-debug-layer-gpu-based-validation)   
> {cite}`Luna` 118p  

## D3D12GetDebugInterface 함수
D3D12GetDebugInterface 함수는 Direct3D 12 디버깅 기능을 제공하는 인터페이스를 가져오는 함수이다. 

이 함수는 주로 디버깅 목적으로 사용되며, 개발자가 디버깅 도구를 활용하여 Direct3D 12 애플리케이션의 문제를 분석할 수 있게 해준다.

함수의 시그니처는 다음과 같다.
```cpp
HRESULT D3D12GetDebugInterface(
  REFIID riid,
  void   **ppvDebug
);
```

인자는 다음과 같다.
* REFIID riid
  * 요청하는 디버그 인터페이스의 식별자이다. 
  * 주로 ID3D12Debug 인터페이스가 요청된다.

* void **ppvDebug
  * 성공 시, 요청된 디버그 인터페이스의 인스턴스를 받는 포인터다. 
  * 이 포인터는 함수가 성공적으로 완료되면, 디버그 인터페이스를 가리키게 된다.

반환값은 다음과 같다.
* 성공 시
  * S_OK를 반환한다.

* 실패 시
  * HRESULT 오류 코드를 반환한다.
  * 디버깅 기능이 활성화되지 않았거나 지원되지 않는 환경에서는 실패할 수 있다.

## ID3D12Debug::EnableDebugLayer 멤버 함수
ID3D12Debug::EnableDebugLayer 함수는 Direct3D 12의 debug layer 를 활성화하는 함수이다. 

주의할점은 이 API를 사용하여 debug layer 을 사용하도록 설정하려면 ID3D12Device 객체를 만들기 전에 호출해야 한다. 만약, ID3D12Device 객체를 만든 후 이 API를 호출하면 런타임에서 디바이스가 제거된다.

함수의 시그니처는 다음과 같다.
```cpp
void EnableDebugLayer();
```

