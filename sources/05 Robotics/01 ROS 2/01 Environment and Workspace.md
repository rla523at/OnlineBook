# ROS 2 Environment and Workspace

## 한 줄 요약

ROS 2 개발 환경은 computer에 설치된 file, 현재 shell에 활성화한 environment, source package를 build한 workspace를 구분해야 정확히 구성하고 검증할 수 있다.

## 세 종류의 상태

ROS 2 command가 동작하지 않을 때는 먼저 어느 상태가 준비되지 않았는지 구분한다.

| 상태 | 의미 | 대표 확인 방법 |
|---|---|---|
| installation | ROS 2 package와 실행 파일이 filesystem에 설치되어 있다. | `apt-cache policy ros-jazzy-desktop` |
| shell environment | 현재 shell의 command 검색 경로와 ROS 관련 environment variable이 구성되어 있다. | `printenv \| grep '^ROS_'` |
| workspace | source package와 build 결과가 workspace directory에 존재한다. | `colcon list`, `ls build install log` |

Package가 설치되어 있어도 새 shell이 ROS 2 setup file을 읽지 않았다면 `ros2` command를 찾지 못할 수 있다. 반대로 현재 shell에서 command가 실행된다는 사실만으로 source workspace가 build되었다고 판단할 수도 없다.

## Distribution과 operating system

ROS 2 `distribution`은 함께 release하고 검증한 ROS 2 package 집합의 version 기준이다. Ubuntu `distribution`은 Linux kernel과 system program, package manager를 함께 배포하는 operating system 계열이다. 둘 다 distribution이라는 말을 사용하지만 서로 다른 대상을 가리킨다.

이 문서의 명령 예시는 다음 조합을 사용한다.

```text
ROS 2 distribution : Jazzy Jalisco
Ubuntu release      : Ubuntu 24.04 LTS (Noble)
shell               : Bash
```

ROS 2 Jazzy의 Debian package는 Ubuntu 24.04를 Tier 1 platform으로 지원한다. 다른 ROS 2 또는 Ubuntu version에서는 package 이름과 지원 상태를 해당 distribution의 공식 문서에서 다시 확인한다.

## Jazzy 설치 예시

다음 명령은 Ubuntu 24.04에서 ROS 2의 공식 APT repository 등록을 완료한 뒤 실행하는 예시다. Repository key와 source 등록 과정은 보안 방식이나 package가 바뀔 수 있으므로 [공식 Ubuntu deb 설치 문서](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Installation/Ubuntu-Install-Debs.rst)의 Jazzy 절차를 따른다.

```bash
sudo apt update
sudo apt install ros-jazzy-desktop ros-dev-tools
```

- `apt update`는 등록된 repository의 package index를 갱신한다. ROS 2 package 자체를 설치하지는 않는다.
- `ros-jazzy-desktop`은 ROS 2 core, RViz, demo와 tutorial을 포함한 desktop variant를 설치한다.
- GUI tool이 필요 없다면 더 작은 `ros-jazzy-ros-base` variant를 선택할 수 있다. 두 variant를 동시에 설치할 필요는 없다.
- `ros-dev-tools`는 `colcon`, `rosdep` 등 ROS 2 source 개발에 필요한 tool을 설치한다.

공식 설치 문서는 `ros-dev-tools`를 설치할 때 Ubuntu의 `noble-updates`와 `noble-backports` repository가 활성화되어 있어야 한다고 안내한다. 현재 설정은 다음처럼 읽기 전용으로 확인할 수 있다.

```bash
grep -R --no-filename '^Suites:' /etc/apt/sources.list.d/ubuntu.sources
```

설치 후보와 실제 설치 상태는 다음 명령으로 확인한다.

```bash
apt-cache policy ros-jazzy-desktop ros-dev-tools
```

`Installed:` 값은 filesystem에 package가 설치되었는지를 보여준다. 이 확인은 현재 shell에 ROS 2 environment를 활성화하지 않는다.

## Setup file과 현재 shell

APT로 설치한 Jazzy의 기본 setup file은 `/opt/ros/jazzy/setup.bash`다.

```bash
source /opt/ros/jazzy/setup.bash
```

`source`는 setup file의 명령을 별도 process가 아니라 현재 Bash에서 실행한다. 그 결과 현재 shell의 `PATH`, package 검색 경로와 ROS 관련 environment variable이 갱신된다. 설치 파일을 복사하거나 수정하는 명령은 아니다.

