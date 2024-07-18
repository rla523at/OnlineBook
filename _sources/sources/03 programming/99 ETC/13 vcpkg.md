# vcpkg
vcpkg는 C++ 패키지 관리자로 C++ 프로젝트에서 라이브러리를 쉽게 설치하고 관리할 수 있도록 도와준다. 

Windows, macOS, Linux를 모두 지원하며, 다양한 라이브러리를 포함하고 있어 개발자는 복잡한 빌드 프로세스 없이 필요한 라이브러리를 간편하게 통합할 수 있다.

> Reference  
> [learn.microsoft - vcpkg](https://learn.microsoft.com/en-us/vcpkg/)  

## 패키지 설치
```
vcpkg install <name>
```

### triplet 옵션
vcpkg의 triplet 옵션은 패키지를 특정 플랫폼 및 아키텍처에 맞춰 빌드하고 설치하기 위한 설정을 제공한다. 

triplet은 다음과 같은 종류가 있다.

* x64-windows: 64비트 Windows
* x86-windows: 32비트 Windows
* x64-windows-static: 64비트 Windows에서 정적 라이브러리
* x64-linux: 64비트 Linux
* x64-osx: 64비트 macOS

예를 들어 64비트 Windows에서 패키지를 설치하려면 다음과 같이 한다.

```
vcpkg install <name> --triplet x64-windows
```

## 명령어

**패키지 검색**
```
vcpkg search <name>
```

**패키지 설치**

**설치된 패키지 목록 보기**
```
vcpkg list
```

**패키지 제거하기**
```
vcpkg remove <name>
```

