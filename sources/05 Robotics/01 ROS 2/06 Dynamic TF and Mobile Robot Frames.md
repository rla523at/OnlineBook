# ROS 2 Dynamic TF and Mobile Robot Frames

## 한 줄 요약

Dynamic transform의 시각별 값은 joint state, odometry 또는 localization을 책임지는 component가 측정값과 model로 계산해 `/tf`에 publish하고, tf2 buffer는 받은 sample을 시간별로 저장·보간·합성한다.

## 문서 범위와 선행 개념

이 문서는 [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)의 transform·broadcaster·listener·buffer와 [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)의 link·joint를 알고 있다고 가정한다. 여기서는 다음 질문에 집중한다.

- Sensor가 측정한 값은 어떤 component로 전달되는가?
- 시간에 따라 변하는 transform은 누가 계산하는가?
- TF2가 계산하는 것과 state estimation component가 계산하는 것은 어떻게 다른가?
- `robot_state_publisher`, `joint_state_broadcaster`, `diff_drive_controller` 같은 기존 package는 어디에 들어가는가?
- `map → odom → base_link` transform은 각각 누가 책임지는가?

## Dynamic transform은 누가 계산하는가

TF2는 encoder, IMU 또는 camera 측정으로 robot의 물리 상태를 직접 추정하지 않는다. **해당 parent-child 관계를 소유한 component**가 시각 $t$의 상대 pose를 계산하고, transform broadcaster를 사용해 `TransformStamped`로 publish한다.

일반적인 흐름은 다음과 같다.

```text
physical sensor
      │ raw signal·packet
      ▼
hardware driver 또는 ros2_control hardware component
      │ ROS message 또는 state interface
      ▼
kinematics·odometry·localization component
      │ state x̂(t)에서 relative pose 계산
      ▼
TransformStamped T(t)
      │ TransformBroadcaster 또는 component 내부 TF publisher
      ▼
     /tf
      │
      ▼
listener별 tf2 buffer
```

이 단계들이 항상 서로 다른 process인 것은 아니다. Driver가 pose까지 계산해 직접 TF를 publish할 수도 있고, odometry component 안에 transform broadcaster가 포함될 수도 있다. 중요한 조건은 각 child transform에 하나의 authoritative publisher를 두고 계산 책임을 명확하게 정하는 것이다.

### 측정값, 상태와 transform

`state`는 시각 $t$에 system의 현재 구성을 나타내며, 필요한 변수는 대상에 따라 다르다. Sensor가 이 상태를 직접 완전하게 측정한다고 가정하면 안 된다.

| 구분 | 예 | 역할 |
|---|---|---|
| Measurement $z(t)$ | Encoder tick, IMU angular velocity·acceleration, camera image | Sensor나 driver가 제공하는 관측값 |
| State $\hat{x}(t)$ | Joint position $q(t)$, robot pose $(x,y,\theta)$ | Measurement와 model로 직접 읽거나 추정한 현재 상태 |
| Transform $\mathbf{T}(t)$ | Translation과 rotation quaternion | 두 frame 사이의 relative pose |

예를 들어 wheel encoder는 보통 `odom`에서 본 robot의 $x$, $y$, $\theta$를 직접 측정하지 않는다. Odometry component가 좌우 wheel의 회전량을 적분해 pose를 추정하고 그 결과로 `odom → base_link` transform을 만든다.

Sensor 측정값은 일반 ROS 2 topic 또는 `ros2_control` state interface를 통해 다음 component로 전달된다. Topic은 publish-subscribe 방식이므로 수신자가 한 node로 고정되지 않으며, 같은 type과 호환되는 QoS로 해당 topic을 subscribe한 여러 node가 받을 수 있다.

## 역할과 기존 ROS 2 component

Broadcaster와 driver는 모두 software로 구현되지만 사용자가 항상 직접 작성하는 것은 아니다. 표준적인 역할에는 설치해서 설정할 수 있는 package가 있고, project 고유의 계산만 직접 구현한다.

