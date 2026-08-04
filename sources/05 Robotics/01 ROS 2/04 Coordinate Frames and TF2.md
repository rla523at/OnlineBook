# ROS 2 Coordinate Frames and TF2

## 한 줄 요약

Coordinate frame은 공간의 원점과 basis를 정의하고, tf2는 frame 사이의 translation과 rotation을 시간과 함께 관리하여 같은 geometric data의 좌표를 서로 다른 frame에서 표현할 수 있게 한다.

## 전체 흐름: Sensor data를 다른 Frame에서 사용하기까지

Sensor나 algorithm이 만드는 수치 data는 특정 source frame의 원점과 축을 기준으로 표현된다. 다른 component가 이 data를 자신의 기준 frame에서 사용하려면 sensor data와 frame 관계라는 두 종류의 정보가 모두 필요하다.

```text
Frame 관계를 만드는 흐름                         Sensor data를 만드는 흐름

URDF·calibration·pose estimation                  sensor·algorithm
                 │                                      │
                 ▼                                      ▼
        transform broadcaster                    stamped data message
                 │                               ├── 수치 data
                 │                               ├── header.frame_id
                 ▼                               └── header.stamp
        /tf 또는 /tf_static                              │
                 │                                      │
                 ▼                                      ▼
        listener의 TF buffer ───────────────────────> consumer
                                                        │
                                                        ├── target frame 선택
                                                        ├── transform 조회
                                                        ├── data에 transform 적용
                                                        └── 처리·표시·재발행
```

두 흐름은 독립적으로 전달된다. `/tf`와 `/tf_static`에는 sensor 측정값이 들어 있지 않고, sensor message에는 다른 frame까지의 transform이 들어 있지 않다. Consumer가 sensor message의 `header.frame_id`와 `header.stamp`를 이용해 tf2 buffer에서 필요한 transform을 조회하면서 두 흐름을 결합한다.

### 1. Frame 관계를 정의하고 배포한다

Robot에 고정된 sensor의 장착 관계는 URDF나 calibration으로 정의할 수 있다. 움직이는 robot이나 joint의 pose는 odometry, localization, joint state와 같은 실행 중의 추정값으로 결정할 수 있다.

이 관계를 책임지는 broadcaster는 parent-child frame 사이의 transform을 `/tf` 또는 `/tf_static`에 publish한다. URDF file이나 calibration 값이 존재하는 것만으로 tf2 buffer에 transform이 생기는 것은 아니며, 실행 중인 broadcaster가 그 관계를 publish해야 한다.

```text
고정된 장착 관계
URDF·calibration
        │
        ▼
static transform broadcaster
        │
        ▼
/tf_static

시간에 따라 변하는 관계
odometry·localization·joint state
        │
        ▼
dynamic transform broadcaster
        │
        ▼
/tf
```

### 2. Sensor data가 source frame과 측정 시각을 명시한다

Sensor message의 수치값은 특정 frame을 기준으로 표현된다. `header.frame_id`는 그 수치값의 source frame을 나타내고, `header.stamp`는 data를 측정한 시각을 나타낸다.

```text
sensor message
├── data            : source frame에서 표현된 측정값
├── header.frame_id : source frame
└── header.stamp    : measurement time
```

예를 들어 `PointCloud2.header.frame_id = lidar_link`라면 point 좌표들은 `lidar_link`의 원점과 축을 기준으로 표현되어 있다. `lidar_link`가 TF tree의 leaf인 경우가 많지만 source frame이 반드시 leaf여야 하는 것은 아니다.

### 3. Listener가 TF buffer를 구성한다

Listener는 `/tf`와 `/tf_static`의 transform을 수신해 자신의 tf2 buffer에 frame 관계와 시간별 transform을 구성한다. TF tree를 보관하는 하나의 중앙 process가 있는 것이 아니라, RViz2나 application처럼 transform을 사용하는 listener마다 자신의 buffer를 가진다.

TF buffer에는 sensor data가 저장되지 않는다. Sensor data는 원래 topic으로 전달되고, buffer에는 frame 사이의 transform만 저장된다.

### 4. Consumer가 target frame을 선택한다

Consumer는 자신의 작업 목적에 맞는 target frame을 선택한다. RViz2에서는 Fixed Frame이 target frame이고, 일반 application에서는 처리 결과를 표현하려는 frame이 target frame이다.

| Target frame 예 | 사용 목적 |
|---|---|
| Sensor frame | Sensor가 측정한 원래 형태를 확인한다. |
| `base_link` | Robot body를 기준으로 sensor data를 처리한다. |
| `odom` | 연속적인 local motion 기준으로 data를 누적한다. |
| `map` | Global map이나 localization 결과에 맞춰 data를 배치한다. |
| `earth` | Georeference된 공통 지구 기준으로 data를 표현한다. |

