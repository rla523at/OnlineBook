# Gauge Freedom and Trajectory Alignment Policy

## 한 줄 요약

Trajectory alignment는 coordinate mismatch를 자동 판별하는 과정이 아니라,
estimator가 관측할 수 없어서 평가에서 제외하기로 한 global 자유도를 미리 정한
protocol에 따라 제거하는 과정이다.

## 같은 차이가 coordinate frame일 수도 error일 수도 있다

다음 두 trajectory를 생각하자.

$$
(0,0)\rightarrow(1,0)\rightarrow(2,0)
$$

$$
(10,5)\rightarrow(10,6)\rightarrow(10,7)
$$

두 번째 trajectory를 clockwise $90^\circ$ 회전하고 translation을 적용하면 첫 번째
trajectory와 정확히 겹친다. 그러나 이 사실만으로 다음 두 상황을 구분할 수는 없다.

1. 두 trajectory가 같은 motion을 서로 다른 arbitrary coordinate frame에서 표현했다.
2. 같은 global frame을 출력해야 하는 estimator가 position과 heading을 잘못 추정했다.

두 상황은 point coordinate만 보면 같은 수학적 관계를 만들 수 있다. Alignment
algorithm은 residual이 가장 작은 transformation을 찾을 뿐, transformation이 필요한
원인을 판별하지 않는다.

## Observability

System의 measurement로 서로 다른 state를 구분할 수 있을 때 그 state component를
observable하다고 한다. 반대로 state를 바꿔도 모든 measurement prediction이 같다면
measurement만으로 그 차이를 알아낼 수 없다.

예를 들어 relative displacement만 측정하는 odometry는 motion의 변화는 추정할 수
있어도 world origin의 절대 위치를 직접 알 수 없다. 모든 estimated position에 같은
translation을 적용해도 relative displacement는 변하지 않는다.

$$
(p_j+t)-(p_i+t)=p_j-p_i
$$

어떤 rotation 자유도까지 관측 가능한지는 sensor와 prior에 따라 달라진다.

- Relative geometry만 사용하고 absolute orientation reference가 없으면 global rotation이 arbitrary할 수 있다.
- IMU가 gravity direction을 제공하면 roll·pitch는 gravity에 대해 관측 가능해질 수 있지만 global yaw는 여전히 arbitrary일 수 있다.
- Monocular camera만 사용하는 경우 global scale도 관측되지 않을 수 있다.
- GNSS, compass, known map landmark나 externally supplied initial pose는 일부 global 자유도를 anchor할 수 있다.

Sensor 종류만 보고 자동으로 단정하지 말고 실제 estimator가 사용하는 measurement,
initialization과 prior를 확인해야 한다.

## Gauge freedom

Measurement와 objective를 바꾸지 않으면서 state 전체에 적용할 수 있는 transformation
자유도를 gauge freedom이라고 한다. 서로 다른 gauge를 선택한 state들은 숫자는
다르지만 available measurement 관점에서는 같은 solution을 나타낸다.

Pure relative pose problem에서 모든 pose 왼쪽에 같은 $G\in SE(3)$를 곱해도 relative
pose는 변하지 않는다.

$$
T_i'=GT_i
$$

