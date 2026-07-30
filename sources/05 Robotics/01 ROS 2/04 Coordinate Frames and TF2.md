# ROS 2 Coordinate Frames and TF2

## 한 줄 요약

Coordinate frame은 공간의 원점과 축을 정의하고, tf2는 여러 frame 사이의 translation과 rotation을 시간과 함께 관리하여 서로 다른 frame에서 표현한 data를 연결한다.

## Coordinate frame이 필요한 이유

`coordinate frame`은 위치와 방향을 수치로 표현하기 위한 기준이다. Frame이 달라지면 같은 물리적 점도 다른 좌표를 갖는다.

예를 들어 lidar가 자신의 앞쪽 1 m 지점에서 점을 측정했다고 하자. 이 점의 lidar 좌표는 `(1, 0, 0)`일 수 있다. 그러나 lidar가 robot 중심에서 앞쪽 0.2 m, 위쪽 0.3 m에 장착되어 있다면 같은 점의 `base_link` 좌표는 장착 위치와 방향을 반영해야 한다.

```text
base_link frame                         lidar_link frame
robot 기준 원점과 축                    lidar 측정 기준 원점과 축
       │                                      │
       └──── mounting transform ──────────────┘
```

Point에 숫자 세 개만 기록하고 어느 frame의 좌표인지 기록하지 않으면 다른 component가 그 값을 같은 공간에 올바르게 배치할 수 없다.

## Frame과 transform

Frame은 원점과 세 축으로 정의된다. `transform`은 두 frame의 상대적인 위치와 방향을 나타내며 다음 두 부분으로 구성된다.

| 구성 | 의미 | 대표 표현 |
|---|---|---|
| translation | 한 frame의 원점이 다른 frame에서 어디에 있는지 나타낸다. | `(x, y, z)` |
| rotation | 한 frame의 축이 다른 frame에 대해 어떻게 회전했는지 나타낸다. | quaternion 또는 rotation matrix |

이 문서에서는 다음 표기를 사용한다.

$$
{}^{A}\mathbf{T}_{B}
$$

${}^{A}\mathbf{T}_{B}$는 frame `B`의 pose를 frame `A`에서 표현한 transform이다. Frame `B`에서 표현한 point ${}^{B}\mathbf{p}$를 frame `A` 좌표로 바꿀 때는 다음 관계를 사용한다.

$$
{}^{A}\mathbf{p}
=
{}^{A}\mathbf{T}_{B}
{}^{B}\mathbf{p}
$$

예를 들어 `base_link`가 parent이고 `lidar_link`가 child라면 transform의 translation과 rotation은 `lidar_link`의 원점과 축이 `base_link`에서 어떻게 배치되는지를 설명한다. Parent와 child를 바꾸면 같은 숫자를 그대로 사용할 수 없고 inverse transform이 필요하다.

## ROS coordinate convention

ROS의 표준 단위와 좌표 convention은 REP-103에 정의되어 있다. 다른 convention을 사용해야 하는 sensor가 있으면 해당 차이를 별도 frame과 transform으로 명시한다.

- 길이는 meter, 각도는 radian을 사용한다.
- Coordinate frame은 right-handed coordinate system을 따른다.
- Robot body frame은 `x` forward, `y` left, `z` up을 사용한다.
- Camera optical frame처럼 다른 축 convention이 필요한 frame은 `_optical` suffix를 사용하며 `z` forward, `x` right, `y` down을 사용한다.

`imu_link`나 `lidar_link`라는 이름만으로 실제 sensor 축이 자동 결정되지는 않는다. Driver가 발행하는 message의 frame convention과 실제 장착 방향을 확인한 뒤 transform의 rotation에 반영해야 한다.

Rotation을 roll, pitch, yaw로 입력할 때는 각각 x, y, z fixed axis에 대한 회전이며 값은 radian이다. Quaternion을 직접 입력할 때는 zero rotation인 `(x, y, z, w) = (0, 0, 0, 1)`처럼 normalized quaternion을 사용한다.

## tf2의 역할

`tf2`는 ROS 2에서 coordinate transform을 배포하고 조회하는 library 집합이다. tf2를 사용하는 process들은 frame 관계를 broadcast하고, transform이 필요한 process는 listener와 buffer를 통해 관계를 수신하고 조회한다.

```text
transform broadcaster
        │
        │ frame 관계 publish
        ▼
     /tf 또는 /tf_static
        │
        ▼
listener와 transform buffer
        │
        │ source frame, target frame, time으로 조회
        ▼
application 또는 RViz2가 data 변환
```

- `broadcaster`는 자신이 책임지는 frame 관계를 ROS graph에 publish한다.
- `listener`는 publish된 관계를 수신한다.
- `buffer`는 시간별 transform을 보관하고 연결된 frame 사이의 transform을 계산한다.
- `RViz2` 같은 consumer는 message의 frame과 표시 기준 frame 사이의 transform을 조회한 뒤 data를 표시한다.

