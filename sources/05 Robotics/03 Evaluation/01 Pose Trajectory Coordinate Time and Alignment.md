# Pose Trajectory Coordinate, Time and Alignment

## 한 줄 요약

두 pose trajectory를 올바르게 평가하려면 pose의 transform 방향, coordinate axis,
unit, quaternion 순서와 timestamp 기준을 먼저 같게 만들고, association과 alignment가
어떤 오차를 보존하거나 제거하는지 명시해야 한다.

## Trajectory는 pose의 시간 순서다

`pose`는 기준 coordinate frame에서 본 물체의 위치와 방향이다. `trajectory`는
서로 다른 시각의 pose를 시간 순서로 모은 sequence다.

```text
trajectory = {(t0, pose0), (t1, pose1), ..., (tn, posen)}
```

Trajectory의 숫자만으로는 pose 의미가 완전하지 않다. 예를 들어 같은
`(1, 0, 0)` translation도 world에서 본 robot 위치인지 robot에서 본 world 위치인지에
따라 반대 transform을 나타낼 수 있다. Timestamp도 Unix epoch, simulation time,
sensor device time 또는 sequence-relative time일 수 있다.

따라서 file format은 column 개수뿐 아니라 frame, unit, time epoch와 transform
direction을 함께 정의해야 한다.

## Transform 방향을 식으로 고정한다

두 frame `A`와 `B` 사이의 rigid transform을 다음처럼 표기할 수 있다.

```text
T_A_B: B frame의 coordinate를 A frame의 coordinate로 변환
p_A = T_A_B * p_B
```

`T_A_B`의 translation은 A에서 표현한 B 원점의 위치이고, rotation은 B의 vector를
A로 회전한다. 이 규칙에서 inverse는 `T_B_A`다.

```text
T_B_A = inverse(T_A_B)
```

“A에서 본 B의 pose”라는 자연어는 library나 문맥에 따라 다르게 읽힐 수 있다.
따라서 interface와 trajectory metadata에는 자연어와 함께 위 좌표 변환 식을
기록하는 편이 안전하다.

Mobile robot의 흔한 canonical pose는 `T_map_base_link`다.

```text
p_map = T_map_base_link * p_base_link
```

이 pose는 `base_link` 원점의 위치와 orientation을 `map`에서 표현한다. Dataset의
camera, IMU 또는 LiDAR pose를 비교하려면 static extrinsic을 합성해 두 trajectory가
같은 body frame을 나타내게 해야 한다.

## Coordinate axis와 unit

ROS REP-103은 Cartesian frame에 right-handed coordinate system과 SI unit을
권장한다. Robot body frame의 기본 축은 다음과 같다.

- `x`: forward
- `y`: left
- `z`: up

Georeferenced short-range world frame은 ENU를 사용한다.

- `x`: east
- `y`: north
- `z`: up

Camera optical frame은 일반 body frame과 달리 `z` forward, `x` right, `y` down을
사용하고 보통 `_optical` suffix로 구분한다. Frame 이름만 바꾸는 것은 coordinate
변환이 아니다. Axis가 다른 source data는 rotation transform을 실제 vector와 pose에
적용해야 한다.

자주 쓰는 SI unit은 다음과 같다.

| Quantity | Unit |
|---|---|
| Position·distance | meter (`m`) |
| Time·duration | second (`s`) |
| Angle | radian (`rad`) |
| Linear velocity | meter per second (`m/s`) |
| Angular velocity | radian per second (`rad/s`) |
| Linear acceleration | meter per second squared (`m/s^2`) |

Degree, millimeter나 device tick을 source에서 사용한다면 canonical trajectory로
변환한 scale과 원래 unit을 metadata에 남긴다.

## Quaternion의 순서와 의미

Quaternion은 singularity 없이 3D rotation을 표현할 수 있지만 component 순서는
API마다 다를 수 있다. ROS `geometry_msgs/Quaternion`과 TUM trajectory text는
vector part 뒤에 scalar part를 두는 순서를 사용한다.

```text
qx qy qz qw
identity = 0 0 0 1
```

다른 math library가 `w x y z` 순서를 사용한다면 memory를 그대로 복사하지 말고
field 이름으로 변환해야 한다. Quaternion은 unit norm을 만족해야 한다.

```text
qx^2 + qy^2 + qz^2 + qw^2 = 1
```

`q`와 `-q`는 같은 rotation을 나타낸다. 따라서 두 quaternion의 component를 단순히
빼면 같은 orientation도 큰 오차처럼 보일 수 있다. Orientation error는 상대
rotation의 shortest angle처럼 부호 동등성을 고려해 계산한다.

연속 trajectory를 저장할 때 인접 quaternion의 dot product가 음수면 현재
quaternion 부호를 뒤집을 수 있다. 이는 rotation을 바꾸지 않으면서 plot과
interpolation에서 불필요한 component jump를 줄인다.

## Rotation convention을 분리해 적는다

