# Robot Trajectory Evaluation

## 한 줄 요약

Robot trajectory evaluation은 추정 pose sequence와 reference trajectory를 같은
좌표·시간 계약으로 대응시킨 뒤 absolute 또는 relative error를 계산해 위치 추정의
전역 일관성과 local drift를 관찰하는 과정이다.

## Metric 앞에 필요한 계약

ATE나 RPE 수식이 같아도 입력 trajectory의 의미가 다르면 결과를 비교할 수 없다.
평가 전에 적어도 다음 질문에 답해야 한다.

- 각 pose는 어느 source frame을 어느 target frame에서 표현하는가?
- Translation, angle과 timestamp의 단위는 무엇인가?
- Quaternion column 순서와 rotation 방향은 무엇인가?
- Reference와 estimate의 clock domain과 timestamp epoch가 같은가?
- Timestamp pair를 어떤 tolerance와 offset으로 association하는가?
- Alignment가 없는지, SE(3)인지, scale을 포함한 Sim(3)인지?
- Ground truth가 estimator 입력과 parameter 식별에서 격리되어 있는가?

이 항목을 명시하면 evaluator가 출력한 숫자뿐 아니라 그 숫자가 만들어진 조건도
재현할 수 있다.

## 문서 지도

- [Pose Trajectory Coordinate, Time and Alignment](<./01 Pose Trajectory Coordinate Time and Alignment.md>)
  - Pose와 transform 방향 표기
  - ROS 좌표축·SI 단위와 quaternion
  - Measurement time과 경과 시간의 구분
  - TUM-compatible trajectory text
  - Timestamp association과 SE(3)·Sim(3) alignment
  - Ground truth의 식별·평가 경계

## References

- [REP-103: Standard Units of Measure and Coordinate Conventions](https://github.com/ros-infrastructure/rep/blob/master/rep-0103.rst)
- [TUM RGB-D Dataset File Formats](https://cvg.cit.tum.de/data/datasets/rgbd-dataset/file_formats)
- [evo: odometry and SLAM evaluation tools](https://github.com/MichaelGrupp/evo)