Target frame이 TF tree의 root일 필요는 없다. Source와 target이 같은 연결 component에 있고 필요한 시각의 transform을 사용할 수 있으면 어느 방향으로도 변환할 수 있다.

### 5. Source, target과 time으로 transform을 조회한다

Source frame을 `S`, target frame을 `T`, 측정 시각을 $t$라고 하면 consumer는 다음과 같이 조회한다.

```text
lookupTransform(T, S, t)
                │  │  └── query time
                │  └───── source frame
                └──────── target frame
```

tf2 buffer는 `S`와 `T` 사이의 TF tree 경로를 찾고, 경로에 있는 transform과 inverse transform을 합성해 다음 transform을 반환한다.

$$
{}^{T}\mathbf T_{S}(t)
$$

`lookupTransform()`이 반환하는 것은 변환된 sensor data가 아니라 source frame의 좌표를 target frame의 좌표로 바꾸는 transform이다.

### 6. Consumer가 transform을 data에 적용한다

Source frame `S`에서 표현된 point 좌표를 target frame `T`에서 표현하려면 반환된 rotation과 translation을 실제 좌표에 적용해야 한다.

$$
{}^{T}\mathbf p
=
{}^{T}\mathbf R_{S}(t)\,{}^{S}\mathbf p
+
{}^{T}\mathbf t_{S}(t)
$$

Point, direction vector, orientation과 pose는 기하학적 의미가 다르므로 consumer는 data type에 맞는 변환 규칙을 사용해야 한다. Point에는 rotation과 translation을 적용하지만, 방향만 나타내는 vector에는 origin 이동을 나타내는 translation을 적용하지 않는다.

Data가 TF tree의 각 중간 frame 좌표로 차례대로 변환되는 것은 아니다. Buffer는 경로의 transform을 합성해 요청한 source-to-target transform 하나를 반환하고, consumer는 그 transform을 data에 적용한다. 여러 target frame에서의 표현이 모두 필요하면 target별로 transform을 조회하고 각각 계산해야 한다.

### 7. 변환 결과를 처리하거나 표시한다

Consumer는 target frame에서 표현된 결과를 계산, 누적 또는 시각화에 사용한다. RViz2는 Fixed Frame을 target으로 transform을 조회한 뒤 변환된 좌표로 data를 화면에 표시한다. 이 과정에서 원래 sensor topic의 message가 변경되는 것은 아니다.

변환 결과를 새로운 message로 publish한다면 좌표값에 transform을 실제로 적용한 뒤 `header.frame_id`를 target frame으로 기록해야 한다. 좌표값은 그대로 둔 채 `frame_id` 문자열만 바꾸면 좌표 변환이 일어나지 않는다.

### 전체 흐름 요약

```text
1. Sensor가 source frame 기준으로 data를 측정한다.
2. Message가 source frame과 measurement time을 기록한다.
3. 별도의 broadcaster가 frame 사이의 transform을 publish한다.
4. Listener가 transform을 받아 TF buffer를 구성한다.
5. Consumer가 사용할 target frame을 선택한다.
6. Consumer가 target, source와 time으로 transform을 조회한다.
7. Consumer가 반환된 transform을 data에 적용한다.
8. Target frame에서 표현된 결과를 처리하거나 표시한다.
```

이 전체 흐름을 기준으로 이후 내용은 다음과 같이 연결된다.

| 이후 내용 | 전체 흐름에서 설명하는 부분 |
|---|---|
| Coordinate frame이 필요한 이유 | Sensor data의 숫자에 source frame이 필요한 이유 |
| Frame과 transform | Source 좌표를 target 좌표로 바꾸는 수학적 관계 |
| tf2의 역할 | Transform을 publish, 저장, 조회하는 runtime 구조 |
| TF tree | 여러 transform을 연결하고 합성하는 구조 |
| 이동 robot에서 사용하는 frame | 작업 목적에 따른 target frame 선택 |
| Static과 dynamic transform | Frame 관계가 시간에 따라 변하는지 여부 |
| Transform과 timestamp | 측정 시각에 맞는 transform을 조회해야 하는 이유 |
| TF tree 확인 | 두 frame 사이의 변환 경로가 실제로 존재하는지 검증하는 방법 |

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

`base_link`는 mobile robot body에 고정된 기준 frame이다. URDF에서는 같은 이름의 link와 그 link에 붙은 coordinate frame을 함께 가리킬 수 있으며, 원점의 정확한 위치는 robot model을 설계할 때 정한다.

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

