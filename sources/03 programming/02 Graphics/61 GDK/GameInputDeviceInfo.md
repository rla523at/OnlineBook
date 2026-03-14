# GameInputDeviceInfo

`GameInputDeviceInfo` 구조체는 Microsoft GDK의 GameInput API에서 사용되는 구조체로, 다양한 입력 장치의 세부 정보를 제공하는 데 사용된다. 이 구조체는 장치의 하드웨어 정보, 지원되는 입력 기능 및 리포트 정보 등 다양한 정보를 포함하고 있어, 장치의 특성 및 기능을 명확히 파악할 수 있도록 한다.

구조체의 정의는 다음과 같다.

```cpp
typedef struct GameInputDeviceInfo {  
    uint32_t infoSize;  
    uint16_t vendorId;  
    uint16_t productId;  
    uint16_t revisionNumber;  
    uint8_t interfaceNumber;  
    uint8_t collectionNumber;  
    GameInputUsage usage;  
    GameInputVersion hardwareVersion;  
    GameInputVersion firmwareVersion;  
    APP_LOCAL_DEVICE_ID deviceId;  
    APP_LOCAL_DEVICE_ID deviceRootId;  
    GameInputDeviceFamily deviceFamily;  
    GameInputDeviceCapabilities capabilities;  
    GameInputKind supportedInput;  
    GameInputRumbleMotors supportedRumbleMotors;  
    uint32_t inputReportCount;  
    uint32_t outputReportCount;  
    uint32_t featureReportCount;  
    uint32_t controllerAxisCount;  
    uint32_t controllerButtonCount;  
    uint32_t controllerSwitchCount;  
    uint32_t touchPointCount;  
    uint32_t touchSensorCount;  
    uint32_t forceFeedbackMotorCount;  
    uint32_t hapticFeedbackMotorCount;  
    uint32_t deviceStringCount;  
    uint32_t deviceDescriptorSize;  
    GameInputRawDeviceReportInfo const * inputReportInfo;  
    GameInputRawDeviceReportInfo const * outputReportInfo;  
    GameInputRawDeviceReportInfo const * featureReportInfo;  
    GameInputControllerAxisInfo const * controllerAxisInfo;  
    GameInputControllerButtonInfo const * controllerButtonInfo;  
    GameInputControllerSwitchInfo const * controllerSwitchInfo;  
    GameInputKeyboardInfo const * keyboardInfo;  
    GameInputMouseInfo const * mouseInfo;  
    GameInputTouchSensorInfo const * touchSensorInfo;  
    GameInputMotionInfo const * motionInfo;  
    GameInputArcadeStickInfo const * arcadeStickInfo;  
    GameInputFlightStickInfo const * flightStickInfo;  
    GameInputGamepadInfo const * gamepadInfo;  
    GameInputRacingWheelInfo const * racingWheelInfo;  
    GameInputUiNavigationInfo const * uiNavigationInfo;  
    GameInputForceFeedbackMotorInfo const * forceFeedbackMotorInfo;  
    GameInputHapticFeedbackMotorInfo const * hapticFeedbackMotorInfo;  
    GameInputString const * displayName;  
    GameInputString const * deviceStrings;  
    void const * deviceDescriptorData;  
} GameInputDeviceInfo;
```

각 멤버 변수는 다음과 같다.

* **infoSize (uint32_t)**:
  * 구조체의 크기를 나타낸다. 이 값은 GameInput API에서 이 구조체를 올바르게 처리하는 데 사용된다.

* **vendorId (uint16_t)**:
  * 장치의 제조사 ID를 나타내는 값이다. 이 값은 USB 표준에 따라 지정된 값으로, 장치의 제조사를 식별할 수 있다.

* **productId (uint16_t)**:
  * 장치의 제품 ID를 나타내는 값으로, 특정 장치 모델을 구분하는 데 사용된다.

* **revisionNumber (uint16_t)**:
  * 장치의 수정 번호로, 하드웨어 버전 또는 개정 수준을 나타낸다.

* **interfaceNumber (uint8_t)**:
  * 장치의 인터페이스 번호를 나타낸다. 여러 인터페이스를 가진 장치에서 특정 인터페이스를 식별하는 데 사용된다.

* **collectionNumber (uint8_t)**:
  * HID 장치에서 사용되는 컬렉션 번호로, 장치가 여러 개의 입력 컬렉션을 가질 때 이를 구분한다.

* **usage (GameInputUsage)**:
  * 장치의 사용 목적(HID Usage)을 나타내며, 장치가 어떤 종류의 입력을 제공하는지를 나타낸다.

* **hardwareVersion (GameInputVersion)**:
  * 장치의 하드웨어 버전을 나타낸다. 하드웨어의 특정 버전 또는 개정 번호를 표현한다.