tf2가 topic에 있는 모든 sensor data를 자동으로 변환하는 것은 아니다. Consumer가 transform을 조회해 point, pose 또는 다른 stamped data에 적용해야 한다.

## TF tree가 존재하는 위치

TF tree를 영구적으로 보관하는 중앙 process나 단일 file은 없다. Broadcaster가
frame 관계를 publish하면 각 listener가 그 message를 받아 자신의 tf2 buffer에
frame 관계를 구성한다. 실제 transform query에 응답하는 것은 해당 listener의
buffer다.

| 대상 | 역할 |
|---|---|
| URDF file | Link와 joint 관계를 저장한 model description |
| `robot_state_publisher` | URDF joint 관계를 `/tf` 또는 `/tf_static` transform으로 publish하는 broadcaster |
| `/tf`, `/tf_static` | 실행 중인 broadcaster와 listener 사이에서 transform message를 전달하는 topic |
| listener의 tf2 buffer | 수신한 transform을 시간과 함께 보관하고 연결된 frame 사이의 transform을 계산하는 memory |
| `view_frames` output | 관찰 시점의 TF 관계를 file로 저장한 diagram snapshot |

URDF를 사용하는 경우 link 이름은 TF tree의 frame이 되고 joint는 parent link와
child link 사이의 transform 관계를 만든다. Joint 이름 자체가 별도의 TF frame이
되는 것은 아니다. Link와 joint의 구체적인 대응은
[URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)에서
설명한다.

```text
URDF file
    │ robot_state_publisher가 읽음
    ▼
/tf 또는 /tf_static
    │ listener가 수신
    ▼
listener별 tf2 buffer ──> transform query
```

따라서 저장된 `frames.pdf`가 존재해도 현재 broadcaster가 실행 중이라는 뜻은
아니다. 반대로 현재 TF tree가 정상이어도 `view_frames`를 실행하지 않았다면
diagram file은 존재하지 않을 수 있다.

## TF tree

tf2는 frame 관계를 tree 구조로 해석한다. 하나의 연결된 robot model을 만들 때는 다음 조건을 지킨다.

- Root가 아닌 각 child frame은 하나의 parent만 갖는다.
- Parent를 따라 올라가면 root에 도달해야 하며 cycle이 없어야 한다.
- 서로 변환해야 하는 frame은 같은 연결 component에 있어야 한다.
- 같은 child transform을 둘 이상의 node가 동시에 publish하지 않도록 transform의 소유자를 하나로 정한다.

Sensor rig의 최소 tree는 다음처럼 만들 수 있다.

```text
base_link
├── imu_link
└── lidar_link
```

`base_link`는 이 tree의 root다. `imu_link`와 `lidar_link`는 robot body에 고정되어 있으므로 두 관계는 static transform으로 표현할 수 있다.

TF graph 전체에 연결되지 않은 frame이 존재할 수는 있지만, 연결되지 않은 두 frame 사이의 transform은 계산할 수 없다. RViz2의 Fixed Frame과 sensor message의 frame이 서로 연결되지 않으면 해당 sensor data를 표시할 수 없다.

## Static transform과 dynamic transform

Frame 관계가 시간에 따라 변하는지에 따라 publish 방법이 달라진다.

| 종류 | 적용 대상 | Topic | 시간 처리 |
|---|---|---|---|
| static transform | body와 고정 sensor처럼 변하지 않는 관계 | `/tf_static` | 한 번 publish한 관계를 late subscriber도 받을 수 있다. |
| dynamic transform | `odom`과 움직이는 `base_link`처럼 변하는 관계 | `/tf` | timestamp별 transform을 buffer에 보관한다. |

`/tf_static`은 transient-local durability를 사용한다. Broadcaster endpoint가 유지되는
동안에는 RViz2처럼 나중에 실행된 호환 listener도 저장된 static transform을 받을
수 있다. Broadcaster process를 종료한 뒤 새로 시작한 graph가 이전 diagram
file에서 transform을 복원하는 것은 아니다. Dynamic transform은 계속 갱신되어야
하며 query time에 사용할 수 있는 transform이 buffer 안에 있어야 한다.

## Static transform을 command로 확인

`tf2_ros` package의 `static_transform_publisher` executable은 고정된 frame 관계를 command line에서 빠르게 확인할 때 사용할 수 있다.

```bash
ros2 run tf2_ros static_transform_publisher \
  --x 0.20 --y 0.00 --z 0.30 \
  --roll 0.00 --pitch 0.00 --yaw 0.00 \
  --frame-id base_link \
  --child-frame-id lidar_link
```