따라서 ${}^{A}\mathbf{R}_{B}$는 같은 displacement vector의 coordinate를 frame `B`의 basis에서 frame `A`의 basis로 바꾸는 [change of coordinate matrix](<../../01 math/07 Linear Algebra/21 Change of Basis and Coordinate Matrix.md>)다. 두 frame의 basis는 orthonormal이므로 ${}^{A}\mathbf{R}_{B}$는 scaling이나 shear를 포함하지 않는 rotation matrix다.

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

### Parent-child 관계와 좌표 변환 방향

TF tree diagram의 화살표와 coordinate가 변환되는 방향은 구분해야 한다. 이 문서의 tree 표기 `A → B`는 frame `A`가 parent이고 frame `B`가 child라는 관계를 뜻하며, 이 관계에는 ${}^{A}\mathbf{T}_{B}$가 저장된다.

| 관점 | `A → B` 관계의 의미 |
|---|---|
| Relative pose | Frame `A`를 기준으로 한 frame `B`의 pose |
| Coordinate transform | Frame `B` 좌표를 frame `A` 좌표로 바꾸는 ${}^{B}\mathbf{p}\mapsto{}^{A}\mathbf{p}$ |
| ROS message 표현 | `geometry_msgs/msg/TransformStamped`에서 `header.frame_id = A`, `child_frame_id = B` |

따라서 tree 화살표 `A → B`는 data coordinate가 `A`에서 `B`로 변환된다는 뜻이 아니다. 예를 들어 `base_link → lidar_link` parent-child 연결에는 ${}^{\mathrm{base\_link}}\mathbf{T}_{\mathrm{lidar\_link}}$가 저장된다. 이 transform은 `base_link`를 기준으로 한 `lidar_link`의 장착 pose를 나타내며, coordinate에 적용하면 lidar point를 `lidar_link` 좌표에서 `base_link` 좌표로 다시 표현한다. 반대 방향으로 변환하려면 inverse transform이 필요하다.

## ROS coordinate convention

ROS의 표준 단위와 좌표 convention은 REP-103에 정의되어 있다. 다른 convention을 사용해야 하는 sensor가 있으면 해당 차이를 별도 frame과 transform으로 명시한다.

- 길이는 meter, 각도는 radian을 사용한다.
- Coordinate frame은 right-handed coordinate system을 따른다.
- Robot body frame은 `x` forward, `y` left, `z` up을 사용한다.
- Camera optical frame처럼 다른 축 convention이 필요한 frame은 `_optical` suffix를 사용하며 `z` forward, `x` right, `y` down을 사용한다.

`imu_link`나 `lidar_link`라는 이름만으로 실제 sensor 축이 자동 결정되지는 않는다. Driver가 발행하는 message의 frame convention과 실제 장착 방향을 확인한 뒤 transform의 rotation에 반영해야 한다.

Rotation을 roll, pitch, yaw로 입력할 때는 각각 x, y, z fixed axis에 대한 회전이며 값은 radian이다. Quaternion을 직접 입력할 때는 zero rotation인 `(x, y, z, w) = (0, 0, 0, 1)`처럼 normalized quaternion을 사용한다.

## tf2의 역할

`tf2`는 ROS 2에서 coordinate transform을 배포하고 조회하는 library 집합이다. Broadcaster는 parent frame과 child frame 사이에서 특정 timestamp의 transform 값인 transform sample을 publish하고, listener는 이를 수신해 자신의 buffer에 보관한다. Transform이 필요한 process는 이 buffer에 source frame, target frame과 query time을 지정해 조회한다.

Parent frame을 `A`, child frame을 `B`라고 하면 시각 $t_i$의 transform sample은 ${}^{A}\mathbf{T}_{B}(t_i)$다. `tf2_ros` broadcaster API에서는 sample 하나를 `geometry_msgs/msg/TransformStamped`로 표현한다. Dynamic transform ${}^{A}\mathbf{T}_{B}(t)$는 timestamp가 서로 다른 여러 `TransformStamped` sample로 전달되므로, `TransformStamped` 하나가 시간에 따른 transform 전체를 뜻하지는 않는다.

`/tf`와 `/tf_static`의 topic type은 `tf2_msgs/msg/TFMessage`이며, `transforms` field에 하나 이상의 `TransformStamped` sample을 담는다. 따라서 broadcaster는 두 frame 자체가 아니라 두 frame 사이의 timestamped translation과 rotation을 publish한다.

