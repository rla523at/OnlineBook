# IGameInput
Xbox 의 GRDK(Game Development Kit) 에서 제공하는 게임 입력 관리 인터페이스다.

IGameInput은 Xbox와 Windows 플랫폼에서 컨트롤러, 키보드, 마우스, 터치, 모션 컨트롤러 등 다양한 입력 장치를 지원하며, 게임 개발자가 이러한 입력을 쉽게 수집하고 처리할 수 있게한다.

## RegisterDeviceCallback 멤버 함수
IGameInput 인터페이스의 메서드로, 입력 장치(예: 게임패드, 키보드, 마우스 등)의 연결 및 해제와 관련된 이벤트를 처리하기 위한 콜백 함수를 등록하는 역할을 한다.

함수의 signature 는 다음과 같다.

```cpp
HRESULT RegisterDeviceCallback(  
         IGameInputDevice* device,  
         GameInputKind inputKind,  
         GameInputDeviceStatus statusFilter,  
         GameInputEnumerationKind enumerationKind,  
         void* context,  
         GameInputDeviceCallback callbackFunc,  
         GameInputCallbackToken* callbackToken  
)
```

인자는 다음과 같다.
* IGameInputDevice* device
  * device 인자가 nullptr 이 아닌 경우 주어진 특정 device 에만 callback 함수가 등록된다.
* GameInputCallbackToken* callbackToken
  * GameInputCallbackToken은 GameInput 콜백 함수를 등록할 때 반환되는 토큰이다.
  * 이 토큰을 사용하면 나중에 해당 콜백을 해제하거나 관리할 수 있습니다

callback 함수로 등록되는 함수는 다음과 같은 시그니쳐를 갖어야 한다.

```cpp
void GameInputDeviceCallback(  
         GameInputCallbackToken callbackToken,  
         void* context,  
         IGameInputDevice* device,  
         uint64_t timestamp,  
         GameInputDeviceStatus currentStatus,  
         GameInputDeviceStatus previousStatus  
)
```

> Reference  
> [learn.microsoft - igameinput_registerdevicecallback](https://learn.microsoft.com/en-us/gaming/gdk/_content/gc/reference/input/gameinput/interfaces/igameinput/methods/igameinput_registerdevicecallback)  
> [learn.microsoft - gameinputdevicecallback](https://learn.microsoft.com/en-us/gaming/gdk/_content/gc/reference/input/gameinput/functions/gameinputdevicecallback) 

## GetCurrentReading 멤버 함수
IGameInput 인터페이스의 메서드로, Microsoft의 GameInput API에서 입력 장치의 현재 상태를 읽어오는 함수이다.

입력 장치의 상태를 폴링(polling)하여 최신 입력 상태를 얻는 데 사용된다.

```cpp
HRESULT GetCurrentReading(  
         GameInputKind inputKind,  
         IGameInputDevice* device,  
         IGameInputReading** reading  
)
```

매개 변수는 다음과 같다.

* GameInputKind inputKind
  * enum GameInputKind으로 입력 장치의 유형을 지정하는 변수다.
  * 열거형 값을 결합하여 여러 입력 유형을 지정할 수 있다.

> Reference  
> [learn.microsoft - igameinput_getcurrentreading](https://learn.microsoft.com/ko-kr/gaming/gdk/_content/gc/reference/input/gameinput/interfaces/igameinput/methods/igameinput_getcurrentreading)


