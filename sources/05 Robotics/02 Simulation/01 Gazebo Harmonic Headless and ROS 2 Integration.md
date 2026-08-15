# Gazebo Harmonic Headless and ROS 2 Integration

## 한 줄 요약

Gazebo Harmonic simulation을 재현 가능하게 검증하려면 SDF world와 load할 system을
명시하고, GUI 없는 server에서 simulation time과 runtime statistics를 확인한 뒤,
Gazebo Transport message가 bridge를 거쳐 ROS 2 message로 실제 전달되는지 검사한다.

## Gazebo Harmonic과 구성 요소

`Gazebo`는 robot과 환경의 dynamics, sensor와 3D scene을 simulation하는 software
project다. `Harmonic`은 Gazebo의 8번째 major release이며 장기 지원 release다.
Harmonic collection은 Gazebo Sim 8, Gazebo Transport 13, SDFormat 14처럼 함께
호환되는 library release를 묶는다.

Gazebo를 실행하고 ROS 2와 연결할 때 다음 구성 요소는 서로 다른 역할을 한다.

| 구성 요소 | 범주 | 역할 |
|---|---|---|
| Gazebo Sim | simulator | world를 load하고 system을 실행하며 simulation state를 갱신한다. |
| SDF | XML 기반 description format | world, model, link, collision, visual, physics 설정을 선언한다. |
| System plugin | Gazebo Sim extension | physics 계산, command 처리, scene state publish 같은 runtime 기능을 제공한다. |
| Gazebo Transport | Gazebo middleware | Gazebo process 사이에서 topic과 service를 전달한다. |
| `ros_gz_bridge` | ROS 2–Gazebo adapter | 지원되는 Gazebo message와 ROS 2 message를 변환해 두 graph를 연결한다. |

`gz sim`은 Gazebo Sim을 실행하는 command다. 이 이름은 Gazebo Classic의
`gazebo`·`gzserver` command와 구분해야 한다. Harmonic 문서나 package에서는 과거
`Ignition` 이름이 남은 자료를 볼 수 있지만, Harmonic의 현재 command와 namespace는
`gz`를 사용한다.

## SDF world와 runtime system

`SDF`(Simulation Description Format)는 simulation world와 model을 표현하는 XML
format이다. 최소 world는 `<sdf>` 아래에 이름 있는 `<world>`를 두고, physics와
필요한 system plugin을 선언할 수 있다.

```xml
<?xml version="1.0"?>
<sdf version="1.8">
  <world name="simple_world">
    <physics name="1ms" type="ignored">
      <max_step_size>0.001</max_step_size>
      <real_time_factor>1.0</real_time_factor>
    </physics>

    <plugin filename="gz-sim-physics-system"
            name="gz::sim::systems::Physics"/>
    <plugin filename="gz-sim-user-commands-system"
            name="gz::sim::systems::UserCommands"/>
    <plugin filename="gz-sim-scene-broadcaster-system"
            name="gz::sim::systems::SceneBroadcaster"/>
  </world>
</sdf>
```

`<max_step_size>`는 한 simulation update가 진행할 수 있는 최대 simulation time
간격이다. 값이 작아지면 더 촘촘하게 state를 갱신하지만 같은 simulated duration을
계산하는 데 더 많은 연산이 필요할 수 있다. `<real_time_factor>`는 simulation time을
wall time에 어느 비율로 맞출지 정하는 목표다.

System plugin은 SDF data 그 자체가 아니다. Physics plugin은 dynamics update를
수행하고, UserCommands plugin은 entity 생성·이동·삭제 같은 command service를
제공하며, SceneBroadcaster plugin은 scene state를 다른 process에 전달한다. World에
plugin을 명시하면 사용자의 default server configuration에 어떤 plugin이 들어
있는지에 덜 의존하는 실행 fixture를 만들 수 있다.

외부 model URI를 SDF에 넣으면 첫 실행에 Gazebo Fuel download가 필요할 수 있다.
Network와 cache 상태에 독립적인 smoke test가 목적이라면 box·plane 같은 geometry를
world file 안에 직접 기술하거나 version-controlled local resource를 사용한다.

