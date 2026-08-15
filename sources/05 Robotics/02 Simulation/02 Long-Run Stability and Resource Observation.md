# Long-Run Stability and Resource Observation

## 한 줄 요약

Long-run stability test는 여러 runtime component를 실제 대표 구성으로 일정 시간
연속 실행하면서, 기능적 진행과 process·system resource 추세, 정상 종료를 같은
시간축에서 관찰하는 검증이다.

짧은 smoke test는 설치, 설정과 기본 message 경로의 즉시 실패를 빠르게 찾는 데
적합하다. Long-run test는 그 경로를 더 오래 유지했을 때 나타나는 조기 종료,
message stall, queue 누적, cache 증가와 cleanup 결함을 찾는다. 한 번의 제한된
실행이 모든 장시간 조건에서 memory leak이 없음을 증명하지는 않는다.

## 무엇을 안정성으로 판단하는가

Process가 살아 있다는 사실만으로 system이 정상이라고 판단할 수 없다. Executor가
callback을 처리하지 못하거나 bridge가 연결된 채 message 전달만 멈출 수도 있다.
따라서 안정성 판정은 세 층을 함께 본다.

| 층 | 관찰 대상 | 실패 예 |
|---|---|---|
| 기능적 liveness | 핵심 topic 수신, timestamp·iteration 진행, pause와 error | Process는 있지만 sensor message가 더 이상 오지 않는다. |
| Resource 추세 | Process tree RSS, system 가용 memory, swap과 필요 시 CPU | Message는 계속 오지만 resident memory가 지속적으로 증가한다. |
| Lifecycle | Child status, signal 처리, log flush와 잔여 process | 시험은 끝났지만 server나 bridge가 background에 남는다. |

성능 정확도와 안정성도 구분한다. 예를 들어 Gazebo real-time factor가 목표보다
조금 낮아도 simulation time과 message가 계속 진행할 수 있다. 반대로 평균 RTF가
좋아 보여도 중간에 긴 stall이 있었다면 안정적이라고 할 수 없다.

## 관찰 구간은 readiness 뒤에 시작한다

Process 시작 시각부터 목표 시간을 세면 package load, DDS discovery, bridge
연결과 allocator warm-up이 측정 구간에 섞인다. Runner는 component를 시작한 뒤
대표 입력이 실제로 흐르는 readiness 조건을 확인하고, 그 시점부터 monotonic wall
clock으로 관찰 시간을 센다.

```text
process start
  └─ middleware discovery
       └─ first valid messages and process roots observed
            └─ measurement window starts
                 └─ fixed-duration observation
```

System wall clock은 NTP 보정이나 수동 변경으로 이동할 수 있다. 경과 시간과 stall
deadline은 뒤로 가지 않는 monotonic clock으로 계산하고, UTC timestamp는 log의
실행 시점을 사람이 연결하는 metadata로 따로 기록한다.

Readiness는 topic 이름이 보이는 것보다 강해야 한다. 다음처럼 각 경로의 실제
payload와 진행값을 확인할 수 있다.

- Sensor topic에서 여러 message를 받고 frame과 timestamp가 유효하다.
- Static TF의 필수 parent-child 관계가 message payload 안에 있다.
- Simulation clock과 world iteration이 증가하며 world가 pause 상태가 아니다.
- 관찰할 process root와 descendant가 존재한다.

## Stream liveness와 stall

각 stream은 마지막으로 유효한 sample을 받은 monotonic 시각을 보관한다. 예상
주기보다 충분히 긴 stall timeout을 미리 정하고, 측정 중 그 값을 넘으면
component가 살아 있어도 실패로 기록한다.

```text
last valid sample ───────────── now
          elapsed <= timeout    PASS candidate
          elapsed > timeout     stall
```

Timeout을 정확한 publish period와 같게 두면 scheduling jitter와 middleware 전달
변동을 오류로 오판한다. 반대로 지나치게 길게 두면 실제 정지를 늦게 발견한다.
정상 rate, 입력 fixture의 반복 길이와 host scheduling 여유를 근거로 정한다.