활성화 결과는 다음처럼 확인한다.

```bash
command -v ros2
printenv | grep -E '^(ROS_DISTRO|ROS_VERSION|ROS_PYTHON_VERSION)='
```

Jazzy 환경이라면 `ROS_DISTRO=jazzy`, `ROS_VERSION=2`가 포함된다. 이 값은 현재 shell 상태이므로 해당 shell을 종료하면 사라진다. 새 shell에서도 사용하려면 그 shell에서 setup file을 다시 source해야 한다.

Shell process, environment variable, startup file과 `source`의 일반 원리는 [Shell Environment](<../../03 programming/05 Linux/Shell Environment.md>)를 참고한다. Windows에서 Ubuntu를 실행한다면 filesystem과 WSL lifecycle은 [WSL](<../../03 programming/05 Linux/WSL.md>)을 참고한다.

### Startup file 자동화의 주의점

다음 줄을 Bash startup file에 넣으면 해당 file을 읽는 새 shell이 Jazzy를 자동으로 활성화할 수 있다.

```bash
source /opt/ros/jazzy/setup.bash
```

그러나 여러 ROS 2 distribution을 번갈아 사용한다면 자동 source가 이전 environment를 숨기거나 섞이게 만들 수 있다. 처음에는 각 terminal에서 명시적으로 source하고, 자동화가 필요할 때 어떤 startup file이 실행되는지 확인한 뒤 한 곳에서 관리하는 편이 안전하다.

## Workspace, package와 build tool

`workspace`는 함께 개발할 ROS 2 package를 담고 build하는 directory다. 다음은 일반적인 workspace 구조다.

```text
ros2_ws/
├─ src/       # 직접 작성하거나 내려받은 package source
├─ build/     # package별 중간 build file
├─ install/   # 실행 파일, library, setup file
└─ log/       # colcon 실행 기록
```

처음에는 `src`만 직접 만든다.

```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
```

`mkdir -p`는 home directory 아래에 `ros2_ws/src` directory가 없으면 만든다. `cd`는 현재 shell의 working directory만 바꾼다. `build`, `install`, `log`는 아직 생기지 않는다.

`package`는 관련 source code, dependency metadata와 build 설정을 묶는 ROS 2의 기본 단위다. C++ package의 최소 구조는 보통 다음과 같다.

```text
example_package/
├─ CMakeLists.txt
├─ package.xml
├─ include/example_package/
└─ src/
```

- `package.xml`은 package 이름, version, maintainer, license와 dependency를 선언한다.
- `CMakeLists.txt`는 C++ source를 어떤 executable과 library로 build하고 어디에 설치할지 선언한다.
- `ament_cmake`는 CMake 기반 ROS 2 package에 사용하는 build system이다.
- `colcon`은 workspace 안의 package를 찾아 dependency 순서에 따라 build하는 build tool이다.

Package directory 안에 다른 ROS package를 중첩하지 않는다. `colcon`이 package 경계를 잘못 해석할 수 있으므로 여러 package는 `src` 바로 아래의 형제 directory로 둔다.

## Dependency 준비와 build

먼저 현재 shell에 underlay를 활성화한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
```

여기서 APT로 설치한 Jazzy 환경은 source workspace가 의존하는 기반이므로 `underlay`다.

`rosdep`은 각 package의 `package.xml`을 읽고, 현재 platform에서 빠진 system dependency를 package manager를 통해 설치한다.

```bash
rosdep install -i --from-path src --rosdistro jazzy -y
```

- `--from-path src`는 `src` 아래 package의 dependency를 조사한다.
- `-i`는 workspace source에 이미 있는 package를 dependency 설치 대상에서 제외한다.
- `--rosdistro jazzy`는 dependency rule을 Jazzy 기준으로 해석한다.
- `-y`는 system package 설치 확인에 자동으로 동의한다.

이 명령은 source code를 compile하지 않는다. 처음 사용하는 system이라 `rosdep` database가 초기화되지 않았다면 공식 `rosdep` 설치 절차에 따라 초기화와 update를 먼저 수행한다.

Workspace root에서 build한다.

```bash
colcon build
```

성공하면 workspace root에 `build`, `install`, `log` directory가 생성된다. Source package는 `src`에 그대로 있고 실행에 필요한 결과는 `install`에 모인다.

특정 package와 필요한 dependency만 build하려면 다음처럼 선택할 수 있다.

```bash
colcon build --packages-up-to example_package
```

`--packages-up-to`는 지정한 package와 그 package가 의존하는 workspace package를 함께 build한다. Package 하나만 선택하고 dependency가 이미 build되어 있다고 확신할 때는 `--packages-select`를 사용할 수 있다.

## Underlay와 overlay

Build가 끝난 workspace를 현재 shell에 추가하면 그 workspace는 `overlay`가 된다. 같은 package가 underlay와 overlay 양쪽에 있으면 overlay의 package가 먼저 선택된다.

Build에 사용한 shell과 분리된 새 terminal을 열고 다음 순서로 활성화한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
```

