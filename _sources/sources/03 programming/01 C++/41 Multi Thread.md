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
func_ptr(static_cast<int&&>(input1), static_cast<int&&>(input2), static_cast<int&&>(output));
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
func_ptr(static_cast<int&&>(input1), static_cast<int&&>(input2), static_cast<reference_wraaper<int>&&>(output));
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

#### join

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

> Reference  
> [modoocode](https://modoocode.com/269)  

#### detach
detach는 말 그대로 해당 쓰레드를 실행 시킨 후 잊어버리는 것이다.

> Reference  
> [modoocode](https://modoocode.com/269)  

### Race condition
race condition이란 두 개 이상의 프로세스가 공통 자원을 병행적으로(concurrently) 읽거나 쓰는 동작을 할 때, 공용 데이터에 대한 접근이 어떤 순서에 따라 이루어졌는지에 따라 그 실행 결과가 같지 않고 달라지는 상황을 말한다.

예를 들어 다음 코드를 보자.

```cpp
void counter_worker(int* result)
{
  for (int i = 0; i < 100000; ++i)
    ++(*result);
}

TEST(multi_thread, counter1)
{
  constexpr int num_thread = 4;

  std::vector<std::thread> workers;
  workers.reserve(num_thread);

  int count = 0;
  for (int i = 0; i < num_thread; ++i)
  {
    workers.push_back(std::thread(counter_worker, &count));
  }

  for (auto& worker : workers)
    worker.join();

  constexpr int ref = 400000;
  //EXPECT_EQ(count, ref);
  EXPECT_LT(count, ref);
}
```

예상대로라면 400000이 되어야 하지만 결과는 400000보다 작은 경우가 종종 생긴다.

왜 이런지 이해하기 위해서는 CPU에서 연산을 어떻게 처리하는지 알아야 한다.

CPU는 연산을 수행하기 위해 register라는 곳에 데이터를 기록한 다음에 연산을 수행한다. 즉, 모든 데이터들은 메모리에 저장되어 있다가 연산할 때 레지스터로 옮겨진 다음 연산을 하고 다시 메모리에 옮겨진다.

관련된 assembly 코드를 보면서 자세히 살펴보자.

```
00007FF7919DE614  mov         rax,qword ptr [result]  
00007FF7919DE619  mov         eax,dword ptr [rax]  
00007FF7919DE61B  inc         eax  
00007FF7919DE61D  mov         rcx,qword ptr [result]  
00007FF7919DE622  mov         dword ptr [rcx],eax  
```

먼저, 첫번째 `mov rax, qword ptr [result]`는 result에 저장된 데이터 8바이트를 rax 레지스터에 저장하는 명령어이다. result에는 변수 count의 주소가 저장되어 있기 때문에, 이제 rax는 count의 주소값을 가지고 있다.

다음으로 `mov eax, dword ptr [rax]`는 rax에 저장된 주소로 가서 저장된 데이터 4바이트를 eax 레지스터에 저장하는 명령어이다. rax에는 count의 주소가 저장되어 있음으로, 이제 eax에는 count의 값이 들어 있다.

다음으로 `inc eax`는 eax 레지스터의 값을 1만큼 증가시키는 명령어이다.

다음 `mov rcx, qword ptr [result]`는 result에 저장된 데이터 8바이트를 rcx 레지스터에 저장하는 명령어이다. result에는 변수 count의 주소가 저장되어 있기 때문에, 이제 rcx는 count의 주소값을 가지고 있다.

다음 `mov dword ptr [rcx], eax`는 eax에 저장된 데이터 4바이트를 rcx 레지스터에 저장된 주소에 저장하는 명령어이다. eax에는 1만큼 증가한 count값이 들어 있고, rcx는 count의 주소값을 가지고 있음으로 count에 1만큼 증가한 count 값을 저장하게 된다.

그러면 이제 왜 이 assembly가 multi thread에서 문제가 될 수 있는지 알아보자.

만약 두개의 thread에서 다음과 같이 실행이 발생했다고 해보자.

```{figure} _image/4104.png
```

thread1에서 처음 2명령어를 실행했을 때, thread1의 eax 레지스터에는 0값이 들어있다.

다음으로 thread2에서 모든 명령어를 실행한경우 count의 값이 1이 된다.

이후에 다시 thread1에서 나머지 명령어를 실행하면 thread1의 eax 레지스터 값을 1로 만들고 그걸 다시 count에 대입하기 때문에 count의 값이 1이 된다.

즉, 증가 연산이 두 thread에서 발생했지만, count 값은 한번만 증가하게 된다.

물론 위와 같은 상황이 아니라서 count의 값이 두번 증가할 수도 있지만, thread를 어떻게 스케쥴링 할지는 운영체제가 결정함으로 항상 이런 행운을 바랄 수는 없다.

> Reference  
> [modoocode](https://modoocode.com/270)  

#### 참고

inc 명령은 역참조한 메모리에서 직접 사용할 수 없다. 따라서 다음과 같은 어셈블리 명령어는 불가능하다.
```
00007FF7919DE614  mov         rax,qword ptr [result]  
00007FF7919DE61B  inc         dword ptr [rax]    
```

### 뮤텍스(mutex)
이런 race condition를 해결하기 위해 `뮤텍스(mutex)` 개념이 사용된다.

mutex라는 단어는 영어의 상호 배제(mutual exclusion)에서 온 단어로 여러 thread 들이 동시에 어떠한 코드에 접근하는 것을 배제한다는 의미이다.

mutex 객체의 lock method를 호출하면 thread에서 mutex 객체의 사용권한을 요청한것이며 한번의 한 thread만 mutex 객체의 사용권한을 얻을 수 있다. 따라서, 현재 mutex 객체의 사용권한을 얻은 thread가 unlock method로 사용권한을 반환하기 전까지 다른 thread들은 계속 사용권한을 요청한 상태로 대기하게 된다.

즉, lock과 unlock 사이에 코드는 mutex 객체의 사용권한을 얻은 단 하나의 thread만 사용할 수 있으며 이 코드 영역을 `임계 영역(critical section)`이라고 한다.

```cpp
void counter_worker2(int* result, std::mutex* m)
{
  for (int i = 0; i < 100000; ++i)
  {
    m->lock();
    ++(*result);
    m->unlock();
  }
}

TEST(multi_thread, counter2)
{
  constexpr int num_thread = 4;

  std::vector<std::thread> workers;
  workers.reserve(num_thread);

  std::mutex m;

  int count = 0;
  for (int i = 0; i < num_thread; ++i)
    workers.push_back(std::thread(counter_worker2, &count, &m));

  for (auto& worker : workers)
    worker.join();

  constexpr int ref = 400000;
   EXPECT_EQ(count, ref);
}
```

만약, 실수해서 unlock을 하지 않으면 프로그램이 끝나지 않게 된다.

왜냐하면 mutex를 취득한 thread가 unlock을 하지 않았음으로, 다른 모든 thread들은 `m->lock()`에서 권한을 획득받지 못해 return 되지 못한채로 대기하게 된다. 그리고 심지어 mutex를 취득한 thread 또한 `m->lock()`을 다시 호출하게 되고 권한을 획득받지 못해 return 되지 못한채로 대기한다.

결국 모든 쓰레드가 연산을 진행하지 못하고 무한정 대기하여 프로그램이 끝나지 않게 되며 이런 상황을 `데드락(dead lock)`이라고 한다.

반드시 mutex를 lock한 뒤에 반드시 unlock해야 하는 문제를 해결하기 위해, 스택에 있는 객체를 통해 lock과 unlock을 관리하는 c++에서는 `lock_gaurd` 클래스를 제공한다.

```cpp
void counter_worker3(int* result, std::mutex* m)
{
  for (int i = 0; i < 1000000; ++i)
  {
    std::lock_guard<std::mutex> lock(*m);
    ++(*result);
  }
}
```

lock_guard 객체를 생성할 때, `m->lock()`가 실행되고 객체가 소멸될 때, `m->unlock()`이 실행된다고 볼 수 있다.

> Reference  
> [modoocode](https://modoocode.com/270)  

### Producer and Consumer Pattern
`생산자 소비자 패턴(Producer and Consumer Pattern)`은 멀티 쓰레드 프로그램에서 자주 등장하는 패턴이다.

Producer의 경우 무언가 처리할 데이터를 받아오는 쓰레드이고 Consumer의 경우 데이터를 처리하는 쓰레드이다.

예를 들어서 DB에 데이터를 요청해서 가공하는 경우라고 해보자.

그러면 Producer는 DB에 처리할 데이터를 요청하여 받아오는 쓰레드이고 Consumer는 받은 데이터를 처리하는 쓰레드 일 것이다.

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

## std::recursive_mutex
std::recursive_mutex 는 C++ 표준 라이브러리에서 제공하는 뮤텍스의 한 종류로, 동일한 스레드가 동일한 뮤텍스를 여러 번 잠글 수 있게 허용하는 특수한 뮤텍스다.

일반 뮤텍스(std::mutex)와 달리, recursive_mutex 는 한 스레드가 이미 잠금 상태인 뮤텍스를 재차 획득해도 데드락(deadlock) 없이 정상적으로 실행된다.

특정한 경우(예: 재귀 호출, 또는 동일 스레드에서 여러 개의 함수를 호출할 때 각 함수가 같은 뮤텍스를 잠글 필요가 있을 때), 동일 스레드가 반복적으로 뮤텍스를 잠가야 하는 상황이 발생하는데 std::recursive_mutex 를 사용하면 하나의 스레드가 여러 번 같은 뮤텍스를 잠그고, 나중에 획득한 만큼 풀어주는(unlock) 것이 가능하다

단, 잠금 횟수만큼 정확하게 unlock해야 한다.

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

중첩된 lock을 다른 순서로 다른 함수에서 사용할 경우 mutex lock 메서드 호출 후 unlock 메서드를 호출 했음에도 lock이 얽히는 경우 데드락이 발생한다. 이 경우는 특정 뮤텍스에 우선순위를 주어 해결할 수 있지만 이 경우 특정 스레드만 많은 일을 하게 되는 기아 상태(starvation)이 발생할 수 있다. 따라서 애초에 중첩된 lock을 사용하지 않거나 lock을 언제나 정해진 순서로 획득해야 한다.
