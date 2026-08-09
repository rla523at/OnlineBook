# ROS 2 Coordinate Frames and TF2

## 한 줄 요약

Coordinate frame은 수치 좌표의 기준이고 transform은 같은 기하학적 대상을 source frame에서 target frame으로 다시 표현하는 관계이며, tf2는 실행 중인 transform을 publish·저장·조회한다.

## 문서 범위와 학습 순서

이 문서는 coordinate frame과 transform의 수학적 의미를 먼저 정의한 뒤, 여러 frame을 연결하는 TF tree와 tf2의 broadcaster·listener·buffer를 설명한다. 고정 transform을 command로 확인하는 최소 예제까지 다루며, transform 값을 만드는 구체적인 robot component는 뒤 문서로 분리한다.

```text
source frame의 coordinate
        │ source-to-target transform 적용
        ▼
target frame의 coordinate
```

학습 순서는 다음과 같다.

1. Coordinate frame이 필요한 이유를 확인한다.
2. Rotation과 translation으로 coordinate를 변환하는 방법을 이해한다.
3. 여러 frame을 연결하는 TF tree를 이해하고 parent-child 관계와 좌표 변환 방향을 구분한다.
4. tf2가 transform sample을 배포하고 listener별 buffer에서 조회하는 과정을 이해한다.
5. Static transform 하나를 실행하고 tree를 검증한다.

URDF와 `robot_state_publisher`는 [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)에서, sensor 측정·joint state·odometry·localization으로 시간별 transform을 만드는 과정은 [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)에서 이어서 설명한다.

## Coordinate frame이 필요한 이유

`coordinate frame`은 위치와 방향을 수치로 표현하기 위한 기준이다. 3D coordinate frame $F$는 origin $O_F$와 서로 직교하고 길이가 $1$이며 순서가 정해진 세 basis vector $\mathcal{B}_F=(\mathbf{e}^{F}_{x},\mathbf{e}^{F}_{y},\mathbf{e}^{F}_{z})$로 구성된다.

$$
F=(O_F,\mathcal{B}_F)
$$

Geometric point $P$ 자체와 그 point를 나타내는 coordinate column vector는 구분해야 한다. 이 문서에서는 frame $F$에서 표현한 $P$의 좌표를 다음과 같이 표기한다.

$$
{}^{F}\mathbf{p}
:=
\left[
\overrightarrow{O_FP}
\right]_{\mathcal{B}_F}
\in \mathbb{R}^{3}
$$

왼쪽 위의 $F$는 거듭제곱이 아니라 좌표를 표현한 frame을 나타낸다. Frame이 달라지면 같은 물리적 point $P$도 다른 origin과 basis를 기준으로 하므로 다른 숫자 좌표를 갖는다. Origin과 basis로 point의 좌표를 정의하는 일반적인 내용은 [Affine space](<../../01 math/08 Geometry/11 Affine space.md>)에서 설명한다.

`base_link`는 mobile robot body에 고정된 기준 frame이다. Robot model에서는 같은 이름의 body 기준과 coordinate frame을 함께 사용할 수 있으며, 원점의 정확한 위치는 model을 설계할 때 정한다.

예를 들어 lidar가 자신의 앞쪽 1 m 지점에서 점을 측정했다고 하자. 이 점의 lidar 좌표는 `(1, 0, 0)`일 수 있다. 그러나 lidar가 robot 중심에서 앞쪽 0.2 m, 위쪽 0.3 m에 장착되어 있다면 같은 점의 `base_link` 좌표는 장착 위치와 방향을 반영해야 한다.

```text
base_link frame                         lidar_link frame
robot 기준 원점과 축                    lidar 측정 기준 원점과 축
       │                                      │
       └──── mounting transform ──────────────┘
```

Point에 숫자 세 개만 기록하고 어느 frame의 좌표인지 기록하지 않으면 다른 component가 그 값을 같은 공간에 올바르게 배치할 수 없다.

## Frame과 transform

### Coordinate 표현과 transform

공간에 있는 point $P$ 자체와 그 point를 나타내는 coordinate는 구분해야 한다. Frame $F$에서 표현한 $P$의 coordinate를 다음과 같이 표기한다.

