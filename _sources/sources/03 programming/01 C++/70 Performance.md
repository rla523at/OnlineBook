# Performance

## If in For VS Two For
### 상황
특정 index에서만 다른 동작을 하고 그 외에는 일관된 동작을 하는 경우이다.

### 결론
For loop를 2개로 나누는게 For loop안에 if를 넣는것 보다 "약간" 빠르다.

### 코드
```cpp
//detail : For_if_two_for.cpp 참고

// for loop 안에 if가 있는 경우
static void BM_if(benchmark::State& state)
{
  const int  n     = static_cast<int>(state.range(0));
  const auto data  = make_data(n);
  const auto index = make_index(n);

  for (auto _ : state)
  {
    int sum = 0;
    benchmark::DoNotOptimize(sum);

    for (int i = 0; i < n; ++i)
    {
      if (i != index)
        sum += data[i];
    }
  }
}

// for loop가 둘로 나뉜 경우
static void BM_two_for(benchmark::State& state)
{
  const int  n     = static_cast<int>(state.range(0));
  const auto data  = make_data(n);
  const auto index = make_index(n);

  for (auto _ : state)
  {
    int sum = 0;
    benchmark::DoNotOptimize(sum);

    for (int i = 0; i < index; ++i)
      sum += data[i];

    for (int i = index + 1; i < n; ++i)
      sum += data[i];
  }
}
```

### 결과

```{figure} _image/7001.png
```