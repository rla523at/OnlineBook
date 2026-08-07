# ROS 2 Environment and Workspace

## 한 줄 요약

ROS 2 개발 환경은 computer에 설치된 file, 현재 shell에 활성화한 environment, workspace 안의 source package와 `colcon` build 산출물을 구분해야 정확히 구성하고 검증할 수 있다.

## 확인해야 할 세 가지 영역

ROS 2 command가 동작하지 않을 때는 installation, 현재 shell environment, workspace의 준비 여부를 각각 구분하여 확인한다.

`colcon`은 workspace의 package를 찾고 build 순서를 조정하는 command-line tool이다. 아래의 `colcon list`는 package를 build하지 않고 source에서 발견되는 package를 확인한다.

| 확인 대상 | 확인할 내용 | 대표 확인 방법 |
|---|---|---|
| ROS 2 installation | ROS 2 package와 실행 파일이 filesystem에 설치되어 있는가? | `apt-cache policy ros-jazzy-desktop` |
| current shell environment | 현재 shell에 command 검색 경로와 ROS 관련 environment variable이 구성되어 있는가? | `printenv \| grep '^ROS_'` |
| workspace | source package가 있으며 필요한 package가 build되었는가? | `colcon list`, `ls build install log` |

Package가 설치되어 있어도 새 shell이 ROS 2 environment setup script를 읽지 않았다면 `ros2` command를 찾지 못할 수 있다. 반대로 현재 shell에서 command가 실행된다는 사실만으로 workspace 안의 source package가 build되었다고 판단할 수도 없다.

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

## Environment setup script와 현재 shell

APT로 설치된 Jazzy를 현재 Bash 환경에서 사용할 수 있도록 활성화하는 script는 `/opt/ros/jazzy/setup.bash`다.

```bash
source /opt/ros/jazzy/setup.bash
```

`source`는 setup script의 명령을 별도 process가 아니라 현재 Bash에서 실행한다. 그 결과 현재 shell의 `PATH`, package 검색 경로와 ROS 관련 environment variable이 갱신된다. ROS 2 package를 설치하거나 설치된 file을 복사 또는 수정하는 명령은 아니다.

활성화 결과는 다음처럼 확인한다.

```bash
command -v ros2
printenv | grep -E '^(ROS_DISTRO|ROS_VERSION|ROS_PYTHON_VERSION)='
```

Jazzy 환경이라면 `ROS_DISTRO=jazzy`, `ROS_VERSION=2`가 포함된다. 이 값은 현재 shell 상태이므로 해당 shell을 종료하면 사라진다. 새 shell에서도 사용하려면 그 shell에서 setup script를 다시 source해야 한다.

Shell process, environment variable, startup file과 `source`의 일반 원리는 [Shell Environment](<../../03 programming/05 Linux/Shell Environment.md>)를 참고한다. Windows에서 Ubuntu를 실행한다면 filesystem과 WSL lifecycle은 [WSL](<../../03 programming/05 Linux/WSL.md>)을 참고한다.

### Startup file 자동화의 주의점

다음 줄을 Bash startup file에 넣으면 해당 file을 읽는 새 shell이 Jazzy를 자동으로 활성화할 수 있다.

```bash
source /opt/ros/jazzy/setup.bash
```

그러나 여러 ROS 2 distribution을 번갈아 사용한다면 자동 source가 이전 environment를 숨기거나 섞이게 만들 수 있다. 처음에는 각 terminal에서 명시적으로 source하고, 자동화가 필요할 때 어떤 startup file이 실행되는지 확인한 뒤 한 곳에서 관리하는 편이 안전하다.

## Workspace와 package

`workspace`는 함께 개발할 ROS 2 package를 담고 build하는 directory다. 다음은 일반적인 workspace 구조다.

```text
ros2_ws/
├─ src/       # 직접 작성하거나 내려받은 package source
├─ build/     # package별 중간 build file
├─ install/   # 실행 파일, library, resource, setup script
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

### 여러 package의 배치

`colcon`의 기본 package 탐색은 ROS package를 발견하면 그 하위 directory를 더 탐색하지 않는다. 따라서 package directory 안에 다른 ROS package를 중첩하지 않는다.

여러 package를 `src` 바로 아래에 형제 directory로 두는 방식이 가장 단순하다.

```text
ros2_ws/
└─ src/
   ├─ package_a/
   │  ├─ package.xml
   │  └─ CMakeLists.txt
   └─ package_b/
      ├─ package.xml
      └─ CMakeLists.txt
```

Repository나 기능 단위로 package를 묶어야 한다면 `package.xml`이 없는 grouping directory 아래에 형제 directory로 둘 수 있다. `colcon`은 grouping directory를 지나 하위 package를 계속 탐색한다.

```text
ros2_ws/
└─ src/
   └─ robot_stack/          # grouping directory: package.xml 없음
      ├─ package_a/
      │  ├─ package.xml
      │  └─ CMakeLists.txt
      └─ package_b/
         ├─ package.xml
         └─ CMakeLists.txt
