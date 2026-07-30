# ROS 2 PointCloud2 and RViz2

## 한 줄 요약

`sensor_msgs/msg/PointCloud2`는 point의 binary layout, 측정 시각과 coordinate frame을 함께 전달하고, RViz2는 message time의 tf2 transform을 사용해 point cloud를 Fixed Frame에 표시한다.

## Point cloud visualization 흐름

`point cloud`는 3D 공간의 point 집합이다. Lidar, depth camera와 3D reconstruction algorithm은 point마다 x, y, z 좌표와 intensity, color 같은 추가 값을 만들 수 있다.

ROS 2에서 point cloud를 RViz2에 표시하려면 message와 transform이 함께 준비되어야 한다.

```text
PointCloud2 publisher
├── topic: synthetic_points
├── header.frame_id: lidar_link
├── header.stamp: measurement time
└── x, y, z point data
          │
          ▼
       RViz2
          │
          ├── Fixed Frame: base_link
          └── tf2 query: lidar_link ↔ base_link at message time
                    │
                    ▼
          point를 base_link 기준으로 표시
```

PointCloud2 topic만 존재하고 TF tree가 없으면 RViz2는 `lidar_link`의 point를 `base_link`로 옮길 수 없다. 반대로 TF tree가 있어도 PointCloud2의 layout이나 `frame_id`가 잘못되면 올바른 cloud를 표시할 수 없다.

## PointCloud2 message

`sensor_msgs/msg/PointCloud2`는 N-dimensional point를 binary byte array로 저장하는 ROS interface다. `data`만으로는 각 byte가 어떤 값인지 알 수 없으므로 `fields`와 step 정보가 layout을 함께 설명한다.

| Field | 의미 |
|---|---|
| `header.stamp` | Sensor data를 측정한 ROS time |
| `header.frame_id` | Point 좌표가 표현된 coordinate frame |
| `height` | Organized cloud의 row 수 |
| `width` | 한 row의 point 수 |
| `fields` | Point 안의 x, y, z, intensity 같은 field 이름, type과 byte offset |
| `is_bigendian` | Binary data의 byte order |
| `point_step` | Point 한 개가 차지하는 byte 수 |
| `row_step` | Row 한 개가 차지하는 byte 수 |
| `data` | 실제 point 값을 담은 byte array |
| `is_dense` | Invalid point가 없으면 `true` |

Lidar처럼 1D point list로 사용할 unorganized cloud는 `height = 1`, `width = point count`로 표현한다. Depth image의 pixel 구조를 유지하는 organized cloud는 `height`와 `width`를 2D 크기로 설정한다.

`PointCloud2`가 `std::vector<Point>`와 같다고 설명할 수는 없다. Point type이 compile time에 고정된 container가 아니라, `fields`가 runtime binary layout을 기술하는 message이기 때문이다.

## Frame과 timestamp

Point 좌표가 `lidar_link` 기준으로 계산되었다면 다음처럼 기록한다.

```cpp
cloud.header.frame_id = "lidar_link";
cloud.header.stamp = now().to_msg();
```

- `frame_id`는 point 숫자의 기준 frame이다.
- `stamp`는 publish를 호출한 시각이 아니라 측정이 이루어진 시각이어야 한다.

합성 cloud를 생성 즉시 publish하는 smoke test에서는 node의 현재 ROS time을 측정 시각으로 사용할 수 있다. Driver queue나 algorithm processing delay가 있는 실제 sensor pipeline에서는 원본 측정 시각을 유지해야 한다.

좌표값을 변환하지 않고 `frame_id`만 `base_link`로 바꾸면 point가 `base_link` 좌표가 되지 않는다. 실제 tf2 transform을 각 point에 적용했을 때만 output frame을 바꿀 수 있다.

## C++에서 작은 합성 cloud 만들기

`sensor_msgs` package는 `PointCloud2Modifier`와 `PointCloud2Iterator`를 제공한다. Modifier는 field layout과 buffer size를 구성하고, iterator는 field offset을 직접 계산하지 않고 각 point 값을 쓸 수 있게 한다.

다음 함수는 `lidar_link` frame에 있는 다섯 point를 xyz float field로 만든다. 이 예제는 little-endian host를 대상으로 하며 모든 point가 finite value이므로 `is_dense`를 `true`로 설정한다.

```cpp
#include <array>

#include "builtin_interfaces/msg/time.hpp"
#include "sensor_msgs/msg/point_cloud2.hpp"
#include "sensor_msgs/point_cloud2_iterator.hpp"

sensor_msgs::msg::PointCloud2 make_cloud(
  const builtin_interfaces::msg::Time & stamp)
{
  const std::array<std::array<float, 3>, 5> points = {{
    {0.0F, 0.0F, 0.0F},
    {1.0F, 0.0F, 0.0F},
    {1.0F, 1.0F, 0.0F},
    {0.0F, 1.0F, 0.0F},
    {0.5F, 0.5F, 0.5F},
  }};

  sensor_msgs::msg::PointCloud2 cloud;
  cloud.header.stamp = stamp;
  cloud.header.frame_id = "lidar_link";
  cloud.height = 1;
  cloud.is_bigendian = false;
  cloud.is_dense = true;

  sensor_msgs::PointCloud2Modifier modifier(cloud);
  modifier.setPointCloud2FieldsByString(1, "xyz");
  modifier.resize(points.size());

  sensor_msgs::PointCloud2Iterator<float> x(cloud, "x");
  sensor_msgs::PointCloud2Iterator<float> y(cloud, "y");
  sensor_msgs::PointCloud2Iterator<float> z(cloud, "z");

  for (const auto & point : points) {
    *x = point[0];
    *y = point[1];
    *z = point[2];
    ++x;
    ++y;
    ++z;
  }

  return cloud;
}
```

