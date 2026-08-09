# ROS 2 URDF and Robot State Publisher

## 한 줄 요약

URDF는 robot의 link와 joint 관계를 표현하기 위한 XML format이다. `robot_state_publisher`는 URDF로 작성된 robot model과 joint state를 바탕으로 tf2 transform을 publish한다.

## URDF의 역할

URDF(Unified Robot Description Format)는 robot의 구조를 표현하는 XML format이며, robot의 link와 joint 관계를 저장하는 설계도 역할을 한다. 그러나 URDF file을 작성하는 것만으로 transform message가 publish되지는 않는다. `robot_state_publisher` 같은 broadcaster가 URDF를 읽고 `/tf` 또는 `/tf_static`에 transform을 publish해야 RViz2와 다른 listener가 frame 관계를 사용할 수 있다.

URDF와 실행 중인 TF tree의 관계를 이해하려면 다음 개념을 구분해야 한다.

| 구성 요소 | 실행 중 역할 | 비유 |
|---|---|---|
| URDF file | Link, joint와 geometry를 text로 저장한다. | 로봇의 설계도 |
| `robot_description` parameter | 실행 중인 node에 URDF text를 전달한다. | 전달된 설계도 |
| `robot_state_publisher` | URDF와 joint state를 사용해 link 사이의 transform을 publish한다. | 설계도를 읽고 좌표 관계를 방송하는 프로그램 |
| `/tf`, `/tf_static` | Broadcaster와 listener 사이에서 transform message를 전달한다. | 방송에 사용하는 통신 채널 |
| RViz2 같은 tf2 listener | Transform message를 수신하고 필요한 transform을 조회한다. | 방송을 듣는 프로그램 |
| listener의 tf2 buffer | 수신한 transform을 저장하고 frame 관계를 구성한다. | 들은 내용을 정리한 좌표 관계도 |

TF tree는 중앙 file이나 database에 영구적으로 저장되는 대상이 아니라, 실행 중인 broadcaster가 publish한 transform을 listener가 수신하면서 각자의 buffer에 구성된다. URDF는 이 과정에서 robot의 구조를 정의하는 입력일 뿐, 그 자체가 TF tree는 아니다.

`robot_state_publisher`는 URDF model을 읽고 link 사이의 transform을 tf2에 publish하는 ROS 2 node다. URDF 자체와 `robot_state_publisher`, publish된 TF tree는 서로 연결된 단계이지만 같은 개념은 아니다.

```text
URDF file
   │
   │ file을 읽어 string parameter 구성
   ▼
robot_description
   │
   ▼
robot_state_publisher
   ├── fixed joint ─────> /tf_static ──┐
   └── movable joint ───> /tf ─────────┤
              ▲                        │ transform 수신
              └── /joint_states        ▼
                              RViz2 같은 tf2 listener
                                        │
                                        ▼
                              listener의 tf2 buffer
                                        │
                                        ▼
                              조회 가능한 TF tree
```

즉, URDF는 robot 구조의 정의이고 TF tree는 그 정의를 바탕으로 publish된 transform을 listener가 수신해 자신의 buffer에 구성한 frame 관계다. Broadcaster, listener와 buffer의 일반적인 동작은 [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)에서 설명한다.

## Link와 joint

URDF의 kinematic tree는 `link`와 `joint`로 구성된다.

| Element | 역할 |
|---|---|
| `<link>` | Robot의 rigid body와 그 body에 붙은 coordinate frame을 정의한다. |
| `<joint>` | Parent link와 child link 사이의 연결과 motion type을 정의한다. |
| `<parent>` | Joint의 기준이 되는 parent link를 지정한다. |
| `<child>` | Joint에 의해 parent에 연결되는 child link를 지정한다. |
| `<origin>` | Child 쪽 joint frame의 위치와 방향을 parent link frame에서 표현한다. |

물리 구조 관점의 link와 joint는 [Robotics](<../Robotics.md>)에서 먼저 설명한다. URDF는 그 기구 구조와 좌표 관계를 XML element로 옮긴 model이다.

### 실제 부품과 model 요소의 대응

