# File Buffering

## File Caching
기본적으로 Windows는 디스크에서 읽어오거나 디스크에 쓸 파일 데이터를 캐시한다.

즉, 읽기 작업은 실제 디스크가 아닌 시스템 파일 캐시라는 시스템 메모리 영역에 있는 데이터를 읽는것이고 쓰기 작업은 디스크가 아닌 시스템 파일 캐시에 데이터를 쓰는 것이다.

예를 들어 disk 에 있는 파일을 읽은 다음에 수정하고 다시 저장하는 과정을 살펴보자.

file read operation 중에 cache manager 의 요청으로 인해 disk 에 있는 data 가 system cache 의 특정 slot 으로 읽어 들여진다.

이후에 user-mode process 에서 system cache slot 에 있는 data 를 고유의 address space 로 복사를 한다.

이후에 address space 에 저장된 수정된 데이터를 다시 동일한 system cache slot 에 write 하게 된다.

일정 시간이 지나 cache manager 가 더이상 system cache slot 에 있는 데이터가 필요하지 않다고 판단을 하면 disk 에 있는 file 에 수정된 data 를 write 한다.


> Reference  
> [learn.microsoft - file-caching](https://learn.microsoft.com/ko-kr/windows/win32/fileio/file-caching)  

FILE_FLAG_NO_BUFFERING 플래그를 사용하여 CreateFile 함수로 열리거나 만들어진 파일의 경우 파일에서 읽거나 파일에 쓰는 데이터의 대한 system caching 을 사용하지 않는다.



buffering 되지 않은 파일의 input/output (I/O)

## 볼륨 섹터 크기

볼륨 섹터 크기(Volume Sector Size)는 디스크 저장 장치에서 데이터를 저장하고 읽을 때 사용하는 기본 단위이다.

이 단위는 섹터라고 불리며, 물리적 디스크와 파일 시스템 모두 이 단위를 사용해 데이터를 관리한다.

일반적으로 하드 드라이브에서는 섹터의 기본 크기가 512바이트 또는 4096바이트(4KB)이다.

그리고 섹터는 논리적 섹터와 물리적 섹터로 나뉜다.

논리적 섹터는 운영체제가 파일 시스템을 통해 접근하는 섹터이고 물리적 섹터는 디스크 드라이브의 실제 하드웨어에 있는 섹터이다.
