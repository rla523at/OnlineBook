# ROS 2 Node Runtime and Middleware

## 한 줄 요약

ROS 2 executable을 실행하면 operating system이 process를 만들고, process 안에서 ROS context, node, executor와 선택된 middleware가 초기화된다. Executor는 callback을 실행하고 middleware는 node 사이의 발견과 message 전달을 담당하며, 종료 요청이 오면 이 구성 요소들이 정리된 뒤 process가 끝난다.

## Build 결과에서 실행 중인 node까지

`runtime`은 executable이 process로 시작된 시점부터 종료될 때까지의 실행 기간을 뜻한다. Source를 build해 executable을 만드는 과정과 그 executable을 실행하는 runtime은 서로 다른 단계다.

```text
C++ source와 build 설정
          │
          │ colcon build
          ▼
install에 배치된 executable
          │
          │ shell, ros2 run 또는 launch system
          ▼
operating system process
          │
          │ main()
          ▼
ROS context와 node 생성
          │
          ├─ publisher / subscription / timer 생성
          │
          └─ executor spin
                    │
                    ▼
             callback 실행과 message 통신
                    │
                    │ shutdown 요청
                    ▼
             자원 정리와 process 종료
```

`colcon build`의 package 선택, build와 install 과정 및 산출물 구조는 [Environment and Workspace](<./01 Environment and Workspace.md>)에서 설명한다. 이 문서처럼 executable target을 선언한 package에서 `colcon build`가 성공했다는 사실은 executable이 준비됐다는 뜻이다. Build tool이 executable을 계속 실행하거나 node를 자동으로 유지한다는 뜻은 아니다.

## Executable 실행을 시작하는 주체

Executable의 실행은 사용자 또는 자동화 도구의 요청에서 시작되고, operating system이 실제 process를 만든다.

| 실행 방법 | 실행을 요청하는 주체 | 역할 |
|---|---|---|
| 설치 경로의 executable 직접 실행 | shell | 지정한 program file의 process 생성을 operating system에 요청한다. |
| `ros2 run <package> <executable>` | ROS 2 CLI | 현재 environment에서 package와 executable을 찾아 process 실행을 요청한다. |
| `ros2 launch ...` | ROS 2 launch system | 설정에 따라 하나 이상의 process를 시작하고 종료 상태를 관리한다. |
| shell script, CI 또는 service manager | 해당 자동화 도구 | 정해진 순서와 조건으로 executable을 실행하고 감시한다. |

`ros2 run`과 launch system은 실행을 돕는 도구다. 이 도구들이 publisher와 subscriber 사이의 topic message를 중계하는 것은 아니다. Process가 시작된 뒤의 message 통신은 선택된 ROS middleware가 담당한다.

예를 들어 다음 명령을 실행하면 `ros2` CLI가 활성화된 environment에서 `cpp_pubsub` package의 `talker` executable을 찾고, operating system에 새 process 실행을 요청한다.

```bash
ros2 run cpp_pubsub talker
```

실행된 process는 고유한 PID(Process ID)를 가지며 executable의 `main()`부터 실행한다.

## `rclcpp::init()`과 ROS context

C++ node의 `main()`은 다음 형태로 시작한다.

```cpp
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  auto node = std::make_shared<Talker>();
  rclcpp::spin(node);
  rclcpp::shutdown();
  return 0;
}
```

`rclcpp`는 ROS 2의 C++ client library다. `rclcpp::init()`은 기본 ROS context를 초기화한다. Context는 해당 process에서 ROS 통신을 사용할 수 있는지와 종료가 요청됐는지 같은 실행 상태를 관리한다.

초기화 과정에서는 ROS 전용 command-line argument와 initialization option을 처리하고, 사용할 middleware를 연결하는 RMW(ROS Middleware Interface) 구현을 준비한다. 기본 option에서는 process 종료 요청을 처리하기 위한 signal handler도 설치한다.

`rclcpp::init()`이 executable을 새로 실행하는 것은 아니다. 이 함수가 호출될 때는 operating system이 이미 process를 만들고 `main()`을 실행하고 있다.

## Node와 통신 endpoint 생성

다음 문장은 process 안에 `Talker` node 객체를 만든다.

```cpp
auto node = std::make_shared<Talker>();
```

Node constructor에서 publisher, subscription 또는 timer를 만들 수 있다.

```cpp
publisher_ = create_publisher<std_msgs::msg::String>("chatter", 10);

subscription_ = create_subscription<std_msgs::msg::String>(
  "chatter",
  10,
  message_callback);
```