```

다음처럼 `package_a` 안에 `package_b`를 두면 `package_a`를 발견한 시점에 하위 탐색이 끝나므로 `package_b`가 별도 package로 발견되지 않는다.

```text
ros2_ws/
└─ src/
   └─ package_a/
      ├─ package.xml
      ├─ CMakeLists.txt
      └─ package_b/         # 발견되지 않는 중첩 ROS package
         ├─ package.xml
         └─ CMakeLists.txt
```

이 제한은 다른 `package.xml`을 가진 ROS package를 중첩하는 경우에만 해당한다. Package 내부의 `src`, `include`, `launch`, `config` 같은 일반 directory는 자유롭게 둘 수 있다. Package 사이 dependency는 directory 중첩이 아니라 `package.xml`에 선언한다.

## Dependency 준비

먼저 현재 shell에 underlay를 활성화한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
```

여기서 APT로 설치한 Jazzy 환경은 workspace 안의 source package가 의존하는 기반이므로 `underlay`다.

`rosdep`은 각 package의 `package.xml`을 읽고, 현재 platform에서 빠진 system dependency를 package manager를 통해 설치한다.

```bash
rosdep install -i --from-path src --rosdistro jazzy -y
```

- `--from-path src`는 `src` 아래 package의 dependency를 조사한다.
- `-i`는 workspace의 `src`에 이미 있는 package를 dependency 설치 대상에서 제외한다.
- `--rosdistro jazzy`는 dependency rule을 Jazzy 기준으로 해석한다.
- `-y`는 system package 설치 확인에 자동으로 동의한다.

이 명령은 source code를 compile하지 않는다. 처음 사용하는 system이라 `rosdep` database가 초기화되지 않았다면 공식 `rosdep` 설치 절차에 따라 초기화와 update를 먼저 수행한다.

## `colcon build`

`colcon`은 workspace의 여러 software package를 찾아 정해진 순서로 처리하는 command-line build orchestration tool이다. Package별 build 규칙을 직접 제공하거나 source code를 직접 compile하지 않고, 각 package가 선언한 build 방식에 작업을 위임한다. 또한 build 결과를 준비할 뿐 executable이나 node를 실행하지 않는다.

Build 전에 현재 workspace에서 발견되는 package를 확인할 수 있다.

```bash
cd ~/ros2_ws
colcon list
```

`colcon list`는 발견한 package의 path, 이름과 type을 보여주며 package를 build하지 않는다. 의도한 package가 보이지 않으면 build보다 먼저 `src`의 directory 배치와 `package.xml`을 확인한다.

### `colcon`, `ament_cmake`, CMake와 compiler의 역할

ROS 2 workspace에는 서로 다른 build 방식을 사용하는 package가 함께 들어갈 수 있다. CMake 기반 ROS 2 package 하나가 build되는 역할 관계는 다음과 같다.

| 도구 | 적용 범위 | 역할 |
|---|---|---|
| `colcon` | workspace 전체 | package 검색, 대상 선택, dependency 순서 결정, package별 build 실행과 결과 관리 |
| `ament_cmake` | CMake 기반 ROS 2 package 하나 | CMake에서 사용할 ROS 2용 dependency, install과 export convention 제공 |
| CMake | CMake package 하나 | `CMakeLists.txt`를 읽고 configure, build와 install 단계 구성 |
| compiler와 linker | C++ target | source code를 object file로 compile하고 executable 또는 library로 link |

처리 관계를 단순화하면 다음과 같다.

```text
colcon build
├─ package 검색과 build 대상 선택
├─ package.xml의 dependency를 이용한 처리 순서 결정
└─ package에 선언된 build 방식 실행
   └─ ament_cmake package
      └─ CMake configure, build와 install
         ├─ compiler와 linker로 C++ target 생성
         └─ install rule에 따라 target과 resource 배치
```

Python package가 `ament_python`과 같은 다른 build 방식을 사용하더라도 package 검색, 대상 선택과 전체 build 순서 조정은 `colcon`이 담당한다.

### Build 대상 선택

Workspace root에서 `colcon build`를 실행한다. Option에 따라 이번 command가 처리할 package 집합이 달라진다.

| Command | 이번 build에서 처리하는 package |
|---|---|
| `colcon build` | workspace에서 발견한 모든 package |
| `colcon build --packages-select example_package` | 이름이 `example_package`인 package만 선택하며 dependency package를 자동으로 추가하지 않음 |
| `colcon build --packages-up-to example_package` | `example_package`와 그 package가 재귀적으로 의존하는 workspace package |

`--packages-select`를 사용할 때 선택한 package의 dependency는 현재 활성화한 underlay에 설치되어 있거나 이전 build 결과로 사용할 수 있어야 한다. Dependency의 source만 현재 workspace에 있고 아직 build하지 않았다면 `--packages-up-to` 또는 option 없는 전체 build를 사용한다. `--packages-up-to`도 underlay에만 있는 dependency를 다시 build하지는 않는다.

### Build 과정: compile과 install

여기서 build는 모든 file을 compile한다는 뜻이 아니라 package가 선언한 build와 install 절차 전체를 처리한다는 뜻이다.

