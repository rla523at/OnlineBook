# ROS 2 Rosbag2 Record, Inspect and Replay

## 한 줄 요약

`rosbag2`는 ROS 2 topic message를 storage에 시간 정보와 함께 기록하고, 나중에 별도의 player process가 같은 topic으로 다시 publish할 수 있게 하는 record·replay 도구다.

## 문서 범위와 선행 개념

이 문서는 live publisher가 보내는 sensor·TF message를 짧게 기록하고, bag의 topic과 message 수를 검사한 뒤, 원본 publisher 없이 replay하는 최소 흐름을 설명한다. 대규모 dataset 압축, bag 변환, 여러 bag 병합과 C++ storage plugin 개발은 다루지 않는다.

먼저 다음 개념을 알고 있으면 흐름을 이해하기 쉽다.

- Node, publisher, subscription과 topic은 [Node and Topic](<./02 Node and Topic.md>)에서 설명한다.
- DDS, discovery와 QoS offered/requested 규칙은 [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)에서 설명한다.
- Sensor message의 frame과 timestamp는 [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)에서 설명한다.
- `/tf`와 `/tf_static`, listener와 buffer는 [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)에서 설명한다.

## Rosbag과 rosbag2

`rosbag`은 timestamp가 있는 ROS message를 저장한 기록물을 뜻한다. `rosbag2`는 ROS 2에서 이 기록물을 생성·검사·재생하는 package와 library 집합이며, CLI에서는 `ros2 bag` command로 사용한다.

Rosbag2를 사용하는 목적은 단순한 file 백업에 그치지 않는다. Live sensor나 simulator가 없어도 기록된 입력을 반복 공급할 수 있으므로 다음 작업에 유용하다.

- 같은 sensor 입력으로 algorithm을 반복 실행한다.
- 장애가 발생한 시점의 topic stream을 보존한다.
- Publisher와 consumer 문제를 분리한다.
- CI나 smoke test에서 작은 고정 입력을 재사용한다.

Bag은 특정 algorithm의 결과를 자동으로 검증하지 않는다. 어떤 topic을 기록했는지, message가 충분히 들어 있는지와 replay consumer가 의도한 data를 받았는지는 별도로 검사해야 한다.

## Record와 replay의 실행 주체

Record 단계에서 recorder는 지정한 topic의 **subscriber**다. Publisher가 보낸 serialized message를 받아 storage에 쓴다. Replay 단계에서 player는 bag을 읽고 같은 topic에 message를 내보내는 **publisher**다.

```text
[record]

live publisher
    │ topic message publish
    ▼
rosbag2 recorder의 subscription
    │ serialized message와 기록 시각 저장
    ▼
bag directory


[replay]

bag directory
    │ 기록 시각 순서로 읽기
    ▼
rosbag2 player의 publisher
    │ topic message publish
    ▼
application subscriber
```

Recorder와 player가 sensor driver나 simulator의 계산을 대신하는 것은 아니다. Recorder는 수신한 message를 보존하고, player는 저장된 message를 다시 전달한다. Replay가 시작된 뒤 같은 topic에 live publisher도 남아 있으면 consumer는 두 source의 message를 함께 받을 수 있으므로, 재현 시험에서는 보통 원본 publisher를 먼저 종료한다.

## 기록할 topic 선택

모든 topic을 기록하려면 다음 command를 사용할 수 있다.

```bash
ros2 bag record --all
```

하지만 작은 재현 bag이나 interface test에서는 필요한 topic을 명시하는 편이 좋다.

```bash
ros2 bag record --topics \
  /robotics_sensor_smoke/points \
  /robotics_sensor_smoke/imu \
  /tf_static \
  --output sensor_smoke
```

명시적 선택에는 다음 장점이 있다.

- Bag의 topic 계약이 분명해진다.
- `/rosout`, `/parameter_events` 같은 보조 topic이 섞이지 않는다.
- File 크기와 기록 부하가 작아진다.
- Expected topic 집합을 자동으로 비교할 수 있다.

Record 시작 시 topic이 아직 없어도 기본 discovery가 실행 중이면 recorder는 나중에 나타난 topic을 찾을 수 있다. 다만 짧은 시험에서는 원본 publisher를 먼저 시작하고, recorder가 필요한 topic과 QoS를 발견해 모두 구독한 시점부터 목표 기록 시간을 재야 discovery 대기 시간을 실제 기록 시간으로 잘못 계산하지 않는다.

