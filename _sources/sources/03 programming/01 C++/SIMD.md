# SIMD

* https://learn.microsoft.com/en-us/cpp/intrinsics/x64-amd64-intrinsics-list?view=msvc-170
  * This document lists intrinsics that the Microsoft C++ compiler supports when x64 (also referred to as amd64) is targeted.
  * _mm_add_ps: float4 벡터 덧셈
  * _mm_sub_ps: float4 벡터 뺄셈
  * _mm_mul_ps: float4 벡터  compoenent wise 곱셈 
  * _mm_set1_ps: 동일한 실수값을 갖는 float4 벡터 만들기
   * 실수 곱을 만들려면 이 함수를 사용하고 _mm_mul_ps 사용
  * _mm_fmadd_ps : float4 벡터 a*b + c
  * _mm256_set_pd: double4 벡터에 4개의 인자를 대입하기
  * _mm256_setzero_pd : double4 벡터를 0.0 으로 초기화
  * _mm_cvtsd_f64 : double2 벡터의 첫번째 요소를 copy 해온다.
  * _mm256_extractf128_pd : double4 벡터에서 double2 벡터를 추출해온다.

__m128
* https://learn.microsoft.com/en-us/cpp/cpp/m128?view=msvc-170
 * You should not access the __m128 fields directly. You can, however, see these types in the debugger. A variable of type __m128 maps to the XMM[0-7] registers.
 
* SSE
  * Streaming SIMD Extensions
  * https://en.wikipedia.org/wiki/Streaming_SIMD_Extensions


## Alignment Requirement

alignas key word 를 사용하면 시작 주소와 크기가 주어진 값으로 align 된다.
```cpp
struct alignas(8) S1
{
    int x;
};

static_assert(alignof(S1) == 8, "alignof(S1) should be 8");
```

> Reference  
> [learn.microsoft - alignas-specifier](https://learn.microsoft.com/en-us/cpp/cpp/alignas-specifier?view=msvc-170)  

* Alignement
 * https://learn.microsoft.com/en-us/cpp/cpp/alignment-cpp-declarations?view=msvc-170
 * One of the low-level features of C++ is the ability to specify the precise alignment of objects in memory to take maximum advantage of a specific hardware architecture.
 * Before Visual Studio 2015 you could use the Microsoft-specific keywords __alignof and __declspec(align) to specify an alignment greater than the default.
 * Starting in Visual Studio 2015 you should use the C++11 standard keywords alignof and alignas for maximum code portability.
 * The C++ standard doesn't specify packing behavior for alignment on boundaries smaller than the compiler default for the target platform, so you still need to use the Microsoft #pragma pack in that case.

* Alignment and Memory Address
 * https://learn.microsoft.com/en-us/cpp/cpp/alignment-cpp-declarations?view=msvc-170#alignment-and-memory-addresses
 * Alignment is a property of a memory address, expressed as the numeric address modulo a power of 2.
 * An address is said to be aligned to X if its alignment is Xn+0.
 * We call a datum naturally aligned if its address is aligned to its size. It's called misaligned otherwise.

* compiler-handling-of-data-alignment
 * https://learn.microsoft.com/en-us/cpp/cpp/alignment-cpp-declarations?view=msvc-170#compiler-handling-of-data-alignment
 * 
