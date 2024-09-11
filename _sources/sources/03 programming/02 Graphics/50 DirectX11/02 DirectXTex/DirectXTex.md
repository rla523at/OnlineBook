# DirectXTex
DirectXTex는 Microsoft가 개발한 DirectX Toolkit의 일부로, 텍스처 처리와 관련된 유틸리티 및 라이브러리다. 

## LoadFromWICFile 함수
LoadFromWICFile 함수는 DirectXTex 라이브러리의 함수로, Windows Imaging Component (WIC) 파일에서 이미지를 로드하는 함수다. 

이 함수는 이미지 파일을 읽고 DirectX 텍스처로 변환할 때 사용된다.

시그니처는 다음과 같다.

```cpp
HRESULT LoadFromWICFile(
    _In_z_ LPCWSTR szFileName,
    _In_ DWORD flags,
    _Out_opt_ TexMetadata* pMetadata,
    _Out_opt_ ScratchImage& image
);
```

매개변수는 다음과 같다.

* LPCWSTR szFileName
  * 로드할 이미지 파일의 경로와 파일 이름
  * 사용 가능한 값
    * 유효한 문자열 포인터 (널 종료 문자열)
  * 기본값: 없음

* DWORD flags
  * 로드 옵션을 지정하는 플래그
  * 사용 가능한 값
    * 예: WIC_FLAGS_NONE, WIC_FLAGS_FORCE_SRGB 등
  * 기본값: 없음

* TexMetadata* pMetadata
  * 로드된 이미지의 메타데이터를 받을 TexMetadata 구조체의 포인터
  * 사용 가능한 값
    * NULL: 메타데이터를 받지 않음
    * 유효한 TexMetadata 구조체의 포인터
  * 기본값: NULL

* ScratchImage& image
  * 로드된 이미지 데이터를 포함할 ScratchImage 객체의 참조
  * 사용 가능한 값
    * 이미 초기화된 ScratchImage 객체
  * 기본값: 없음

## CreateTextureEx 함수
CreateTextureEx 함수는 DirectXTex 라이브러리의 함수로, DirectX 11 텍스처를 생성하는 함수다. 

이 함수는 텍스처를 초기화하고 생성할 때 사용된다.

시그니처는 다음과 같다.

```cpp
HRESULT __cdecl CreateTextureEx(
    _In_ ID3D11Device* pDevice,
    _In_reads_(nimages) const Image* srcImages,
    _In_ size_t nimages,
    _In_ const TexMetadata& metadata,
    _In_ D3D11_USAGE usage,
    _In_ unsigned int bindFlags,
    _In_ unsigned int cpuAccessFlags,
    _In_ unsigned int miscFlags,
    _In_ CREATETEX_FLAGS flags,
    _Outptr_ ID3D11Resource** ppResource
) noexcept;
```

매개변수는 다음과 같습니다.

* `ID3D11Device* pDevice`
  * 텍스처를 생성할 DirectX 장치 인터페이스
  * 사용 가능한 값
    * 유효한 ID3D11Device 객체
  * 기본값: 없음

* `const Image* srcImages`
  * 텍스처를 생성할 이미지 데이터 배열
  * 사용 가능한 값
    * Image 구조체 배열의 유효한 메모리 주소
  * 기본값: 없음

* `size_t nimages`
  * 생성할 이미지 데이터 배열의 요소 수
  * 사용 가능한 값
    * 0 이상의 정수 값
  * 기본값: 없음

* `const TexMetadata& metadata`
  * 텍스처의 메타데이터를 포함하는 TexMetadata 구조체
  * 사용 가능한 값
    * 유효한 TexMetadata 구조체
  * 기본값: 없음

* `D3D11_USAGE usage`
  * 텍스처의 사용 방법을 지정하는 열거형 값
  * 사용 가능한 값
    * D3D11_USAGE_DEFAULT: 기본 사용 방법
    * D3D11_USAGE_IMMUTABLE: 불변 데이터
    * D3D11_USAGE_DYNAMIC: 자주 갱신되는 데이터
    * D3D11_USAGE_STAGING: 리소스 복사용
  * 기본값: 없음

* `unsigned int bindFlags`
  * 텍스처를 바인딩할 파이프라인 스테이지를 지정하는 비트 플래그
  * 사용 가능한 값
    * 예: D3D11_BIND_SHADER_RESOURCE, D3D11_BIND_RENDER_TARGET 등
  * 기본값: 없음

* `unsigned int cpuAccessFlags`
  * CPU 접근을 제어하는 비트 플래그
  * 사용 가능한 값
    * 0: CPU 접근 불가
    * D3D11_CPU_ACCESS_WRITE: 쓰기 접근 가능
    * D3D11_CPU_ACCESS_READ: 읽기 접근 가능
  * 기본값: 없음

* `unsigned int miscFlags`
  * 기타 텍스처 옵션을 제어하는 비트 플래그
  * 사용 가능한 값
    * 예: D3D11_RESOURCE_MISC_TEXTURECUBE, D3D11_RESOURCE_MISC_GENERATE_MIPS 등
  * 기본값: 없음

* `CREATETEX_FLAGS flags`
  * 텍스처 생성 플래그를 지정하는 열거형 값
  * 사용 가능한 값
    * 특정 CREATETEX_FLAGS 값 (라이브러리에 따라 다름)
  * 기본값: 없음

* `ID3D11Resource** ppResource`
  * 생성된 텍스처 리소스에 대한 포인터의 포인터
  * 사용 가능한 값
    * NULL: 유효한 텍스처 리소스를 반환하지 않음
    * 유효한 ID3D11Resource 객체의 포인터
  * 기본값: NULL
```