$$
{}^{F}\mathbf{p}
:=
\left[
\overrightarrow{O_FP}
\right]_{\mathcal{B}_F}
$$

기준 frame이 달라지면 같은 point $P$를 나타내는 coordinate도 달라진다. `transform`은 point를 물리적으로 움직이지 않고 한 frame에서 표현된 coordinate를 다른 frame에서 표현된 coordinate로 다시 표현한다.

이 문서에서는 frame `B` coordinate를 frame `A` coordinate로 바꾸는 transform을 다음과 같이 표기한다.

$$
{}^{A}\mathbf{T}_{B}
:
{}^{B}\mathbf{p}
\longmapsto
{}^{A}\mathbf{p}
$$

Superscript `A`는 coordinate를 새로 표현할 target frame이고 subscript `B`는 기존 coordinate의 source frame이다. 따라서 ${}^{A}\mathbf{T}_{B}$는 “`B` coordinate를 `A` coordinate로 변환한다”고 읽는다. 이 문서에서 transform은 같은 기하학적 대상을 다른 frame의 coordinate로 다시 표현하는 passive coordinate transformation이다. 고정된 frame에서 point나 vector 자체를 움직이는 active transformation과 구분해야 한다.

### Rotation과 translation

서로 다른 두 Cartesian frame 사이의 coordinate transform은 rotation과 translation으로 구성된다. 이 transform의 rotation과 translation을 각각 ${}^{A}\mathbf{R}_{B}$와 ${}^{A}\mathbf{t}_{B}$로 표기한다.

| 표기 | 의미 |
|---|---|
| ${}^{A}\mathbf{R}_{B} \in \mathbb{R}^{3\times3}$ | Frame `B`의 basis vector를 frame `A`의 basis로 표현한 rotation matrix |
| ${}^{A}\mathbf{t}_{B} \in \mathbb{R}^{3}$ | $\overrightarrow{O_AO_B}$를 frame `A`의 basis로 표현한 translation vector |

Rotation matrix의 각 column은 frame `B`의 basis vector 하나를 frame `A`의 basis로 표현한 좌표다.

$$
{}^{A}\mathbf{R}_{B}
=
\begin{bmatrix}
\left[\mathbf{e}^{B}_{x}\right]_{\mathcal{B}_A} &
\left[\mathbf{e}^{B}_{y}\right]_{\mathcal{B}_A} &
\left[\mathbf{e}^{B}_{z}\right]_{\mathcal{B}_A}
\end{bmatrix}
$$

따라서 ${}^{A}\mathbf{R}_{B}$는 같은 displacement vector의 coordinate를 frame `B`의 basis 기준에서 frame `A`의 basis 기준으로 바꾸는 [change of coordinate matrix](<../../01 math/07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)다. 두 frame의 basis는 orthonormal이므로 ${}^{A}\mathbf{R}_{B}$는 scaling이나 shear를 포함하지 않는 rotation matrix다.

$$
{}^{A}\mathbf{R}_{B}\,{}^{B}\mathbf{p}
=
\left[
\overrightarrow{O_BP}
\right]_{\mathcal{B}_A}
$$

두 frame은 basis뿐 아니라 origin도 다를 수 있다. 같은 point $P$에 대해 다음 geometric vector 관계가 성립한다.

$$
\overrightarrow{O_AP}
=
\overrightarrow{O_AO_B}
+
\overrightarrow{O_BP}
$$

각 vector를 frame `A`의 basis로 표현하면 frame `B` 좌표를 frame `A` 좌표로 바꾸는 식을 얻는다.

$$
{}^{A}\mathbf{p}
=
{}^{A}\mathbf{R}_{B}
{}^{B}\mathbf{p}
+
{}^{A}\mathbf{t}_{B}
$$

Origin 변경이 포함된 전체 관계는 $\mathbb{R}^{3}$에서의 linear transformation이 아니라 [affine transformation](<../../01 math/08 Geometry/13 Affine Transformation.md>)이다.

### Coordinate transform과 frame pose

