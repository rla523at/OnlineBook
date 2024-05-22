# Multi thread

## Thread
`스레드(thread)`는 프로세스의 실행 단위이다. 즉, CPU 코어에서 프로세스가 실행될 때 thread 단위로 실행된다.

thread는 하나의 프로세스 내에서 여러개의 실행 흐름을 두기 위해 나온 개념으로 하나의 프로세스에 여러개의 스레드로 구성될 수 있다.

## Multi thread
하나의 process가 여러 thread로 구성되어 있는 것을 multi thread라고 한다.

```{figure} _image/4101.png
```

그렇다면 multi thread는 single thread 대비 어떤 장점이 있는건지 알아보자.

첫번째로 multi thread의 경우, single thread에 비해 CPU를 효율적으로 사용해 처리속도를 높일 수 있다.

예를 들어, CPU가 작업은 안하지만 대기시간이 긴 작업들이 있다고 하자. 그러면, 이 작업들을 기다리는 동안 CPU는 아무런 일을 하지 않고 기다리게 된다. 이 떄, 대기시간이 긴 작업들을 처리하는 thread와 다른 일을 처리하는 thread를 만들어 놓는다면 대기하는 동안에도 CPU는 다른 일을 하고 있기 때문에 CPU를 효율적으로 사용할 수 있다.

```{figure} _image/4102.png
```

위 그림을 보면 single thread일 경우 웹사이트에 데이터를 요청하고 데이터를 다운로드 받는 사이 CPU가 일을 하지 않고 대기하는 시간이 존재한다. 하지만 multi thread일 경우 다른 thread에서 웹사이트 2에 데이터를 요청하면서 CPU가 일을 하지 않고 대기하는 시간이 상당히 줄어든다. 이처럼 multi thread를 사용하면 CPU를 효율적으로 사용할 수 있고 그만큼 처리 속도를 높일 수 있다. 

두번째로 multi thread의 경우, 다중 CPU를 가진 컴퓨터에서 동시에 여러 thread를 처리해주어 single thread에 비해 처리속도를 높일 수 있다.

여러개의 독립적인 작은 작업으로 분할할 수 있는 작업이 있다고 하자. 이 떄, 각각의 thread마다 독립적인 작은 작업을 할당하면 다중 CPU가 process의 각기 다른 thread를 동시에 처리하게 되고 하나의 thread는 작은 작업만을 처리하기 때문에 process의 처리 시간이 단축된다.

```cpp
void sum_worker(const std::vector<int>::iterator start, const std::vector<int>::iterator end, int* result)
{
  int sum = 0;
  for (auto iter = start; iter < end; ++iter)
    sum += *iter;

  *result = sum;
}

TEST(multi_thread, sum1)
{
  constexpr int num_thread = 4;
  constexpr int num_data   = 10000;

  std::vector<int> data(num_data);
  for (int i = 0; i < num_data; ++i)
    data[i] = i;

  std::vector<int> partial_sums(num_thread);

  std::vector<std::thread> workers;
  workers.reserve(num_thread);

  constexpr int data_per_thread = num_data / num_thread;
  for (int i = 0; i < num_thread; ++i) {
    const auto start_iter = data.begin() + i * data_per_thread;
    const auto end_iter   = data.begin() + (i + 1) * data_per_thread;
    workers.push_back(std::thread(sum_worker, start_iter, end_iter, &partial_sums[i]));
  }

  for (auto& worker : workers)
    worker.join();

  int result = 0;
  for (const int val : partial_sums)
    result += val;

  constexpr int ref = 49995000;
  EXPECT_EQ(result, ref);
}
```

이처럼 여러개의 독립적인 작은 작업으로 분할할 수 있는 작업을 병렬화가 가능(Parallelizable)한 작업이라고 한다.

---


process 내에 thread는 stack과 레지스터를 제외한 메모리 영역을 공유한다.