Link와 joint는 실제 부품 이름과 반드시 일대일로 대응하지 않는다. 서로 단단히 고정되어 항상 함께 움직이는 여러 부품을 하나의 link로 묶을 수 있다. 반대로 상대 운동이 있거나 별도의 측정 좌표계가 필요하면 별도 link로 나눌 수 있다.

| 실제 구성 | URDF에서 가능한 표현 |
|---|---|
| 차체에 고정된 battery와 computer | 모두 하나의 `base_link`에 포함 |
| 차체에 대해 회전하는 wheel | `base_link`와 `wheel_link`를 회전 joint로 연결 |
| 차체에 고정된 IMU | `base_link`와 `imu_link`를 fixed joint로 연결 |
| Camera의 optical 기준축 | 형상 없는 `camera_optical_frame` link를 fixed joint로 연결 |

실제 회전 joint assembly에는 motor, 감속기, bearing과 shaft가 함께 있을 수 있다. URDF joint는 이 부품들의 형상을 뜻하지 않고, 두 link 사이에서 허용되는 운동축과 범위를 추상화한다. Motor 같은 actuator는 힘이나 torque를 만들고, joint는 그 결과로 허용되는 상대 운동을 정의하므로 두 개념은 같지 않다.

Link를 나눌 때는 다음 두 질문을 먼저 확인한다.

1. 두 부분이 서로 상대적으로 움직이는가?
2. Sensor 측정이나 계산을 위한 별도 coordinate frame이 필요한가?

상대 운동도 없고 별도 frame도 필요하지 않다면 하나의 link로 묶을 수 있다. 상대 운동이 있으면 link를 나누고 movable joint로 연결한다. 상대 운동은 없지만 별도 frame이 필요하면 두 link를 fixed joint로 연결할 수 있다.

### Joint 종류와 fixed의 의미

`degree of freedom`(DOF)은 joint의 상태를 독립적으로 결정하는 값의 개수다. URDF에서 자주 사용하는 joint 종류는 다음과 같다.

| Joint type | DOF | 운동 |
|---|---:|---|
| `fixed` | 0 | Parent와 child의 상대 pose가 변하지 않는다. |
| `revolute` | 1 | 지정된 축을 중심으로 제한된 각도 범위에서 회전한다. |
| `continuous` | 1 | 지정된 축을 중심으로 위치 제한 없이 회전한다. |
| `prismatic` | 1 | 지정된 축을 따라 제한된 범위에서 직선 이동한다. |

URDF에는 `planar`와 `floating` type도 있지만, 이 문서의 sensor rig와 1-DOF joint 설명에서는 사용하지 않는다. 사용하는 consumer가 해당 type을 어떻게 지원하는지는 별도로 확인해야 한다.

`fixed`는 child link가 world에서 절대 움직이지 않는다는 뜻이 아니다. Parent와 child 사이의 상대 pose만 일정하다는 뜻이다. 예를 들어 `base_link`가 world에서 이동하면 `imu_link`와 `lidar_link`도 함께 이동하지만, `base_link`에 대한 sensor 장착 위치는 변하지 않는다.

```text
world_T_lidar = world_T_base × base_T_lidar
```

여기서 fixed joint가 일정하게 유지하는 값은 `base_T_lidar`다.

### 고정된 URDF와 시간에 따라 변하는 joint state

URDF file이 실행 중에 바뀌지 않는다고 해서 URDF로 static transform만 만들 수 있는 것은 아니다. URDF는 joint의 parent·child, 원점, 운동축과 joint type처럼 **변하지 않는 운동 규칙**을 저장하고, movable joint의 현재 position $q(t)$는 `/joint_states` message로 별도로 전달한다.

```text
변하지 않는 입력                           시간에 따라 변하는 입력
URDF: parent·child·origin·axis·type         /joint_states: joint name·position q(t)
                    │                         │
                    └──────┬──────────────────┘
                           ▼
                 robot_state_publisher
                           │ forward kinematics
                           ▼
                  link 사이의 dynamic TF
                           │
                           ▼
                          /tf
```

