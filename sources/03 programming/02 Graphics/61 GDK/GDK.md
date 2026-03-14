# GDK
Microsoft GDK (Game Development Kit) 는 Xbox 및 Windows 플랫폼에서 게임을 개발하기 위한 Microsoft의 공식 게임 개발 키트이다.

Microsoft의 Xbox 콘솔 및 Windows 10/11을 포함한 다양한 플랫폼에서 게임을 제작하고 배포하는 데 필요한 도구와 라이브러리를 제공한다.

Microsoft GDK는 Xbox Series X|S, Xbox One, 그리고 Windows PC에서 게임을 개발할 수 있도록 통합된 환경을 제공한다.

## GRDK(Game runtime Development Kit)

## GXDK(Game eXtension Development Kit)
GXDK는 Xbox 개발 환경에서 확장된 기능을 제공하는 키트다.

GRDK의 기능에 더하여 특정 기능을 실험하거나 플랫폼 확장 기능을 사용하고자 하는 개발자들을 위해 설계되었다.

## Memory
### Title Memory
타이틀 메모리는 게임 타이틀이 실행될 때 해당 게임이 사용하는 메모리이다.

### Tools Memory
툴 메모리는 게임 개발을 지원하는 디버깅 및 개발 도구들이 사용하는 메모리이다.

Xbox 개발 키트(GDK)나 디버거, 성능 모니터링 도구와 같은 비게임 기능에 할당된다.

## XGameRuntimeInitialize 함수
XGameRuntimeInitialize는 Microsoft GDK 의 게임 런타임 기능을 초기화하는 함수이다.

이 함수는 Windows 및 Xbox 플랫폼에서 개발되는 게임이 Microsoft의 다양한 서비스와 기능을 사용하기 전에 필수적으로 호출해야 한다. 예를 들어, Xbox Live 서비스, XSAPI와 같은 기능을 사용하려면 이 런타임 초기화가 필요하다.

함수의 시그니쳐는 다음과 같다.
```cpp
HRESULT XGameRuntimeInitialize()
```

반환값은 다음과 같다.
* 성공한 경우
  * S_OK
* 실패한 경우
  * [Error Code](https://learn.microsoft.com/ko-kr/gaming/gdk/_content/gc/reference/errorcodes)  


> Reference  
> [learn.microsoft - xgameruntimeinitialize](https://learn.microsoft.com/ko-kr/gaming/gdk/_content/gc/reference/system/xgameruntimeinit/functions/xgameruntimeinitialize)

## Xbox Services API
Xbox Services API(XSAPI) 는 Xbox Live 와 같은 Microsoft 의 게임 관련 서비스에 접근할 수 있도록 해주는 API다. 

이 API는 게임 개발자가 Xbox 네트워크 기능을 통합하고 멀티플레이어, 도전 과제, 리더보드 같은 기능을 구현할 수 있도록 설계되었다. 

XSAPI는 Microsoft GDK의 일부로 제공되며 Windows와 Xbox 플랫폼에서 사용된다.

> Reference  
> [learn.microsoft - live-gs-xbl-apis](https://learn.microsoft.com/en-us/gaming/gdk/_content/gc/live/get-started/live-gs-xbl-apis)  

## NoAccess

https://learn.microsoft.com/en-us/gaming/gdk/_content/gc/reference/system/xmem/functions/xmemgetworkingsetstatistics

https://learn.microsoft.com/en-us/gaming/gdk/_content/gc/reference/system/xmem/structs/xmem_working_set_statistics