입력은 measurement timestamp이고, 처리 결과는 다음 조건을 만족하는 `PointCloud2` message다.

- `height = 1`, `width = 5`인 unorganized cloud
- `x`, `y`, `z` float field
- `lidar_link` 기준 point 좌표
- 0 또는 1 m 범위에 있는 확인 가능한 pyramid 형태

`modifier.resize()`는 point 수에 맞게 `data`, `width`와 `row_step`을 조정한다. `setPointCloud2FieldsByString()`은 xyz field의 offset과 `point_step`을 구성한다. Field offset과 byte buffer를 수동으로 작성할 수도 있지만, layout 계산 실수를 줄이기 위해 제공된 helper를 사용하는 편이 이 예제의 목적에 맞다.

## Publisher에서 사용

Full C++ node의 생성과 build 구조는 [Node and Topic](<./02 Node and Topic.md>)에서 설명한다. 같은 `rclcpp::Node` 안에서 PointCloud2 publisher를 다음처럼 만들 수 있다.

```cpp
cloud_publisher_ =
  create_publisher<sensor_msgs::msg::PointCloud2>(
    "synthetic_points",
    rclcpp::QoS(10));
```

Timer callback에서는 현재 ROS time으로 cloud를 만들고 publish한다.

```cpp
cloud_publisher_->publish(make_cloud(now().to_msg()));
```

RViz2를 publisher보다 늦게 실행해도 cloud를 받을 수 있도록 smoke test에서는 1 Hz처럼 낮은 주기로 계속 publish할 수 있다. Volatile durability로 message 한 개만 먼저 publish하고 process가 기다리면 뒤늦게 연결된 subscriber는 이전 message를 받지 못한다.

`CMakeLists.txt`에서는 `sensor_msgs`를 찾고 target dependency에 추가한다.

```cmake
find_package(sensor_msgs REQUIRED)

ament_target_dependencies(
  synthetic_cloud_publisher
  rclcpp
  sensor_msgs
)
```

`package.xml`에도 package가 build와 runtime에 사용하는 `sensor_msgs` dependency를 선언한다.

```xml
<depend>sensor_msgs</depend>
```

## Message와 topic 확인

Interface definition을 확인한다.

```bash
ros2 interface show sensor_msgs/msg/PointCloud2
```

Publisher를 실행한 뒤 topic type, endpoint와 QoS를 확인한다.

```bash
ros2 topic list -t
ros2 topic find sensor_msgs/msg/PointCloud2
ros2 topic info /synthetic_points --verbose
ros2 topic hz /synthetic_points
ros2 topic echo /synthetic_points --once --field header
```

- `list -t`는 topic 이름과 message type을 확인한다.
- `find`는 현재 graph에서 `PointCloud2` type을 사용하는 topic만 찾는다.
- `info --verbose`는 publisher와 subscriber의 QoS profile을 확인한다.
- `hz`는 cloud가 기대한 주기로 계속 publish되는지 확인한다.
- `echo --field header`는 큰 binary `data` 대신 frame과 timestamp를 우선 확인한다.

Point 수와 layout은 다음 field를 함께 확인한다.

```bash
ros2 topic echo /synthetic_points --once \
  --field height

ros2 topic echo /synthetic_points --once \
  --field width

ros2 topic echo /synthetic_points --once \
  --field fields
```

CLI version에 따라 한 번에 지정할 수 있는 `--field` 범위가 다를 수 있으므로 필요한 field를 개별 command로 확인한다.

## RViz2에서 표시