Revolute joint의 parent-to-child transform은 개념적으로 다음처럼 계산할 수 있다.

$$
{}^{\mathrm{parent}}\mathbf{T}_{\mathrm{child}}(t)
=
\mathbf{T}_{\mathrm{origin}}
\mathbf{R}_{\mathrm{axis}}(q(t))
$$

`origin`과 `axis`는 URDF에서 읽고 $q(t)$는 같은 joint 이름의 `sensor_msgs/msg/JointState.position`에서 읽는다. Prismatic joint라면 rotation 대신 joint axis 방향의 translation이 $q(t)$에 따라 달라진다. `robot_state_publisher`는 이 입력으로 forward kinematics를 계산하지만 joint position 자체를 sensor에서 측정하거나 추정하지는 않는다.

`/joint_states`를 만드는 주체는 실행 환경에 따라 달라진다.

| 실행 환경 | Joint position의 출처 | `/joint_states`를 publish하는 일반적인 component |
|---|---|---|
| 실제 robot | Encoder를 읽은 hardware state interface | `ros2_control`의 `joint_state_broadcaster` 또는 robot driver |
| Simulator | Simulation 안의 joint state | Simulator와 연결된 plugin 또는 bridge |
| Model 학습·화면 확인 | 사용자가 지정한 test position | `joint_state_publisher` 또는 `joint_state_publisher_gui` |

이름이 비슷하지만 `joint_state_publisher`와 `robot_state_publisher`의 출력은 다르다.

- `joint_state_publisher`는 test나 model 확인에 사용할 joint position을 `/joint_states`로 publish한다.
- `robot_state_publisher`는 URDF와 `/joint_states`를 결합해 link transform을 `/tf`로 publish한다.
- 실제 hardware에서는 encoder 값을 제공하는 driver나 `joint_state_broadcaster`가 `joint_state_publisher`의 자리를 대신할 수 있다.

따라서 dynamic joint transform의 전체 흐름은 “sensor가 TF를 직접 만든다”가 아니라 “상태 공급자가 $q(t)$를 전달하고 `robot_state_publisher`가 URDF 운동 규칙으로 TF를 계산한다”로 이해해야 한다. Odometry와 localization처럼 URDF joint 밖에서 만들어지는 dynamic transform은 [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)에서 설명한다.

### Link frame과 joint 이름

`robot_state_publisher`가 만드는 TF 관계의 parent와 child 이름은 link 이름이다. Joint 이름은 URDF에서 연결을 식별하고 movable joint의 경우 `/joint_states`에서 현재 position을 대응시키는 데 사용하며, 별도의 TF frame 이름이 아니다.

```text
[base_link] --(base_to_imu: fixed joint)--> [imu_link]
```

이 관계에서 TF display에 나타나는 frame은 `base_link`와 `imu_link`이고, `base_to_imu`는 두 frame 사이의 관계를 정의한 joint 이름이다.

URDF model은 하나의 root link를 가진 tree여야 한다. 하나의 child link에 parent joint를 둘 이상 지정하거나 joint가 cycle을 만들면 일반적인 URDF kinematic tree로 해석할 수 없다.

## 최소 sensor rig URDF

다음 model에서 `base_link`, `imu_link`와 `lidar_link`는 각각 link다. `base_to_imu`와 `base_to_lidar`라는 두 fixed joint가 sensor link를 `base_link`에 연결한다. 위치 값은 URDF와 TF 동작을 설명하기 위한 임의의 예제이며 특정 project나 실제 장비의 calibration 값이 아니다.

```xml
<?xml version="1.0"?>
<robot name="sensor_rig">
  <link name="base_link"/>
  <link name="imu_link"/>
  <link name="lidar_link"/>

  <joint name="base_to_imu" type="fixed">
    <parent link="base_link"/>
    <child link="imu_link"/>
    <origin xyz="0.00 0.00 0.10" rpy="0.00 0.00 0.00"/>
  </joint>

  <joint name="base_to_lidar" type="fixed">
    <parent link="base_link"/>
    <child link="lidar_link"/>
    <origin xyz="0.20 0.00 0.30" rpy="0.00 0.00 0.00"/>
  </joint>
</robot>
```

