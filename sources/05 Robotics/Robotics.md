# Robotics

## 한 줄 요약

Robotics는 sensor로 주변과 자기 상태를 관측하고, 목표를 달성할 동작을 계산한 뒤, actuator로 물리 세계에 영향을 주는 시스템을 다루는 분야다.

## 기본 구조

로봇 시스템의 동작은 다음 기능이 연결된 흐름으로 볼 수 있다.

```text
physical world
      │
      ▼
    sensor ──> perception / state estimation ──> planning / control
      ▲                                               │
      │                                               ▼
      └────────────────── actuator <──────────────────┘
```

- `sensor`는 camera, lidar, encoder처럼 물리량을 측정해 data를 만든다.
- `perception`은 sensor data에서 물체, 공간과 같은 의미 있는 정보를 추출한다.
- `state estimation`은 불완전하거나 noisy한 측정으로부터 위치, 속도 같은 시스템 상태를 추정한다.
- `planning`은 목표에 도달하기 위한 경로 또는 행동 순서를 계산한다.
- `control`은 목표 상태를 실제 시스템이 따라가도록 actuator command를 계산한다.
- `actuator`는 motor처럼 command를 물리적 움직임으로 바꾼다.

각 기능을 하나의 program에 모두 넣을 수도 있지만, 실제 시스템에서는 기능을 여러 module로 나누고 message를 주고받게 만드는 경우가 많다. ROS 2는 이러한 robot software module을 구성하고 통신시키는 데 사용하는 framework와 tool을 제공한다.

## Robot의 기구 구조

Robot의 물리 구조는 서로 상대적으로 변형되지 않는 부분과 그 사이에서 허용되는
움직임으로 나누어 모델링할 수 있다. 이때 `rigid body`는 힘을 받아도 변형되지
않는다고 가정하는 물체이고, `link`는 model에서 하나의 rigid body로 취급하는
단위다. `joint`는 두 link를 연결하고 두 link 사이에서 허용되는 상대 운동을
정의한다.

| 개념 | 모델에서 나타내는 것 | 실제 robot의 예 |
|---|---|---|
| link | 한 덩어리로 함께 움직이는 rigid body | 차체, robot arm의 각 마디, wheel |
| joint | 두 link 사이의 운동 방향과 제약 | 회전축, hinge, slide mechanism |
| actuator | 힘이나 torque를 만들어 joint를 움직이는 장치 | motor, hydraulic cylinder |

Link와 joint는 실제 부품 이름과 반드시 일대일로 대응하지 않는다. 차체 frame,
battery와 computer가 서로 단단히 고정되어 함께 움직인다면 하나의 `base_link`로
묶을 수 있다. 반대로 wheel이 차체에 대해 회전한다면 차체와 wheel을 별도 link로
나누고 두 link 사이에 회전 joint를 둔다.

두 link 사이에 상대 운동은 없지만 sensor 측정처럼 별도의 좌표계가 필요하면
두 link를 분리하고 `fixed joint`로 연결할 수 있다. Fixed joint는 두 link의
상대 위치와 방향이 변하지 않는 연결이다.

```text
실제 구성                              기구 model

차체, battery, computer                base_link
차체에 대해 회전하는 wheel             wheel_link
차체와 wheel 사이의 회전축              wheel_joint
차체에 고정된 lidar의 측정 기준         lidar_link + fixed joint
```

Joint는 motor 자체를 뜻하지 않는다. 실제 joint assembly에는 motor, 감속기,
bearing과 shaft가 함께 들어갈 수 있지만, model의 joint는 이 assembly가 만드는
상대 운동의 축과 범위를 추상화한다. Actuator는 힘이나 torque를 만들고, joint는
그 결과로 어떤 상대 운동이 허용되는지를 정한다.

센서 측정 기준이나 계산 기준처럼 물리적 형상이 없는 좌표계를 link로 둘 수도
있다. 따라서 link를 나눌 때는 부품 개수보다 “두 부분이 서로 상대적으로
움직이는가”와 “별도의 좌표계가 필요한가”를 먼저 판단한다. 이 구조를 ROS 2의
URDF로 기술하는 방법은 [URDF and Robot State Publisher](<./01 ROS 2/05 URDF and Robot State Publisher.md>)에서 설명한다.

## 문서 범위

이 문서군은 robotics의 수학·역학뿐 아니라 robot software를 구성하고 실행하는 데 필요한 일반 개념을 정리한다. 특정 프로젝트의 경로, 수행 기록과 장비별 설정값은 포함하지 않는다.

현재 포함된 software 문서는 다음과 같다.

- [ROS 2](<./01 ROS 2/ROS 2.md>): ROS 2의 목적과 핵심 구성 요소를 설명한다.