```text
transform broadcaster
        │
        │ timestamped transform sample publish
        ▼
     /tf 또는 /tf_static (`TFMessage`)
        │
        ▼
listener와 transform buffer
        │
        │ target frame A, source frame B, time으로 조회
        ▼
     B 좌표 → A 좌표 transform 반환
        │
        ▼
application 또는 RViz2가 data 좌표를 B에서 A로 다시 표현
```

- `broadcaster`는 자신이 책임지는 parent-child frame 사이의 transform sample을 ROS graph에 publish한다.
- `listener`는 `TFMessage`에 담긴 `TransformStamped` sample을 수신해 buffer에 전달한다.
- `buffer`는 시간별 transform을 보관하고 source frame에서 target frame으로 좌표를 바꾸는 transform을 계산한다.
- `RViz2` 같은 consumer는 message의 frame을 source, 표시 기준 frame을 target으로 사용해 transform을 조회한 뒤 변환된 좌표로 data를 표시한다.

tf2 Buffer API의 기본 조회 형태는 다음과 같다.

```text
lookupTransform(target_frame, source_frame, time)
```

Target frame을 `A`, source frame을 `B`라고 하면 반환값은 ${}^{A}\mathbf{T}_{B}$다. 즉 source frame에서 표현된 data 좌표를 target frame에서 다시 표현할 때 사용한다.

tf2가 topic에 있는 모든 sensor data를 자동으로 변환하는 것은 아니다. Consumer가 transform을 조회하고 point, pose 또는 다른 stamped data의 수치 좌표에 적용해 target frame 표현을 만들어야 한다.

## TF tree가 존재하는 위치

TF tree를 영구적으로 보관하는 중앙 process나 단일 file은 없다. Broadcaster가 transform sample을 publish하면 각 listener가 message를 받아 자신의 tf2 buffer에 TF tree와 시간별 transform을 구성한다. 실제 transform query에 응답하는 것은 해당 listener의 buffer다.

| 대상 | 역할 |
|---|---|
| URDF file | Link와 joint 관계를 저장한 model description |
| `robot_state_publisher` | URDF joint 관계를 `/tf` 또는 `/tf_static` transform으로 publish하는 broadcaster |
| `/tf`, `/tf_static` | 실행 중인 broadcaster와 listener 사이에서 `tf2_msgs/msg/TFMessage`를 전달하는 topic |
| listener의 tf2 buffer | 수신한 transform을 시간과 함께 보관하고 연결된 frame 사이의 transform을 계산하는 memory |
| `view_frames` output | 관찰 시점의 TF tree를 file로 저장한 diagram snapshot |

URDF를 사용하는 경우 link 이름은 TF tree의 frame이 되고 joint는 parent link와 child link 사이의 transform을 정의한다. Joint 이름 자체가 별도의 TF frame이 되는 것은 아니다. Link와 joint의 구체적인 대응은 [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)에서 설명한다.

```text
URDF file
    │ robot_state_publisher가 읽음
    ▼
/tf 또는 /tf_static
    │ listener가 수신
    ▼
listener별 tf2 buffer ──> transform query
```

따라서 저장된 `frames.pdf`가 존재해도 현재 broadcaster가 실행 중이라는 뜻은 아니다. 반대로 현재 TF tree가 정상이어도 `view_frames`를 실행하지 않았다면 diagram file은 존재하지 않을 수 있다.

## TF tree

tf2는 frame을 node로, parent-child frame 사이의 transform을 edge로 하는 tree 구조를 사용한다. 아래 diagram에서는 위쪽 frame이 parent이고 아래쪽 frame이 child다. 하나의 연결된 robot model을 만들 때는 다음 조건을 지킨다.

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

## 이동 robot에서 자주 사용하는 frame

REP-105는 mobile robot에서 `map`, `odom`과 `base_link`라는 frame 이름에 공통 의미를 부여한다. 여기서 `world-fixed` frame은 robot body에 붙어서 함께 움직이는 frame이 아니라 환경을 기준으로 robot pose를 표현하는 frame이다.

| Frame | 역할 | Robot pose의 특성 |
|---|---|---|
| `map` | Localization이 사용하는 장기적인 world-fixed 기준 | 장기 drift를 억제하지만 새 관측을 반영할 때 pose가 불연속적으로 보정될 수 있다. |
| `odom` | Odometry가 상대 이동을 누적하는 world-fixed 기준 | 값이 연속적으로 변하지만 장시간에는 drift가 누적될 수 있다. |
| `base_link` | Robot body에 고정된 기준 | Robot이 이동하면 `map`과 `odom`에 대한 pose가 변한다. |