| 책임 | 입력 | 출력 | 일반적인 구현 |
|---|---|---|---|
| 실제 joint state 읽기 | Encoder와 hardware feedback | `ros2_control` state interface | Vendor driver 또는 custom hardware component |
| Joint state 배포 | `ros2_control` state interface | `/joint_states` | `joint_state_broadcaster` plugin |
| Link transform 계산 | URDF와 `/joint_states` | Movable joint `/tf`, fixed joint `/tf_static` | `robot_state_publisher` node |
| Differential-drive odometry | Wheel position·velocity feedback | Odometry와 선택적인 `odom → base_link` TF | `diff_drive_controller` plugin |
| Sensor fusion | Wheel odometry, IMU 등 | Filtered pose·odometry와 설정에 따른 TF | State estimation package 또는 custom node |
| Global pose 보정 | Map, lidar·camera·GPS 관측 | 일반적으로 `map → odom` TF | SLAM 또는 localization component |
| 고정 장착 관계 | Calibration 또는 상수 pose | `/tf_static` | `robot_state_publisher`, `static_transform_publisher` |
| Project 고유 관계 | Application별 state | `/tf` 또는 `/tf_static` | `tf2_ros::TransformBroadcaster`를 사용하는 custom node |

`tf2_ros::TransformBroadcaster`는 계산 알고리즘이 아니라 `TransformStamped`를 ROS graph에 보내는 API다. Custom node를 작성할 때도 먼저 application logic으로 translation과 rotation을 계산한 뒤 `sendTransform()`에 전달한다.

### Joint state 경로

실제 robot에서 `joint_state_broadcaster`는 encoder를 직접 구동하는 device driver가 아니다. Hardware component가 채운 state interface를 읽어 표준 `sensor_msgs/msg/JointState` message로 `/joint_states`에 publish한다.

```text
encoder
  → hardware component
  → joint state interface
  → joint_state_broadcaster
  → /joint_states: q(t)
  → robot_state_publisher + URDF
  → link dynamic TF
```

`robot_state_publisher`가 수행하는 forward kinematics와 movable joint 계산은 [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)에서 설명한다.

## Dynamic transform의 시간별 sample

Parent frame을 `A`, child frame을 `B`라고 하면 실제 관계는 시간 함수 ${}^{A}\mathbf{T}_{B}(t)$로 생각할 수 있다. 그러나 `/tf`에는 연속 함수가 아니라 timestamp가 붙은 sample이 전달된다.

```text
t0:  TransformStamped(A, B, T(t0))
t1:  TransformStamped(A, B, T(t1))
t2:  TransformStamped(A, B, T(t2))
```

Broadcaster는 새 상태를 계산할 때마다 현재 sample을 publish한다. Listener의 tf2 buffer는 일정 시간 동안 sample history를 보관한다. Consumer가 중간 시각을 조회하면 buffer는 사용 가능한 두 sample 사이에서 translation을 선형 보간하고 rotation quaternion을 spherical linear interpolation하며, TF tree 경로에 여러 edge가 있으면 같은 query time의 transform을 inverse·합성한다.

이때 두 종류의 계산을 구분해야 한다.

| 계산 | 담당 |
|---|---|
| Sensor measurement에서 joint position이나 robot pose를 구한다. | Driver, kinematics, odometry, state estimation, SLAM·localization component |
| 이미 publish된 transform sample에서 query time과 source-to-target transform을 구한다. | Listener의 tf2 buffer |

TF2 buffer는 보관 범위 밖의 미래 motion을 예측하거나 오래되어 삭제된 transform을 복원하지 않는다.

## 이동 robot에서 자주 사용하는 frame

REP-105는 mobile robot에서 `map`, `odom`과 `base_link`라는 frame 이름에 공통 의미를 부여한다. `map`과 `odom`은 환경에 고정된 기준이고 `base_link`는 robot body에 붙어 함께 움직이는 기준이다.

