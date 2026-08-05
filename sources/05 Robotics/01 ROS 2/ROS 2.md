# ROS 2

## 한 줄 요약

ROS 2(Robot Operating System 2)는 robot application을 여러 software component로 나누어 개발하고, component 사이의 통신·실행·관찰을 지원하는 library와 tool의 집합이다.

## ROS 2의 역할

이름에 operating system이 들어가지만 ROS 2 자체가 Linux나 Windows를 대체하는 operating system은 아니다. ROS 2 program은 Ubuntu 같은 기존 operating system 위에서 process로 실행된다.

ROS 2가 제공하는 주요 역할은 다음과 같다.

- Robot 기능을 package와 executable 단위로 구성한다.
- 분산된 software component가 message를 주고받게 한다.
- 실행 중인 component와 통신 관계를 ROS graph로 관찰한다.
- build, 실행, interface 확인과 진단에 공통 command-line tool을 제공한다.

ROS 2는 여러 package를 검증된 조합으로 묶어 `distribution`으로 배포한다. Distribution은 언어의 문법이 아니라 ROS 2 package 집합의 version 기준이다. 이 문서군의 명령 예시는 ROS 2 Jazzy Jalisco와 Ubuntu 24.04를 기준으로 하지만, node와 topic 같은 핵심 개념은 특정 distribution에만 한정되지 않는다.

## ROS graph

`ROS graph`는 실행 중인 ROS 2 system에서 node와 그 통신 관계를 나타낸다. 같은 graph에 참여한 node들은 discovery를 통해 서로를 찾는다.

```text
/camera_node ── image topic ──> /detector_node ── detection topic ──> /planner_node
```

`node`는 ROS graph에 참여하는 논리적인 계산 단위다. 하나의 operating system process가 node 하나를 실행하는 구성이 흔하지만, node와 process는 같은 개념이 아니다. 한 process가 여러 node를 포함할 수도 있고 node들이 서로 다른 computer에서 실행될 수도 있다.

Node 사이의 대표적인 상호작용 방식은 다음과 같다.

| 방식 | 관계 | 적합한 용도 |
|---|---|---|
| topic | publisher가 stream을 보내고 subscriber가 받는다. | sensor data, 주기적인 상태 |
| service | client가 request를 보내고 server가 response를 반환한다. | 짧게 끝나는 질의나 명령 |
| action | goal을 보낸 뒤 feedback과 result를 받고 취소할 수 있다. | 시간이 걸리는 작업 |
| parameter | node가 자신의 설정값을 이름과 값으로 관리한다. | 실행 중 확인하거나 바꿀 설정 |

이 가운데 node와 topic의 관계 및 실행 가능한 C++ 예제는 [Node and Topic](<./02 Node and Topic.md>)에서 설명한다.

## Package와 workspace

`package`는 관련 source code, build 설정, metadata와 실행 자원을 묶는 ROS 2의 기본 배포 단위다. C++ package는 보통 `ament_cmake`를 build system으로 사용하고 `package.xml`과 `CMakeLists.txt`에 dependency와 build 방법을 기록한다.

`workspace`는 하나 이상의 ROS 2 package를 source에서 함께 개발하고 build하는 directory다. `colcon`은 workspace의 package dependency를 읽어 정해진 순서로 build하는 command-line tool이다.

설치된 ROS 2를 기반 환경으로 사용하고 source에서 build한 workspace를 그 위에 추가할 수 있다. 이때 기반 환경을 `underlay`, 그 위에 추가하는 workspace 환경을 `overlay`라고 한다.

설치, shell environment, underlay/overlay와 workspace 산출물의 관계는 [Environment and Workspace](<./01 Environment and Workspace.md>)에서 설명한다.

## 문서 지도

- [Environment and Workspace](<./01 Environment and Workspace.md>)
  - ROS 2 distribution 설치와 현재 shell 활성화
  - package, workspace, `rosdep`, `colcon`
  - underlay와 overlay
- [Node and Topic](<./02 Node and Topic.md>)
  - node, publisher, subscriber와 topic
  - 최소 C++ package 작성, build, 실행과 graph 관찰
- [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)
  - executable, process, node와 실행 주체의 관계
  - executor의 `spin()`, RMW와 DDS의 역할
  - signal을 통한 정상 종료와 process 수명 관리
- [Coordinate Frames and TF2](<./04 Coordinate Frames and TF2.md>)
  - coordinate frame, transform과 ROS coordinate convention
  - source·target과 parent·child 방향 구분
  - broadcaster, listener, buffer와 TF tree
  - static·dynamic transform의 기본 구분과 static transform 검증
  - runtime TF tree와 저장된 diagram snapshot의 차이
- [URDF and Robot State Publisher](<./05 URDF and Robot State Publisher.md>)
  - 실제 robot 구조와 URDF link·joint의 대응
  - 고정된 URDF model과 시간에 따라 변하는 joint state의 결합
  - fixed와 movable joint의 `/tf_static`·`/tf` publish 규칙
  - `robot_description`과 `robot_state_publisher`
  - package 설치, launch와 TF tree 검증
- [Dynamic TF and Mobile Robot Frames](<./06 Dynamic TF and Mobile Robot Frames.md>)
  - sensor measurement, state estimate와 transform의 구분
  - driver, `joint_state_broadcaster`, `robot_state_publisher`, `diff_drive_controller`의 책임
  - `map`, `odom`, `base_link`의 역할과 localization 보정
  - timestamp별 transform, buffer 보간과 extrapolation error
- [PointCloud2 and RViz2](<./07 PointCloud2 and RViz2.md>)
  - PointCloud2의 binary layout, frame과 timestamp
  - C++ 합성 point cloud 작성과 topic 확인
  - TF와 PointCloud2를 RViz2에서 확인하는 절차와 문제 진단

## References

- [ROS 2 Documentation - Jazzy Jalisco](https://docs.ros.org/en/jazzy/)
- [ROS 2 Documentation - About Nodes](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Basic/About-Nodes.rst)
- [ROS 2 Documentation - About Topics](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Basic/About-Topics.rst)
- [ROS 2 Documentation - Creating a Workspace](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.rst)
- [ROS 2 Documentation - Creating Your First ROS 2 Package](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.rst)
- [ROS 2 Documentation - Release Jazzy Jalisco](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Releases/Release-Jazzy-Jalisco.rst)
