# Linking Method
컴파일러가 여러 목적 파일들을 링킹하는 방식은 `정적 링킹(static linking)`과 `동적 링킹(dynamic linking)` 두 가지로 구분된다. 정적 링킹은 `정적 라이브러리(static library)`를 링킹하는 방식이고, 동적 링킹은 `동적 라이브러리(dynamic library)`, 다른 말로 `공유 라이브러리(shared library)`를 링킹하는 방식이다. 

이 두 라이브러리의 차이를 이해하기 위해서 먼저 정적 라이브러리가 뭔지 알아보자.