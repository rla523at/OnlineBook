# ID3D12Device

## CreateCommandAllocator 멤버함수
CreateCommandAllocator 함수는 ID3D12Device 인터페이스의 멤버 함수로, DirectX 12의 커맨드 리스트를 기록하기 위한 커맨드 할당기를 생성하는 함수다. 이 함수는 GPU 커맨드의 실행을 위해 메모리를 할당하는 객체를 만든다.

시그니처는 다음과 같다.

```cpp
HRESULT ID3D12Device::CreateCommandAllocator(
    D3D12_COMMAND_LIST_TYPE type,
    REFIID riid,
    void **ppvObject
);
```

매개변수는 다음과 같다.

* D3D12_COMMAND_LIST_TYPE type
  * 생성할 커맨드 할당기의 타입을 지정
  * 사용 가능한 값
    * D3D12_COMMAND_LIST_TYPE_DIRECT: 직접 커맨드 리스트
    * D3D12_COMMAND_LIST_TYPE_BUNDLE: 번들 커맨드 리스트
    * D3D12_COMMAND_LIST_TYPE_COMPUTE: 컴퓨트 커맨드 리스트
    * D3D12_COMMAND_LIST_TYPE_COPY: 복사 커맨드 리스트
  * 기본값: 없음

* REFIID riid
  * 요청하는 인터페이스의 고유 식별자 (GUID)
  * 사용 가능한 값
    * IID_ID3D12CommandAllocator: ID3D12CommandAllocator 인터페이스
  * 기본값: 없음

* void** ppvObject
  * 생성된 커맨드 할당기의 인터페이스 포인터를 받을 변수
  * 사용 가능한 값
    * 유효한 포인터
    * NULL: 포인터가 필요함
  * 기본값: 없음

반환값은 다음과 같다.

* S_OK: 커맨드 할당기가 성공적으로 생성됨
* E_OUTOFMEMORY: 메모리 부족
* E_INVALIDARG: 잘못된 인수
* 기타 HRESULT 오류 코드

이 함수는 커맨드 리스트의 기록을 위해 메모리를 할당하는 `ID3D12CommandAllocator` 객체를 생성하며, 다양한 커맨드 리스트 유형을 지원한다. `type` 매개변수는 커맨드 할당기의 용도를 결정하며, `riid`와 `ppvObject`를 통해 요청된 인터페이스를 반환받는다. 이 함수는 주로 GPU 명령의 효율적인 관리를 위해 사용된다.