이 예제의 조건은 다음과 같다.

- `xyz`의 단위는 meter다.
- `rpy`의 단위는 radian이며 roll, pitch, yaw 순서다.
- `imu_link`는 `base_link`보다 z 방향으로 0.1 m 위에 있다.
- `lidar_link`는 `base_link`보다 x 방향으로 0.2 m 앞, z 방향으로 0.3 m 위에 있다.
- 두 sensor frame의 축은 `base_link` 축과 같은 방향이다.

따라서 예상 TF tree는 다음과 같다.

```text
base_link
├── imu_link       xyz = (0.00, 0.00, 0.10)
└── lidar_link     xyz = (0.20, 0.00, 0.30)
```

실제 장비에서는 숫자 예제를 그대로 사용하지 않고 sensor의 장착 위치와 축 방향을 측정해 기록한다. `origin`은 visual model을 보기 좋게 옮기는 값이 아니라 sensor data를 다른 frame으로 변환할 때 사용되는 calibration의 일부다.

## `robot_state_publisher`의 publish 규칙

`robot_state_publisher`는 시작할 때 `robot_description` parameter로 유효한 URDF string을 받아야 한다.

- Fixed joint의 transform은 시작할 때 `/tf_static`에 publish한다.
- Movable joint의 transform은 `/joint_states` message로 해당 joint가 갱신될 때 `/tf`에 publish한다.
- `/tf_static`과 `robot_description` topic은 transient-local durability를 사용하므로 호환되는 late subscriber가 마지막 상태를 받을 수 있다.

RViz2의 기본 TF listener는 `/tf_static`에 호환되는 QoS로 구독하므로 `robot_state_publisher`가 실행 중인 동안 RViz2가 나중에 시작해도 retained static transform을 받을 수 있다. RViz2 내부 listener, transformation backend와 TF display의 역할 구분은 [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)에서 설명한다.

앞의 sensor rig에는 fixed joint만 있다. 따라서 이 최소 예제에서는 별도의 `JointState` publisher가 없어도 `base_link` → `imu_link`, `base_link` → `lidar_link` transform을 만들 수 있다.

Root인 `base_link`에는 URDF 내부 parent가 없지만, 이것이 전체 runtime TF tree에서도 root라는 뜻은 아니다. 이 model만 실행하면 `world` 또는 `odom`에서 `base_link`로 이어지는 transform은 생기지 않는다. Odometry component가 `odom → base_link`를 publish하면 이 URDF subtree 전체가 `odom` 아래에 연결된다. `robot_state_publisher`는 `map → odom`이나 `odom → base_link`를 자동으로 만들지 않는다. RViz2의 Fixed Frame을 `base_link`로 선택하면 최소 model을 확인할 수 있고, 외부 global frame이 필요할 때는 그 관계를 담당하는 별도 component를 추가한다.

## URDF file을 package에 설치

ROS 2 executable은 build 전 source directory가 아니라 install space에서 runtime resource를 찾도록 구성하는 편이 재실행과 배포에 유리하다. 예제 package 이름을 `sensor_description`이라고 하면 다음 구조를 사용할 수 있다.

```text
sensor_description/
├── CMakeLists.txt
├── package.xml
├── launch/
│   └── publish_sensor_rig.launch.py
└── urdf/
    └── sensor_rig.urdf
```

`CMakeLists.txt`에 다음 install rule을 추가한다.

```cmake
install(
  DIRECTORY launch urdf
  DESTINATION share/${PROJECT_NAME}
)
```

이 rule은 build할 때 `launch`와 `urdf` directory를 package의 install share directory로 복사한다. Source file을 compile하는 rule이 아니라 runtime resource의 설치 위치를 정하는 rule이다.

`package.xml`에는 launch file이 runtime에 사용하는 dependency를 선언한다.

```xml
<exec_depend>ament_index_python</exec_depend>
<exec_depend>launch_ros</exec_depend>
<exec_depend>robot_state_publisher</exec_depend>
```