Frame의 `pose`는 기준 frame에서 표현한 origin의 position과 basis의 orientation을 묶은 개념이다. ${}^{A}\mathbf{t}_{B}$는 frame `A`에서 본 frame `B` origin의 position이고, ${}^{A}\mathbf{R}_{B}$는 frame `A`에서 본 frame `B` basis의 orientation이다. 따라서 두 값을 함께 담은 ${}^{A}\mathbf{T}_{B}$는 frame `A`를 기준으로 한 frame `B`의 relative pose도 나타낸다.

| 관점 | ${}^{A}\mathbf{T}_{B}$의 의미 |
|---|---|
| Coordinate transform | Frame `B`에서 표현된 coordinate를 frame `A`에서 표현된 coordinate로 바꾼다. |
| Relative pose | Frame `B`의 origin과 basis가 frame `A`에서 어떻게 배치되어 있는지 나타낸다. |

두 frame의 origin과 basis 관계가 정해지면 coordinate transform이 유일하게 정해지고, coordinate transform으로부터 두 frame의 관계도 알 수 있다. 두 관점은 같은 rotation과 translation을 해석하는 서로 동등한 방법이지, 별개의 transform을 뜻하지 않는다.

### Homogeneous coordinate

Affine coordinate transformation을 행렬곱 하나로 표현하려면 point 좌표에 마지막 성분 `1`을 추가한 homogeneous coordinate를 사용한다.

$$
{}^{F}\overline{\mathbf{p}}
:=
\begin{bmatrix}
{}^{F}\mathbf{p} \\
1
\end{bmatrix}
\in \mathbb{R}^{4}
$$

$$
{}^{A}\overline{\mathbf{p}}
=
\underbrace{
\begin{bmatrix}
{}^{A}\mathbf{R}_{B} & {}^{A}\mathbf{t}_{B} \\
\mathbf{0}^{\mathsf T} & 1
\end{bmatrix}
}_{ {}^{A}\mathbf{T}_{B} }
{}^{B}\overline{\mathbf{p}}
$$

로보틱스 문헌에서는 homogeneous coordinate 표시를 생략하고 ${}^{A}\mathbf{p}={}^{A}\mathbf{T}_{B}{}^{B}\mathbf{p}$로 줄여 쓰기도 한다. 이 문서에서는 $\mathbb{R}^{3}$ point 좌표에는 ${}^{A}\mathbf{R}_{B}{}^{B}\mathbf{p}+{}^{A}\mathbf{t}_{B}$를 사용하고, $4\times4$ matrix 곱에는 $\overline{\mathbf{p}}$를 사용한다.

### Inverse와 transform 합성

반대로 frame `A` 좌표를 frame `B` 좌표로 바꿀 때는 inverse transform을 사용한다. Rotation matrix는 orthogonal matrix이므로 inverse가 transpose와 같다.

$$
{}^{B}\mathbf{T}_{A}
=
\left({}^{A}\mathbf{T}_{B}\right)^{-1}
=
\begin{bmatrix}
\left({}^{A}\mathbf{R}_{B}\right)^{\mathsf T}
&
-\left({}^{A}\mathbf{R}_{B}\right)^{\mathsf T}{}^{A}\mathbf{t}_{B}
\\
\mathbf{0}^{\mathsf T} & 1
\end{bmatrix}
$$

$$
{}^{B}\mathbf{p}
=
({}^{A}\mathbf{R}_{B})^{\mathsf T}
\left(
{}^{A}\mathbf{p}
-
{}^{A}\mathbf{t}_{B}
\right)
$$

따라서 translation을 먼저 빼고 basis를 바꾸는 계산은 ${}^{A}\mathbf{T}_{B}$가 나타내는 좌표 변환과 반대 방향에서 나타난다.

Frame `C` 좌표를 frame `B`를 거쳐 frame `A` 좌표로 바꾸면 transform은 다음 순서로 합성된다.

$$
{}^{A}\mathbf{T}_{C}
=
{}^{A}\mathbf{T}_{B}
{}^{B}\mathbf{T}_{C}
$$

행렬은 오른쪽의 ${}^{B}\mathbf{T}_{C}$부터 적용된다. 가운데 frame `B`가 이어지도록 superscript와 subscript를 맞추면 합성 방향을 확인하기 쉽다.

