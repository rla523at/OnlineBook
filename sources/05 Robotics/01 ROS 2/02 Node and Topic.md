# ROS 2 Node and Topic

## 한 줄 요약

Node는 ROS graph에 참여하는 논리적인 계산 단위이고, topic은 publisher endpoint가 보낸 message stream을 subscription endpoint가 받는 비동기 통신 경로다.

## Node, executable과 process

`node`는 ROS graph에 참여하는 논리적인 계산 단위다.

보통 sensor 읽기, localization, motor command 계산처럼 하나의 책임을 맡도록 node를 나눈다.

다음 세 용어는 같은 대상을 가리키지 않는다.

| 용어 | 의미 |
|---|---|
| executable | filesystem에 저장되어 실행할 수 있는 program file |
| process | executable을 실행했을 때 operating system이 관리하는 실행 instance |
| node | process 안에서 ROS graph에 참여하는 논리적 계산 단위 |

하나의 executable이 실행될 때 node 하나만 만드는 구성이 흔하지만, 한 process가 여러 node를 포함할 수도 있다. 따라서 `node 하나 = process 하나`를 ROS 2의 규칙으로 가정하면 안 된다.

Node는 이름을 가지고 ROS graph에 참여한다. Topic으로 message를 주고받기 위해 각 node는 `publisher`와 `subscription`이라는 통신 endpoint를 만든다. 이 endpoint들은 독립된 node가 아니라 이를 만든 node에 속하는 통신 객체다. 하나의 node는 publisher와 subscription을 각각 여러 개 만들 수 있으며, 두 종류를 동시에 가질 수도 있다.

Node들은 같은 process, 같은 computer 또는 network로 연결된 서로 다른 computer에서 실행될 수 있다. 같은 ROS domain의 node는 middleware discovery를 통해 서로를 찾고 ROS graph를 구성한다.

## Topic, publisher와 subscription

`topic`은 publisher endpoint가 보낸 message stream을 subscription endpoint가 받는 비동기 통신 경로다. 연속해서 발생하는 data를 전달하는 데 적합하며 publish-subscribe 방식을 사용한다.

Topic은 node나 process가 아니며 자체적으로 실행되지 않는다. 사용자가 `/chatter`라는 별도의 topic node를 만드는 것도 아니다. 사용자는 node 안에 publisher endpoint 또는 subscription endpoint를 만들면서 topic 이름, message type과 QoS를 지정한다. ROS 2 client library와 middleware는 같은 topic 이름과 호환되는 설정을 사용하는 endpoint를 발견하고 message를 전달한다. 실행 중인 endpoint가 해당 이름을 사용하면 `/chatter`가 ROS graph에 topic으로 나타난다.

예제 source code의 `"chatter"`는 현재 node의 namespace를 기준으로 해석하는 상대 이름이다. `/`는 ROS namespace의 root를 뜻하며, root namespace에서 실행되는 예제에서는 `"chatter"`가 완전히 해석된 이름인 `/chatter`가 된다. 예를 들어 node가 `/robot1` namespace에 있다면 상대 이름 `"chatter"`는 `/robot1/chatter`가 되지만, `/chatter`처럼 `/`로 시작하는 이름은 node의 namespace와 관계없이 그대로 유지된다.

```text
Node 구성
/talker node                  /listener node
└── publisher endpoint        └── subscription endpoint

Message 흐름
publisher endpoint ── publish ──> topic 이름: /chatter ── deliver ──> subscription endpoint
```

`publisher`와 `subscriber`는 각각 발행하거나 수신하는 node의 역할을 가리킬 때도 사용한다. 이 문서에서는 endpoint 자체를 `publisher endpoint`와 `subscription endpoint`, node의 역할을 `publisher node`와 `subscriber node`로 구분해 부른다. 예를 들어 `/listener`는 subscriber node이며, 내부에 `rclcpp::Subscription` 객체를 만든다.

그림의 각 요소는 다음 역할을 한다.

- `publisher endpoint`는 정해진 topic에 message를 보낸다.
- `subscription endpoint`는 정해진 topic에서 message를 받는다.
- `publisher endpoint`와 `subscription endpoint`는 직접 상대 process의 주소를 application code에 고정하지 않는다. 각 endpoint는 topic을 기준으로 연결된다.
- 하나의 topic에는 publisher endpoint와 subscription endpoint가 각각 0개 이상 존재할 수 있다.

### Topic 이름 정하기