Package 이름과 dependency는 실제 package 구성에 맞춰 조정한다. 이미 C++ node가 있는 package에 URDF를 함께 둘 수도 있고, 여러 package가 같은 robot model을 사용한다면 description 전용 package로 분리할 수도 있다.

## Launch file로 model publish

ROS 2 `launch` system은 하나 이상의 process와 parameter를 같은 실행 description으로 시작하는 도구다. 다음 launch file은 install space의 URDF를 읽어 `robot_description` parameter로 전달하고 `robot_state_publisher` process를 시작한다.

```python
from pathlib import Path

from ament_index_python.packages import get_package_share_directory
from launch import LaunchDescription
from launch_ros.actions import Node


def generate_launch_description():
    package_share = Path(get_package_share_directory("sensor_description"))
    urdf_path = package_share / "urdf" / "sensor_rig.urdf"
    robot_description = urdf_path.read_text(encoding="utf-8")

    return LaunchDescription(
        [
            Node(
                package="robot_state_publisher",
                executable="robot_state_publisher",
                name="robot_state_publisher",
                output="screen",
                parameters=[{"robot_description": robot_description}],
            )
        ]
    )
```

이 예제에서 `get_package_share_directory()`는 현재 sourced environment에서 package의 install share directory를 찾는다. `read_text()`는 URDF file을 string으로 읽고, `Node` action의 `parameters`가 그 string을 `robot_description`에 설정한다.

Build와 실행 순서는 다음과 같다.

```bash
cd ~/ros2_ws
source /opt/ros/jazzy/setup.bash
colcon build --packages-select sensor_description

# 새 terminal
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
ros2 launch sensor_description publish_sensor_rig.launch.py
```

첫 terminal의 `--packages-select sensor_description`은 `sensor_description` package만 선택하고 그 package의 CMake install rule을 실행해 URDF와 launch file을 install space에 배치한다. URDF 자체를 compile하는 과정은 아니다. Package 선택 방식과 compile 및 install의 차이는 [Environment and Workspace](<./01 Environment and Workspace.md>)를 참고한다. 새 terminal의 두 setup script는 Jazzy underlay와 workspace overlay를 활성화한다. 마지막 command가 `robot_state_publisher` process를 실제로 시작한다.

## 단계별 검증

### 1. URDF syntax와 tree

`check_urdf` executable이 설치되어 있다면 XML parsing과 link tree를 먼저 확인할 수 있다.

```bash
check_urdf src/sensor_description/urdf/sensor_rig.urdf
```

이 command가 성공해도 ROS node가 실행됐다는 뜻은 아니다. File이 URDF parser에서 유효하고 tree를 만들 수 있다는 뜻이다.

### 2. Node와 parameter

Launch 후 node와 parameter를 확인한다.

```bash
ros2 node list
ros2 param list /robot_state_publisher
```

Node 목록에 `/robot_state_publisher`가 있고 parameter 목록에 `robot_description`이 있어야 한다. Node가 시작 직후 종료됐다면 terminal log에서 URDF parsing error와 parameter 누락을 먼저 확인한다.

### 3. TF tree

```bash
ros2 run tf2_tools view_frames
ros2 run tf2_ros tf2_echo base_link imu_link
ros2 run tf2_ros tf2_echo base_link lidar_link
```

`view_frames`는 일정 시간 동안 `/tf`와 `/tf_static`에서 관측한 frame 관계를 `frames.pdf`로 저장한다. 이 file은 명령을 실행한 당시 TF tree의 snapshot이므로, file이 남아 있어도 현재 broadcaster가 실행 중이라는 뜻은 아니다.

`frames.pdf`에서 예상한 세 frame이 하나의 tree로 연결되는지 확인한다. `tf2_echo`의 translation이 URDF의 `origin xyz`와 일치하고 rotation이 identity인지도 확인한다.

### 4. RViz2

RViz2에서 Global Options의 Fixed Frame을 `base_link`로 설정하고 TF display를 추가한다. Empty link에는 visual geometry가 없으므로 RobotModel display에 body mesh가 보이지 않아도 TF display의 세 frame 축은 보여야 한다.