Message count는 보조 증거다. 시작·종료 경계와 discovery 때문에 이론적인 rate의
정확한 곱과 다를 수 있으므로 최소 진행량과 stall을 함께 사용한다. Timestamp
monotonicity, frame, TF 관계처럼 payload 계약도 별도로 검사한다.

## 반복 replay와 시간 불연속

작은 bag을 loop replay하면 같은 검증 입력을 큰 저장공간 없이 오래 공급할 수
있다. 다만 한 cycle 안의 sensor timestamp와 전체 반복 stream의 timestamp는
같은 계약이 아니다.

```text
cycle 1: t1 < t2 < t3
cycle 2: t1 < t2 < t3
boundary: t3 > t1
```

Loop 경계의 stamp 감소는 기록된 payload를 처음부터 다시 재생한 결과다. 이를
전역 timestamp 역행으로 오판하지 않으려면 bag metadata의 topic별 count와 duration을
알고, 완전한 각 cycle 내부의 순서와 count를 검증한다. 첫 subscription이 cycle
중간에 연결되거나 측정이 cycle 중간에 끝날 수 있으므로 처음과 마지막의 부분
cycle은 완전한 cycle과 구분한다.

같은 topic에 live publisher와 replay publisher를 함께 두면 consumer가 두 stream을
섞어 받을 수 있다. Replay 기반 재현 시험에서는 원본 publisher를 종료하거나 ROS
domain과 topic을 분리한다.

## Process RSS를 읽는 방법

Linux의 `/proc/<pid>/status`는 `VmRSS`를 KiB 단위로 제공한다. RSS(resident set
size)는 process의 virtual address space 중 현재 physical memory에 resident한
page의 크기다. Runner가 shell, launch process, node, bridge와 simulator를 만들면
root process 하나만 보지 않고 descendant process의 RSS도 같은 sample 시점에
수집해야 한다.

```text
component RSS sample
  = root process RSS
  + descendant A RSS
  + descendant B RSS
  + ...
```

RSS에는 해석상 한계가 있다.

- Shared library와 shared memory page는 여러 process RSS에 중복될 수 있다.
- `/proc/<pid>/status`의 RSS 값은 빠른 관찰용이며 kernel 문서가 설명하듯 정확한
  순간 합계가 아닐 수 있다.
- Process가 종료하고 새 PID가 재사용될 수 있으므로 runner가 만든 root와 수명을
  함께 추적해야 한다.
- Process tree를 합한 RSS는 system 전체가 실제로 독점 사용한 memory와 같지 않다.

정확한 proportional 분배가 필요하면 `/proc/<pid>/smaps_rollup`의 PSS를 별도로
검토할 수 있다. 일반 안정성 smoke에서는 같은 구성 안의 RSS 추세를 일정한 방식으로
반복 측정하고, system memory도 함께 보는 것이 우선이다.

## System memory와 swap을 함께 보는 이유

`/proc/meminfo`의 `MemFree`는 완전히 사용되지 않는 page만 나타낸다. Linux는
회수 가능한 page cache를 적극적으로 사용하므로, 새 application을 swap 없이
시작할 수 있는 여유의 추정에는 `MemAvailable`이 더 적합하다.

다음 값은 서로 다른 질문에 답한다.

| 값 | 답하는 질문 |
|---|---|
| Component RSS | 이 runner가 소유한 process tree의 resident working set 추세는 어떤가? |
| `MemAvailable` | System이 새 작업에 사용할 수 있다고 추정되는 memory가 얼마나 남았는가? |
| Swap used | 관찰 구간에 anonymous memory pressure가 swap 사용 증가로 나타났는가? |

System 값은 다른 application, kernel cache와 WSL host 상태의 영향도 받는다. 따라서
component RSS 증가 없이 `MemAvailable`만 감소했다면 바로 해당 component의 leak로
결론 내리지 않고 같은 시각의 다른 process와 반복 run을 확인한다.

## 단일 sample 대신 관찰 구간을 비교한다

Startup 직후와 종료 직전 한 sample만 비교하면 allocator warm-up이나 일시적인
queue 변동에 민감하다. 일정 간격으로 sample을 모으고 처음·마지막의 여러 sample을
각각 하나의 window로 묶어 중앙값을 비교하면 순간 spike의 영향을 줄일 수 있다.