## ROS coordinate convention

ROS의 표준 단위와 좌표 convention은 REP-103에 정의되어 있다. 다른 convention을 사용해야 하는 sensor가 있으면 해당 차이를 별도 frame과 transform으로 명시한다.

- 길이는 meter, 각도는 radian을 사용한다.
- Coordinate frame은 right-handed coordinate system을 따른다.
- Robot body frame은 `x` forward, `y` left, `z` up을 사용한다.
- Camera optical frame처럼 다른 축 convention이 필요한 frame은 `_optical` suffix를 사용하며 `z` forward, `x` right, `y` down을 사용한다.

`imu_link`나 `lidar_link`라는 이름만으로 실제 sensor 축이 자동 결정되지는 않는다. Driver가 발행하는 message의 frame convention과 실제 장착 방향을 확인한 뒤 transform의 rotation에 반영해야 한다.

Rotation을 roll, pitch, yaw로 입력할 때는 각각 x, y, z fixed axis에 대한 회전이며 값은 radian이다. Quaternion을 직접 입력할 때는 zero rotation인 `(x, y, z, w) = (0, 0, 0, 1)`처럼 normalized quaternion을 사용한다.

## TF tree

지금까지는 두 frame 사이의 transform을 다뤘다. 실제 robot에는 body, sensor와 joint처럼 여러 coordinate frame이 있으므로 이들의 관계를 함께 구성해야 한다.

`TF tree`는 coordinate frame을 vertex로, 직접 연결된 parent-child frame 사이의 transform을 edge로 표현한 논리적 구조다. 여기서 vertex는 graph 이론의 구성 요소이며 ROS node를 뜻하지 않는다. TF tree diagram은 이 논리적 구조를 시각화한 표현이지 tree 자체가 아니다.

하나의 연결된 robot model을 만들 때는 다음 조건을 지킨다.

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

`base_link`는 이 최소 sensor rig subtree의 root다. `imu_link`와 `lidar_link`는 robot body에 고정되어 있으므로 두 관계는 static transform으로 표현할 수 있다. 움직이는 robot의 전체 TF tree에서는 `base_link`가 `odom` 같은 외부 frame의 child가 될 수 있다.

TF graph 전체에 연결되지 않은 frame이 존재할 수는 있지만, 연결되지 않은 두 frame 사이의 transform은 계산할 수 없다. RViz2의 Fixed Frame과 sensor message의 frame이 서로 연결되지 않으면 해당 sensor data를 표시할 수 없다.

### Parent-child 관계와 좌표 변환 방향

TF tree에서 `A`가 parent이고 `B`가 child인 관계를 `A → B`로 나타내면, 이 edge에는 ${}^{A}\mathbf{T}_{B}$가 저장된다.

| TF tree 표기 | 저장되는 transform | Coordinate 적용 방향 |
|---|---|---|
| `A` parent → `B` child | ${}^{A}\mathbf{T}_{B}$ | ${}^{B}\mathbf{p}\mapsto{}^{A}\mathbf{p}$ |

`A → B`는 parent-child 연결 방향을 나타내며 data coordinate의 변환 방향을 나타내지 않는다. 예를 들어 `base_link → lidar_link` parent-child 연결에는 ${}^{\mathrm{base\_link}}\mathbf{T}_{\mathrm{lidar\_link}}$가 저장된다. 이 transform은 `base_link`를 기준으로 한 `lidar_link`의 장착 pose를 나타내며, coordinate에 적용하면 lidar point를 `lidar_link` 좌표에서 `base_link` 좌표로 다시 표현한다. 반대 방향으로 변환하려면 inverse transform이 필요하다.

## tf2의 역할

`tf2`는 ROS 2에서 coordinate transform을 배포하고 조회하는 library 집합이다. Broadcaster는 parent frame과 child frame 사이에서 특정 timestamp의 transform 값인 transform sample을 publish하고, listener는 이를 수신해 자신의 buffer에 보관한다. Transform이 필요한 process는 이 buffer에 source frame, target frame과 query time을 지정해 조회한다. Listener와 buffer는 모든 consumer가 함께 사용하는 중앙 process를 뜻하지 않는다. RViz2, `tf2_echo` 또는 tf2를 사용하는 custom node처럼 transform이 필요한 각 consumer process가 내부에 listener와 buffer를 두고 자신의 buffer를 조회한다.