`odom` 원점은 system 시작 위치 근처로 초기화할 수 있지만 모든 구현이 `odom → base_link`를 정확히 identity로 시작해야 하는 것은 아니다. 핵심 조건은 robot pose가 `odom` 기준에서 불연속적으로 뛰지 않고 연속적으로 변하는 것이다. 연속성, drift와 localization의 일반적인 의미는 [Robotics](<../Robotics.md>)에서 설명한다.

REP-105를 따르는 최소 TF 관계는 다음과 같다.

```text
map
└── odom
    └── base_link
        ├── imu_link
        └── lidar_link
```

각 parent-child 관계는 서로 다른 component가 책임질 수 있다.

| Parent → child | 일반적인 broadcaster | 나타내는 pose |
|---|---|---|
| `map → odom` | SLAM 또는 localization component | `map`에서 표현한 `odom`의 pose와 odometry 보정 |
| `odom → base_link` | Wheel·visual odometry 또는 state estimation component | `odom`에서 표현한 `base_link`의 연속적인 pose |
| `base_link → sensor frame` | URDF를 읽는 `robot_state_publisher` | `base_link`에서 표현한 sensor의 장착 pose |

각 행의 transform을 coordinate에 적용하면 child frame 좌표를 parent frame 좌표로 바꾼다.

Localization component는 sensor 관측을 map이나 다른 외부 기준과 비교해 `map`에서 표현한 `base_link` pose ${}^{\mathrm{map}}\mathbf{T}_{\mathrm{base\_link}}$를 다시 추정한다. TF tree에서 `base_link`는 이미 `odom`의 child이므로 localization component는 보통 `map → base_link` 관계를 직접 broadcast하지 않고 `map → odom` 관계를 계산해 publish한다.

$$
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{odom}}
=
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{base\_link}}
\left(
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}
\right)^{-1}
$$

Translation만 있는 1D 예제에서 `map`과 `odom`의 축 방향이 같다고 하자. Odometry는 출발 후 robot이 10.3 m 이동했다고 누적했지만 localization은 map의 wall과 lidar 관측을 비교해 robot의 map pose를 10.0 m로 추정할 수 있다.

TF tree의 transform 합성은 다음과 같다.

$$
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{base\_link}}
=
{}^{\mathrm{map}}\mathbf{T}_{\mathrm{odom}}
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}
$$

이 예제에서는 세 frame의 basis 방향이 같고 translation만 있으므로 x 좌표를 다음처럼 더할 수 있다.

$$
\begin{aligned}
{}^{\mathrm{odom}}t_{\mathrm{base\_link},x} &= 10.3\ \mathrm{m} \\
{}^{\mathrm{map}}t_{\mathrm{odom},x} &= -0.3\ \mathrm{m} \\
{}^{\mathrm{map}}t_{\mathrm{base\_link},x}
&= -0.3 + 10.3
= 10.0\ \mathrm{m}
\end{aligned}
$$

이때 `odom → base_link`를 10.0 m로 갑자기 바꾸지 않으므로 odometry의 연속성은 유지된다. 대신 `map → odom`이 바뀌어 robot의 `map` 기준 pose가 보정된다. 실제 robot이 순간 이동한 것이 아니라 위치 추정값이 바뀐 것이다. `map` 자체가 보정된 숫자라는 뜻도 아니다. `map`은 좌표 기준이고 localization이 보정하는 대상은 그 기준에서 표현한 robot pose다.

### map frame의 원점

`map`의 원점은 map 영역의 기하학적인 중심으로 정해져 있지 않다. Map을 만드는 system이나 application이 `(0, 0, 0)`의 기준 위치와 축 방향을 정하고 사용자에게 그 선택을 명시해야 한다.

- 외부 위치 기준 없이 SLAM을 시작하면 시작 시점의 robot 위치를 원점으로 초기화할 수 있다.
- 미리 만든 indoor map은 건물의 corner, 출입구 또는 설계도 기준점을 원점으로 사용할 수 있다.
- Georeference된 outdoor map은 측량 기준 또는 `earth` frame과의 관계로 원점을 정할 수 있다.

따라서 `map` 원점은 robot의 출발 위치와 일치할 수 있지만 반드시 일치하지는 않는다. 이미 만들어진 map에서 localization을 시작하면 robot의 초기 `map` pose는 원점이 아닌 임의의 위치일 수 있다.

