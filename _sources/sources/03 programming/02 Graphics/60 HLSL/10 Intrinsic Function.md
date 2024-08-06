# Intrinsic Function

> Reference  
> [learn.microsoft - dx-graphics-hlsl-intrinsic-functions](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-intrinsic-functions)  

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