Quaternion 순서만 같아도 rotation 의미가 같다고 단정할 수 없다. 다음 정보도
필요하다.

- Quaternion이 어느 source frame에서 어느 target frame으로 회전하는가?
- Vector를 실제로 회전하는 active rotation인지 coordinate basis를 바꾸는 passive
  rotation인지?
- Matrix가 column vector에 왼쪽에서 곱해지는지?
- Euler angle을 거친다면 axis와 적용 순서는 무엇인지?

Coordinate 식 `p_A = T_A_B p_B`를 기준으로 quaternion이 B vector를 A로 회전한다고
정하면 active·passive 용어만으로 생기는 모호성을 줄일 수 있다.

## Data time과 경과 시간

Robot software에서는 여러 clock이 서로 다른 책임을 가진다.

| 시간 | 의미 | 적합한 사용 |
|---|---|---|
| Measurement timestamp | Measurement나 pose가 유효한 data time | Sensor association, TF lookup, trajectory |
| ROS time | System time 또는 `/clock`이 선택한 node time | Live·simulation·replay data flow |
| Steady time | 뒤로 가지 않는 process-local clock | Timeout, 실행 duration, stall 검사 |
| Storage/record time | Recorder가 message를 저장한 시각 | Bag ordering·storage 진단 |
| UTC wall time | Calendar 기준 실행 시각 | Log와 run 식별 |

ROS time은 simulation pause나 bag replay에서 느려지거나 빨라질 수 있고 뒤로 jump할
수 있다. 반면 timeout을 measurement timestamp로 계산하면 simulation pause 중
영원히 끝나지 않을 수 있다. Data association에는 measurement time을, process
deadline에는 steady time을 사용하는 식으로 책임을 분리한다.

## Timestamp epoch와 정밀도

`timestamp`는 숫자와 unit만으로 충분하지 않다. 다음 epoch 예시는 서로 직접
비교할 수 없다.

- Unix epoch 이후 second
- ROS simulation 시작 이후 second
- Sensor boot 이후 device tick
- Sequence 공통 origin 이후 second

Reference와 estimate를 association하려면 같은 clock domain과 epoch로 변환해야 한다.
각 trajectory의 첫 sample을 따로 0으로 만들면 실제 start delay나 time offset을
숨길 수 있다. Sequence별 공통 origin을 사용한다면 원본 integer timestamp와 공통
origin을 metadata에 보존한다.

큰 absolute second를 binary floating-point로 저장하면 sub-second 정밀도가 줄 수
있다. 원본 nanosecond integer를 보존하고 text export에서 필요한 precision으로
second를 계산하면 변환을 다시 검증할 수 있다.

## TUM-compatible trajectory text

TUM RGB-D trajectory 형식은 pose 하나를 다음 8개 column으로 저장한다.

```text
timestamp tx ty tz qx qy qz qw
```

- `timestamp`: second 단위 time
- `tx ty tz`: fixed reference frame에서 표현한 body origin 위치
- `qx qy qz qw`: unit quaternion orientation
- `#`으로 시작하는 line: comment

원래 TUM RGB-D ground truth는 Unix epoch second와 camera optical center pose를
사용한다. 같은 column layout을 다른 robot·epoch에 재사용할 수 있지만, 이 경우
“TUM dataset semantics”가 아니라 “TUM-compatible layout”이라고 구분하고 frame과
epoch를 sidecar metadata에 기록한다.

Text file에 frame id, transform direction, original timestamp와 conversion provenance를
모두 넣을 수 없으므로 manifest나 evaluation config가 이를 보완해야 한다.

## Timestamp association

Reference와 estimate는 보통 같은 시각에 pose를 출력하지 않는다. `association`은
timestamp가 가까운 pose를 metric pair로 만드는 전처리다.

```text
reference times:  0.00  0.10  0.20  0.30
estimate times:   0.01  0.11        0.29
matched pairs:   (0.00,0.01) (0.10,0.11) (0.30,0.29)
```

Association 정책에는 최소한 다음 값이 필요하다.

- 최대 timestamp 차이
- Estimate에 더할 fixed time offset
- Nearest 또는 interpolation 방식
- 한 pose를 여러 pair에 재사용하는지
- 평가할 공통 시작·종료 구간

Tolerance가 너무 크면 다른 motion state를 짝지어 spatial error를 time-sync error와
섞는다. 자동 offset 추정도 실제 synchronization 문제를 숨길 수 있으므로 사용
여부와 추정 근거를 결과에 기록한다.

## Alignment가 제거하는 차이

두 trajectory가 같은 motion을 나타내도 arbitrary world origin이나 initial heading이
다를 수 있다. `alignment`는 associated pose로 estimate에 적용할 하나의 transform을
fit하는 평가 전처리다.