예제의 `chatter`는 ROS 2가 미리 정의한 이름이 아니라 application이 정한 topic 이름이다. 따라서 `chatterrrrr`처럼 다른 이름도 ROS 2 이름 규칙을 만족하면 사용할 수 있다.

`chatter`처럼 `/`로 시작하지 않는 이름은 relative topic name이다. Node namespace가 `/robot1`이면 최종 이름은 `/robot1/chatter`가 된다. 반면 `/chatter`처럼 `/`로 시작하는 fully qualified topic name은 node namespace의 영향을 받지 않는다.

이 문서의 예제처럼 특수한 치환 문법을 쓰지 않는 일반적인 topic 이름에는 영문자, 숫자, `_`와 namespace를 구분하는 `/`를 사용한다. 대표적인 제약으로 각 name token은 숫자로 시작할 수 없으며, 공백, 연속된 `_`, 빈 token을 만드는 `//`, 이름 끝의 `/`는 허용되지 않는다. 예를 들어 `chatterrrrr`, `robot1/chatter`, `/robot1/chatter`는 유효하지만 `123chatter`, `my chatter`, `foo__bar`, `foo//bar`, `foo/`는 유효하지 않다.

`publisher endpoint`와 `subscription endpoint`가 실제로 연결되려면 다음 조건을 만족해야 한다.

- Node namespace가 적용된 최종 topic 이름이 같아야 한다.
- Message type이 같아야 한다. Topic은 strongly typed이므로 이름만 같고 type이 다르면 통신하지 않는다.
- QoS 정책이 서로 호환되어야 한다. 두 endpoint의 모든 QoS 설정이 반드시 같을 필요는 없다.

Node가 늦게 시작하거나 종료되어도 다른 node의 source code를 바꿀 필요는 없다. 다만 discovery가 연결을 만드는 데 짧은 시간이 걸릴 수 있고, subscription endpoint가 연결되기 전에 지나간 message를 나중에 항상 받을 수 있는 것은 아니다.

다음 예제의 `10`은 publisher endpoint와 subscription endpoint에 최근 10개 sample을 보관하는 `Keep Last` history depth를 간단히 지정한다. 모든 message의 영구 보관이나 전달을 보장한다는 뜻은 아니다.

### Message와 message type

`message`는 publisher endpoint가 topic에 한 번 publish하는 하나의 data sample이다. 같은 message type으로 만든 message라도 instance마다 field 값은 달라질 수 있다.

`message type`은 message가 어떤 field를 가지며 각 field의 type이 무엇인지 정의한 ROS interface다. 예제의 `std_msgs/msg/String`은 `data`라는 string field 하나를 정의한다.

`std_msgs/msg/String` 형식의 이름은 다음 세 부분으로 구성된다.

| 부분 | 예제 | 의미 |
|---|---|---|
| Package | `std_msgs` | Interface를 제공하는 package |
| Interface 종류 | `msg` | Topic 통신에 사용하는 message interface |
| Type 이름 | `String` | Field 구조를 정의한 구체적인 message type |

CLI와 interface 이름에서는 `std_msgs/msg/String`으로 표기하고, C++ source에서는 같은 type을 namespace 형식인 `std_msgs::msg::String`으로 표기한다.

```cpp
std_msgs::msg::String message;
message.data = "Hello ROS 2";
publisher_->publish(message);
```

이 예제의 개념 대응은 다음과 같다.

| 개념 | 실제 대응 |
|---|---|
| Message type | `std_msgs/msg/String` |
| C++ message type | `std_msgs::msg::String` |
| Message instance | 변수 `message` |
| Message field | `message.data` |
| Topic | Publisher를 만들 때 지정한 `/chatter` |

Topic은 message가 전달되는 이름 있는 경로이고, message type은 그 경로로 전달할 data의 구조다. Message instance 자체에 topic 이름이 들어 있는 것은 아니며, 같은 message type을 여러 topic에서 사용할 수도 있다.

다음 명령으로 설치된 message type의 실제 field 정의를 확인할 수 있다.

```bash
ros2 interface show std_msgs/msg/String
```

핵심 출력은 다음과 같다.

```text
string data
```

## 최소 C++ publisher/subscriber package

다음 예제는 하나의 `cpp_pubsub` package에 두 executable을 만든다.

| Executable | Node 이름 | 역할 |
|---|---|---|
| `talker` | `/talker` | `/chatter`에 문자열을 0.5초마다 publish한다. |
| `listener` | `/listener` | `/chatter`를 subscribe하고 받은 문자열을 출력한다. |