| Frame | 역할 | Robot pose의 특성 |
|---|---|---|
| `map` | Localization이 사용하는 장기적인 world-fixed 기준 | 장기 drift를 억제하지만 새 관측을 반영할 때 pose가 불연속적으로 보정될 수 있다. |
| `odom` | Odometry가 상대 이동을 누적하는 world-fixed 기준 | 값이 연속적으로 변하지만 장시간에는 drift가 누적될 수 있다. |
| `base_link` | Robot body에 고정된 기준 | Robot이 이동하면 `map`과 `odom`에 대한 pose가 변한다. |

대표적인 TF tree는 다음과 같다.

```text
map
└── odom
    └── base_link
        ├── arm_link
        ├── imu_link
        └── lidar_link
```

각 edge는 서로 다른 component가 책임질 수 있다.

| Parent → child | 일반적인 소유자 | 나타내는 pose |
|---|---|---|
| `map → odom` | SLAM 또는 localization component | Global 관측으로 계산한 odometry drift 보정 |
| `odom → base_link` | Wheel·visual odometry 또는 state estimation component | `odom`에서 표현한 `base_link`의 연속적인 pose |
| `base_link → movable link` | `robot_state_publisher` | URDF와 joint position으로 계산한 relative pose |
| `base_link → fixed sensor` | `robot_state_publisher` | URDF 또는 calibration에 기록한 고정 장착 pose |

URDF root인 `base_link`에는 model 내부 parent가 없지만, runtime TF tree에서는 odometry component가 publish한 `odom → base_link` 아래에 연결될 수 있다. `robot_state_publisher`만 실행한다고 `map`이나 `odom` frame이 자동으로 생기지는 않는다.

## Wheel odometry가 `odom → base_link`를 만드는 과정

평면에서 wheel slip 없이 움직이고 양쪽 wheel의 forward 회전 부호와 radius가 같다고 가정하자. Differential-drive robot의 wheel radius를 $r$, 좌우 wheel 사이 거리를 $b$, 한 update 동안 좌우 wheel 각도 변화를 $\Delta\phi_L$, $\Delta\phi_R$라고 두면 먼저 좌우 이동 거리를 계산할 수 있다.

$$
\Delta s_L = r\Delta\phi_L,
\qquad
\Delta s_R = r\Delta\phi_R
$$

Robot 중심의 이동 거리와 heading 변화는 다음과 같다.

$$
\Delta s = \frac{\Delta s_R + \Delta s_L}{2},
\qquad
\Delta\theta = \frac{\Delta s_R - \Delta s_L}{b}
$$

Odometry component는 이 상대 이동을 이전 pose에 누적해 $(x(t), y(t), \theta(t))$를 갱신한다. 이 pose의 translation과 yaw를 quaternion으로 바꾼 값이 시각 $t$의 `odom → base_link` transform이 된다.

```text
wheel encoder feedback
  → 좌우 wheel 이동량 계산
  → pose (x, y, θ) 누적
  ├── nav_msgs/msg/Odometry publish
  └── odom → base_link TransformStamped publish
```

`diff_drive_controller`는 이 역할을 제공하는 `ros2_control` controller plugin이다. 기본 feedback 구성에서는 wheel position 또는 velocity state interface로 odometry를 계산하며, `enable_odom_tf=true`일 때 `odom_frame_id → base_frame_id` transform도 publish한다. `open_loop=true`인 구성은 hardware feedback 대신 command 값으로 odometry를 계산하므로 실제 이동 측정과 동일하게 취급하면 안 된다.

`/odom` topic과 `odom` frame도 구분해야 한다.

| 대상 | 의미 |
|---|---|
| `odom` frame | Pose를 표현하는 coordinate frame 이름 |
| `/odom` topic | 관례적으로 `nav_msgs/msg/Odometry` message를 전달하는 topic 이름 |
| `odom → base_link` TF | `odom`에서 표현한 `base_link`의 시간별 pose |

