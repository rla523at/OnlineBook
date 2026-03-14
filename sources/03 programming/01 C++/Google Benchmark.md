# Google Benchmark
## 설치
vcpkg를 통해 설치할 수 있다.

```
vcpkg install benchmark:x64-windows
```

## 공통 Data 초기화하기
### 초기화 객체 만들기
Bench mark에서 공통으로 사용할 data를 static으로 생성한 후 생성자에서 공통 data를 초기화하는 클래스를 만들어서 static 객체를 만들어준다.

```cpp
 static std::vector<int> _unique_data;

 class Init
{
 public:
   Init(void)
   {
     _unique_data.resize(n);

     std::random_device              rd;
     std::mt19937                    gen(rd());
     std::uniform_int_distribution<> dis(1, 10000);

     std::generate(_unique_data.begin(), _unique_data.end(), [&]() { return dis(gen); });
   }
 };

 static Init init;
```

### 나만의 main 함수 만들기
BENCHMARK_MAIN(); 매크로 대신에 main문을 만들어서 공통 데이터를 초기화하는 코드를 넣는다.

이 때, main 함수는 BENCHMARK_MAIN(); 매크로의 정의를 그대로 이용해서 만든다.

```cpp
int main(int argc, char** argv)
{
  char  arg0_default[] = "benchmark";
  char* args_default   = arg0_default;
  if (!argv)
  {
    argc = 1;
    argv = &args_default;
  }

  //my initialization
 
  ::benchmark::Initialize(&argc, argv);
  if (::benchmark::ReportUnrecognizedArguments(argc, argv)) return 1;
  ::benchmark::RunSpecifiedBenchmarks();
  ::benchmark::Shutdown();
  return 0;
}
```