$$
(T_i')^{-1}T_j'
=
T_i^{-1}G^{-1}GT_j
=
T_i^{-1}T_j
$$

따라서 global $G$가 measurement로 고정되지 않았다면 하나의 representative를
선택하기 위해 첫 pose를 identity로 두거나 evaluation에서 global alignment를 사용할
수 있다.

Gauge freedom과 estimator error는 같은 말이 아니다. Gauge는 measurement model이
원래 구분하지 못하는 자유도이고, error는 estimator가 구분하거나 추정해야 할 값을
잘못 계산한 차이다.

## 판단 근거는 trajectory가 아니라 estimator contract다

Alignment 종류를 정하기 전에 다음 질문에 답해야 한다.

1. Estimator가 어떤 global reference를 입력받는가?
2. Output world frame은 arbitrary local frame인가, 지정된 map frame인가?
3. Initial position과 orientation은 제공되는가?
4. Gravity, heading와 metric scale 중 무엇이 measurement로 관측 가능한가?
5. 평가 목표는 relative odometry인가, global localization인가?
6. Dataset frame을 canonical frame으로 바꾸는 known transform이 있는가?

이 질문에 답하지 못하면 aligned와 unaligned 숫자는 만들 수 있어도 어느 숫자가
요구사항을 평가하는지는 결정할 수 없다. 이는 metric 문제가 아니라 benchmark
contract가 불완전한 상태다.

## Alignment mode가 제거하는 자유도

| Mode | Fit하는 자유도 | 제거하는 차이 | 주의점 |
|---|---:|---|---|
| None | 0 | 없음 | 두 trajectory가 이미 같은 global frame이어야 한다. |
| Known transform | 미리 알려진 값 | Dataset·sensor frame 표현 차이 | GT에 fit한 값이 아니라 calibration·metadata에서 독립적으로 얻어야 한다. |
| Origin alignment | 첫 pose 기준 | 초기 origin·orientation 차이 | 첫 pose noise에 민감하다. |
| 4-DoF | Translation 3 + yaw 1 | Global position·heading | Gravity가 roll·pitch를 고정하는 system에 적합할 수 있다. |
| $SE(3)$ | Translation 3 + rotation 3 | Global rigid frame | Observable roll·pitch error까지 제거할 수 있다. |
| $Sim(3)$ | $SE(3)$ + scale 1 | Global rigid frame과 scale | Metric-scale estimator의 scale error를 숨길 수 있다. |

자유도를 많이 fit할수록 residual은 같거나 작아진다. 따라서 더 작은 aligned ATE가
항상 더 정직하거나 더 좋은 protocol이라는 뜻은 아니다.

## 평가 목적별 primary metric

| 평가 대상 | Output contract | 권장 primary | 함께 볼 diagnostic |
|---|---|---|---|
| Arbitrary-frame odometry | Local origin·heading 또는 full global pose가 arbitrary | 허용된 gauge만 정렬한 ATE와 RPE | Unaligned overlay, initial offset |
| Gravity-aligned odometry | Roll·pitch와 scale은 관측, position·yaw는 arbitrary | 4-DoF alignment와 RPE | $SE(3)$ 결과와 unaligned 결과 |
| Global localization | 지정된 map frame pose를 출력 | None 또는 independently known transform만 적용한 ATE | Aligned ATE로 trajectory 내부 shape 확인 |
| Monocular trajectory | Global scale이 관측되지 않음 | Protocol에 명시한 $Sim(3)$ 또는 scale-aware metric | $SE(3)$ result와 fitted scale |
| Metric LiDAR·stereo·inertial trajectory | Scale을 추정해야 함 | $SE(3)$ 이하의 필요한 gauge만 정렬 | Fitted scale을 보정하지 않고 별도 보고 |

표는 일반적인 출발점이며 estimator specification보다 우선하지 않는다. 예를 들어
LiDAR-inertial system이라도 externally supplied global heading을 사용한다면 yaw를
alignment로 제거하는 것이 실제 heading error를 숨길 수 있다.

## Aligned와 unaligned 결과가 답하는 질문

Aligned와 unaligned 결과는 같은 metric의 단순한 두 버전이 아니라 서로 다른 질문에
답한다.

### Aligned ATE

허용한 global 자유도를 제거한 뒤 trajectory shape와 global consistency가 얼마나
정확한지를 본다.

$$
\operatorname{ATE}_{\mathrm{aligned}}
\left(
\{T_i^{GT}\},
\{T_{\mathrm{align}}T_i^{est}\}
\right)
$$

### Unaligned ATE

Global initialization과 frame agreement까지 포함한 end-to-end 차이를 본다.

$$
\operatorname{ATE}_{\mathrm{raw}}
\left(
\{T_i^{GT}\},
\{T_i^{est}\}
\right)
$$

다만 estimator frame이 원래 arbitrary라면 raw ATE의 큰 값은 localization failure가
아니라 단순 gauge 차이일 수 있다. Unaligned 결과를 absolute localization accuracy로
해석하려면 두 output이 같은 global frame을 사용해야 한다는 계약이 먼저 있어야 한다.

### RPE

Relative pose error(RPE)는 두 시각 사이의 relative motion을 비교한다.

$$
\Delta T_{ij}
:=
T_i^{-1}T_j
$$

모든 pose에 같은 global left transform을 적용하면 relative motion에서 소거되므로,
RPE는 constant global frame 차이보다 local motion error와 drift를 보는 데 적합하다.
그러나 timestamp association, delta와 pose relation convention은 여전히 동일해야
한다.

## 두 결과를 모두 보고하는 이유

Aligned와 unaligned 결과를 모두 남기는 것은 어느 것이 맞는지 판단을 포기한다는
뜻이 아니다.

- Primary metric은 estimator contract에 따라 미리 선택한다.
- Secondary result는 primary alignment가 숨긴 차이를 진단한다.
- Alignment mode와 fit 구간을 공개해 서로 다른 benchmark를 같은 이름으로 비교하지 않는다.

Odometry benchmark에서는 aligned ATE가 primary이고 unaligned result가 diagnostic일
수 있다. Global localization benchmark에서는 반대로 unaligned ATE가 primary이고
aligned ATE가 path-shape diagnostic일 수 있다.

## Known transform과 fitted alignment를 구분한다

Dataset의 source frame을 canonical frame으로 바꾸는 calibration transform과 GT를
사용해 residual을 최소화한 fitted transform은 출처가 다르다.

- Known transform: sensor calibration, TF tree, dataset metadata처럼 평가 pair와 독립적으로 정해진다.
- Fitted alignment: GT·estimate correspondence를 사용해 evaluation 단계에서 계산한다.

같은 matrix 형태라도 known transform 적용은 data representation을 맞추는 과정이고,
fitted alignment는 metric에서 특정 자유도를 제거하는 과정이다. Manifest와 result에
각 transform의 source를 구분해 기록해야 한다.

## Fit 구간도 metric의 일부다

### Full-trajectory alignment

전체 associated pair로 하나의 transform을 fit한다. 평균 residual을 가장 작게 만들기
때문에 trajectory shape 비교에는 유용하지만 evaluation 구간 전체의 GT를 사용한다.

### Initial-pose alignment

첫 pose만 맞춘다. 이후 drift를 그대로 남기지만 첫 measurement noise와 initialization
오류에 민감하다.

### Calibration-prefix alignment

초기 구간에서 transform을 fit하고 나머지 holdout 구간을 평가한다. Alignment
parameter fitting과 evaluation data를 분리해야 하는 benchmark에 사용할 수 있다.

### Segment별 alignment

각 짧은 segment를 별도로 맞추면 accumulated drift 일부가 제거된다. Full-trajectory
ATE와 다른 metric이므로 같은 이름으로 보고하면 안 된다.

## Data를 본 뒤 alignment를 선택하지 않는다

다음 절차는 benchmark result를 유리하게 만드는 선택 편향을 만든다.

1. None, 4-DoF, $SE(3)$와 $Sim(3)$를 모두 실행한다.
2. 가장 작은 error를 주는 mode를 고른다.
3. 선택한 결과만 최종 성능으로 보고한다.

대신 estimator input과 observability를 근거로 mode를 먼저 고정하고 다음 항목을
config와 result에 남긴다.

- Alignment mode와 fitted degree of freedom
- Scale correction 허용 여부
- Fit에 사용한 pair와 time range
- Association tolerance와 offset
- Fitted transform과 fitted scale
- Primary·diagnostic metric label

## Alignment는 estimator output 수정이 아니다

Aligned trajectory는 evaluator가 metric을 계산하기 위해 만든 derived data다.
Estimator가 실제로 출력한 raw trajectory를 덮어쓰거나 다음 run의 input으로 되돌리면
독립적인 평가가 아니다.

Deployment에서 필요한 것이 global map pose라면 evaluator의 post-hoc alignment가
아니라 estimator가 사용할 global measurement, initialization 또는 map-registration
절차를 system design에 포함해야 한다.

## 평가 정책 체크리스트

- [ ] Estimator output world frame이 arbitrary인지 지정된 map인지 적었다.
- [ ] Sensor·prior가 관측하는 translation, rotation과 scale 자유도를 적었다.
- [ ] Known coordinate conversion과 GT-fitted alignment를 구분했다.
- [ ] Primary alignment mode를 result를 보기 전에 정했다.
- [ ] Full, initial, prefix 또는 segment 중 fit 범위를 적었다.
- [ ] Aligned와 unaligned 결과에 서로 다른 의미의 label을 붙였다.
- [ ] Metric-scale system에 $Sim(3)$ correction을 적용하지 않았다.
- [ ] Fitted trajectory가 estimator output이나 다음 run input으로 되돌아가지 않는다.

## 관련 문서

- [Pose Trajectory Coordinate, Time and Alignment](<./01 Pose Trajectory Coordinate Time and Alignment.md>)
- [Rigid Transformation and SE(3)](<../../01 math/08 Geometry/23 Rigid Transformation and SE(3).md>)
- [Rigid Point Set Alignment with Kabsch and Umeyama](<../../01 math/08 Geometry/24 Rigid Point Set Alignment with Kabsch and Umeyama.md>)
- [Least Squares Problem](<../../01 math/07 Linear Algebra/06 Matrix Subspaces and Approximation/38 Least Squares Problem.md>)

## References

- Jürgen Sturm et al., “A Benchmark for the Evaluation of RGB-D SLAM Systems,” 2012
- Zichao Zhang and Davide Scaramuzza, “A Tutorial on Quantitative Trajectory Evaluation for Visual(-Inertial) Odometry,” 2018
- Shinji Umeyama, “Least-Squares Estimation of Transformation Parameters Between Two Point Patterns,” 1991