예제 환경은 ROS 2 Jazzy, Ubuntu 24.04와 Bash다. 먼저 [Environment and Workspace](<./01 Environment and Workspace.md>)의 설치 및 workspace 개념을 확인한다.

### 1. Package 뼈대 생성

새 terminal에서 Jazzy underlay를 활성화하고 workspace의 `src`로 이동한다.

```bash
source /opt/ros/jazzy/setup.bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
```

Package 뼈대를 생성한다.

```bash
ros2 pkg create --build-type ament_cmake --license Apache-2.0 cpp_pubsub
```

이 명령은 `src` 아래에 `cpp_pubsub` directory와 기본 `package.xml`, `CMakeLists.txt`, `include`, `src`를 만든다. 아직 `talker`와 `listener` executable을 build하지는 않는다.

완성될 source 구조는 다음과 같다.

```text
~/ros2_ws/src/cpp_pubsub/
├─ CMakeLists.txt
├─ package.xml
├─ include/cpp_pubsub/
└─ src/
   ├─ publisher.cpp
   └─ subscriber.cpp
```

### 2. `package.xml`

`package.xml`은 package가 사용하는 build tool과 dependency를 선언한다.

`~/ros2_ws/src/cpp_pubsub/package.xml`을 다음 내용으로 교체한다.

```xml
<?xml version="1.0"?>
<?xml-model
  href="http://download.ros.org/schema/package_format3.xsd"
  schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>cpp_pubsub</name>
  <version>0.0.0</version>
  <description>Minimal C++ publisher and subscriber example.</description>

  <maintainer email="maintainer@example.com">Maintainer</maintainer>
  <license>Apache-2.0</license>

  <buildtool_depend>ament_cmake</buildtool_depend>

  <depend>rclcpp</depend>
  <depend>std_msgs</depend>

  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>
```

- `ament_cmake`는 이 CMake 기반 package의 build system이다.
- `rclcpp`는 ROS 2의 C++ client library다.
- `std_msgs`는 이 예제에서 사용할 표준 문자열 message type을 제공한다.

실제 package에서는 placeholder maintainer 이름과 email을 관리 정보로 교체한다.

### 3. `CMakeLists.txt`

`CMakeLists.txt`는 source file을 executable target으로 만들고 필요한 dependency와 설치 위치를 연결한다.

`~/ros2_ws/src/cpp_pubsub/CMakeLists.txt`을 다음 내용으로 교체한다.

```cmake
cmake_minimum_required(VERSION 3.8)
project(cpp_pubsub)

if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

add_executable(talker src/publisher.cpp)
ament_target_dependencies(talker rclcpp std_msgs)

add_executable(listener src/subscriber.cpp)
ament_target_dependencies(listener rclcpp std_msgs)

install(
  TARGETS talker listener
  DESTINATION lib/${PROJECT_NAME}
)

ament_package()
```

- `add_executable`은 source file로 `talker`와 `listener` executable target을 만든다.
- `ament_target_dependencies`는 각 target에 필요한 ROS 2 dependency를 연결한다.
- `install`은 두 executable의 설치 위치를 정한다. 이 규칙이 있어야 `ros2 run cpp_pubsub talker`처럼 설치된 executable을 찾을 수 있다.
- `ament_package()`는 package를 ament index에 등록하는 데 필요한 설정을 생성한다.

### 4. Publisher source

`publisher.cpp`는 `/talker` node를 만들고 `/chatter`에 문자열 message를 500 ms마다 publish한다.

`~/ros2_ws/src/cpp_pubsub/src/publisher.cpp`를 만든다.

```cpp
#include <chrono>
#include <cstddef>
#include <memory>
#include <string>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

using namespace std::chrono_literals;

class Talker : public rclcpp::Node
{
public:
  Talker()
  : Node("talker")
  {
    publisher_ = create_publisher<std_msgs::msg::String>("chatter", 10);

    timer_ = create_wall_timer(
      500ms,
      [this]() {
        std_msgs::msg::String message;
        message.data = "Hello ROS 2: " + std::to_string(count_++);

        RCLCPP_INFO(get_logger(), "Publishing: '%s'", message.data.c_str());
        publisher_->publish(message);
      });
  }

private:
  rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
  rclcpp::TimerBase::SharedPtr timer_;
  std::size_t count_{0};
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<Talker>());
  rclcpp::shutdown();
  return 0;
}
```

- `Node("talker")`는 node 이름을 `talker`로 정한다.
- `create_publisher<std_msgs::msg::String>("chatter", 10)`은 message type, topic 이름과 history depth를 지정한다.
- 500 ms timer callback은 새로운 message를 만들고 `/chatter`에 publish한다.
- `rclcpp::spin`은 timer callback이 실행되도록 node의 callback을 처리한다.