`/odom` message를 publish하는 것만으로 tf2 buffer에 transform이 자동으로 생기지는 않는다. 같은 component가 topic과 TF를 모두 제공하거나 별도 component가 TF를 publish하도록 system interface를 구성해야 한다. 같은 child인 `base_link`의 TF를 두 component가 동시에 publish하지 않도록 한다.

## Localization이 `map → odom`을 만드는 과정

Localization component는 lidar, camera 또는 GPS 같은 관측을 map이나 외부 기준과 비교해 `map`에서 본 `base_link` pose ${}^{\mathrm{map}}\mathbf{T}_{\mathrm{base\_link}}$를 추정할 수 있다. 그러나 TF tree에서 `base_link`의 parent가 이미 `odom`이면 `map → base_link`를 두 번째 edge로 직접 publish하지 않는다. 같은 시각의 odometry pose와 합성했을 때 global pose가 되도록 `map → odom`을 계산한다.

$$
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{odom}}
=
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{base\_link}}
\left(
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}
\right)^{-1}
$$

전체 관계는 다음처럼 합성된다.

$$
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{base\_link}}
=
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{odom}}
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}
$$

예를 들어 odometry가 누적 이동을 `10.3 m`로 계산했지만 localization이 map의 wall과 lidar 관측을 비교해 global pose를 `10.0 m`로 추정했다면 `map → odom`의 x 보정은 `-0.3 m`가 될 수 있다.

```text
map → odom       -0.3 m
odom → base_link 10.3 m
합성한 map → base_link 10.0 m
```

이때 실제 robot이 순간 이동한 것이 아니다. `odom → base_link`의 연속적인 단기 motion은 유지하고 `map → odom`을 바꿔 global pose 추정값을 보정한 것이다.

`map`의 원점은 map 영역의 기하학적 중심으로 자동 결정되지 않는다. SLAM system은 시작 pose를 원점으로 사용할 수 있고, 미리 만든 map은 건물 기준점이나 측량 기준을 원점으로 사용할 수 있다. Application이 원점과 축 방향을 명시해야 한다.

## Transform과 measurement timestamp

Sensor message의 `header.stamp`는 일반적으로 data가 측정된 시각을 나타낸다. 움직이는 frame에서 측정한 data를 다른 frame에서 표현하려면 consumer가 message를 처리하는 현재 시각이 아니라 **측정 시각의 transform**이 필요하다.

예를 들어 lidar가 body에 고정되어 있으면 `base_link → lidar_link`는 static이지만 robot이 움직일 때 `odom → lidar_link`는 다음처럼 시간에 따라 달라진다.

$$
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{lidar\_link}}(t)
=
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}(t)
{}^{\mathrm{base\_link}}\mathbf{T}_{\mathrm{lidar\_link}}
$$

따라서 PointCloud2의 `header.frame_id`가 계속 `lidar_link`여도 `odom`에서 본 point 위치는 측정 시각마다 달라질 수 있다.

tf2 buffer가 보관한 한 dynamic transform의 시간 범위를 기준으로 query 결과를 나누면 다음과 같다.

| Query time 조건 | 의미 | 결과 |
|---|---|---|
| `query time < oldest transform time` | 필요한 sample이 buffer에서 이미 삭제되었거나 처음부터 수신되지 않았다. | Past extrapolation error |
| `oldest transform time ≤ query time ≤ latest transform time` | 저장된 sample 또는 앞뒤 sample 사이의 보간을 사용할 수 있다. | 시간 범위 조건을 만족한다. |
| `latest transform time < query time` | 필요한 시각의 sample이 아직 buffer에 도착하지 않았다. | Future extrapolation error |

예를 들어 sensor message의 stamp가 `10.20 s`인데 listener가 받은 최신 dynamic transform이 `10.15 s`까지라면 `10.20 s` query는 buffer 기준의 미래다. 필요한 transform이 도착할 때까지 기다리거나 message filter 같은 동기화 방법을 사용해야 한다.