## Server-only와 headless rendering은 다르다

GUI 없이 Gazebo Sim server만 실행할 때는 `-s` option을 사용한다. `-r`은 world를
load한 뒤 paused 상태로 기다리지 않고 simulation을 실행한다.

```bash
gz sim -s -r simple_world.sdf
```

이 실행에는 GUI client와 3D view가 없지만 physics와 Gazebo Transport는 동작할 수
있다. 따라서 display를 열 수 없는 CI나 remote shell에서 world load, clock과
physics 진행을 검사할 수 있다.

`--headless-rendering`은 server-only의 다른 철자가 아니다. Camera나 GPU LiDAR처럼
server 쪽 rendering scene이 필요한 sensor를 display server 없이 EGL로 실행하는
option이다. Rendering sensor가 없는 physics-only world는 `-s`만으로 GUI 의존성을
제거할 수 있다. 나중에 rendering sensor를 추가할 때는 지원 rendering engine과
GPU·EGL 환경을 별도로 검증한다.

## Simulation time, wall time과 RTF

`wall time`은 실행 중인 computer의 실제 경과 시간이고, `simulation time`은
simulator가 계산해 진행시킨 가상 시간이다. Simulation이 pause되면 wall time은
흐르지만 simulation time은 멈춘다.

`real-time factor`(RTF)는 일정 구간에서 simulation time 증가량을 wall time
증가량으로 나눈 값이다.

```text
RTF = Δ simulation time / Δ wall time
```

- RTF가 `1.0`이면 simulation time이 실제 시간과 비슷한 속도로 진행된다.
- RTF가 `0.5`이면 wall time 2초 동안 simulation time이 약 1초 진행된다.
- RTF가 `2.0`이면 wall time 1초 동안 simulation time이 약 2초 진행된다.

SDF의 `<real_time_factor>`는 목표 설정이고 관측된 RTF는 runtime 결과다. 계산량이
많거나 host가 바쁘면 실제 RTF가 목표보다 낮을 수 있다. 반대로 update rate를
제한하지 않은 simulation은 실제 시간보다 빠르게 진행될 수 있다. 따라서 설정
file을 읽는 것만으로 성능을 판정하지 않고 world statistics message의
`sim_time`, `real_time`, `iterations`, `real_time_factor`를 실행 중 확인한다.

World 이름이 `simple_world`라면 statistics는 일반적으로 다음 topic에서 확인할 수
있다.

```bash
gz topic -e -t /world/simple_world/stats
```

Statistics 한 개만으로는 시작 순간의 transient인지 지속 상태인지 구분하기
어렵다. Smoke test에서는 여러 sample을 수집해 simulation time과 iteration이
증가하는지 확인하고, RTF sample 수와 대표 통계를 함께 기록한다.

## Gazebo Transport와 ROS graph의 경계

Gazebo Transport와 ROS 2 middleware는 별도의 discovery와 message type system을
사용한다. Gazebo topic이 존재한다고 해서 같은 이름의 ROS 2 topic이 자동으로
생기지 않는다. `ros_gz_bridge` process는 양쪽 middleware에 참여해 지원되는 message
type 사이를 변환한다.

Simulation clock을 Gazebo에서 ROS 2로만 전달하는 예시는 다음과 같다.

```bash
source /opt/ros/jazzy/setup.bash
ros2 run ros_gz_bridge parameter_bridge \
  '/clock@rosgraph_msgs/msg/Clock[gz.msgs.Clock'
```

Bridge argument는 topic, ROS message type과 Gazebo message type을 함께 지정한다.
첫 type separator가 `[`이면 Gazebo에서 ROS 2로 전달하는 단방향 bridge다. `@`는
양방향, `]`는 ROS 2에서 Gazebo 방향을 뜻한다. Clock은 simulator가 기준 source가
되어야 하므로 보통 Gazebo→ROS 2 단방향으로 연결한다.

ROS 2 node가 simulation time을 사용하려면 `/clock` 수신 외에도 해당 node의
`use_sim_time` parameter가 `true`여야 한다. `/clock`을 bridge했다는 사실이 모든
node의 time source를 자동으로 바꾸지는 않는다.