## Storage plugin과 bag directory

Rosbag2는 storage plugin 구조를 사용한다. ROS 2 Jazzy의 기본 storage plugin은 MCAP이며, 기본 제공 reader·writer에는 MCAP과 SQLite3가 있다. 환경이나 distribution의 기본값에 의존하지 않고 재현하려면 storage ID를 명시한다.

```bash
ros2 bag record --storage mcap \
  --topics /robotics_sensor_smoke/points \
  --output sensor_smoke
```

MCAP으로 한 번 기록하면 보통 다음 구조가 만들어진다.

```text
sensor_smoke/
├── metadata.yaml
└── sensor_smoke_0.mcap
```

- `metadata.yaml`은 storage ID, 기록 시작 시각과 기간, file 목록, 전체 message 수, topic별 이름·type·message 수와 기록 당시 publisher QoS를 설명한다.
- `.mcap` file은 serialized message와 index 같은 storage data를 담는다.

Bag은 directory 전체를 하나의 입력으로 취급하는 것이 일반적이다. `metadata.yaml`만 복사하거나 storage file만 이름을 바꾸면 CLI가 bag을 정상적으로 열지 못할 수 있다.

## `ros2 bag info`가 확인하는 것

기록이 끝난 뒤 다음 command로 metadata를 확인한다.

```bash
ros2 bag info sensor_smoke
```

대표적으로 다음 내용을 보여준다.

- Storage ID와 file 크기
- 기록 시작·종료 시각과 duration
- 전체 message 수
- Topic 이름과 message type
- Topic별 message 수

`ros2 bag info`가 성공했다고 모든 message payload가 올바르다는 뜻은 아니다. 예를 들어 PointCloud2의 `header.frame_id`, point field layout과 timestamp 순서는 metadata만으로 알 수 없다. 이런 조건은 bag을 replay하여 실제 message를 받거나 storage reader API로 payload를 읽어 별도로 검사한다.

## Bag 기록 시각과 message의 `header.stamp`

Sensor bag에는 서로 목적이 다른 두 시간 정보가 함께 들어간다. 두 값은 같은 시각을 중복 저장한 것이 아니며 생성 주체, 저장 위치와 사용 목적이 다르다.

| 구분 | Bag 기록 시각 | `header.stamp` |
|---|---|---|
| 생성·관리 주체 | rosbag2 recorder와 storage | Message publisher |
| 저장 위치 | Bag의 개별 message record | Serialized message payload 내부 |
| 의미 | Recorder가 message를 받아 기록한 시점 | Sensor data가 측정되거나 message가 유효한 기준 시점 |
| 주요 용도 | Player의 topic 간 재생 순서와 message 간격 복원 | TF 조회, sensor 동기화와 topic 내부 측정 순서 판단 |
| 대표 검사 | `ros2 bag info`의 start·end·duration | Replay subscriber가 message field를 직접 확인 |

모든 bag record에는 기록 시각이 있지만 모든 ROS 2 message type에 `header`가 있는 것은 아니다. `PointCloud2`와 `Imu`는 최상위에 `header`가 있고, `TFMessage`는 자체 `header` 없이 내부 `TransformStamped`가 `header`를 갖는다. Message field 합성은 [Node and Topic](<./02 Node and Topic.md>)에서 설명한다.

### 두 시간이 달라지는 예

LiDAR가 `10.000초`에 점군을 측정하고 처리와 DDS 전달에 12ms가 걸렸다고 하자.

```text
측정과 header.stamp 설정              recorder 수신과 Bag 기록
header.stamp = 10.000초  ──────────>  기록 시각 = 10.012초
```

Player는 `10.012초`에 해당하는 Bag 기록 시각을 replay schedule에 사용하지만, serialized payload 안의 `header.stamp = 10.000초`를 replay 현재 시각으로 바꾸지 않는다. 따라서 Bag start·end로 계산한 duration과 각 sensor topic의 첫·마지막 `header.stamp` 범위가 달라도 정상이다.

### 서로 다른 topic은 독립적으로 검사한다