Parent frame을 `A`, child frame을 `B`라고 하면 시각 $t_i$의 transform sample은 ${}^{A}\mathbf{T}_{B}(t_i)$다. `tf2_ros` broadcaster API에서는 sample 하나를 `geometry_msgs/msg/TransformStamped`로 표현하며, `header.frame_id = A`, `child_frame_id = B`로 parent-child edge를 나타낸다. Dynamic transform ${}^{A}\mathbf{T}_{B}(t)$는 timestamp가 서로 다른 여러 `TransformStamped` sample로 전달되므로, `TransformStamped` 하나가 시간에 따른 transform 전체를 뜻하지는 않는다.

`/tf`와 `/tf_static`에서 사용하는 message type은 `tf2_msgs/msg/TFMessage`이며, `transforms` field에 하나 이상의 `TransformStamped` sample을 담는다. 따라서 broadcaster는 두 frame 자체가 아니라 두 frame 사이의 timestamped translation과 rotation을 publish한다.

Broadcaster가 publish한 transform sample을 여러 consumer가 topic을 통해 수신해 각자의 local buffer에 저장하고 조회하는 흐름은 다음과 같다.

```text
Transform 배포

dynamic transform broadcaster(s)
  └─ publish → /tf (`TFMessage`)

static transform broadcaster(s)
  └─ publish → /tf_static (`TFMessage`)

/tf와 /tf_static
  │ topic을 통해 전달
  ├─▶ consumer 1의 listener
  │       └─▶ local buffer
  ├─▶ consumer 2의 listener
  │       └─▶ local buffer
  └─▶ ...


각 consumer process 내부의 조회

application logic
  │ lookupTransform(
  │   target frame A,
  │   source frame B,
  │   time)
  ▼
같은 process의 local buffer
  │ A ← B transform 반환
  ▼
data 변환 또는 다른 계산에 사용
```

- `broadcaster`는 자신이 책임지는 parent-child frame 사이의 transform sample을 ROS graph에 publish한다.
- `listener`는 `TFMessage`에 담긴 `TransformStamped` sample을 수신해 같은 consumer process의 buffer에 전달한다. Listener 자체가 반드시 별도 node나 process인 것은 아니다.
- `buffer`는 consumer process마다 별도로 존재하며, 시간별 transform을 보관하고 source frame에서 target frame으로 좌표를 바꾸는 transform을 계산한다.
- `RViz2`는 별도 listener process 뒤에 연결되는 것이 아니라 내부에 listener와 buffer를 가진 tf2 consumer다. Message의 frame을 source, 표시 기준 frame을 target으로 사용해 transform을 조회한 뒤 변환된 좌표로 data를 표시한다.

RViz2에서는 선택된 transformation backend가 listener와 buffer의 생명주기를 관리한다. 표준 TF backend가 초기화되면 listener가 `/tf`와 `/tf_static` subscription을 자동으로 생성하지만, TF display는 이 구독을 생성하는 구성 요소가 아니라 buffer의 frame 관계를 시각화하는 consumer다. RViz2 configuration에서 transformation backend, Fixed Frame과 TF display를 구분하는 방법은 [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)에서 설명한다.

tf2 Buffer API의 기본 조회 형태는 다음과 같다.

```text
lookupTransform(target_frame, source_frame, time)
```

Target frame을 `A`, source frame을 `B`라고 하면 반환값은 ${}^{A}\mathbf{T}_{B}$다. 즉 source frame에서 표현된 data 좌표를 target frame에서 다시 표현할 때 사용한다.

tf2가 topic에 있는 모든 sensor data를 자동으로 변환하는 것은 아니다. Consumer가 transform을 조회하고 point, pose 또는 다른 stamped data의 수치 좌표에 적용해 target frame 표현을 만들어야 한다.

### TF tree는 listener별 buffer에 구성된다