## Visual, collision과 inertial

URDF link에는 frame 관계 외에 geometry와 dynamics 정보를 추가할 수 있다.

| Element | 역할 |
|---|---|
| `<visual>` | RViz2 같은 visualizer에 표시할 geometry와 material |
| `<collision>` | Collision checking에 사용할 geometry |
| `<inertial>` | Mass, center of mass와 inertia tensor |

이 세 element는 link의 용도를 확장하지만 fixed joint transform을 publish하는 데 필수는 아니다. TF tree와 sensor frame을 확인하는 최소 smoke test에서는 empty link로 시작하고, RobotModel 자체를 표시해야 할 때 visual geometry를 추가할 수 있다.

URDF에 `<visual>`을 추가했다고 sensor 측정값이 생기거나 Gazebo simulation이 자동으로 실행되는 것은 아니다. Sensor simulation과 physics는 simulator plugin과 simulation description을 별도로 구성해야 한다.

## 자주 혼동하는 관계

| 표현 | 정확한 구분 |
|---|---|
| URDF file을 만들면 TF가 publish된다. | `robot_state_publisher` 같은 broadcaster process가 URDF를 읽고 실행되어야 한다. |
| URDF와 TF tree는 같은 data다. | URDF는 model description이고 TF tree는 실행 중 publish된 frame 관계다. |
| URDF file이 고정되어 있으면 dynamic TF를 만들 수 없다. | URDF의 운동 규칙은 고정되어 있지만 movable joint의 position은 `/joint_states`로 갱신되므로 `robot_state_publisher`가 dynamic TF를 만들 수 있다. |
| `robot_state_publisher`가 encoder를 읽어 joint state를 추정한다. | `robot_state_publisher`는 다른 component가 publish한 `/joint_states`를 사용해 link transform을 계산한다. |
| Fixed joint에도 `JointState`가 필요하다. | Fixed joint는 startup에 `/tf_static`으로 publish되며 joint position이 필요하지 않다. |
| `imu_link`와 `lidar_link`가 fixed joint다. | 두 이름은 link다. `base_to_imu`와 `base_to_lidar`가 fixed joint다. |
| Fixed child link는 world에서 움직이지 않는다. | Parent와 child의 상대 pose만 고정되며 parent가 움직이면 child도 함께 움직인다. |
| Joint 이름이 TF frame으로 나타난다. | TF의 parent와 child는 link 이름이며 joint 이름은 그 관계를 식별한다. |
| Root link는 자동으로 `world`에 연결된다. | URDF root에는 내부 parent가 없으며 global frame transform은 별도 publisher가 담당한다. |
| URDF root는 전체 runtime TF tree의 root다. | URDF root는 odometry 같은 외부 broadcaster가 publish하는 `odom` 또는 `world` transform 아래에 연결될 수 있다. |
| Empty link는 잘못된 link다. | Geometry가 없어 보이지 않을 뿐 fixed sensor frame을 정의하는 최소 link로 사용할 수 있다. |
| URDF와 command가 같은 child transform을 publish해도 안전하다. | 같은 child의 transform source가 중복되므로 하나의 authoritative publisher만 사용해야 한다. |

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)
- [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)
- [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)
- [Environment and Workspace](<./01 Environment and Workspace.md>)

## References

- [ROS 2 Documentation - Using URDF with robot_state_publisher](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/URDF/Using-URDF-with-Robot-State-Publisher.rst)
- [ROS 2 Documentation - Building a Visual Robot Model from Scratch](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/URDF/Building-a-Visual-Robot-Model-with-URDF-from-Scratch.rst)
- [robot_state_publisher - Jazzy Branch](https://github.com/ros/robot_state_publisher/tree/jazzy)
- [joint_state_publisher - Jazzy Documentation](https://docs.ros.org/en/jazzy/p/joint_state_publisher/)
- [ros2_control - joint_state_broadcaster](https://control.ros.org/jazzy/doc/ros2_controllers/joint_state_broadcaster/doc/userdoc.html)
- [URDF XML Joint Specification](http://wiki.ros.org/urdf/XML/joint)