Bag의 topic 간 replay 순서는 기록 시각을 기준으로 정해진다. 서로 다른 timer, 처리 시간이나 sensor clock을 사용하는 topic의 측정 순서와 recorder 도착 순서는 달라질 수 있다.

```text
PointCloud2: header.stamp=10.000초, 기록 시각=10.015초
IMU:         header.stamp=10.005초, 기록 시각=10.007초

Bag·replay 순서: IMU(10.005) → PointCloud2(10.000)
```

두 topic의 replay 수신 message를 하나의 sequence로 합치면 `header.stamp`가 감소해 보이지만 각 sensor stream이 손상된 것은 아니다. 단순 smoke test는 다음처럼 topic 내부의 진행을 각각 검사한다.

```text
PointCloud2: P1.header.stamp < P2.header.stamp < P3.header.stamp
IMU:         I1.header.stamp < I2.header.stamp < I3.header.stamp
```

이 검사는 두 sensor의 시간 동기화 정확도를 판단하지 않는다. Sensor 간 동기화를 검증하려면 sample을 짝짓는 기준과 허용 오차를 별도로 정의해야 한다.

### Timestamp 진행성 검사와 완전 일치 검사

Replay 검증의 강도를 구분해야 한다.

- **진행성 검사**는 replay된 각 topic의 첫 stamp보다 마지막 stamp가 크고 모든 stamp가 엄격히 증가하는지 확인한다.
- **완전 일치 검사**는 storage reader로 Bag payload의 원본 stamp sequence를 추출하고 replay subscriber가 받은 sequence와 값별로 비교한다.
- Metadata의 기록 message 수와 replay 수신 수 비교는 누락·중복을 검사하지만 stamp 값의 완전 일치를 증명하지는 않는다.

일반적인 smoke test는 message 수, frame과 topic별 stamp 진행을 확인할 수 있다. Replay가 원본 timestamp를 한 값도 바꾸지 않았음을 시험 자체로 증명해야 한다면 완전 일치 검사를 추가한다.

## Replay와 `/clock`

기본 `ros2 bag play`는 저장된 message를 기록 간격에 맞추어 publish하지만, ROS graph에 `/clock`을 자동으로 publish하지 않는다.

```bash
ros2 bag play sensor_smoke
```

Simulation time을 사용하는 node까지 bag의 재생 시간에 맞추려면 player의 `--clock` 계열 option과 consumer node의 `use_sim_time` 설정을 함께 구성해야 한다.

```bash
ros2 bag play sensor_smoke --clock 100
```

`--clock`은 replay용 ROS time source를 추가하는 기능이지 sensor message의 `header.stamp`를 새 값으로 덮어쓰는 기능이 아니다. Wall clock을 사용하는 단순 message 전달 smoke test에는 반드시 필요하지 않지만, TF 조회와 algorithm timer를 기록 시간에 동기화해야 하는 system replay에서는 중요하다.

## QoS와 `/tf_static`

Topic 이름과 type이 같아도 publisher가 offer한 QoS와 subscription이 request한 QoS가 필요한 방식으로 맞지 않으면 message를 기록하거나 retained sample을 받을 수 없다. 일반적인 호환 규칙은 [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)에서 설명한다.

Rosbag2의 record와 replay에는 다음 QoS 흐름이 있다.

```text
원본 publisher의 offered QoS
        │
        ├─ recorder가 subscription QoS 자동 선택
        │
        └─ metadata.yaml의 offered_qos_profiles에 원본 offer 저장
                                      │
                                      ▼
                         player가 publisher QoS 자동 선택
                                      │
                                      ▼
                         replay subscription과 matching
```

### Recorder의 subscription QoS

`--qos-profile-overrides-path`를 지정하지 않으면 rosbag2 Jazzy recorder는 topic을 발견할 때 publisher endpoint의 offered QoS를 조사해 subscription QoS를 자동으로 선택한다. 해당 topic의 모든 publisher가 reliable을 offer하면 reliable을 request하고, 모든 publisher가 transient-local을 offer하면 transient-local을 request해 retained sample을 받을 수 있게 한다.