`publisher`와 `subscription`은 node가 topic 통신에 참여하기 위해 만드는 endpoint다. Endpoint가 생성되면 RMW와 그 아래의 middleware가 송수신 및 discovery에 필요한 내부 자원을 준비한다. 여기서 `discovery`는 실행 중인 통신 endpoint들이 서로의 존재와 조건을 찾는 과정이다.

Node는 ROS graph의 논리적 계산 단위이고 process나 특정 middleware entity와 같은 개념이 아니다. 한 process에 여러 node가 존재할 수 있으며, middleware가 node와 endpoint를 내부 자원에 어떻게 대응시키는지는 RMW 구현에 따라 달라질 수 있다.

## Executor와 `spin()`

`executor`는 subscription message, timer 만료, service request처럼 처리할 준비가 된 event를 기다리고 해당 callback을 호출하는 실행 관리자다.

```cpp
rclcpp::spin(node);
```

위 호출은 기본적으로 다음과 같은 Single-Threaded Executor 구성에 대응한다.

```cpp
rclcpp::executors::SingleThreadedExecutor executor;
executor.add_node(node);
executor.spin();
```

Single-Threaded Executor는 현재 thread에서 callback을 하나씩 실행한다. `spin()`이 실행되는 동안 현재 thread는 준비된 event를 기다리거나 callback을 처리하므로 `spin()` 아래의 코드는 바로 실행되지 않는다.

```text
executor
   │
   ├─ timer가 만료됐는가? ──────────> timer callback
   ├─ subscription data가 왔는가? ──> subscription callback
   ├─ service request가 왔는가? ────> service callback
   └─ shutdown이 요청됐는가? ──────> spin 종료
```

Publisher 예제의 주기적 publish는 timer callback 안에 있으므로 executor가 spin해야 실행된다. Subscriber 예제도 executor가 spin해야 준비된 message를 가져와 subscription callback을 호출한다.

`spin()`이 process 자체를 생성하거나 middleware를 대신 실행하는 것은 아니다. 이미 생성된 process의 thread를 사용해 application callback 실행을 관리한다.

## Middleware와 RMW

`middleware`는 application code와 다른 process 또는 network 통신 사이에 위치해 discovery, data 전달과 serialization 같은 공통 기능을 제공하는 software 계층이다. `Serialization`은 memory의 message data를 전송 가능한 byte 표현으로 바꾸는 과정이다.

ROS 2 application은 특정 middleware API를 직접 호출하는 대신 앞에서 정의한 RMW를 거친다. RMW는 여러 middleware 구현을 같은 ROS 2 client library에서 사용할 수 있도록 만든 추상 interface다.

```text
application code
      │
      ▼
    rclcpp          C++ client library
      │
      ▼
      rcl           공통 ROS client support
      │
      ▼
     RMW            middleware 추상 interface
      │
      ├─ rmw_fastrtps_cpp   ──> Fast DDS
      ├─ rmw_cyclonedds_cpp ──> Cyclone DDS
      └─ 다른 RMW 구현       ──> 해당 middleware
```

DDS(Data Distribution Service)는 data 중심 publish-subscribe 통신을 정의한 표준이다. Fast DDS와 Cyclone DDS는 DDS를 구현한 서로 다른 software다. ROS 2 Jazzy의 기본 RMW는 Fast DDS 기반이지만, 설치 상태와 `RMW_IMPLEMENTATION` environment variable에 따라 다른 RMW를 선택할 수 있다. ROS 2는 DDS 기반이 아닌 RMW도 지원할 수 있으므로 `ROS 2 middleware = 항상 DDS`라고 일반화하면 안 된다.

DDS 기반 RMW를 사용하는 일반적인 process-local 구성에서는 middleware library가 각 ROS 2 process 안에 load되어 동작한다. 사용자가 publisher와 subscriber 외에 별도의 DDS executable을 반드시 실행할 필요는 없다. DDS Discovery Server와 같은 별도 service를 사용하도록 명시적으로 구성한 경우에는 추가 process가 필요할 수 있다.

## Discovery와 endpoint 연결

Discovery는 실행 중인 middleware 참여자와 endpoint가 서로의 존재와 통신 조건을 찾는 과정이다. Application code가 상대 process의 IP address를 직접 지정하지 않아도 publisher와 subscription이 연결될 수 있는 이유다.

Publisher와 subscription이 연결되려면 적어도 다음 조건을 확인해야 한다.

