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

## 문서 범위

이 문서군은 robotics의 수학·역학뿐 아니라 robot software를 구성하고 실행하는 데 필요한 일반 개념을 정리한다. 특정 프로젝트의 경로, 수행 기록과 장비별 설정값은 포함하지 않는다.

현재 포함된 software 문서는 다음과 같다.

- [ROS 2](<./01 ROS 2/ROS 2.md>): ROS 2의 목적과 핵심 구성 요소를 설명한다.