### 5. Subscriber source

`subscriber.cpp`는 `/listener` node를 만들고 `/chatter`에서 받은 문자열 message를 출력한다.

`~/ros2_ws/src/cpp_pubsub/src/subscriber.cpp`를 만든다.

```cpp
#include <memory>

#include "rclcpp/rclcpp.hpp"
#include "std_msgs/msg/string.hpp"

class Listener : public rclcpp::Node
{
public:
  Listener()
  : Node("listener")
  {
    subscription_ = create_subscription<std_msgs::msg::String>(
      "chatter",
      10,
      [this](std_msgs::msg::String::ConstSharedPtr message) {
        RCLCPP_INFO(get_logger(), "Received: '%s'", message->data.c_str());
      });
  }

private:
  rclcpp::Subscription<std_msgs::msg::String>::SharedPtr subscription_;
};

int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<Listener>());
  rclcpp::shutdown();
  return 0;
}
```

- `Node("listener")`는 node 이름을 `listener`로 정한다.
- `create_subscription<std_msgs::msg::String>("chatter", 10, ...)`은 message type, topic 이름과 history depth를 지정한다.
- Message가 도착하면 executor가 subscription callback을 호출하고 `message->data`를 log로 출력한다.
- `rclcpp::spin`은 message가 도착할 때 subscription callback이 실행되도록 node의 callback을 처리한다.

## Dependency 설치와 build

먼저 `package.xml`에 선언한 dependency를 준비하고 package를 build한 뒤, 새 shell에서 설치 결과를 확인한다.

Workspace root로 이동해 `package.xml`에 선언한 system dependency를 확인한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
rosdep install -i --from-path src --rosdistro jazzy -y
```

`rosdep`은 빠진 dependency를 설치하지만 C++ source를 compile하지 않는다.

Package를 build한다. `--packages-select cpp_pubsub`은 workspace의 다른 package를 자동으로 build하지 않고 `cpp_pubsub`만 선택한다. 여기서 `rclcpp`와 `std_msgs`는 앞에서 활성화한 Jazzy underlay에 이미 있으므로 함께 build하지 않는다. Package 선택 방식과 build 산출물의 일반 원리는 [Environment and Workspace](<./01 Environment and Workspace.md>)를 참고한다.

```bash
colcon build --packages-select cpp_pubsub
```

성공하면 `talker`와 `listener`가 `install/cpp_pubsub/lib/cpp_pubsub` 아래에 설치되고 workspace용 setup script가 갱신된다.

Build에 사용한 shell과 분리된 새 terminal을 열고 설치 대상에 등록된 executable을 확인한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
ros2 pkg executables cpp_pubsub
```

예상되는 핵심 출력은 다음 두 항목이다.

```text
cpp_pubsub listener
cpp_pubsub talker
```

## 두 node 실행

`talker`와 `listener` executable은 각각 별도 process를 시작하고, 각 process는 `/talker`와 `/listener` node를 하나씩 만든다. 비동기 통신을 확인하기 위해 두 executable을 서로 다른 terminal에서 실행한다.

Build terminal과 분리된 terminal A를 열어 `talker` executable을 실행하고 publisher node를 시작한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
ros2 run cpp_pubsub talker
```

다음과 같은 log가 약 0.5초 간격으로 증가한다.

```text
[INFO] [...][talker]: Publishing: 'Hello ROS 2: 0'
[INFO] [...][talker]: Publishing: 'Hello ROS 2: 1'
```

새 terminal B를 열어 `listener` executable을 실행하고 subscriber node를 시작한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
ros2 run cpp_pubsub listener
```

연결이 discovery된 뒤 다음과 같은 log가 출력된다. Listener를 나중에 시작했으므로 첫 count는 `0`이 아닐 수 있다.

```text
[INFO] [...][listener]: Received: 'Hello ROS 2: 7'
[INFO] [...][listener]: Received: 'Hello ROS 2: 8'
```

`ros2 run`은 package에서 executable을 찾아 process를 시작할 뿐 topic message를 중계하지 않는다. Process가 시작된 뒤 `rclcpp::init()`, node 생성, executor의 `spin()`과 middleware 통신이 어떻게 이어지는지는 [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)에서 설명한다.

## ROS graph와 topic 관찰

실행 중인 node와 endpoint가 ROS graph에 어떻게 나타나는지 `node → topic → message` 순서로 확인한다.