`nav_msgs/msg/OccupancyGrid`의 `info.origin`도 `map` frame 자체의 원점과 구분해야 한다. `OccupancyGrid`는 `header.frame_id`로 grid 좌표의 기준 frame을 지정하고, `info.origin`으로 grid cell `(0, 0)`의 왼쪽 아래 corner가 그 frame에서 어디에 있는지 표현한다. 예를 들어 `header.frame_id`가 `map`이고 `info.origin`이 `(-10, -10)`이면 cell `(0, 0)`은 `map` 원점에서 왼쪽 아래에 있다. `map` frame은 OccupancyGrid message가 없어도 coordinate frame으로 존재할 수 있다.

### odom frame, `/odom` topic과 TF

같은 `odom`이라는 문자열이 들어가도 frame, topic과 transform은 서로 다른 대상이다.

| 대상 | 의미 |
|---|---|
| `odom` frame | Pose를 표현하는 coordinate frame 이름 |
| `/odom` topic | 관례적으로 `nav_msgs/msg/Odometry` message를 전달하는 topic 이름이며 remap할 수 있다. |
| `odom → base_link` TF | `odom`에서 표현한 `base_link`의 시간별 pose이며, `base_link` 좌표를 `odom` 좌표로 바꿀 때 적용 |

`nav_msgs/msg/Odometry`는 pose와 velocity 추정값을 전달한다. Pose의 기준은 message의 `header.frame_id`, velocity의 기준은 `child_frame_id`로 표현한다. `/odom` topic을 publish하는 것만으로 tf2 buffer에 transform이 자동으로 생기지는 않는다. Odometry component가 topic과 TF를 모두 제공할 수도 있고, 별도 component가 `odom → base_link`를 broadcast할 수도 있으므로 실행 중인 system의 interface를 확인해야 한다.

## Static transform과 dynamic transform

일반적인 TF tree에서는 parent와 child의 연결 구조를 유지한다. Static과 dynamic의 차이는 그 연결을 나타내는 translation과 rotation 값이 시간에 따라 변하는지 여부다.

| 종류 | 적용 대상 | Topic | 시간 처리 |
|---|---|---|---|
| static transform | body와 고정 sensor처럼 변하지 않는 관계 | `/tf_static` | 한 번 publish한 관계를 late subscriber도 받을 수 있다. |
| dynamic transform | `odom → base_link`, `map → odom` 또는 움직이는 joint처럼 변하는 관계 | `/tf` | timestamp별 transform을 buffer에 보관한다. |

Robot이 world에서 움직여도 body에 고정된 lidar의 장착 pose는 변하지 않는다. 반대로 robot이 정지해 있어도 localization이 새 관측으로 pose를 다시 추정하면 `map → odom` 값은 바뀔 수 있다.

```text
base_link → lidar_link
t=0 s, 1 s, 2 s: translation = (0.2, 0.0, 0.3)

odom → base_link
t=0 s: x=0.0 m
t=1 s: x=1.0 m
t=2 s: x=2.0 m
```

`/tf_static`은 transient-local durability를 사용한다. Broadcaster endpoint가 유지되는 동안에는 RViz2처럼 나중에 실행된 호환 listener도 저장된 static transform을 받을 수 있다. Broadcaster process를 종료한 뒤 새로 시작한 graph가 이전 diagram file에서 transform을 복원하는 것은 아니다. Dynamic transform은 계속 갱신되어야 하며 query time에 사용할 수 있는 transform이 buffer 안에 있어야 한다.

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

## Transform과 timestamp

ROS sensor message의 `header.stamp`는 측정 시각을 나타낸다. Consumer가 움직이는 frame에서 측정한 data 좌표를 다른 frame에서 표현하려면 현재 시간이 아니라 측정 시각의 transform이 필요하다.

Sensor message의 `header.frame_id`는 data가 표현된 frame의 이름만 기록하며, 그 frame의 시간별 origin과 basis pose를 message 안에 저장하지는 않는다. 움직이는 frame의 pose는 transform broadcaster가 timestamp가 서로 다른 `geometry_msgs/msg/TransformStamped` sample을 `/tf`를 통해 계속 전달하여 나타낸다. Parent frame을 `A`, child frame을 `B`라고 하면 각 sample에는 다음 정보가 기록된다.

```text
TransformStamped
├── header.stamp          : transform이 나타내는 time t
├── header.frame_id       : parent frame A
├── child_frame_id        : child frame B
├── transform.translation : A에서 표현한 B origin의 position
└── transform.rotation    : A에서 표현한 B basis의 orientation
```