| Alignment | Fit하는 자유도 | 제거되는 차이 |
|---|---:|---|
| None | 0 | 아무것도 제거하지 않는다. |
| Origin alignment | Initial pose 기준 | 시작 위치·방향 차이 |
| SE(3) | Rotation 3 + translation 3 | Global rigid frame 차이 |
| Sim(3) | SE(3) + scale 1 | Global frame과 scale 차이 |

Umeyama alignment는 corresponding point set의 least-squares error를 줄이는
rotation·translation과 선택적으로 scale을 구하는 방법이다. Metric-scale LiDAR,
stereo 또는 LiDAR-inertial estimator에 Sim(3)을 적용하면 scale error를 숨길 수
있다. Monocular SLAM처럼 scale이 원래 관측되지 않는 경우에만 scale correction을
평가 protocol에 명시적으로 포함한다.

Alignment를 trajectory 전체에서 한 번 fit하는 것과 짧은 segment마다 다시 fit하는
것은 다른 metric이다. Segment별 alignment는 accumulated drift 일부를 제거할 수
있으므로 fit 구간과 pair 집합을 결과에 남긴다.

Aligned 결과만 보고하면 frame convention 오류가 가려질 수 있다. Primary protocol이
SE(3) alignment를 사용하더라도 unaligned result나 overlay를 diagnostic으로 함께
보존하면 초기 pose·extrinsic 문제를 찾는 데 도움이 된다.

## ATE와 RPE

`absolute pose error`(APE, trajectory 문맥에서 ATE로도 부름)는 associated reference
pose와 estimate pose를 직접 비교해 global consistency를 본다. Global frame 차이가
평가 대상이 아니라면 ATE 전에 한 번의 SE(3) alignment를 사용할 수 있다.

`relative pose error`(RPE)는 두 시각 사이의 reference motion과 estimate motion을
비교해 local accuracy와 drift를 본다. Relative pair를 frame, second, meter 또는
radian 간격으로 선택할 수 있다. 각 pair마다 estimate를 GT에 다시 align하면
비교해야 할 relative error를 제거할 수 있으므로 alignment 범위를 별도로 정한다.

ATE와 RPE는 서로 대체하지 않는다. ATE가 작아도 짧은 구간 drift가 클 수 있고,
local RPE가 작아도 장시간 누적으로 global trajectory가 벗어날 수 있다.

## Ground truth는 평가 경계 안에 둔다

`ground truth`(GT)는 실제 상태에 대한 reference measurement 또는 reference
trajectory다. Evaluator가 GT를 읽는 것은 필요하지만 estimator가 GT로 자신의
state나 parameter를 정하면 독립적인 평가가 아니다.

다음 책임을 분리한다.

```text
sensor·control input ──> estimator ──> estimated trajectory
dataset GT ──> coordinate/time adapter ──> reference trajectory
estimated trajectory + reference trajectory ──> evaluator
```

Estimator는 GT topic·file을 입력으로 받지 않아야 한다. GT에서 time offset,
extrinsic, scale, initial state, bias나 estimator parameter를 식별하면 결과가
reference에 맞춰진다. Source GT를 canonical frame과 time으로 변환하는 adapter와,
run 종료 뒤 association·alignment·metric을 계산하는 evaluator는 평가 경계 안에서
GT를 사용할 수 있다.

Alignment된 trajectory는 metric 계산용 파생 결과다. 이를 estimator output으로
대체하거나 다음 estimator run에 feedback하지 않는다.

## 평가 계약 점검표

- Pose transform을 `p_target = T_target_source p_source` 식으로 정의했는가?
- Reference와 estimate가 같은 body frame·world frame을 나타내는가?
- Axis, unit, quaternion order와 normalization을 검사했는가?
- Clock domain, timestamp unit·epoch와 공통 origin을 기록했는가?
- Association tolerance, offset, interpolation과 평가 구간이 고정됐는가?
- Alignment 종류, scale correction과 fit pair 범위를 기록했는가?
- Aligned metric이 숨길 수 있는 unaligned 차이를 별도로 관찰했는가?
- GT가 estimator·parameter identification 경로와 분리됐는가?

## References

- [REP-103: Standard Units of Measure and Coordinate Conventions](https://github.com/ros-infrastructure/rep/blob/master/rep-0103.rst)
- [REP-105: Coordinate Frames for Mobile Platforms](https://github.com/ros-infrastructure/rep/blob/master/rep-0105.rst)
- [ROS 2 Clock and Time](https://design.ros2.org/articles/clock_and_time.html)
- [geometry_msgs/Quaternion](https://github.com/ros2/common_interfaces/blob/jazzy/geometry_msgs/msg/Quaternion.msg)
- [TUM RGB-D Dataset File Formats](https://cvg.cit.tum.de/data/datasets/rgbd-dataset/file_formats)
- [TUM RGB-D Benchmark Tools](https://cvg.cit.tum.de/data/datasets/rgbd-dataset/tools)
- [evo Formats](https://github.com/MichaelGrupp/evo/wiki/Formats)
- [evo Metrics](https://github.com/MichaelGrupp/evo/wiki/Metrics)