TF tree를 영구적으로 보관하는 중앙 process나 단일 file은 없다. Broadcaster가 transform sample을 publish하면 각 listener가 message를 받아 자신의 tf2 buffer에 TF tree와 시간별 transform을 구성한다. 실제 transform query에 응답하는 것은 해당 listener의 buffer다.

| 대상 | 역할 |
|---|---|
| Model·calibration·pose estimate | Parent-child transform을 결정하는 입력 |
| Transform broadcaster | 입력에서 얻은 transform sample을 `/tf` 또는 `/tf_static`에 publish하는 node 또는 library object |
| `/tf`, `/tf_static` | 실행 중인 broadcaster와 listener 사이에서 `tf2_msgs/msg/TFMessage`를 전달하는 topic |
| listener의 tf2 buffer | 수신한 transform을 시간과 함께 보관하고 연결된 frame 사이의 transform을 계산하는 memory |

Broadcaster가 transform 값을 얻는 방법은 관계의 종류에 따라 다르다. 고정 관계는 calibration 값에서, 움직이는 관계는 joint state·odometry·localization 결과에서 얻을 수 있다. TF2는 이 물리 상태를 sensor에서 직접 추정하지 않고 broadcaster가 보낸 transform을 전달·저장·조회한다. URDF model을 읽는 구체적인 broadcaster는 다음 문서인 [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)에서 설명하고, 동적 transform의 계산 주체는 [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)에서 설명한다.

## Static transform과 dynamic transform

TF tree의 parent-child 연결 구조가 같아도 그 관계를 나타내는 translation과 rotation이 시간에 따라 변하는지에 따라 publish 방법이 달라진다.

| 종류 | 적용 대상 | Topic | 시간 처리 |
|---|---|---|---|
| static transform | Body와 고정 sensor처럼 상대 pose가 변하지 않는 관계 | `/tf_static` | 한 번 publish한 관계를 호환되는 late subscriber도 받을 수 있다. |
| dynamic transform | 움직이는 joint나 이동 robot처럼 상대 pose가 변하는 관계 | `/tf` | Timestamp가 다른 transform sample을 listener buffer에 보관한다. |

Static과 dynamic은 child가 world에서 움직이는지 여부가 아니라 **직접 연결된 parent에 대한 상대 pose가 변하는지**로 구분한다. 예를 들어 body에 고정된 lidar는 robot과 함께 world에서 움직여도 `base_link → lidar_link`가 static이다. 반대로 wheel이나 arm link는 `base_link`에 대한 관절 위치가 변하므로 dynamic이다.

Dynamic transform의 값을 TF2가 sensor에서 추정하는 것은 아니다. 해당 관계를 책임지는 component가 joint state, odometry 또는 localization 결과로 시각별 translation과 rotation을 계산하고 broadcaster를 통해 publish한다. 계산 주체와 실제 ROS 2 package는 [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)에서 설명한다.

## Static transform을 command로 확인

`tf2_ros` package의 `static_transform_publisher` executable은 두 frame 사이의 static transform을 command line에서 빠르게 확인할 때 사용할 수 있다.

```bash
ros2 run tf2_ros static_transform_publisher \
  --x 0.20 --y 0.00 --z 0.30 \
  --roll 0.00 --pitch 0.00 --yaw 0.00 \
  --frame-id base_link \
  --child-frame-id lidar_link
```

이 명령은 `header.frame_id = base_link`, `child_frame_id = lidar_link`인 static transform을 publish한다. Translation `(0.2, 0.0, 0.3)`은 `lidar_link`의 origin을 `base_link`에서 표현한 좌표이고 두 frame의 basis 방향은 같다. 이 transform은 `lidar_link` point 좌표를 `base_link` 좌표로 바꿀 때 사용할 수 있다. Command를 실행하는 process가 transform broadcaster이며, process를 종료하면 새 graph에서 이 broadcaster도 사라진다.

최종 robot model을 URDF와 `robot_state_publisher`로 publish한다면 같은 `base_link` → `lidar_link` static transform을 `static_transform_publisher`로 동시에 publish하지 않는다. 위 command는 값과 연결을 독립적으로 확인하는 임시 수단으로 사용한다.

## Timestamped transform의 기본 구조