같은 topic에 transient-local publisher와 volatile publisher가 섞여 있으면 recorder는 모든 publisher와 연결하기 위해 volatile로 완화한다. 이 경우 transient-local publisher가 연결 전에 보낸 retained sample은 가져오지 못할 수 있다. 짧은 record에서는 recorder를 시작하기 전에 필요한 publisher와 topic을 먼저 발견하고, subscription 완료를 확인한 뒤 기록 시간을 재는 편이 안전하다.

Bag metadata의 `offered_qos_profiles`는 원본 publisher들이 제공한 QoS다. Recorder가 실제로 요청한 subscription QoS 자체를 저장한 field로 해석하면 안 된다.

### Player의 publisher QoS

Player도 override가 없으면 metadata에 기록된 원본 offered QoS를 이용해 replay publisher의 QoS를 고른다. 기록된 publisher profile이 서로 같으면 해당 compatibility 정책을 재현하고, 서로 다른 profile이 섞여 있으면 rosbag2 default로 fallback하면서 경고할 수 있다. Replay consumer까지 원본 graph와 같은 동작을 하려면 player의 offer와 consumer subscription의 request를 함께 확인해야 한다.

### `/tf_static`과 retained transform

합성 sensor에서 흔한 관계는 다음과 같다.

| Topic 종류 | 일반적인 durability·reliability | Record·replay에서 주의할 점 |
|---|---|---|
| PointCloud2·IMU sensor data | volatile·best effort | Subscriber를 player보다 먼저 준비하면 시작 message 누락을 줄일 수 있다. |
| `/tf_static` | transient local·reliable | Recorder도 transient-local을 request해야 연결 전에 publish된 transform을 받을 수 있다. |
| `/tf` dynamic transform | volatile | Bag에 기록된 timestamp 범위와 sensor timestamp가 맞아야 replay consumer가 transform을 조회할 수 있다. |

Transient-local sample은 static broadcaster 쪽 middleware endpoint가 보관한다. Broadcaster가 recorder 연결 시점까지 실행 중이어야 late-joining recorder가 이전 sample을 받을 수 있으며, broadcaster가 먼저 종료된 뒤에도 DDS가 이를 영구 보관한다고 가정해서는 안 된다.

`/tf_static`의 한 `TFMessage`에는 여러 `TransformStamped`가 들어갈 수 있다. 따라서 TF 관계 두 개가 있다고 `/tf_static` topic message 수도 반드시 두 개인 것은 아니다. Message 수와 함께 각 `TransformStamped`의 `header.frame_id`와 `child_frame_id`를 확인해야 한다.

### QoS override가 필요한 경우

Publisher가 같은 topic에 서로 다른 QoS를 제공하거나 자동 적응 결과를 환경과 무관하게 고정해야 한다면 record와 play 양쪽에서 `--qos-profile-overrides-path`로 topic별 QoS를 명시할 수 있다. Override를 사용했다면 bag 재현 절차와 함께 해당 YAML도 보관한다.

## 안전한 짧은 record·inspect·replay 절차

다음 순서는 live publisher와 replay publisher가 섞이지 않도록 한다.

1. 사용할 `ROS_DOMAIN_ID`와 underlay·overlay를 활성화한다.
2. 원본 publisher를 시작하고 필요한 topic과 type을 확인한다.
3. 명시한 topic으로 recorder를 시작하고 모든 subscription이 준비됐는지 확인한 뒤 목표 시간만큼 record한다.
4. Recorder를 정상 종료해 storage와 `metadata.yaml`을 finalize한다.
5. `ros2 bag info`로 topic·type·message 수와 duration을 확인한다.
6. 원본 publisher를 종료하고 graph에서 해당 publisher가 사라졌는지 확인한다.
7. Replay subscriber를 player보다 먼저 시작한다.
8. 새 process에서 `ros2 bag play`를 실행한다.
9. 기록 수와 replay 수신 수, frame, timestamp와 TF 관계를 비교한다.
10. Player와 subscriber가 종료되고 잔여 process가 없는지 확인한다.

Recorder가 비정상 종료되면 metadata나 storage index가 finalize되지 않을 수 있다. 자동화에서는 recorder가 실제로 처리하는 정상 종료 신호를 보내고 cache flush와 `Recording stopped`, `metadata.yaml` 생성을 확인해야 한다. Background shell에서 시작한 process는 signal 상속 상태에 따라 `SIGINT`를 무시할 수 있으므로 실행 방식을 고정한 뒤 `SIGINT` 또는 `SIGTERM`의 실제 동작을 검증한다. 제한 시간 안에 정상 종료되지 않을 때만 `SIGKILL`을 fallback으로 사용한다.