```text
baseline = median(first stable window)
end      = median(last window)
growth   = end - baseline
```

판정 기준은 실행 전에 정한다. Absolute growth와 baseline 대비 percentage 중 더
큰 허용치를 적용하면 작은 process에서 작은 absolute 변화가 큰 비율로 보이는
문제와, 큰 process에서 큰 absolute 변화를 무시하는 문제를 함께 줄일 수 있다.
Peak, window median, absolute·percentage growth와 모든 raw sample을 남겨야 threshold
통과 여부만 보고 변동을 숨기지 않는다.

고정 시간의 bounded run은 그 시간 안의 증가만 보여 준다. 다음 경우에는 더 긴
실행과 heap allocation profiler가 필요하다.

- 마지막 window까지 RSS가 꾸준히 상승한다.
- 같은 입력과 환경의 반복 run에서 증가량이 재현된다.
- Swap 사용이 늘거나 `MemAvailable`이 계속 감소한다.
- Message rate가 떨어지는 동시에 queue 또는 RSS가 증가한다.

## 종료는 시험의 일부다

Long-run 관찰이 끝났어도 child process와 log가 정리되지 않으면 완료가 아니다.
Runner는 자신이 만든 process와 process group만 소유하고, 기존 ROS graph나 다른
Gazebo instance를 광범위하게 종료하지 않는다.

일반적인 종료 순서는 다음과 같다.

1. Observer가 마지막 sample과 summary를 flush한다.
2. Application이 처리할 수 있는 `SIGINT` 같은 정상 종료 요청을 보낸다.
3. 제한 시간 안에 끝나지 않은 process에 `SIGTERM`을 보낸다.
4. 그래도 남은 대상에만 `SIGKILL`을 fallback으로 사용한다.
5. Exit status, 강제 종료 단계와 잔여 PID를 기록한다.

`SIGKILL`은 handler, destructor와 log flush를 실행할 기회를 주지 않는다. 정상
종료를 검증하려는 시험에서 첫 선택으로 사용하면 cleanup 결함을 가릴 수 있다.
Child가 새 session을 만들 수 있으므로 process group만 확인할지 descendant tree도
확인할지는 launcher 구조에 맞춰 정한다.

## 재현 가능한 증거

최소한 다음 정보를 같은 run identifier로 연결한다.

- 시작 UTC와 실제 measurement duration
- Source commit, config, ROS domain과 simulator partition
- 입력 bag metadata와 반복 방식
- Topic별 count, stall, timestamp·TF·simulation progression 판정
- Sample interval, component별 RSS baseline·end·peak·growth
- System `MemAvailable` minimum·end와 swap 변화
- Process별 exit status, signal escalation과 error log
- 원본 sample table과 사람이 읽는 최종 summary

짧은 regression mode는 orchestration과 cleanup을 빠르게 확인할 수 있지만, 목표
시간을 채운 long-run evidence로 대체할 수 없다. Summary에는 실제 duration과
장시간 요구 충족 여부를 명시한다.

## 관련 문서

- [Robot Simulation](<./Simulation.md>)
- [Gazebo Harmonic Headless and ROS 2 Integration](<./01 Gazebo Harmonic Headless and ROS 2 Integration.md>)
- [Node Runtime and Middleware](<../01 ROS 2/03 Node Runtime and Middleware.md>)
- [Coordinate Frames and TF2](<../01 ROS 2/04 Coordinate Frames and TF2.md>)
- [Rosbag2 Record, Inspect and Replay](<../01 ROS 2/08 Rosbag2 Record Inspect and Replay.md>)

## References

- [Linux kernel documentation: The `/proc` Filesystem](https://docs.kernel.org/filesystems/proc.html)
- [ROS 2 Jazzy launch architecture](https://docs.ros.org/en/jazzy/p/launch/doc/source/architecture.html)
- [rosbag2 Jazzy README](https://github.com/ros2/rosbag2/blob/jazzy/README.md)
- [Gazebo Harmonic: Use ROS 2 to interact with Gazebo](https://gazebosim.org/docs/harmonic/ros2_integration/)
