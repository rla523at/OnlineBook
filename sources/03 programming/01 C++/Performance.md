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

## If in For VS Two For Version2
### 상황
특정 index보다 큰 경우와 작은 경우 동작이 다른 상황

### 결론
For loop를 2개로 나누는게 For loop안에 if를 넣는것 보다 "약간" 빠르다.

### 코드
```cpp
//detail : For_if_two_for_v2.cpp 참고

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
      if (i < index)
        sum += data[i];
      else
        sum -= data[i];
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

    for (int i = index; i < n; ++i)
      sum -= data[i];
  }
}
```

### 결과

```{figure} _image/7002.png
```

## Find Second Max Element 
### 상황
주어진 임의의 배열에서 두번째로 큰 원소를 찾는데, Winner Tree를 이용한 방법과 단순한 방법을 비교한다.

### 결론
Winner Tree를 이용한 방법보다 단순한 방법이 빠르다.

비교 횟수만 보면 Winner Tree는 $n + \lceil \log_2n \rceil -2$번이고 배열 순회 방식은 $2n-3$번이라 Winner Tree방식이 더 적다.

하지만 실제로 코드를 성능 프로파일링 해보면 Winner Tree를 이용한 알고리즘은 대부분의 비용이 Winner Tree를 만들때 발생한다.

즉, Winner Tree를 만들기 위한 비용이 비교 횟수가 줄어듬에 따라 줄어든 비용보다 커 오히려 느려진다.

```{figure} _image/7004.png
```

### 코드
```cpp
//detail : SecondMax.cpp 참고

static int find_second_max_with_winner_tree_opt2(const std::vector<int>& data)
{
  int max2 = std::numeric_limits<int>::min();

  const int n = static_cast<int>(data.size());
  const int h = static_cast<int>(std::ceil(std::log2(n)));

  // intialize leaf nodes
  {
    const int start = static_cast<int>(std::pow(2, h));
    std::copy(data.begin(), data.end(), _winner_tree.begin() + start);
  }

  // fill other nodes
  {
    const int start = static_cast<int>(std::pow(2, h)) - 1;
    for (int i = start; i > 0; --i)
      _winner_tree[i] = std::max(_winner_tree[2 * i], _winner_tree[2 * i + 1]);
  }

  const int max = _winner_tree[1];

  int index = 1;
  for (int i = 0; i < h; ++i)
  {
    const int left_child  = index * 2;
    const int right_child = left_child + 1;

    if (_winner_tree[left_child] == max)
    {
      index = left_child;
      max2  = std::max(max2, _winner_tree[right_child]);
    }
    else
    {
      index = right_child;
      max2  = std::max(max2, _winner_tree[left_child]);
    }
  }

  return max2;
}

int find_second_max_simple(const std::vector<int>& data)
{
  const int ndata = data.size();

  int max1 = 0;
  int max2 = 0;

  if (data[0] < data[1])
  {
    max1 = data[1];
    max2 = data[0];
  }
  else
  {
    max1 = data[0];
    max2 = data[1];
  }

  for (int i = 2; i < ndata; ++i)
  {
    if (data[i] > max1)
    {
      max2 = max1;
      max1 = data[i];
    }
    else if (data[i] > max2)
    {
      max2 = data[i];
    }
  }

  return max2;
}

```

### 결과

```{figure} _image/7003.png
```

### 참고
BM_winner_tree_opt는 heap allocation이 발생하는 횟수를 줄이는 방식으로 최적화 한 알고리즘이다. 자세히 말하자면, make_winner_tree 함수에서 vector 생성이 이루어지지 않게 미리 만들어진 vector를 사용한다. 이럴 경우 2배 정도 성능이 개선된다.

BM_winner_tree_opt2는 공간을 조금 더 써 Winner Tree 만드는 알고리즘을 조금 더 단순한 코드로 가능하게 최적화 한 알고리즘이다. 자세히 말하자면, winner tree 배열을 BM_winner_tree_opt에서는 $2^h - 1 + n$개 만큼 사용하였다면, BM_winner_tree_opt2에서는 $2^{h+1} - 1$개 만큼 사용한다. 이럴 경우 코드는 많이 간결해지지만, 성능 차이는 없다.

```{figure} _image/7005.png
```