## 합성 PointCloud2·IMU·TF 예시의 검증 기준

다음 세 topic을 기록했다고 하자.

| Topic | 기대 type | Payload 검사 |
|---|---|---|
| `/robotics_sensor_smoke/points` | `sensor_msgs/msg/PointCloud2` | 모든 frame이 `lidar_link`이고 timestamp가 증가한다. |
| `/robotics_sensor_smoke/imu` | `sensor_msgs/msg/Imu` | 모든 frame이 `imu_link`이고 timestamp가 증가한다. |
| `/tf_static` | `tf2_msgs/msg/TFMessage` | `base_link`가 parent이고 `imu_link`, `lidar_link`가 child인 관계가 들어 있다. |

검증은 metadata와 replay 관찰을 결합해야 한다.

```text
metadata 검사
├── topic 집합과 type
├── topic별 기록 message 수
└── storage와 duration

replay subscriber 검사
├── topic별 실제 수신 message 수
├── sensor frame과 header timestamp 진행
└── TFMessage 내부 parent-child 관계

최종 판정
└── 기록 계약과 replay 관찰이 모두 일치할 때 PASS
```

## 문제 확인 순서

| 관찰 | 확인할 내용 |
|---|---|
| Bag directory가 생성되지 않는다. | Output path의 기존 directory 충돌, 쓰기 권한과 storage plugin 설치를 확인한다. |
| Topic은 metadata에 있지만 count가 0이다. | Publisher 실행 상태, discovery 완료 여부와 recorder subscription QoS를 확인한다. |
| `/tf_static`이 기록되지 않는다. | Topic을 명시했는지, recorder subscription이 transient-local을 요청했는지와 static broadcaster가 연결 시점까지 실행 중인지 확인한다. |
| `ros2 bag info`가 metadata를 열지 못한다. | Recorder가 정상 종료되어 finalize됐는지와 directory 안의 `metadata.yaml`·storage file을 확인한다. |
| Replay subscriber가 첫 message 일부를 놓친다. | Subscriber를 먼저 시작하고 player 시작 전 discovery 시간을 확보한다. |
| Replay message 수가 기록 수보다 많다. | Live publisher가 종료되지 않았거나 bag player를 둘 이상 실행하지 않았는지 확인한다. |
| Sensor timestamp가 현재 시각이 아니다. | 정상일 수 있다. Replay는 기록된 payload의 `header.stamp`를 유지한다. |
| TF lookup에서 extrapolation error가 난다. | Sensor header timestamp, dynamic TF 기록 범위, `/clock`과 `use_sim_time` 구성을 확인한다. |
| 다른 ROS 2 distribution에서 bag을 열지 못한다. | Storage plugin 설치와 rosbag2 metadata format 호환성을 확인하고 필요하면 같은 distribution에서 변환한다. |

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Node and Topic](<./02 Node and Topic.md>)
- [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)
- [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)
- [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)
- [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)

## References

- [rosbag2 - Jazzy README](https://github.com/ros2/rosbag2/blob/jazzy/README.md)
- [ROS 2 Jazzy Documentation - Recording and Playing Back Data](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.rst)
- [rosbag2_storage_mcap - Jazzy README](https://github.com/ros2/rosbag2/blob/jazzy/rosbag2_storage_mcap/README.md)
- [rosbag2_transport - Jazzy Recorder Source](https://github.com/ros2/rosbag2/blob/jazzy/rosbag2_transport/src/rosbag2_transport/recorder.cpp)
- [rosbag2_transport - Jazzy Player Source](https://github.com/ros2/rosbag2/blob/jazzy/rosbag2_transport/src/rosbag2_transport/player.cpp)
- [rosbag2_storage - Jazzy QoS Source](https://github.com/ros2/rosbag2/blob/jazzy/rosbag2_storage/src/rosbag2_storage/qos.cpp)
- [ROS 2 Jazzy Documentation - Quality of Service Settings](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Intermediate/About-Quality-of-Service-Settings.rst)