* 왜 multi thread가 필요할까?
  * 병렬 가능한 (Parallelizable) 작업들의 계산 속도를 높일 수 있다 (i.e. 1~10000까지 더하기)
  * 대기시간이 긴 작업(인터넷에 자료 긁어오기)을 기다리는 동안 다른 작업을 하여 CPU를 효율적으로 사용할 수 있다.


멀티 process 대비 장점
* 공유 메모리 영역을 통해  thread 간의 통신을 간단히 할 수 있다.
* thread의 context switching은 기존 thread의 stack만 저장하고 새로운 thread의 stack만 불러오면 되기 때문에  process의 context switching에 비해 overhead가 상대적으로 적다.

싱글 thread 대비 장점
* process 내의 한 thread가 다른 작업을 수행중이더라도 다른 thread가 사용자의 작업 요구에 응답이 가능하기 때문에 응답성이 향상된다.
* 다중 CPU를 가진 컴퓨터에서 multi thread를 사용하면 다중 CPU가 process의 각기 다른 thread를 동시에 처리하여 process의 처리 시간이 단축된다.

단점
* `경쟁 상태(race condition)`가 발생할 수 있다.
* 하나의 thread에서 발생한 문제가 동일 process 내의 다른 thread로 확산될 수 있다.

> Reference  
> [modoocode](https://modoocode.com/269)  
> [blog](https://code-lab1.tistory.com/43)  

### Race condition
race condition이란 두 개 이상의 프로세스가 공통 자원을 병행적으로(concurrently) 읽거나 쓰는 동작을 할 때, 공용 데이터에 대한 접근이 어떤 순서에 따라 이루어졌는지에 따라 그 실행 결과가 같지 않고 달라지는 상황을 말한다.

multi thread의 race condition를 해결하기 위해 `뮤텍스(mutex)` 개념이 사용된다.

> Reference  
> [blog](https://iredays.tistory.com/125)  
> [modoocode](https://modoocode.com/270)  



### 뮤텍스(mutex)
mutex 라는 단어는 영어의 상호 배제(mutual exclusion)에서 온 단어로 여러 thread 들이 동시에 어떠한 코드에 접근하는 것을 배제한다는 의미이다.

여러 thread가 동시에 코드를 실행하지 못하게 std::mutex 객체에서 lock과 unlock 메서드를 제공한다. 

mutex 객체의 lock method를 호출하면 thread에서 mutex 객체의 사용권한을 요청한것이며 한번의 한 thread만 mutex 객체의 사용권한을 얻을 수 있다. 따라서, 현재 mutex 객체의 사용권한을 얻은 thread가 unlock method로 사용권한을 반환하기 전까지 다른 thread들은 계속 사용권한을 요청한 상태로 대기하게 된다.

즉, lock과 unlock 사이에 코드는 mutex 객체의 사용권한을 얻은 단 하나의 thread만 사용할 수 있으며 이 코드 영역을 `임계 영역(critical section)`이라고 한다.

`데드락(dead lock)`이란 mutex를 lock하지 못해 모든 스레드들이 연산을 진행하지 못하고 무한정 대기하는 상태이다. 데드락이 발생할 수 있는 상황은 첫번째로 mutex 객체로 lock 메서드를 호출한 후 unlock 하지 않은 경우이다. 이 경우는 std::lock_guard 객체를 사용할 경우 소멸자에서 mutex 객체를 자동으로 unlock하게 하여 해결할 수 있다. 두번째 경우에는 중첩된 lock을 다른 순서로 다른 함수에서 사용할 경우 mutex lock 메서드 호출 후 unlock 메서드를 호출 했음에도 lock이 얽히는 경우이다. 이 경우는 특정 뮤텍스에 우선순위를 주어 해결할 수 있지만 이 경우 특정 스레드만 많은 일을 하게 되는 기아 상태(starvation)이 발생할 수 있다. 따라서 애초에 중첩된 lock을 사용하지 않거나 lock을 언제나 정해진 순서로 획득해야 한다.

> Reference  
> [modoocode](https://modoocode.com/270)  