이 명령은 `lidar_link`가 `base_link` 기준으로 x 방향 0.2 m, z 방향 0.3 m에 있고 축 방향은 같다는 static transform을 publish한다. Command를 실행하는 process가 transform broadcaster이며, process를 종료하면 새 graph에서 이 broadcaster도 사라진다.

최종 robot model을 URDF와 `robot_state_publisher`로 publish한다면 같은 `base_link` → `lidar_link` 관계를 `static_transform_publisher`로 동시에 publish하지 않는다. 위 command는 값과 연결을 독립적으로 확인하는 임시 수단으로 사용한다.

## Transform과 timestamp

ROS sensor message의 `header.stamp`는 측정 시각을 나타낸다. Consumer가 움직이는 frame의 data를 변환하려면 현재 시간이 아니라 측정 시각의 transform이 필요하다.

```text
sensor message
├── header.frame_id : data가 표현된 frame
└── header.stamp    : data를 측정한 time

transform query
├── source frame
├── target frame
└── query time
```

Dynamic transform이 너무 늦게 도착하거나 query time이 buffer 범위보다 과거 또는 미래라면 extrapolation error가 발생할 수 있다. Static transform은 시간에 따라 값이 변하지 않으므로 연결만 올바르면 모든 측정 시각에 사용할 수 있다.

Message의 좌표값은 그대로 둔 채 `frame_id` 문자열만 다른 frame 이름으로 바꾸면 좌표 변환이 일어나지 않는다. 좌표값을 실제 transform으로 변환하거나 원래 측정 frame을 `frame_id`에 기록해야 한다.

## TF tree 확인

먼저 두 frame 사이의 transform을 계속 조회한다.

```bash
ros2 run tf2_ros tf2_echo base_link lidar_link
```

Translation과 rotation이 반복해서 출력되면 두 frame 사이의 연결을 조회할 수 있다는 뜻이다. Static transform이라면 출력 값이 변하지 않아야 한다.

전체 frame 관계를 diagram으로 저장하려면 Linux에서 다음 command를 실행한다.

```bash
ros2 run tf2_tools view_frames
```

이 command는 일정 시간 동안 transform을 수신한 뒤 현재 directory에 `frames.pdf`를 생성한다. Diagram에서 `base_link`가 root이고 `imu_link`, `lidar_link`가 직접 child인지 확인한다.

Topic과 publisher 상태도 함께 확인할 수 있다.

```bash
ros2 node list
ros2 topic list -t
ros2 topic info /tf_static --verbose
```

`view_frames`는 관찰한 관계를 저장하고, `node list`와 `topic info`는 현재 실행
상태를 확인한다. 과거에 저장한 diagram만으로 현재 TF가 활성 상태라고 판단하지
않는다.

`view_frames`의 tree 모양만 확인하지 않고 translation, rotation과 broadcaster도 확인해야 잘못된 축 방향이나 중복 publisher를 찾을 수 있다.

## 문제 확인 순서

| 관찰 | 확인할 내용 |
|---|---|
| `tf2_echo`가 frame이 없다고 보고한다. | Frame 이름, broadcaster process, ROS domain과 setup sourcing을 확인한다. |
| 저장된 TF diagram은 있지만 현재 frame을 찾지 못한다. | Diagram은 snapshot이므로 `robot_state_publisher` 같은 broadcaster가 현재 실행 중인지 확인한다. |
| 두 frame을 각각 찾지만 transform을 계산하지 못한다. | 서로 다른 root를 가진 disconnected tree인지 확인한다. |
| Point cloud 위치가 반대 방향으로 이동한다. | Parent/child 방향과 translation 부호를 확인한다. |
| Frame 방향이 예상과 다르다. | Degree를 radian 값으로 잘못 넣지 않았는지와 sensor axis convention을 확인한다. |
| RViz2가 extrapolation error를 표시한다. | Message timestamp와 dynamic transform의 publish time, clock source를 확인한다. |
| Frame이 흔들리거나 parent가 바뀐다. | 같은 child frame을 둘 이상의 broadcaster가 publish하는지 확인한다. |

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)
- [PointCloud2 and RViz2](<./06 PointCloud2 and RViz2.md>)
- [Node and Topic](<./02 Node and Topic.md>)

## References

- [REP-103 - Standard Units of Measure and Coordinate Conventions](https://github.com/ros-infrastructure/rep/blob/master/rep-0103.rst)
- [REP-105 - Coordinate Frames for Mobile Platforms](https://github.com/ros-infrastructure/rep/blob/master/rep-0105.rst)
- [ROS 2 Documentation - Introducing tf2](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Introduction-To-Tf2.rst)
- [ROS 2 Documentation - Writing a Static Broadcaster in C++](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Writing-A-Tf2-Static-Broadcaster-Cpp.rst)
- [tf2 Jazzy Documentation](https://docs.ros.org/en/jazzy/p/tf2/)