공식 tutorial은 build에 사용한 terminal에서 바로 overlay를 source하면 복잡한 문제가 생길 수 있으므로 새 terminal을 사용하도록 권장한다.

Workspace의 두 setup file은 포함 범위가 다르다.

| File | 현재 shell에 추가하는 범위 |
|---|---|
| `install/local_setup.bash` | 현재 workspace의 package만 추가한다. |
| `install/setup.bash` | build할 때 연결된 underlay를 먼저 불러오고 현재 workspace도 추가한다. |

Underlay를 명시적으로 source한 뒤 `local_setup.bash`를 source해도 같은 층을 직접 구성할 수 있다. 일상적인 실행에서는 누락을 줄이기 위해 새 shell에서 underlay와 `install/setup.bash`를 순서대로 명시하는 방식이 이해하기 쉽다.

```text
Ubuntu filesystem
└─ /opt/ros/jazzy          underlay
   └─ ~/ros2_ws/install    overlay
```

## 새 shell에서 검증

현재 shell에서만 우연히 남아 있는 environment에 의존하지 않는지 확인하려면 terminal session을 완전히 종료하고 새 terminal을 연다. 기존 shell에서 `bash`만 실행하면 parent environment를 상속하므로 깨끗한 검증이 아닐 수 있다.

새 terminal에서 다음 순서로 확인한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash

printf 'ROS_DISTRO=%s\n' "$ROS_DISTRO"
command -v ros2
colcon list
ros2 pkg list
```

- `ROS_DISTRO`와 `command -v`는 현재 shell에 Jazzy command 환경이 구성되었는지 확인한다.
- `colcon list`는 source space에서 발견한 package를 보여준다.
- `ros2 pkg list`는 현재 environment에서 실행 대상으로 찾을 수 있는 package를 보여준다.

Source package가 `colcon list`에는 있지만 `ros2 pkg list`에는 없다면 build가 실패했거나 현재 shell에 올바른 `install/setup.bash`를 source하지 않았을 가능성이 있다.

## 자주 혼동하는 상태

| 관찰 | 실제 의미 |
|---|---|
| `apt install`이 성공했다. | File이 설치된 것이며 현재 shell environment까지 자동으로 바뀐 것은 아니다. |
| 현재 terminal에서 `ros2`가 실행된다. | 현재 shell이 ROS 2를 찾는다는 뜻이며 새 shell도 자동으로 찾는다는 뜻은 아니다. |
| `src`에 package가 있다. | Source가 있다는 뜻이며 executable이 build되었다는 뜻은 아니다. |
| `colcon build`가 성공했다. | `install`에 결과가 생겼다는 뜻이며 다른 shell이 그 overlay를 자동으로 찾는다는 뜻은 아니다. |
| `install/setup.bash`를 source했다. | 현재 shell의 검색 경로가 바뀐 것이며 source나 build 결과를 수정한 것은 아니다. |
| overlay의 package가 선택된다. | Underlay package가 삭제된 것이 아니라 현재 environment의 검색 우선순위가 overlay를 앞세운 것이다. |

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Node and Topic](<./02 Node and Topic.md>)
- [Shell Environment](<../../03 programming/05 Linux/Shell Environment.md>)
- [WSL](<../../03 programming/05 Linux/WSL.md>)

## References

- [ROS 2 Documentation - Installing ROS 2 via Debian Packages](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Installation/Ubuntu-Install-Debs.rst)
- [ROS 2 Documentation - Configuring Environment](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.rst)
- [ROS 2 Documentation - Creating a Workspace](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.rst)
- [ROS 2 Documentation - Creating Your First ROS 2 Package](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.rst)