Publisher와 TF broadcaster가 실행 중인 상태에서 같은 ROS domain을 사용하는 새 terminal을 연다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
rviz2
```

RViz2에서 다음 순서로 설정한다.

1. Global Options의 **Fixed Frame**을 `base_link`로 설정한다.
2. **Add**에서 **TF** display를 추가한다.
3. TF tree에 `base_link`, `imu_link`, `lidar_link`가 보이는지 확인한다.
4. **Add**에서 **PointCloud2** display를 추가한다.
5. Topic을 `/synthetic_points`로 선택한다.
6. Point가 너무 작으면 Size 값을 조정하되 position 자체가 맞는지 먼저 확인한다.
7. Display status가 `OK`인지 확인한다.

자주 확인하는 property는 다음과 같다.

| Property | 확인할 내용 |
|---|---|
| Global Options → Fixed Frame | Cloud의 `header.frame_id`와 TF로 연결된 공통 기준 frame |
| TF → Show Axes·Show Names | Frame 축과 이름을 화면에 표시할지 여부 |
| PointCloud2 → Topic | `PointCloud2` message를 publish하는 topic |
| PointCloud2 → Reliability Policy | Publisher endpoint와 호환되는 reliability |
| PointCloud2 → Style·Size | Point의 화면 표현 크기이며 좌표값 자체를 바꾸지는 않는다. |
| PointCloud2 → Color Transformer | `RGB`, `intensity` 같은 실제 field 구성에 맞는 색상 규칙 |

Fixed Frame은 모든 data를 표시할 공통 기준이다. Cloud의 `frame_id`가 `lidar_link`이면 RViz2는 message timestamp에서 `lidar_link`와 `base_link` 사이의 tf2 transform을 조회한다. 앞 문서의 static sensor transform을 사용하면 cloud 전체가 lidar 장착 위치와 방향을 반영해 표시된다.

따라서 화면 표시에는 PointCloud2 message가 실제로 수신되는 조건과
`header.frame_id`에서 Fixed Frame까지 transform을 조회할 수 있는 조건이 모두
필요하다. Style, Size와 Color Transformer는 이 두 조건이 충족된 뒤 화면 표현을
조정하는 property다.

설정을 다시 사용하려면 **File → Save Config As**로 `.rviz` file을 저장한다. 저장한 configuration은 다음 command로 다시 열 수 있다.

```bash
rviz2 -d path/to/sensor_smoke.rviz
```

RViz configuration은 display, topic, Fixed Frame과 view 설정을 재현하지만 publisher나 TF broadcaster process를 시작하지는 않는다.

## QoS compatibility

QoS(Quality of Service)는 message 전달 신뢰성, history와 durability 같은 통신 동작을 정한다. Publisher와 RViz2 PointCloud2 subscriber의 QoS가 호환되지 않으면 topic 이름과 type이 같아도 data가 전달되지 않는다.

RViz2 PointCloud2 display의 Reliability Policy를 publisher와 호환되게 설정하고 다음 command로 실제 endpoint를 확인한다.

```bash
ros2 topic info /synthetic_points --verbose
```

Smoke test에서는 publisher와 RViz2가 모두 지원하는 reliability를 명시하고, publish rate와 point 수를 작게 유지하면 frame과 layout 문제를 QoS 또는 성능 문제와 분리하기 쉽다.

## TF와 PointCloud2 통합 검증

다음 항목을 순서대로 확인한다.

```text
[ ] /synthetic_points type이 sensor_msgs/msg/PointCloud2다.
[ ] header.frame_id가 lidar_link다.
[ ] header.stamp가 publish마다 진행한다.
[ ] height=1이고 width가 예상 point 수와 같다.
[ ] base_link에서 lidar_link transform을 조회할 수 있다.
[ ] RViz2 Fixed Frame이 base_link다.
[ ] TF display에 세 frame이 모두 보인다.
[ ] PointCloud2 display status가 OK다.
[ ] cloud가 lidar_link의 장착 위치와 방향에 표시된다.
```

Topic, message, TF, RViz 설정을 이 순서로 분리하면 화면에 point가 없다는 하나의 관찰에서 원인을 단계적으로 좁힐 수 있다.

## 문제 확인 순서

| 관찰 | 확인할 내용 |
|---|---|
| Topic이 목록에 없다. | Publisher process, package sourcing과 topic 이름을 확인한다. |
| Topic은 있지만 RViz2가 message를 받지 못한다. | Endpoint QoS compatibility와 publisher가 계속 publish하는지 확인한다. |
| RViz2가 Fixed Frame이 존재하지 않는다고 표시한다. | Frame 이름과 `robot_state_publisher` 같은 TF broadcaster의 현재 실행 상태를 확인한다. |
| RViz2가 `No transform`을 표시한다. | Fixed Frame, `header.frame_id`와 TF tree 연결을 확인한다. |
| RViz2가 extrapolation error를 표시한다. | Message timestamp, dynamic TF timestamp와 clock source를 확인한다. |
| Point가 예상 위치에서 일정하게 어긋난다. | URDF origin의 translation, rotation과 parent/child 방향을 확인한다. |
| Point 모양이 깨지거나 값이 비정상적이다. | `fields`, `point_step`, `row_step`, endianness와 iterator type을 확인한다. |
| Frame 이름만 바꾸자 위치가 더 틀어진다. | 좌표값에 transform을 적용하지 않고 `frame_id`만 다시 붙였는지 확인한다. |

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Node and Topic](<./02 Node and Topic.md>)
- [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)

## References

- [sensor_msgs - PointCloud2 Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/sensor_msgs/msg/PointCloud2.msg)
- [sensor_msgs - PointCloud2 Iterator](https://github.com/ros2/common_interfaces/blob/jazzy/sensor_msgs/include/sensor_msgs/point_cloud2_iterator.hpp)
- [ROS 2 Documentation - Quality of Service Settings](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Intermediate/About-Quality-of-Service-Settings.rst)
- [RViz - ROS 3D Robot Visualizer](https://github.com/ros2/rviz)
- [ROS 2 Documentation - Introducing tf2](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Introduction-To-Tf2.rst)
