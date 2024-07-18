# OpenMP

## omp 함수
### omp_get_max_threads
프로그램이 병렬 영역을 진입할 때 사용할 수 있는 최대 스레드 수를 반환한다.

### omp_get_num_threads
현재 병렬 영역에서 사용되는 스레드의 수를 반환한다.

### omp_get_thread_num
현재 병렬 영역에서 실행중인 스레드의 아이디를 반환한다.

## Visaul Studio Setting
OpenMP를 visual studio 프로젝트에 사용하려면 프로젝트 설정을 수정해야 한다.

```
프로젝트 속성 >> C/C++ >> 언어 >> OpenMP 지원 >> 예
```

## paralleize fol loop 
for loop를 병렬화 하기 위해서는 `#pragma omp parallel for` 구문을 추가하면 된다.

```cpp
#include <iostream>
#include <omp.h>

int main(void)
{
	constexpr int n = 5;

#pragma omp parallel for
	for (int i = 0; i < n; ++i)
	{
		for (int j = 0; j < n; ++j)
		{
			printf("i = %d, j= %d, threadId = %d \n", i, j, omp_get_thread_num());
		}
		printf("\n\n");
	}
}
```

### 주의사항
모든 for loop를 병렬화 할 수 있는것은 아니다.

data dependency가 있는 경우, 결과값이 잘못 될 수 있다.

```cpp
#include <vector>
#include <iostream>
#include <omp.h>

int main(void)
{
	constexpr int n = 10;
	std::vector<int> vec(n, 1);

#pragma omp parallel for
	for (int i = 1; i < n; ++i)
	{
		vec[i] += vec[i - 1];
	}

	for (int i = 1; i < n - 1; ++i)
	{
		if (vec[i] != i + 1)
			printf("index %d has %d, it should be %d \n", i, vec[i], i + 1);
	}
}
```
