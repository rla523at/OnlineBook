# DirectXMath
DirectXMath는 Microsoft에서 제공하는 벡터 및 행렬 연산 라이브러리다.

## XMVerifyCPUSupport
DirectXMath 라이브러리가 현재 플랫폼을 지원하는지 나타내는 함수이다.

directxmath.h 에 정의되어 있다.

함수의 signature 는 다음과 같다.

```cpp
bool XMVerifyCPUSupport() noexcept;
```

반환값은 다음과 같다.
* DirectXMath 라이브러리가 지정된 플랫폼을 지원하는 경우
  * true
* DirectXMath 라이브러리가 지정된 플랫폼을 지원하지 않는 경우
  * false
