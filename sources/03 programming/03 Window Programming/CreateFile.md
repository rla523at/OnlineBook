# CreateFileA

CreateFileA는 Windows API에서 제공되는 함수로, 파일이나 장치(예: 디스크, 파이프, 포트)와의 I/O 작업을 위한 핸들을 생성하거나 여는 함수이다. 

이 함수는 ANSI 문자열을 사용해 경로를 지정하며, 파일 생성, 읽기, 쓰기, 삭제 등의 작업을 수행하기 위해 사용된다.

함수의 signature 는 다음과 같다.

```cpp
HANDLE CreateFileA(
  [in]           LPCSTR                lpFileName,
  [in]           DWORD                 dwDesiredAccess,
  [in]           DWORD                 dwShareMode,
  [in, optional] LPSECURITY_ATTRIBUTES lpSecurityAttributes,
  [in]           DWORD                 dwCreationDisposition,
  [in]           DWORD                 dwFlagsAndAttributes,
  [in, optional] HANDLE                hTemplateFile
);
```

* DWORD dwFlagsAndAttributes
  * 파일 또는 디바이스 특성 및 플래그이다.
  * 사용 가능한 파일 특성(FILE_ATTRIBUTE_*)의 조합을 포함할 수 있다.
  * 파일 또는 디바이스 캐싱 동작, 액세스 모드 및 기타 특수 용도 플래그를 제어하기 위한 플래그(FILE_FLAG_*)의 조합을 포함할 수 있다.

## FILE_FLAG_NO_BUFFERING
파일 또는 디바이스가 데이터 읽기 및 쓰기에 대한 시스템 캐싱 없이 열리도록 한다.

이 플래그를 사용하여 CreateFile 로 열린 파일을 성공적으로 사용하기 위해서는 파일 버퍼링과 관련된 엄격한 요구 사항을 만족해야 한다.




## std::ifstream
CreateFileA는 저수준 파일 I/O를 제공하며, 파일 속성 제어, 비동기 I/O 등 고급 기능을 필요로 할 때 적합하다. 

반면에, std::ifstream은 고수준 C++ 스트림 API로 간단한 파일 읽기 작업을 효율적으로 처리한다.

| **특성**                      | **CreateFileA**                             | **std::ifstream** |
|--------------------------------|---------------------------------------------|------------------|
| **플랫폼 의존성**               | **Windows 전용**                           | **플랫폼 독립적** |
| **제어 수준**                  | **고급 제어 가능** (파일 속성, 비동기 I/O 등) | 단순 파일 I/O 용도 |
| **비동기 작업 지원**            | **지원** (`OVERLAPPED` 구조체 사용)         | 지원하지 않음 |
| **복잡성**                     | **더 복잡** (오류 처리 필요)                | 사용이 **간단** |
| **파일 외 장치 접근**           | **지원** (디바이스, 파이프 등)              | 지원하지 않음 |
| **유니코드 지원**              | `CreateFileW` 사용 필요                     | 기본적으로 **유니코드 지원** |
| **스트림 기반 처리**           | 지원하지 않음                               | **스트림** 처리에 적합 |
| **성능**                       | 저수준 접근으로 **고성능**                  | 간단한 파일 I/O에는 충분 |