* **firmwareVersion (GameInputVersion)**:
  * 장치의 펌웨어 버전을 나타내며, 현재 설치된 펌웨어의 버전 정보를 제공한다.

* **deviceId (APP_LOCAL_DEVICE_ID)**:
  * 장치의 고유 ID로, 시스템에서 특정 장치를 식별하는 데 사용된다.

* **deviceRootId (APP_LOCAL_DEVICE_ID)**:
  * 장치의 루트 ID로, 장치가 연결된 상위 장치나 컨트롤러의 ID를 나타낸다.

* **deviceFamily (GameInputDeviceFamily)**:
  * 장치가 속하는 범주를 나타내며, 예를 들어 게임패드, 키보드, 마우스 등의 장치 유형을 구분한다.

* **capabilities (GameInputDeviceCapabilities)**:
  * 장치의 기능을 나타내는 비트 필드 값이다. 장치가 지원하는 입력 및 기능의 세부 사항을 포함한다.

* **supportedInput (GameInputKind)**:
  * 장치가 지원하는 입력 종류를 나타낸다. 예를 들어, 게임패드 입력, 터치 입력, 모션 입력 등 다양한 입력 유형을 포함한다.

* **supportedRumbleMotors (GameInputRumbleMotors)**:
  * 장치가 지원하는 진동 모터의 종류를 나타낸다. 예를 들어, 장치에 진동 기능이 포함된 경우 이를 통해 지원 여부를 확인할 수 있다.

* **inputReportCount (uint32_t)**:
  * 장치가 지원하는 입력 리포트의 개수를 나타낸다.

* **outputReportCount (uint32_t)**:
  * 장치가 지원하는 출력 리포트의 개수를 나타낸다.

* **featureReportCount (uint32_t)**:
  * 장치가 지원하는 특수 기능 리포트의 개수를 나타낸다.

* **controllerAxisCount (uint32_t)**:
  * 장치가 지원하는 컨트롤러 축의 개수를 나타낸다.

* **controllerButtonCount (uint32_t)**:
  * 장치가 지원하는 버튼의 개수를 나타낸다.

* **controllerSwitchCount (uint32_t)**:
  * 장치에 있는 스위치의 개수를 나타낸다.

* **touchPointCount (uint32_t)**:
  * 터치 포인트의 개수를 나타낸다. 멀티터치를 지원하는 경우 이 값은 1보다 클 수 있다.

* **touchSensorCount (uint32_t)**:
  * 터치 센서의 개수를 나타낸다.

* **forceFeedbackMotorCount (uint32_t)**:
  * 장치에 포함된 포스 피드백 모터의 개수를 나타낸다.

* **hapticFeedbackMotorCount (uint32_t)**:
  * 햅틱 피드백 모터의 개수를 나타낸다.

* **deviceStringCount (uint32_t)**:
  * 장치와 관련된 문자열의 개수를 나타낸다.

* **deviceDescriptorSize (uint32_t)**:
  * 장치 설명자의 크기를 나타낸다.

* **inputReportInfo, outputReportInfo, featureReportInfo**:
  * 각각 입력, 출력, 특수 기능 리포트에 대한 세부 정보를 나타내는 포인터이다.

* **controllerAxisInfo, controllerButtonInfo, controllerSwitchInfo**:
  * 각각 컨트롤러의 축, 버튼, 스위치에 대한 정보를 제공하는 포인터이다.

* **keyboardInfo, mouseInfo**:
  * 키보드와 마우스에 대한 구체적인 정보를 제공하는 포인터이다.

* **touchSensorInfo**:
  * 터치 센서에 대한 세부 정보를 제공하는 포인터이다.

* **motionInfo**:
  * 모션 센서의 정보를 제공하는 포인터이다.

* **arcadeStickInfo, flightStickInfo, gamepadInfo, racingWheelInfo**:
  * 각각 아케이드 스틱, 플라이트 스틱, 게임패드, 레이싱 휠의 정보에 대한 포인터이다.

* **uiNavigationInfo**:
  * UI 내비게이션 장치의 정보를 제공하는 포인터이다.

* **forceFeedbackMotorInfo, hapticFeedbackMotorInfo**:
  * 각각 포스 피드백 모터와 햅틱 피드백 모터에 대한 세부 정보를 제공하는 포인터이다.

* **displayName**:
  * 장치의 표시 이름을 나타내는 문자열이다.

* **deviceStrings**:
  * 장치와 관련된 기타 문자열 정보를 포함하는 포인터이다.

* **deviceDescriptorData**:
  * 장치 설명자 데이터를 포함하는 포인터이다. 이 설명자는 장치의 세부 특성 및 기능을 설명하는 데 사용된다.
