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

## Robot의 위치 추정

Robot의 `pose`는 기준 좌표계에서 표현한 위치와 방향이다. 실제 pose를 직접
완벽하게 알 수는 없으므로 sensor 측정과 motion model을 사용해 현재 pose를
추정한다. 이때 상대 이동을 누적하는 odometry와 외부 기준에서 현재 위치를
다시 찾는 localization은 서로 다른 역할을 한다.

### Odometry와 drift

`odometry`는 wheel encoder, IMU, camera 같은 sensor로 이전 상태에서 얼마나
이동했는지를 추정하고 그 이동량을 누적하는 방법이다.

```text
이전 pose 추정값 + 이번 이동량 추정값 = 현재 pose 추정값
```

예를 들어 wheel 회전량으로 1 m씩 전진했다고 계산하면 출발 위치에서 1 m,
2 m, 3 m 떨어진 위치로 pose를 갱신할 수 있다. 이 계산은 매 순간의 상대
이동량만으로 계속 진행할 수 있으므로 짧은 구간의 motion을 추적하는 데
적합하다.

하지만 wheel slip, sensor bias와 측정 해상도 때문에 매번 계산한 이동량에는
작은 오차가 포함될 수 있다. Odometry는 이전 추정값에 새 이동량을 계속 더하므로
이 오차도 함께 누적된다. 시간이 지날수록 추정 pose가 실제 pose에서 벗어나는
현상을 `drift`라고 한다.

```text
실제 이동 거리       10.0 m
odometry 추정 거리   10.3 m
누적된 차이           0.3 m
```

### 연속성과 정확도

Pose 추정값이 `연속적`이라는 것은 robot이 조금 움직일 때 값도 조금씩 변하고,
추정값이 갑자기 멀리 떨어진 위치로 뛰지 않는다는 뜻이다. 예를 들어 일정한
속도로 움직일 때 위치가 다음처럼 갱신될 수 있다.

```text
시간       추정 x
0.0 s      0.00 m
0.1 s      0.02 m
0.2 s      0.04 m
0.3 s      0.06 m
```

연속적이라는 말이 측정 noise가 전혀 없거나 실제 pose와 정확히 일치한다는 뜻은
아니다. 앞의 값들이 `9.80 → 9.90 → 10.00 → 10.10 → 10.20 → 10.30 m`처럼
자연스럽게 변해도 실제 위치가 10.0 m라면 추정값에는 0.3 m의 drift가 있다.
따라서 pose의 연속성과 장기 정확도는 별도로 판단해야 한다.

### Localization과 pose 보정

`localization`은 sensor 관측을 기존 map, landmark 또는 GPS 같은 외부 기준과
비교하여 robot이 그 기준에서 어디에 있는지 추정하는 과정이다. Odometry가
상대 이동만 누적하는 동안 localization은 주변 환경과 다시 대조하여 누적
오차를 찾을 수 있다.

예를 들어 odometry가 robot 위치를 10.3 m로 추정했더라도 map에 기록된 wall과
lidar로 측정한 wall 거리의 조합이 10.0 m 위치에서 더 잘 맞을 수 있다.

```text
odometry로 누적한 위치       10.3 m
외부 기준으로 다시 추정한 위치 10.0 m
확인된 차이                  -0.3 m
```

Localization 결과를 반영하면 pose 추정값이 10.3 m에서 10.0 m로 한 번에
바뀔 수 있다. 실제 robot이 순간 이동한 것이 아니라 현재 위치에 대한 판단이
수정된 것이다. 사람으로 비유하면 odometry는 눈을 감고 걸음 수와 방향으로
위치를 추정하는 과정이고, localization은 눈을 떠서 건물이나 표지판을 확인한
뒤 자신의 위치를 바로잡는 과정이다.

ROS에서는 연속적인 단기 기준과 보정되는 장기 기준을 각각 `odom`과 `map`
frame으로 표현할 수 있다. 두 frame과 robot body의 `base_link`를 연결하는
방법은 [Dynamic TF and Mobile Robot Frames](<./01 ROS 2/06 Dynamic TF and Mobile Robot Frames.md>)에서
설명한다.

## 문서 범위

이 문서군은 robotics의 수학·역학뿐 아니라 robot software를 구성하고 실행하는 데 필요한 일반 개념을 정리한다. 특정 프로젝트의 경로, 수행 기록과 장비별 설정값은 포함하지 않는다.

현재 포함된 software 문서는 다음과 같다.

- [ROS 2](<./01 ROS 2/ROS 2.md>): ROS 2의 목적과 핵심 구성 요소를 설명한다.
- [Robot Simulation](<./02 Simulation/Simulation.md>): simulator의 역할과 Gazebo를 이용한 world·시간·ROS 2 연결을 설명한다.
- [Robot Trajectory Evaluation](<./03 Evaluation/Evaluation.md>): pose trajectory의 좌표·시간 계약과 association·alignment·GT 평가 경계를 설명한다.

