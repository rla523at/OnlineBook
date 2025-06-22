# condition_variable

## wait 멤버 함수

<details> <summary> <h3 style="display:inline-block"> Predicate </h3></summary>

wait 함수에 predicate 를 전달한 경우 내부적으로 다음과 같은 형태의 루프가 실행된다.
```cpp
while (!predicate()) {
    wait(lock);
}
```

따라서, notify에 의해 스레드가 깨어나면 전달한 predicate를 다시 평가하게 되고, 만약 predicate가 false를 반환하면 다시 wait 상태로 들어간다. 

notify 에 의해 thread 가 일어나고, predicate 까지 true 여야 함수를 탈출할 수 있게 된다.

</details>

<details> <summary> <h3 style="display:inline-block"> lock 의 필요성 </h3></summary>

wait 함수가 정상적으로 동작하기 위해서는 lock 과 atomic 실행이 핊요하다.

원자적 동작 보장:
락 해제와 스레드를 대기 목록에 등록하는 작업이 분리되어 있다면, 락 해제와 대기 상태 전환 사이에 다른 스레드가 접근할 수 있는 작은 시간 간격이 발생합니다. 이 시간 동안 다른 스레드가 조건 변수를 notify할 경우, 대기 상태에 진입하기 전에 신호가 전달되어 스레드가 그 신호를 놓칠 수 있습니다.

락이 없으면 스레드가 대기 상태에 진입하기 전에 공유 데이터가 변경되어 notify가 발생할 수 있고, 이 경우 신호를 놓치거나 잘못된 상태에서 깨어날 위험이 있습니다

wait 함수에서 락을 해제하는 주된 이유는 다른 스레드가 공유 데이터를 수정하고 조건을 변경할 수 있도록 허용하기 위함입니다. 구체적으로 설명하면:

공유 데이터 업데이트 허용:
대기 중인 스레드가 락을 계속 보유하고 있다면, 다른 스레드가 해당 락을 획득할 수 없어 공유 데이터를 변경하거나 조건 변수에 대한 notify 호출을 할 수 없습니다. 락을 해제함으로써, 다른 스레드가 접근해 조건을 만족시키고 notify를 호출할 수 있게 됩니다.

데드락 방지:
만약 wait 중인 스레드가 락을 계속 잡고 있다면, 다른 스레드가 락을 획득하지 못해 결국 조건이 갱신되지 않고 데드락 상황에 빠질 위험이 있습니다.

</details>


> Reference  
> [cppreference - condition_variable/wait](https://en.cppreference.com/w/cpp/thread/condition_variable/wait)  