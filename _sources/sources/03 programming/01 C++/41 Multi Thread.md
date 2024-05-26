# Multi thread

## Thread
`스레드(thread)`는 프로세스의 실행 단위이다. 그래서 CPU 코어에서 프로세스가 실행될 때 thread 단위로 실행된다.

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

예를 들어 1부터 1000까지 더하는 코드가 있다고 하자. 다음에 thread 2개에 1-500까지 더하는 작업과 501-1000까지 더하는 작업을 할당하고 다중 CPU에서 두 thread를 동시에 처리한다고 해보자. 그러면 처리 시작이 거의 2배로 단축될 것이다.

```{figure} _image/4103.png
```

이처럼 여러개의 독립적인 작은 작업으로 분할할 수 있는 작업을 병렬화가 가능(Parallelizable)한 작업이라고 한다.

## std::thread
C++ 11부터 표준에 thread가 추가되면서 thread 사용이 매우 편리해졌다.

### 생성자
<thread>를 추가해주고 std::thread 객체를 생성하면 thread가 생성된다.

```cpp
#include <iostream>
#include <thread>

void func(void)
{
  std::cout << "thread in func!\n";
}

int main(void)
{
  std::thread t1(func);

  return 0;
}
```

main thread가 t1 객체를 생성하는 순간, t1은 인자로 전달받은 함수 func를 새로운 thread에서 실행하게 되고 main thread는 다음 줄(`return 0;`)을 수행하게 된다.

이 때, 함수에 인자가 있다면 어떻게 해야 할까?

함수에 인자가 있는 경우에는 thread 객체를 생성할 때, 함수를 전달하고 그 다음에 인자들을 쭉 적어주면 된다.

```cpp

void func(const int input1, const int input2)
{
  std::cout << "thread in func!\n";
}

int main(void)
{
  const int input1 = 1;
  const int input2 = 2;

  std::thread t1(func, input1, input2);

  return 0;
}

```

그렇다면 만약에 리턴값이 필요한 함수라면 어떻게 해야 할까?

thread 객체는 리턴값이란것이 없기 때문에 어떤 결과를 반환하고 싶으면 포인터 형태로 전달하면 된다.

```cpp
#include <iostream>
#include <thread>

void func(const int input1, const int input2, int* output)
{
  *output = input1 + input2;
  std::cout << "thread in func!\n";
}

int main(void)
{
  const int input1 = 1;
  const int input2 = 2;
  int       output = 0;

  std::thread t1(func, input1, input2, output);

  return 0;
}
```

만약 결과값을 reference로 전달하게 되면 'invoke'에서 일치하는 오버로드된 함수를 찾을 수 없다는 compile error가 발생하게 된다.

```cpp
#include <iostream>
#include <thread>

void func(const int input1, const int input2, int& output)
{
  output = input1 + input2;
  std::cout << "thread in func!\n";
}

int main(void)
{
  const int input1 = 1;
  const int input2 = 2;
  int       output = 0;

  std::thread t1(func, input1, input2, output);

  return 0;
}
```

이제 위 코드가 왜 compile error를 발생시키는지 thread 생성자 관련 코드를 보면서 하나씩 살펴보자.

```cpp
    template <class _Fn, class... _Args, enable_if_t<!is_same_v<_Remove_cvref_t<_Fn>, thread>, int> = 0>
    _NODISCARD_CTOR_THREAD explicit thread(_Fn&& _Fx, _Args&&... _Ax) {
        _Start(_STD forward<_Fn>(_Fx), _STD forward<_Args>(_Ax)...);
    }

    template <class _Fn, class... _Args>
    void _Start(_Fn&& _Fx, _Args&&... _Ax) {
        using _Tuple                 = tuple<decay_t<_Fn>, decay_t<_Args>...>;
        auto _Decay_copied           = _STD make_unique<_Tuple>(_STD forward<_Fn>(_Fx), _STD forward<_Args>(_Ax)...);
        constexpr auto _Invoker_proc = _Get_invoke<_Tuple>(make_index_sequence<1 + sizeof...(_Args)>{});

        _Thr._Hnd =
            reinterpret_cast<void*>(_CSTD _beginthreadex(nullptr, 0, _Invoker_proc, _Decay_copied.get(), 0, &_Thr._Id));
        //....
    }
```

_Start 함수를 보면 내부에서 _Tuple이라는 새로운 타입을 정의하는데 template parameter를 decay_t로 감싼 tuple이다.

decay_t는 주어진 argument가 함수 타입이거나 배열 타입인 경우 포인터로 변환하고 그 외에는 reference와 cv qualifier를 제거하는 역할을 한다.

따라서 _Tuple은 `tuple<void(*)(int,int,int,int&), int, int, int>` 타입이 되고 _Decay_copied는 unqiue_ptr<_Tuple> 타입의 객체로 기존의 값들을 복사한 값을 가지게 된다.

다음으로 _Get_invoke 함수를 살펴보자.