Gazebo는 같은 Transport partition에서 다른 `/clock` publisher를 발견하면 충돌을
피하려고 world-qualified clock topic만 만들 수 있다. 반복 시험에서는 기존
publisher가 섞이지 않았는지 확인하고 실제 Gazebo topic 이름에 맞춰 bridge한다.

## 두 middleware의 실행 격리

`ROS_DOMAIN_ID`는 ROS 2 discovery domain을 나누지만 Gazebo Transport에는 적용되지
않는다. Gazebo Transport node는 `GZ_PARTITION` 값이 같은 process끼리 topic과
service를 공유한다. 따라서 독립된 시험은 두 값을 각각 설정하고, Gazebo server,
`gz topic`과 bridge process에는 같은 `GZ_PARTITION`을 전달해야 한다.

```bash
export ROS_DOMAIN_ID=42
export GZ_PARTITION=gazebo_smoke_42
```

두 값은 namespace prefix가 아니라 각 middleware의 discovery 경계를 선택한다.
Bridge는 두 middleware에 동시에 참여하므로 ROS domain과 Gazebo partition을 모두
같은 실행 구성에 맞춰야 한다.

## 최소 검증 흐름

화면에 world가 보이는 것만으로 physics, time과 ROS 연결이 모두 검증되지는 않는다.
GUI 없는 자동 검증은 다음 책임을 분리한다.

1. `gz sim -s -r`로 local SDF world를 load한다.
2. Gazebo clock과 world statistics topic이 나타나는지 확인한다.
3. Statistics에서 pause가 해제됐고 simulation time·iteration이 증가하는지 확인한다.
4. 여러 RTF sample을 수집해 유한한 양수 값과 대표 통계를 기록한다.
5. Gazebo→ROS 2 `/clock` bridge를 시작한다.
6. ROS 2에서 여러 Clock message를 받아 timestamp가 감소하지 않고 실제로
   증가하는지 확인한다.
7. 시작한 server와 bridge만 정상 종료하고 종료 code와 log를 보존한다.

이 검증은 world의 물리 정확도, sensor realism이나 장시간 resource 안정성을
보장하지 않는다. 각각은 reference motion·measurement 비교와 장시간 실행처럼
별도 완료 기준이 필요하다.

장시간 대표 구성에서 기능적 진행, process memory와 종료를 함께 관찰하는 방법은
[Long-Run Stability and Resource Observation](<./02 Long-Run Stability and Resource Observation.md>)에서
설명한다.

## 설치 조합

Ubuntu 24.04와 ROS 2 Jazzy에서는 Gazebo Harmonic이 권장 조합이다. ROS package
repository가 준비된 환경에서는 다음 meta-package가 해당 ROS distribution과
호환되는 Gazebo library와 `ros_gz` package를 설치한다.

```bash
sudo apt install ros-jazzy-ros-gz
```

설치 후에는 package file 존재만 확인하지 말고 새 shell에서 ROS environment를
활성화한 뒤 `gz sim --versions`, `ros2 pkg executables ros_gz_bridge`와 최소 world
실행으로 조합을 확인한다.

## References

- [Gazebo Harmonic](https://gazebosim.org/docs/harmonic/install/)
- [Installing Gazebo with ROS](https://gazebosim.org/docs/harmonic/ros_installation/)
- [SDF worlds](https://gazebosim.org/docs/harmonic/sdf_worlds/)
- [Gazebo Sim 8 Server Configuration](https://gazebosim.org/api/sim/8/server_config.html)
- [Gazebo Sim 8 Pause and Run Simulation](https://gazebosim.org/api/sim/8/pause_run_simulation.html)
- [Gazebo Sim Headless Rendering](https://gazebosim.org/api/sim/8/headless_rendering.html)
- [Use ROS 2 to interact with Gazebo](https://gazebosim.org/docs/harmonic/ros2_integration/)
- [ros_gz_bridge Jazzy documentation](https://docs.ros.org/en/jazzy/p/ros_gz_bridge/)
- [Gazebo Transport Environment Variables](https://gazebosim.org/api/transport/13/envvars.html)