- 같은 ROS domain에 참여해야 한다. ROS domain은 discovery 범위를 나누는 논리적 통신 영역이며 `ROS_DOMAIN_ID`로 선택한다.
- Topic 이름과 message type이 대응해야 한다.
- Publisher와 subscription의 QoS(Quality of Service) 정책이 호환되어야 한다. QoS는 message 보관과 전달 신뢰성 같은 통신 동작을 정한다.
- 서로 다른 computer나 container라면 middleware discovery packet과 data가 통과할 network 경로가 있어야 한다.
- 보안 기능을 사용한다면 양쪽의 보안 설정이 통신을 허용해야 한다.

같은 `ROS_DOMAIN_ID`는 서로를 발견할 수 있는 범위를 맞추는 조건이지, topic 이름·type·QoS가 달라도 연결해 주는 설정은 아니다.

DDS 기반 RMW에서는 ROS publisher와 subscription이 DDS의 송수신 endpoint에 대응하도록 구현된다. 그러나 ROS node 자체를 DDS의 특정 entity 하나와 항상 일대일로 같다고 설명해서는 안 된다.

## Message 한 개가 처리되는 흐름

Publisher가 다음 함수를 호출한다고 하자.

```cpp
publisher_->publish(message);
```

Message는 개념적으로 다음 계층을 거친다.

```text
publisher callback
      │
      ▼
rclcpp publisher
      │
      ▼
rcl과 선택된 RMW
      │
      ▼
middleware의 serialization과 전송
      │
      ▼
subscriber 쪽 middleware 수신
      │
      ▼
executor에 준비된 subscription event 표시
      │
      ▼
executor가 message를 가져와 callback 호출
```

Middleware와 executor의 책임은 다음처럼 구분한다.

| 구성 요소 | 주된 책임 |
|---|---|
| middleware | Endpoint discovery, serialization, 전송, 수신과 QoS 적용 |
| executor | 준비된 event 확인과 application callback 호출 |
| callback | 수신 data 처리, 상태 변경 또는 다음 message publish 같은 application 동작 |

Middleware가 data를 수신하는 것과 application callback이 실행되는 것은 같은 단계가 아니다. Callback을 실행하려면 해당 node를 관리하는 executor가 동작해야 한다.

## Signal과 정상 종료

`signal`은 Ubuntu가 따르는 POSIX(Unix 계열 operating system interface 표준)에서 실행 중인 process에 특정 event나 제어 요청을 알리는 mechanism이다. 이 문서의 Ubuntu 예제에서는 다음 signal이 종료 과정과 관련된다.

| Signal | 대표 발생 방법 | 의미 |
|---|---|---|
| `SIGINT` | foreground terminal에서 `Ctrl+C` | foreground process group에 현재 작업을 중단해 달라는 요청 |
| `SIGTERM` | `kill -TERM <PID>` | process를 종료해 달라는 요청 |
| `SIGKILL` | `kill -KILL <PID>` | handler와 정리 코드를 실행할 기회 없이 강제로 종료 |

`kill` command는 이름과 달리 지정한 signal을 process에 전달하는 도구다. `SIGINT`와 `SIGTERM`은 process가 handler로 처리할 수 있지만 `SIGKILL`은 처리하거나 무시할 수 없다.

기본 option으로 호출한 `rclcpp::init()`은 `SIGINT`와 `SIGTERM`을 위한 rclcpp signal handler를 설치한다. Signal이 도착하면 handler는 초기화된 ROS context에 shutdown을 요청하고, executor처럼 대기 중인 ROS 동작을 깨운다.

```text
Ctrl+C 또는 kill -TERM <PID>
              │
              ▼
operating system이 process에 signal 전달
              │
              ▼
rclcpp signal handler가 context shutdown 요청
              │
              ▼
executor의 대기 해제와 spin 종료
              │
              ▼
main()에서 spin 다음 코드 실행
              │
              ▼
남은 ROS context, 객체와 middleware 자원 정리
              │
              ▼
process 종료
```

Signal handler option을 바꾸거나 별도 handler를 설치한 program에서는 동작이 달라질 수 있다. 따라서 `Ctrl+C`가 모든 ROS 2 program을 항상 같은 방식으로 종료한다고 일반화하지 않고, 기본 `rclcpp::init()`을 사용하는 예제의 동작으로 이해한다.

Signal handler가 이미 context shutdown을 시작했을 수 있다. 따라서 `spin()` 다음의 `rclcpp::shutdown()`은 signal을 새로 발생시키는 함수가 아니며, 호출 시 context가 이미 shutdown 상태일 수도 있다.