- C++ executable이나 library target이 있으면 compiler와 linker가 binary를 만들고 install rule이 정한 위치에 배치한다.
- Python package는 선언된 module과 executable entry point를 install space에 배치한다.
- URDF, launch, configuration file은 compile하지 않는다. `CMakeLists.txt`의 `install(DIRECTORY ...)` 같은 rule에 따라 runtime에서 찾을 수 있도록 install space에 복사한다.

따라서 “URDF package를 build한다”는 표현은 URDF 자체를 compile한다는 뜻이 아니다. 해당 package의 build system을 실행하고 선언된 URDF와 launch file을 install space에 배치한다는 뜻이다.

### Build 산출물과 기본 install 구조

처음 만든 workspace에는 `src`만 있다. 기본 설정으로 `colcon build`를 실행하면 workspace root의 `build`, `install`, `log` directory를 사용하며 source file은 `src`에 그대로 둔다.

| Path | 역할 |
|---|---|
| `build/<package>/` | configure 결과, object file 등 package별 중간 build 산출물 |
| `install/<package>/` | 기본 isolated install에서 package별 실행 파일, library와 runtime resource를 두는 prefix |
| `install/setup.bash` | 연결된 underlay와 현재 install space를 Bash 환경에 추가하는 workspace setup script |
| `install/local_setup.bash` | underlay를 다시 불러오지 않고 현재 install space만 Bash 환경에 추가하는 setup script |
| `log/` | `colcon` command 실행과 package별 stdout, stderr 기록 |

다음은 이 문서 모음의 C++ executable package와 resource package를 함께 build했을 때 관련 부분만 단순화한 예시다. 기본 isolated install에서는 각 package가 `install/<package>` 아래에 별도 prefix를 갖는다.

```text
ros2_ws/
├─ build/
│  ├─ cpp_pubsub/
│  └─ sensor_description/
├─ install/
│  ├─ cpp_pubsub/
│  │  └─ lib/cpp_pubsub/
│  │     ├─ talker
│  │     └─ listener
│  ├─ sensor_description/
│  │  └─ share/sensor_description/
│  │     ├─ launch/
│  │     └─ urdf/
│  ├─ local_setup.bash
│  └─ setup.bash
└─ log/
```

`install/setup.bash`는 workspace directory를 처음 만들 때부터 존재하는 파일이 아니라 `colcon`이 install space를 준비하면서 생성하는 build 산출물이다. 이 문서 모음은 workspace root 아래의 기본 `install` 경로를 사용한다.

### Build 결과 확인

Build 성공 여부는 `install` directory나 `setup.bash`의 존재만으로 판단하지 않는다. 이전 build 산출물이 남아 있는 상태에서 최근 build가 실패할 수도 있고, 여러 package 중 일부만 성공할 수도 있다. `colcon build`가 끝날 때의 summary와 exit status를 먼저 확인하고, 오류가 있으면 `log/latest_build/<package>/` 아래의 기록을 확인한다.

```bash
colcon build --packages-select example_package
printf 'exit=%s\n' "$?"
```

Exit status가 `0`이면 이번 command가 선택한 package 집합의 처리가 성공했다는 뜻이다. 성공한 build는 package가 선언한 target과 resource를 `install`에 준비하지만 process를 시작하지 않는다. Executable이나 launch file의 runtime은 새 shell에서 overlay를 활성화한 뒤 `ros2 run` 또는 `ros2 launch`를 실행할 때 시작된다.

## Underlay와 overlay

`colcon build`가 workspace의 install space에 생성한 `setup.bash`를 현재 Bash에서 source하면, build된 package들의 환경이 기존 ROS 2 환경 위에 추가된다. 이때 install space는 해당 shell에서 `overlay`로 동작한다. 같은 package가 underlay와 overlay 양쪽에 있으면 overlay의 package가 먼저 선택된다.

Build에 사용한 shell과 분리된 새 terminal을 열고 다음 순서로 활성화한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
```

공식 tutorial은 build에 사용한 terminal에서 바로 overlay를 source하면 복잡한 문제가 생길 수 있으므로 새 terminal을 사용하도록 권장한다.

Workspace의 두 setup script는 포함 범위가 다르다.

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
| `colcon build`가 성공했다. | 선택된 package의 build와 install 단계가 완료됐다는 뜻이며 executable이나 node가 실행됐다는 뜻은 아니다. |
| `install/setup.bash`가 있다. | Install space가 이전에 생성됐다는 뜻이며 최근 build에서 모든 package가 성공했다는 증거는 아니다. |
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
- [colcon Documentation - What is a Workspace?](https://colcon.readthedocs.io/en/released/user/what-is-a-workspace.html)
- [colcon Documentation - Package Selection Arguments](https://colcon.readthedocs.io/en/released/reference/package-selection-arguments.html)
- [colcon Documentation - Isolated vs Merged Workspaces](https://colcon.readthedocs.io/en/released/user/isolated-vs-merged-workspaces.html)
- [colcon-recursive-crawl - Recursive package discovery](https://github.com/colcon/colcon-recursive-crawl/blob/master/colcon_recursive_crawl/package_discovery/recursive_crawl.py)
