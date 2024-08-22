# Intrinsic Function

> Reference  
> [learn.microsoft - dx-graphics-hlsl-intrinsic-functions](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-intrinsic-functions)  

## All
모든 값이 0이면 Ture 아니면 False를 return 하는 함수이다.

시그니쳐는 다음과 같다.
```
ret all(x)
```

### vector 간의 equal 연산
예를 들어 uint3 변수 a,b가 있을 때, 다음과 같은 코드가 있다고 하자.
```cpp
if (a==b)
```

이 경우에 if 문에서 오류가 발생한다.

왜냐하면 uint3 끼리 비교연산을하면 bool3가 return이 되지만 if문에는 단일 bool값이 필요하기 때문이다.

이를 해결하기 위해서 다음과 같이 코드를 수정하면 된다.

```cpp
if(all(a==b))
```

> Reference  
> [learn.microsoft - dx-graphics-hlsl-all](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-all)  
> [learn.microsoft - dx-graphics-hlsl-operators#comparison-operators](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/dx-graphics-hlsl-operators#comparison-operators)  

## Any
하나라도 0이 아닌 값이 있으면 True 아니면 False를 return 하는 함수이다.

시그니쳐는 다음과 같다.
```
ret any(x)
```

### vector 간의 nequal 연산
예를 들어 uint3 변수 a,b가 있을 때, 다음과 같은 코드가 있다고 하자.
```cpp
if (a!=b)
```

이 경우에 if 문에서 오류가 발생한다.

왜냐하면 uint3 끼리 비교연산을하면 bool3가 return이 되지만 if문에는 단일 bool값이 필요하기 때문이다.

이를 해결하기 위해서 다음과 같이 코드를 수정하면 된다.

```cpp
if(any(a!=b))
```

> Reference  
> [learn.microsoft - dx-graphics-hlsl-any](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-any)

## firstbithigh
비트 중 가장 높은 위치에 있는 1값의 위치를 return 한다.

시그니쳐는 다음과 같다.
```
int2 firstbithigh(int2 value);
int3 firstbithigh(int3 value);
int4 firstbithigh(int4 value);
uint firstbithigh(uint value);
uint2 firstbithigh(uint2 value);
uint3 firstbithigh(uint3 value);
uint4 firstbithigh(uint4 value);
```

> Reference  
> [learn.microsoft - firstbithigh](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/firstbithigh)  

## InterlockedAdd
원자적 덧셈을 보장하는 함수이다.

여러 Thread Group간의 원자적 동작까지 보장한다.

시그니쳐는 다음과 같다.

```
void InterlockedAdd(
  in  R dest,
  in  T value,
  out T original_value
);
```

* dest
  * 기존의 값이 저장되어 있는 값이다.
  * 새로운 값이 저장된다.
* value
  * 더해질 값
* original_value
  * dest에 원래 있던 값이 return 된다.

> Reference  
> [learn.microsoft - direct3dhlsl/interlockedadd](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/interlockedadd)

## clip
인자가 0보다 작으면 현재 픽셀을 버린다.

시그니쳐는 다음과 같다.
```
clip(x)
```

> Reference  
> [learn.microsoft - hlsl-clip](https://learn.microsoft.com/ko-kr/windows/win32/direct3dhlsl/dx-graphics-hlsl-clip)  

## Length
floating point vector의 길이를 리턴한다.

시그니쳐는 다음과 같다.
```
ret length(x)
```

> Reference  
> [learn.microsoft - dx-graphics-hlsl-length](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-length)  

## dst
distance vector를 계산해준다.

시그니쳐는 다음과 같다.
```
fVector dst(
  in fVector src0,
  in fVector src1
);
```

distance vector의 정의는 다음과 같다.

```
dest.x = 1;
dest.y = src0.y * src1.y;
dest.z = src0.z;
dest.w = src1.w;
```

> Reference  
> [learn.microsoft - direct3dhlsl/dst](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dst)  
> [learn.microsoft - direct3dhlsl/dst---vs](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dst---vs)  
> [stackoverflow - what-is-the-hlsl-dst-instruction-for](https://stackoverflow.com/questions/8525803/what-is-the-hlsl-dst-instruction-for)  

## GroupMemoryBarrierWithGroupSync 
GroupMemoryBarrierWithGroupSync 함수는 HLSL(High-Level Shader Language)에서 사용하는 동기화 함수로, 주로 Compute Shader에서 워크그룹 내 스레드 간의 메모리 일관성을 보장하고 동기화하는 데 사용된다. 

이 함수는 그룹 공유 메모리에 대한 모든 이전 읽기 및 쓰기 연산이 완료될 때까지 현재 스레드를 일시 중지시키고, 같은 워크그룹 내의 모든 스레드가 이 함수에 도달할 때까지 기다린다.

> Reference  
> [learn.microsoft - windows/win32/direct3dhlsl/groupmemorybarrierwithgroupsync](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/groupmemorybarrierwithgroupsync)

### 조건문
다음과 같은 코드가 있다고 하자.

```
for (uint stride = NUM_THREAD / 2; 0 < stride; stride /= 2)
{
  if (stride <= Gindex)
    continue;
  
  shared_data[Gindex] = max(shared_data[Gindex], shared_data[Gindex + stride]);
  
  GroupMemoryBarrierWithGroupSync();
}
```

이 경우에는 Gindex가 stride보다 큰 경우에는 GroupMemoryBarrierWithGroupSync() 함수에 도달할 수 없다. 즉, 모든 thread들이 GroupMemoryBarrierWithGroupSync()에 도달할 수 없는 경우이다.

이 경우에는 컴파일 에러가 발생한다.

이를 해결하기 위해서는, 모든 thread들이 GroupMemoryBarrierWithGroupSync 함수에 도달할 수 있게 아래와 같이 수정하면 된다.

```
  for (uint stride = NUM_THREAD / 2; 0 < stride; stride /= 2)
  {
    if (Gindex < stride)
      shared_data[Gindex] = max(shared_data[Gindex], shared_data[Gindex + stride]);
    
    GroupMemoryBarrierWithGroupSync();
  }
```