`SIGKILL`, process crash 또는 전원 중단에서는 `spin()` 아래의 코드, destructor와 log flush가 끝까지 실행된다고 보장할 수 없다. 결과 저장이나 장치 정리가 필요한 program은 처리 가능한 종료 요청을 사용하고 종료 경로를 검증해야 한다.

## 한 process가 먼저 종료되면

Publisher process와 subscriber process는 각각 독립적인 실행 instance다. 한쪽이 종료되었다고 다른 쪽 process가 자동으로 종료되는 것은 아니다.

- Publisher가 종료되면 subscriber는 새 message 없이 계속 spin할 수 있다.
- Subscriber가 종료되면 publisher는 subscriber가 없는 상태에서도 계속 publish하거나 application 정책에 따라 대기할 수 있다.
- 두 process를 함께 종료하거나 실패 시 재시작해야 한다면 launch system, shell script 또는 service manager가 그 수명 정책을 관리해야 한다.

Endpoint가 정상적으로 제거되면 middleware discovery 정보도 갱신되고 다른 node의 ROS graph에서 해당 endpoint가 사라진다. 강제 종료나 network 단절에서는 다른 참여자가 종료 사실을 판단하기까지 middleware 설정에 따른 시간이 필요할 수 있다.

## 실행 상태 관찰

서로 다른 terminal에서 같은 ROS environment와 domain 설정을 활성화한 뒤 다음 명령으로 각 계층을 구분해 확인할 수 있다.

```bash
ros2 pkg executables cpp_pubsub
ros2 node list
ros2 topic info /chatter --verbose
ros2 doctor --report
```

- `ros2 pkg executables`는 environment에서 찾을 수 있는 설치된 executable을 확인한다.
- `ros2 node list`는 현재 ROS graph에 참여한 node를 확인한다.
- `ros2 topic info --verbose`는 topic endpoint와 QoS를 확인한다.
- `ros2 doctor --report`는 현재 ROS 2와 middleware 정보를 확인하는 데 사용할 수 있다.

Executable이 검색되지만 `ros2 node list`에 node가 없다면 executable이 설치된 것과 process가 현재 실행 중인지를 따로 확인해야 한다.

## 자주 혼동하는 관계

| 표현 | 정확한 구분 |
|---|---|
| Build가 끝나면 node가 실행된다. | Build는 executable을 준비하며 실행은 별도 요청이 필요하다. |
| Executable과 process는 같다. | Executable은 file이고 process는 그 file을 실행한 instance다. |
| Process와 node는 같다. | Process는 operating system 실행 단위이고 node는 ROS graph의 논리 단위다. |
| `ros2 run`이 topic message를 전달한다. | `ros2 run`은 executable을 찾아 실행하고 message 전달은 middleware가 담당한다. |
| `spin()`이 DDS를 실행한다. | `spin()`은 executor를 통해 callback을 실행하고 middleware는 별도 계층에서 통신을 처리한다. |
| ROS 2는 항상 DDS만 사용한다. | ROS 2는 RMW를 통해 middleware를 선택하며 Jazzy의 기본 구성은 DDS 기반 Fast DDS다. |
| 한 node가 종료되면 연결된 node도 종료된다. | 독립 process의 종료 정책은 script, launch system 또는 service manager가 별도로 관리해야 한다. |

이 문서에서 다룬 runtime 시작과 종료는 process 실행 수명에 관한 설명이다. 상태 전이를 명시적으로 제공하는 ROS 2 managed lifecycle node의 `unconfigured`, `inactive`, `active` 상태와는 다른 개념이다.

## 관련 문서

- [ROS 2](<./ROS 2.md>)
- [Environment and Workspace](<./01 Environment and Workspace.md>)
- [Node and Topic](<./02 Node and Topic.md>)
- [Shell Environment](<../../03 programming/05 Linux/Shell Environment.md>)
- [Process](<../../03 programming/99 ETC/2 Process.md>)

## References

- [ROS 2 Documentation - Executors](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Intermediate/About-Executors.rst)
- [ROS 2 Documentation - RMW Implementations](https://docs.ros.org/en/jazzy/Installation/RMW-Implementations.html)
- [ROS 2 Documentation - Creating an RMW Implementation](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Tutorials/Advanced/Creating-An-RMW-Implementation.rst)
- [ROS 2 Documentation - About Domain ID](https://github.com/ros2/ros2_documentation/blob/jazzy/source/Concepts/Intermediate/About-Domain-ID.rst)
- [rclcpp Jazzy - utilities.hpp](https://github.com/ros2/rclcpp/blob/jazzy/rclcpp/include/rclcpp/utilities.hpp)
