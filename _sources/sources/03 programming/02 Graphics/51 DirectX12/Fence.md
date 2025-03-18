# Fence
CPU 와 GPU processor 가 병렬적으로 동작하기 때문에 필연적으로 동기화 문제가 발생하게 된다.

> Reference  
> {cite}`Luna` chapter 4.2.2  

## 호출 시점
일반적으로 Signal 함수는 ExecuteCommandLists를 호출한 후에 실행합니다.

왜냐하면 Signal 함수는 Command Queue 에 바로 등록되기 때문에 ExecuteCommandList 를 호출하기 전에 Signal 함수를 호출하면 다른 명령어들 보다 Signal 명렁이 먼저 COmmand Queue 에 등록되어서 Fence 값으로 다른 명령어의 완료 상태를 판단할 수 없게 된다.