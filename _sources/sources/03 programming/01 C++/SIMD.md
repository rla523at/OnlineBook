# SIMD

* https://learn.microsoft.com/en-us/cpp/intrinsics/x64-amd64-intrinsics-list?view=msvc-170
  * This document lists intrinsics that the Microsoft C++ compiler supports when x64 (also referred to as amd64) is targeted.
  * _mm_add_ps: float4 벡터 덧셈
  * _mm_sub_ps: float4 벡터 뺄셈
  * _mm_mul_ps: float4 벡터  compoenent wise 곱셈 
  * _mm_set1_ps: 동일한 실수값을 갖는 float4 벡터 만들기
   * 실수 곱을 만들려면 이 함수를 사용하고 _mm_mul_ps 사용

__m128
* https://learn.microsoft.com/en-us/cpp/cpp/m128?view=msvc-170
 * You should not access the __m128 fields directly. You can, however, see these types in the debugger. A variable of type __m128 maps to the XMM[0-7] registers.
 
* SSE
  * Streaming SIMD Extensions
  * https://en.wikipedia.org/wiki/Streaming_SIMD_Extensions