따라서 이 message는 시각 $t$의 ${}^{A}\mathbf{T}_{B}(t)$를 나타낸다. 각 listener의 tf2 buffer는 수신한 dynamic transform을 timestamp별로 보관한다. Consumer가 `lookupTransform(A, B, t)`를 호출하면 buffer는 시각 $t$에 `B` 좌표를 `A` 좌표로 바꾸는 transform을 계산하며, 여러 frame을 거치는 경우에는 TF tree의 transform을 합성한다.

예를 들어 lidar가 robot body에 고정되어 있으면 `base_link → lidar_link`는 static transform이다. Robot이 움직일 때 시간에 따라 달라지는 transform은 `odom → base_link` 같은 상위 transform이며, tf2는 다음과 같이 시각 $t$의 전체 transform을 계산한다.

$$
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{lidar\_link}}(t)
=
{}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}(t)
{}^{\mathrm{base\_link}}\mathbf{T}_{\mathrm{lidar\_link}}
$$

따라서 sensor message의 `header.frame_id`는 계속 `lidar_link`여도 `odom`에서 본 `lidar_link`의 origin과 basis는 시간에 따라 달라질 수 있다. Lidar가 회전 joint나 gimbal에 장착되어 `base_link`에 대한 장착 pose 자체가 변한다면 `base_link → lidar_link`도 timestamp별 dynamic transform으로 `/tf`에 publish한다.

```text
sensor message
├── header.frame_id : data가 표현된 frame
└── header.stamp    : data를 측정한 time

transform query
├── source frame : message 좌표가 표현된 frame
├── target frame : 좌표를 다시 표현할 frame
└── query time
```

tf2 buffer는 dynamic transform을 timestamp별로 일정 시간 동안 보관한다. Consumer가 sensor message의 `header.stamp`를 query time으로 사용한다면, 조회 경로에 포함된 각 dynamic transform이 그 시각에 사용 가능해야 한다. 한 dynamic transform의 저장 범위를 기준으로 query 결과를 나누면 다음과 같다.

| Query time 조건 | 의미 | 결과 |
|---|---|---|
| `query time < oldest transform time` | 필요한 시각의 transform이 buffer에 보관된 가장 오래된 transform보다 이전이다. | past extrapolation error |
| `oldest transform time ≤ query time ≤ latest transform time` | 저장된 transform을 사용하거나 필요한 경우 앞뒤 timestamp의 transform 사이를 보간할 수 있다. | 시간 범위 조건을 만족한다. |
| `latest transform time < query time` | 필요한 시각의 transform이 아직 buffer에 도착하지 않았다. | future extrapolation error |

예를 들어 sensor message의 `header.stamp`가 `10.20 s`인데 listener가 수신한 최신 dynamic transform이 `10.15 s`까지라면, `10.20 s`는 buffer의 최신 transform을 기준으로 미래다. Consumer가 이때 transform을 조회하면 future extrapolation error가 발생할 수 있으며, `10.20 s`에 사용할 transform이 buffer에 도착한 뒤에는 같은 query가 성공할 수 있다.

여기서 past와 future는 sensor data가 현재 시각보다 과거인지 미래인지를 뜻하지 않는다. Query time과 buffer에 저장된 transform timestamp를 비교한 표현이다. 여러 dynamic transform을 합성하는 query에서는 경로에 포함된 모든 dynamic transform이 query time을 지원해야 한다. Static transform은 시간에 따라 값이 변하지 않으므로 연결만 올바르면 모든 측정 시각에 사용할 수 있다.

Message의 좌표값은 그대로 둔 채 `frame_id` 문자열만 다른 frame 이름으로 바꾸면 좌표 변환이 일어나지 않는다. 좌표값을 실제 transform으로 다시 계산해 target frame을 기록하거나, 계산하지 않았다면 원래 측정 frame을 `frame_id`에 기록해야 한다.

## TF tree 확인

먼저 두 frame 사이의 transform을 계속 조회한다.

```bash
ros2 run tf2_ros tf2_echo base_link lidar_link
```

이 명령은 ${}^{\mathrm{base\_link}}\mathbf{T}_{\mathrm{lidar\_link}}$를 출력한다. 좌표 변환 관점에서는 `base_link`가 target frame이고 `lidar_link`가 source frame인 조회와 같으며, 출력된 transform으로 lidar 좌표를 base 좌표로 바꿀 수 있다. Translation과 rotation이 반복해서 출력되면 두 frame 사이의 연결을 조회할 수 있다는 뜻이다. Static transform이라면 출력 값이 변하지 않아야 한다.

전체 TF tree를 diagram으로 저장하려면 Linux에서 다음 command를 실행한다.

```bash
ros2 run tf2_tools view_frames
```

