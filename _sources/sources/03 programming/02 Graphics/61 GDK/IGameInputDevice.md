# IGameInputDevice

IGameInputDevice 는 Xbox Game Development Kit (GDK) 에서 제공하는 인터페이스로 입력 장치를 관리하고 정보를 제공하는 역할을 한다.

## GetDeviceInfo 멤버 함수

GetDeviceInfo 함수는 IGameInputDevice 인터페이스의 멤버 함수로, 입력 장치에 대한 상세한 정보를 가져오는 역할을 한다.

함수의 시그니쳐는 다음과 같다.

```cpp
GameInputDeviceInfo* GetDeviceInfo( void )
```

반환 값은 다음과 같다.

* GameInputDeviceInfo* deviceInfo
  * 장치 정보를 저장할 GameInputDeviceInfo 구조체에 대한 포인터이다.

> Reference  
> [learn.microsoft - igameinputdevice_getdeviceinfo](https://learn.microsoft.com/ko-kr/gaming/gdk/_content/gc/reference/input/gameinput/interfaces/igameinputdevice/methods/igameinputdevice_getdeviceinfo)  
