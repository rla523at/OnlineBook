# Robot Simulation

## 한 줄 요약

Robot simulation은 실제 robot과 환경에서 일어날 상태 변화와 sensor·actuator의
입출력을 software model로 계산하여, 실제 hardware 없이도 robot software를
반복 실행하고 관찰하게 하는 방법이다.

## Simulation이 담당하는 경계

Simulator는 단순한 3D viewer가 아니다. 일반적으로 world와 robot model, 시간,
physics, sensor model을 결합해 다음 상태를 계산한다.

```text
world·robot model + actuator command
                 │
                 ▼
        physics·sensor simulation
                 │
                 ├──> 다음 robot·world state
                 └──> simulated sensor data
```

Simulation에서 얻은 값은 실제 hardware가 측정한 ground truth가 아니라 선택한
model과 parameter의 결과다. 따라서 simulation이 안정적으로 실행된다는 사실과
실제 sensor를 충분히 닮았다는 사실은 별도로 검증해야 한다.

Robot application은 simulator 내부 API를 직접 사용할 수도 있지만, ROS 2처럼
별도 communication framework를 사이에 둘 수도 있다. 이때 simulator의 내부
transport와 ROS graph는 서로 다른 통신 영역이며, message type과 topic을 변환하는
bridge가 두 영역을 연결한다.

## 문서 지도

- [Gazebo Harmonic Headless and ROS 2 Integration](<./01 Gazebo Harmonic Headless and ROS 2 Integration.md>)
  - Gazebo Harmonic, Gazebo Sim과 SDF world의 관계
  - GUI 없는 server-only simulation과 headless rendering의 구분
  - simulation time, wall time과 real-time factor
  - Gazebo Transport와 ROS 2 `ros_gz_bridge`
  - `/clock`과 world statistics를 이용한 최소 실행 검증

## References

- [Gazebo Harmonic Documentation](https://gazebosim.org/docs/harmonic/)
- [Gazebo Harmonic: Architecture](https://gazebosim.org/docs/harmonic/architecture/)