이 command는 일정 시간 동안 transform을 수신한 뒤 현재 directory에 `frames.pdf`를 생성한다. 이 최소 sensor rig 예제에서는 diagram에서 `base_link`가 root이고 `imu_link`, `lidar_link`가 직접 child인지 확인한다.

Odometry와 localization component를 함께 실행하는 mobile robot system에서는 대표적인 dynamic transform도 확인할 수 있다.

```bash
ros2 run tf2_ros tf2_echo map odom
ros2 run tf2_ros tf2_echo odom base_link
```

첫 번째 명령은 ${}^{\mathrm{map}}\mathbf{T}_{\mathrm{odom}}$, 두 번째 명령은 ${}^{\mathrm{odom}}\mathbf{T}_{\mathrm{base\_link}}$를 출력한다.

해당 component를 실행하지 않았다면 `map`이나 `odom` frame이 없는 것이 정상일 수 있다. URDF와 `robot_state_publisher`만으로 두 transform이 자동 생성되지는 않는다.

Topic과 publisher 상태도 함께 확인할 수 있다.

```bash
ros2 node list
ros2 topic list -t
ros2 topic info /tf_static --verbose
```

`view_frames`는 관찰한 관계를 저장하고, `node list`와 `topic info`는 현재 실행 상태를 확인한다. 과거에 저장한 diagram만으로 현재 TF가 활성 상태라고 판단하지 않는다.

`view_frames`의 tree 모양만 확인하지 않고 translation, rotation과 broadcaster도 확인해야 잘못된 축 방향이나 중복 publisher를 찾을 수 있다.

## 문제 확인 순서

| 관찰 | 확인할 내용 |
|---|---|
| `tf2_echo`가 frame이 없다고 보고한다. | Frame 이름, broadcaster process, ROS domain과 setup sourcing을 확인한다. |
| 저장된 TF diagram은 있지만 현재 frame을 찾지 못한다. | Diagram은 snapshot이므로 `robot_state_publisher` 같은 broadcaster가 현재 실행 중인지 확인한다. |
| 두 frame을 각각 찾지만 transform을 계산하지 못한다. | 서로 다른 root를 가진 disconnected tree인지 확인한다. |
| `/odom` topic은 있지만 `odom` frame을 찾지 못한다. | Topic publisher가 `odom → base_link` TF도 broadcast하는 구성인지 확인한다. |
| Point cloud 위치가 반대 방향으로 이동한다. | TF tree의 parent/child 관계와 좌표 변환의 target/source 방향, translation이 어느 frame에서 표현됐는지 확인한다. |
| Frame 방향이 예상과 다르다. | Degree를 radian 값으로 잘못 넣지 않았는지와 sensor axis convention을 확인한다. |
| RViz2가 extrapolation error를 표시한다. | Error에 표시된 query time과 oldest/latest transform time을 비교한다. Future error라면 dynamic transform의 timestamp와 publish 또는 수신 지연을 확인하고, past error라면 오래된 message가 처리되고 있는지와 buffer 보관 범위를 확인한다. 두 경우 모두 node들의 clock source와 `use_sim_time` 설정이 일치하는지 확인한다. |
| Frame이 흔들리거나 parent가 바뀐다. | 같은 child frame을 둘 이상의 broadcaster가 publish하는지 확인한다. |

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)
- [PointCloud2 and RViz2](<./06 PointCloud2 and RViz2.md>)
- [Node and Topic](<./02 Node and Topic.md>)

## References

- [REP-103 - Standard Units of Measure and Coordinate Conventions](https://github.com/ros-infrastructure/rep/blob/master/rep-0103.rst)
- [REP-105 - Coordinate Frames for Mobile Platforms](https://github.com/ros-infrastructure/rep/blob/master/rep-0105.rst)
- [geometry_msgs - TransformStamped Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/geometry_msgs/msg/TransformStamped.msg)
- [nav_msgs - Odometry Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/nav_msgs/msg/Odometry.msg)
- [nav_msgs - MapMetaData Message Definition](https://github.com/ros2/common_interfaces/blob/jazzy/nav_msgs/msg/MapMetaData.msg)
- [tf2_ros - Buffer Interface](https://github.com/ros2/geometry2/blob/jazzy/tf2_ros/include/tf2_ros/buffer_interface.hpp)
- [ROS 2 Documentation - Introducing tf2](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Introduction-To-Tf2.rst)
- [ROS 2 Documentation - Writing a Static Broadcaster in C++](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Intermediate/Tf2/Writing-A-Tf2-Static-Broadcaster-Cpp.rst)
- [tf2 Jazzy Documentation](https://docs.ros.org/en/jazzy/p/tf2/)
