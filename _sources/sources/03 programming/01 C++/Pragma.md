# Pragma

## pragma comment

```
#pragma comment(comment-type [ , "comment-string" ])
```

comment-type 은 predefined identifiers 중에 하나로 object file 또는 excutable file 에 기록될 comment 의 타입을 결정한다.

### lib
lib identifiers 는 library-search 와 관련된 기록을 object file 에 남긴다.

"comment-string" 위치에는 linker 가 탐색할 library 의 이름이나 경로를 작성하면 된다.

```
#pragma comment( lib, "A.lib" )
```

#### VS 링커 설정에서 추가 종속성 설정

| **특성**                       | **`#pragma comment(lib, "A.lib")`**               | **링커 설정에서 추가 종속성 설정** |
|--------------------------------|--------------------------------------------------|-----------------------------------|
| **설정 위치**                  | 소스 코드 내부 (`.cpp` 또는 `.h` 파일)            | Visual Studio 프로젝트 설정      |
| **적용 범위**                  | 해당 소스 파일에만 적용                          | 프로젝트 전체에 적용            |
| **협업 시 의존성 관리**         | 코드에 명시되어 있어 쉽게 추적 가능                | 프로젝트 설정에서 관리 필요     |
| **중복 선언 문제**              | 여러 파일에서 중복 선언될 수 있음                 | 프로젝트 설정에서 한 번만 설정  |
| **플랫폼 의존성**               | Visual Studio 전용(`pragma`는 표준 C++가 아님)    | 다양한 도구 및 IDE에 적용 가능   |
| **유연성**                     | 코드와 빌드 시스템이 밀접하게 결합               | 코드와 설정 분리, 유연한 관리 가능 |


> Reference  
> [learn.microsoft - comment pragma](https://learn.microsoft.com/ko-kr/cpp/preprocessor/comment-c-cpp?view=msvc-170)

