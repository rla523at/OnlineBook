# Chrono
Chrono 는 시간과 관련된 기능을 제공하는 C++ 표준 라이브러리다.

Chrono 는 크게 Clocks, Time point, Duration 이라는 3가지 요소들로 구성되어 있다.

* Clocks 은 시간을 어떻게 나타낼지 결정하는 요소로 각각의 clock 마다 고유의 시작점과 tick rate 가 있다.
* Time Point 는 특정 시간을 나타내는 요소로, clock 의 고유의 시작점으로 부터 몇번의 tick 이 발생했는지를 나타낸다.
* Duration 은 시간의 간격을 나타내는 요소로, 두개의 Time Point 사이에 몇번의 tick 이 발생했고 한 틱이 몇초의 시간을 의미하는지를 나타낸다.
  *  Period : 1 틱이 몇 초인지를 나타낸다.

> Referecne  
> [cppreference - chrono](https://en.cppreference.com/w/cpp/chrono)  
> [모두의코드 - 씹어먹는 C++ - <17 - 3. 난수 생성(<random>)과 시간 관련 라이브러리(<chrono>) 소개>](https://modoocode.com/304)  

## Time Point 변환
임의의 Clock 에서 얻은 Time Point 를 다른 Clock 에서 얻은 Time Point 로 변환하기 위해서는 std::chrono::clock_cast 함수를 사용하면 된다.

```cpp
  std::chrono::file_clock::time_point   fc_now_tp = std::chrono::file_clock::now();  
  std::chrono::system_clock::time_point sc_now_tp = std::chrono::clock_cast<std::chrono::system_clock>( fc_now_tp );
```


