# Texture

## Texture 2D
Texture 2D 변수는 다음과 같은 형태를 가지고 있다.
```
Texture2D<Type> Name;
```

> Reference  
> [learn.microsoft - windows/win32/direct3dhlsl/dx-graphics-hlsl-to-type](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/dx-graphics-hlsl-to-type)  


### 멤버함수  

지원하는 멤버함수는 [learn.microsoft - windows/win32/direct3dhlsl/sm5-object-texture2d](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d)를 참고하면 된다.

#### GetDimensions
Texture2D의 dimension 정보를 리턴한다.

함수의 시그니쳐는 다음과 같다.
```
void GetDimensions(
  in  uint MipLevel,
  out uint Width,
  out uint Height,
  out uint NumberOfLevels
);
```

다음과 같은 overloading도 제공한다.
```
void GetDimensions(
  out uint Width,
  out uint Height,
);
```

> Reference  
> [learn.microsoft - windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions](https://learn.microsoft.com/en-us/windows/win32/direct3dhlsl/sm5-object-texture2d-getdimensions)  