Dynamic transform은 시간에 따른 함수 전체를 message 하나에 담지 않는다. Broadcaster가 서로 다른 timestamp의 `geometry_msgs/msg/TransformStamped` sample을 계속 publish한다.

```text
TransformStamped
├── header.stamp          : transform이 나타내는 time t
├── header.frame_id       : parent frame A
├── child_frame_id        : child frame B
├── transform.translation : A에서 표현한 B origin의 position
└── transform.rotation    : A에서 표현한 B basis의 orientation
```

각 listener의 buffer는 수신한 sample을 시간별로 보관한다. Consumer가 `lookupTransform(A, B, t)`를 호출하면 buffer는 시각 `t`에 source `B` 좌표를 target `A` 좌표로 바꾸는 transform을 반환한다. Sensor data를 처리할 때는 보통 message의 `header.stamp`를 query time으로 사용한다. 보간, buffer 범위와 extrapolation error는 [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)에서 자세히 설명한다.

## TF tree 확인

두 frame 사이의 transform을 계속 조회한다.

```bash
ros2 run tf2_ros tf2_echo base_link lidar_link
```

이 명령은 ${}^{\mathrm{base\_link}}\mathbf{T}_{\mathrm{lidar\_link}}$를 출력한다. `base_link`가 target frame이고 `lidar_link`가 source frame이다. Static transform이라면 출력되는 translation과 rotation이 시간에 따라 변하지 않아야 한다.

전체 TF tree를 diagram으로 저장하려면 Linux에서 다음 command를 실행한다.

```bash
ros2 run tf2_tools view_frames
```

이 command는 일정 시간 동안 transform을 수신한 뒤 현재 directory에 `frames.pdf`를 생성한다. Diagram은 관찰 시점의 snapshot이며 현재 broadcaster process를 대신하지 않는다.

현재 ROS graph의 node, topic과 publisher도 함께 확인한다.

```bash
ros2 node list
ros2 topic list -t
ros2 topic info /tf_static --verbose
```

Tree 모양뿐 아니라 translation, rotation과 broadcaster도 확인해야 잘못된 축 방향이나 중복 publisher를 찾을 수 있다.

## 문제 확인 순서

| 관찰 | 확인할 내용 |
|---|---|
| `tf2_echo`가 frame이 없다고 보고한다. | Frame 이름, broadcaster process, ROS domain과 setup sourcing을 확인한다. |
| 저장된 TF diagram은 있지만 현재 frame을 찾지 못한다. | Diagram은 snapshot이므로 broadcaster가 현재 실행 중인지 확인한다. |
| 두 frame을 각각 찾지만 transform을 계산하지 못한다. | 서로 다른 root를 가진 disconnected tree인지 확인한다. |
| Point cloud 위치가 반대 방향으로 이동한다. | Parent-child 관계, target/source 방향과 translation이 어느 frame에서 표현됐는지 확인한다. |
| Frame 방향이 예상과 다르다. | Degree를 radian 값으로 잘못 넣지 않았는지와 sensor axis convention을 확인한다. |
| Frame이 흔들리거나 parent가 바뀐다. | 같은 child frame을 둘 이상의 broadcaster가 publish하는지 확인한다. |

Dynamic timestamp와 `map → odom → base_link` 문제는 [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)의 진단 절에서 이어서 확인한다.

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Node and Topic](<./02 Node and Topic.md>)
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)
- [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)
- [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)

## References

- [REP-103 - Standard Units of Measure and Coordinate Conventions](https://github.com/ros-infrastructure/rep/blob/master/rep-0103.rst)
- [geometry_msgs - TransformStamped Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/geometry_msgs/msg/TransformStamped.msg)
- [tf2_ros - Buffer Interface](https://github.com/ros2/geometry2/blob/jazzy/tf2_ros/include/tf2_ros/buffer_interface.hpp)
- [ROS 2 Documentation - Introducing tf2](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Introduction-To-Tf2.rst)
- [ROS 2 Documentation - Writing a Static Broadcaster in C++](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Writing-A-Tf2-Static-Broadcaster-Cpp.rst)
- [tf2 Jazzy Documentation](https://docs.ros.org/en/jazzy/p/tf2/)