여기서 past와 future는 wall-clock 현재와의 비교가 아니라 query time과 buffer의 oldest·latest transform time을 비교한 표현이다. 여러 dynamic edge를 합성할 때는 경로의 모든 edge가 query time을 지원해야 한다. Static transform은 값이 변하지 않으므로 연결이 존재하면 모든 측정 시각에 사용할 수 있다.

## 실행 중 책임과 시간 확인

대표적인 dynamic transform을 계속 조회한다.

```bash
ros2 run tf2_ros tf2_echo map odom
ros2 run tf2_ros tf2_echo odom base_link
```

첫 번째 명령은 ${}^{\mathrm{map}}\mathbf{T}_{\mathrm{odom}}$, 두 번째 명령은 ${}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}$를 출력한다. 해당 SLAM, localization 또는 odometry component를 실행하지 않았다면 frame이 없는 것이 정상일 수 있다.

누가 `/tf`를 publish하는지도 확인한다.

```bash
ros2 node list
ros2 topic info /tf --verbose
ros2 run tf2_tools view_frames
```

`/tf`에는 여러 broadcaster가 함께 publish할 수 있으므로 topic publisher가 여러 개라는 사실 자체는 오류가 아니다. 문제는 같은 child frame의 transform을 둘 이상의 broadcaster가 동시에 소유하는 경우다.

## 문제 확인 순서

| 관찰 | 확인할 내용 |
|---|---|
| `/joint_states`는 있지만 movable link TF가 없다. | Joint 이름이 URDF와 일치하는지, position field가 있는지와 `robot_state_publisher`가 실행 중인지 확인한다. |
| `/odom` topic은 있지만 `odom` frame이 없다. | Odometry component가 TF도 publish하도록 설정됐는지 확인한다. `diff_drive_controller`라면 `enable_odom_tf`를 확인한다. |
| `map`과 `base_link`는 있지만 tree가 끊겨 있다. | `map → odom`과 `odom → base_link`의 소유 component가 각각 실행 중인지 확인한다. |
| Future extrapolation error가 발생한다. | Query time, latest TF time, publish·network·processing 지연과 clock source를 확인한다. |
| Past extrapolation error가 발생한다. | 오래된 sensor message가 처리되는지, buffer 보관 시간과 bag replay clock을 확인한다. |
| Dynamic frame이 튀거나 parent가 바뀐다. | 같은 child transform을 둘 이상의 broadcaster가 publish하는지 확인한다. |
| 실제 motion과 TF 방향이 반대다. | Encoder 부호, wheel 이름, parent-child 방향과 quaternion convention을 확인한다. |

Simulation이나 rosbag replay에서는 관련 node가 모두 같은 ROS clock을 사용하도록 `use_sim_time` 설정을 맞춘다.

## 관련 문서

- [Robotics](<../Robotics.md>)
- [Node and Topic](<./02 Node and Topic.md>)
- [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)
- [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)

## References

- [REP-105 - Coordinate Frames for Mobile Platforms](https://github.com/ros-infrastructure/rep/blob/master/rep-0105.rst)
- [geometry_msgs - TransformStamped Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/geometry_msgs/msg/TransformStamped.msg)
- [nav_msgs - Odometry Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/nav_msgs/msg/Odometry.msg)
- [robot_state_publisher - Jazzy Branch](https://github.com/ros/robot_state_publisher/tree/jazzy)
- [ros2_control - joint_state_broadcaster](https://control.ros.org/jazzy/doc/ros2_controllers/joint_state_broadcaster/doc/userdoc.html)
- [ros2_control - diff_drive_controller](https://control.ros.org/jazzy/doc/ros2_controllers/diff_drive_controller/doc/userdoc.html)
- [tf2 - Time Cache](https://github.com/ros2/geometry2/blob/jazzy/tf2/include/tf2/time_cache.hpp)
- [ROS 2 Documentation - Writing a TF2 Broadcaster in C++](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Writing-A-Tf2-Broadcaster-Cpp.rst)