```cpp
    template <class _Tuple, size_t... _Indices>
    _NODISCARD static constexpr auto _Get_invoke(index_sequence<_Indices...>) noexcept {
        return &_Invoke<_Tuple, _Indices...>;
    }

    template <class _Tuple, size_t... _Indices>
    static unsigned int __stdcall _Invoke(void* _RawVals) noexcept {
        const unique_ptr<_Tuple> _FnVals(static_cast<_Tuple*>(_RawVals));
        _Tuple& _Tup = *_FnVals.get(); // avoid ADL, handle incomplete types
        _STD invoke(_STD move(_STD get<_Indices>(_Tup))...);
        // ...        
    }
```

_Get_invoke 함수는 _Invoke 함수를 호출하고 _Invoke 함수는 _Tuple의 요소들을 std::invoke를 사용하여 호출한다

이 떄, `_STD invoke(_STD move(_STD get<_Indices>(_Tup))...);` 코드를 parameter pack expansion을 해보면 다음과 같이 된다.

```cpp
std::invoke(
    std::move(std::get<0>(_Tup)),  // func_ptr (void(*)(const int, const int, int&))
    std::move(std::get<1>(_Tup)),  // input1 (int&&)
    std::move(std::get<2>(_Tup)),  // input2 (int&&)
    std::move(std::get<3>(_Tup))   // output (int&&)
);
```

그러면 std::invoke 함수는 다음과 같이 인스턴스화가 된다.

```cpp
func_ptr(std::forward<int>(input1), std::forward<int>(input2), std::forward<int>(output));
```

결론적으로 input1, input2, output은 각각 int&& 타입으로 전달된다.

하지만 func 함수의 output은 int& 타입임으로 int&& 타입을 인자로 받을 수 없기 때문에 컴파일 에러가 발생하게 된다.

이를 해결하기 위해서는 thread를 생성할 떄, std::ref 함수를 사용해서 std::reference_wrapper 객체를 넘겨주면 된다.

```cpp
int main(void)
{
  const int input1 = 1;
  const int input2 = 2;
  int       output = 0;

  std::thread t1(func, input1, input2, std::ref(output));

  return 0;
}
```

이럴 경우 기존에 문제가 됐던 부분이 어떻게 바뀌었길래 문제가 생기지 않는지 살펴보자.

```cpp
std::invoke(
    std::move(std::get<0>(_Tup)),  // func_ptr (void(*)(const int, const int, int&))
    std::move(std::get<1>(_Tup)),  // input1 (int&&)
    std::move(std::get<2>(_Tup)),  // input2 (int&&)
    std::move(std::get<3>(_Tup))   // output (reference_wraaper<int>&&)
);
```

그러면 std::invoke 함수는 다음과 같이 인스턴스화가 된다.

```cpp
func_ptr(std::forward<int>(input1), std::forward<int>(input2), std::forward<reference_wraaper<int>>(output));
```

결론적으로 input1, input2은 int&& 타입으로 output은 reference_wraaper<int>&& 타입으로 전달된다.

이 때, reference_wraaper<int>는 내부적으로 int& 타입으로 형변환하는 코드가 있기 때문에 output에 정상적으로 값이 전달되고 컴파일 에러가 발생하지 않는다.

> Reference  
> [stackoverflow - why-is-ref-cref-needed...](https://stackoverflow.com/questions/76398544/why-is-ref-cref-needed-for-reference-arguments-to-a-function-passed-to-stdth)


### join & detach
```cpp
#include <iostream>
#include <thread>

void func(void)
{
  std::cout << "thread in func!\n";
}

int main(void)
{
  std::thread t1(func);

  return 0;
}
```

실제로 위 코드를 실행하면 thread의 소멸자에서 assert가 발생할 수 있다.

왜냐하면 t1 thread가 func 함수를 전부 실행하고 종료하기전에 main thread가 main 함수를 끝내버려서 t1의 소멸자가 호출될 수 있고 C++ 표준에 따라 join 되거나 detach 되지 않은 thread의 소멸자가 호출되면 예외가 발생하기 때문이다.

그러면 먼저 join 함수에 대해서 알아보자.

join 함수는 생성된 스레드가 완료될 때까지 return되지 않아 호출 스레드를 블록(block)하는 역할을 한다. 또한, thread가 종료되면 join 함수는 thread와 관련된 모든 자원을 회수한다.

```cpp
#include <iostream>
#include <thread>

void func(void)
{
  std::this_thread::sleep_for(std::chrono::milliseconds(100));
  std::cout << "thread in func!\n";
}

int main(void)
{
  std::thread t1(func);
  std::cout << "thread in main!\n";
  t1.join();

  return 0;
}
```

따라서 위와 같은 코드에서는 먼저 thread in main!이 출력되고 그다음에 thread in func! 출력되는 반면에

```cpp
#include <iostream>
#include <thread>

void func(void)
{
  std::this_thread::sleep_for(std::chrono::milliseconds(100));
  std::cout << "thread in func!\n";
}

int main(void)
{
  std::thread t1(func);
  t1.join();
  std::cout << "thread in main!\n";

  return 0;
}
```

위와 같은 코드에서는 t1.join() 함수는 t1 thread가 끝날 떄 까지 return 되지 않음으로 thread in func!이 출력되고 thread in main!이 출력된다.

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