세 번째 terminal C에서도 같은 underlay와 overlay를 활성화한다.

```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_ws
source install/setup.bash
```

실행 중인 node를 확인한다.

```bash
ros2 node list
ros2 node info /talker
ros2 node info /listener
```

`ros2 node list`에는 최소한 `/talker`와 `/listener`가 나타난다. `node info`는 각 node에 속한 endpoint를 `Publishers`와 `Subscribers` 항목으로 보여준다.

Topic 이름과 type을 확인한다.

```bash
ros2 topic list -t
ros2 topic info /chatter --verbose
```

핵심 관계는 다음과 같다.

```text
/chatter [std_msgs/msg/String]
publisher count : 1
subscriber count: 1
```

실제 출력 형식에는 ROS 2가 자동으로 사용하는 `/rosout`, `/parameter_events`와 QoS 정보도 포함될 수 있다.

CLI에서 `/chatter` message를 직접 관찰한다.

```bash
ros2 topic echo /chatter
```

Publish 주기를 측정할 수도 있다.

```bash
ros2 topic hz /chatter
```

예제 timer가 500 ms이므로 충분히 관찰하면 약 2 Hz에 가까운 값이 나타난다. Operating system scheduling과 측정 구간 때문에 정확히 2.000 Hz일 필요는 없다.

`ros2 topic echo`와 `ros2 topic hz`도 관찰하는 동안 임시 subscription endpoint를 만든다. 따라서 이 명령이 실행 중일 때 `ros2 topic info`의 subscriber count가 늘어날 수 있다.

## 종료와 다시 확인

각 실행 terminal에서 `Ctrl+C`를 눌러 `talker`와 `listener` process를 각각 종료한다.

`talker` process를 먼저 종료하면 listener는 새 message를 받지 않은 채 계속 대기할 수 있으므로 listener도 별도로 종료한다.

종료 후 다시 확인한다.

```bash
ros2 node list
ros2 topic info /chatter
```

다른 publisher endpoint나 subscription endpoint가 없다면 `/talker`, `/listener`가 사라지고 `/chatter`를 찾지 못한다. Node와 topic은 source file에 적혀 있다는 이유만으로 graph에 계속 존재하는 것이 아니라, endpoint를 만든 process가 실행 중일 때 graph에 나타난다.

`Ctrl+C`는 foreground process group에 `SIGINT`를 보낸다. 기본 `rclcpp::init()`이 설치한 signal handler가 ROS context에 shutdown을 요청하면 executor의 `spin()`이 끝나고 process가 종료 경로로 진행한다. Signal, `spin()`과 middleware 자원 정리의 관계는 [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)에서 설명한다.

## 문제 확인 순서

### Package를 찾지 못한다

```text
Package 'cpp_pubsub' not found
```

현재 shell이 올바른 overlay를 읽었는지 확인한다.

```bash
cd ~/ros2_ws
source /opt/ros/jazzy/setup.bash
source install/setup.bash
ros2 pkg executables cpp_pubsub
```

그래도 보이지 않으면 `colcon build --packages-select cpp_pubsub`의 오류와 `install` 생성 여부를 확인한다.

### Node는 실행되지만 message가 오지 않는다

Node 존재 여부부터 topic 이름, message type, QoS와 discovery 순서로 확인한다.

```bash
ros2 node list
ros2 topic list -t
ros2 topic info /chatter --verbose
```

- `/talker` 또는 `/listener`가 없다면 해당 process가 실행 중인지 확인한다.
- Topic 이름이 다르면 서로 다른 통신 경로를 사용하므로 연결되지 않는다.
- Message type이 다르면 같은 topic 이름이어도 연결되지 않는다.
- QoS 정책이 서로 호환되지 않으면 topic 이름과 message type이 같아도 연결되지 않는다.
- 서로 다른 machine이나 container라면 `ROS_DOMAIN_ID`, network와 middleware discovery 설정도 확인한다.

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Environment and Workspace](<./01 Environment and Workspace.md>)
- [Node Runtime and Middleware](<./03 Node Runtime and Middleware.md>)

## References

- [ROS 2 Documentation - About Nodes](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Basic/About-Nodes.rst)
- [ROS 2 Documentation - About Topics](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Basic/About-Topics.rst)
- [ROS 2 Design - Topic and Service name mapping to DDS](https://design.ros2.org/articles/topic_and_service_names.html)
- [ROS 2 Documentation - Writing a Simple C++ Publisher and Subscriber](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.rst)
- [ROS 2 Documentation - Creating Your First ROS 2 Package](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.rst)
