# Xbox

## Xbox Dev Kit 콘솔
일반 소비자용 Xbox 콘솔과는 다르게, 개발자들이 게임을 최적화하고 디버그하며, 기능을 테스트할 수 있도록 다양한 기능과 도구들이 추가되어 있다

일반 Xbox 콘솔과 유사한 외형을 가지고 있지만, 개발 및 디버깅 기능을 위해 특별히 조정된 하드웨어를 갖추고 있다.

## 컨트롤러
### 구성요소

view 버튼은 Xbox 360의 back 버튼, menu 버튼은 Xbox 360의 start 버튼이다.

> Reference  
> [support.xbox - xbox-series-x-s-controller](https://support.xbox.com/ko-KR/help/hardware-network/controller/get-to-know-your-xbox-series-x-s-controller)  
> [gist.github - xbox 360 controllers](https://gist.github.com/palmerj/586375bcc5bc83ccdaf00c6f5f863e86)  

### Motor
Xbox 컨트롤러에서 left motor와 low frequency motor, right motor와 high frequency motor는 일반적으로 같은 것을 의미한다.

left motor는 Xbox 컨트롤러의 왼쪽 진동 모터를 의미하며, 일반적으로 저주파수(low frequency) 모터로 작동한다.

right motor는 Xbox 컨트롤러의 오른쪽 진동 모터를 의미하며, 고주파수(high frequency) 모터로 작동한다.

> Reference  
> [learn.microsoft - xinput_vibration](https://learn.microsoft.com/en-us/gaming/gdk/_content/gc/reference/input/xinputongameinput/structs/xinput_vibration)  

### 아날로그 스틱
스틱을 "아날로그 스틱"이라고 부르는 이유는 아날로그 방식의 입력을 제공하기 때문이다.

아날로그 스틱은 연속적인 입력 값을 감지하고 전달하는 방식으로, 디지털 방식과 비교했을 때 훨씬 더 미세하고 세밀한 조작이 가능하다.

아날로그 스틱은 중심을 기준으로 모든 방향으로 0에서 1 사이의 값을 연속적으로 전달할 수 있다.

### 디저틸 방식
디지털 방식은 이진(on/off) 신호를 전달한다. 

예를 들어, 버튼을 누르면 1(켜짐), 버튼을 떼면 0(꺼짐) 값만 전달되는 방식이다.

방향 패드(D-Pad)가 디지털 입력의 대표적인 예이다.

### Dead Zone
아날로그 스틱의 입력 범위에서 특정 중심 영역을 설정하여, 그 영역 내에서는 입력 값을 무시하고, 그 외의 영역에서는 정상적으로 입력을 감지하는 방식으로 작동한다.

축방향 Dead Zone (Axial Dead Zone) 은 아날로그 스틱의 각 축(X, Y)에서 중심을 기준으로 특정 거리 내의 움직임을 무시합니다.

원형 Dead Zone (Radial Dead Zone) 은 아날로그 스틱의 중심을 기준으로 원형 영역을 설정하여, 그 원 안에서는 입력이 무시되고, 원을 벗어나면 입력을 받는 방식이다

## Xbox 콘솔 세대

Xbox 360 (2세대) -> Xbox One (3세대) -> Xbox Series X|S (4세대)

## Xbox Series X|S
Microsoft 가 출시한 최신 게임 콘솔 제품군의 제품명이다

Xbox Series X 와 Xbox Series S는  같은 세대의 콘솔이지만, 성능과 기능에서 차이가 있다

Xbox Series X 는 고성능 모델로, 현재 Xbox 콘솔 중 가장 강력한 성능을 가지고 있다. 
 
Xbox Series S 는 상대적으로 저렴한 옵션으로, 더 작은 크기와 낮은 가격대가 특징이다. 

## 제품별 코드명(프로젝트명)
* Xbox Series X : Scalett
* Xbox Series S : Lockhart
* Xbox One      : Durango
* PlayStation 5 : Prospero
* Playstation 4 : Orbis


