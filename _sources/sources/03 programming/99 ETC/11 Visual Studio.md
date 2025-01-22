# Visual Studio

특정 프로젝트가 빌드가 안될 때는 구성 관리자에가서 빌드 체크가 되어 있는지 확인해본다.

## 프로젝트 속성

### 링커
명령줄 >> 추가옵션에 /Force:MULTIPLE 을 추가하면 라이브러리 충돌을 무시하고 링크를 강제로 진행할 수 있다.

### 미리 컴파일된 헤더
프로젝트 전체적으로는 미리 컴파일된 헤더 사용으로 되어 있어야 하지만, 미리 컴파일된 헤더를 생성하는 .cpp 는 개별적으로 미리 컴파일 된 헤더 만들기 옵션으로 되어 있어야 한다.

### .props
속성 관리자에서 정의된 .props 의 값을 그대로 사용하려면 속성에서 부모 또는 프로젝트 기본값에서 상속을 선택하면 된다.
* .props 파일의 값이 다 평가된 후 .vcxproj 의 값을 평가한다.
* 프로젝트 속성에서 직접 값을 수정하면 .vcxproj 파일에 그 내용이 저장됨으로 .props 파일의 값은 프로젝트 속성을 수정한 값으로 전부 덮어씌워진다.

### 매크로
$(TargetDir) 은 C/C++ 탭이나 링커탭에서는 사용할 수 있지만, 일반 또는 VC++ 디렉터리 등에서는 사용할 수 없다. 왜인지는 모르겠다.

> Reference  
> [learn.microsoft - common-macros-for-build-commands-and-properties?view=msvc-170](https://learn.microsoft.com/en-us/cpp/build/reference/common-macros-for-build-commands-and-properties?view=msvc-170)   

## Git Bash 사용하기
도구 >> 옵션 >> 환경 >> Terminal >> 추가 

```
이름 : Git Bash
위치 : C:\Program Files\Git\bin\sh.exe
인수 : --login -i (선택)
```

ctrl + ` 로 Terminal 연 후 왼쪽 위에서 Git Bash 선택

> Reference  
> [stackoverflow - integrating Git Bash with Visual Studio](https://stackoverflow.com/a/65386291)  

## 수정사항이 없어도 만료된 프로젝트로 판단되서 재빌드 하는 경우
* 클린 빌드를 수행해 본다.
  * 클린 빌드는 프로젝트의 모든 빌드 출력 파일을 제거한 다음 다시 빌드하는 것이다. 
  * 이는 빌드 시스템이 잘못된 파일을 사용하고 있는 경우에 도움이 될 수 있다.
  * 솔루션 탐색기에서 솔루션을 마우스 오른쪽 버튼으로 클릭하고 "솔루션 정리"를 선택한다.
  * 그런 다음 "솔루션 빌드"를 선택한다.


## Map File
`맵 파일(Map File)`은 함수나 변수가 어느 주소에 배치되었는지에 대한 일종의 컴파일 결과 보고서이다.

visual studio에서 Map file을 보고 싶은 경우에는 다음과 같이 하면 된다.

```
프로젝트 >> 속성 >> 링커 >> 디버깅 >> 맵 파일 생성 >> 예
```

이 경우 `project/x64/Debug`에 .map 파일이 생성된다.

> Reference  
> [blog](http://soen.kr/lecture/ccpp/cpp3/31-1-2.htm)
> [blog - map file 읽는법](https://kuaaan.tistory.com/102)

### \$pdata\$, \$unwind\$

> Reference  
> [stackoverflow](https://stackoverflow.com/questions/34609354/how-to-read-dumpbin-for-windows-library-lib)

## Disassembly
디버그 모드 실행 시 다음과 같이 디스 어셈블리 창을 띄울 수 있다.
```
디버그 >> 창 >> 디스 어셈블리
```

## Dumpbin

obj 파일에있는 symbol 결과를 보고 싶으면 VS 2017에 대한 개발자 명령 프롬프트을 실행한 뒤 다음 명령어로 실행한다.

```
cd {file path} 
dumpbin /symbols [filename].obj 
```

symbol 결과를 어떤 파일에 쓰고 싶으면 다음 명령어를 실행하면 된다.
```
dumpbin /symbols [filename].obj > [writefilename].txt
```

### /symbols

> Reference
> [MSDN](https://learn.microsoft.com/ko-kr/cpp/build/reference/symbols?view=msvc-170)

## Clang Format
Clang Format을 사용하기 위해서는 다음 경로로 들어가서 `Clang Format 지원 사용` 옵션을 체크해야 한다.

```
도구 > 옵션 > 텍스트 편집기 > C/C++ > 서식 > 일반 
```

.clang_format 파일은 다음과 같이 추가 할 수 있다.

```
솔루션 탐색기 > 프로젝트 > 추가 > 새항목 > 서식 
```

.clang_format 파일의 수정은 [LLVM-Clang-Format Style Options](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)를 참고하면 된다.

### 참고
custom .clang_format을 사용하고 싶은 경우 `사용자 지정 clang-format.exe 파일 사용` 옵션은 체크하지 않아야 한다.

## 파일형식

`솔루션 탐색기 >> 파일 우클릭 >> 속성 >> 구성 속성 >> 일반`에 가서 보면 항목 형식에서 cpp 파일이면 `C/C++ 컴파일러`로 되어 있고 h 파일이면 `C/C++ 헤더`로 되어 있다.

## Command Arguments
`프로젝트 >> 속성 >> Debugging >> Command Arguments`에서 `-iMec2009`처럼 `-`가 하나만 나온경우에는 `i`가 option 이름이고 option 값이 Mec2009가 된다. 그리고 `--runbyopt=on`처럼 `--`인 경우에는 `runbytop`라는 단어 전체가 option 이름이 되고 option 값이 on이 된다.


## 프로젝트 종속성
`솔루션 탐색기 >> 프로젝트 오른쪽 마우스 >> 빌드 종속성 >> 프로젝트 종속성`에서 종속성을 선택하면 이 프로젝트를 빌드하기전에 미리 빌드할 프로젝트를 결정할 수 있다. 

프로젝트 종속성이 반영된 빌드 순서를 보려면 프로젝트 종속성 대화 상자에서 빌드 순서 탭으로 전환하여 볼 수 있다.

> Reference  
> [learn.microsoft](https://learn.microsoft.com/ko-kr/visualstudio/ide/how-to-create-and-remove-project-dependencies?view=vs-2022)  

## 참조
`솔루션 탐색기 >> 프로젝트 오른쪽 마우스 >> 참조`에서 library를 만들어내는 프로젝트를 추가하면 별도의 과정없이 참조에 속한 프로젝트가 만들어낸 library를 header file 추가만으로 사용할 수 있다.

## vcxitems
.vcxitems 파일은 Visual Studio에서 여러 프로젝트가 공통으로 사용할 수 있는 공유 코드를 정의하는 데 사용된다. 공유 프로젝트는 솔루션 안에서 다른 여러 프로젝트가 동일한 소스 코드 파일을 참조하고 사용할 수 있도록 해주며, 이때 .vcxitems 파일이 각 파일의 정보와 구성을 관리하는 역할을 한다.

.vcxproj는 특정 플랫폼과 빌드 설정에 종속된 프로젝트 파일이다. 하지만 .vcxitems 파일을 사용하면 여러 프로젝트에서 플랫폼에 독립적인 코드를 공유할 수 있다. 공유 프로젝트는 특정 플랫폼을 타겟으로 하지 않으며, 여러 프로젝트가 이 공유 코드를 빌드할 때 자신들의 플랫폼에 맞게 컴파일할 수 있다.


## VAssisX

### 문제점
Microsoft Visual Studio Professional 2017 버전 15.9.49와 VA_X.dll file version 10.9.2217.0  built 2017.04.26를 사용하면 multi-catet을 사용해서 typing을 할 때, main caret만 연속해서 글을 쓸 수 있고 나머지 caret들은 한글자만 작성되고 자동 취소된다.

[What's New in Visual Assist](https://www.wholetomato.com/features/whats-new)를 보면 General Release Build 2291에 
```
[VS2017 15.8+] VA no longer interferes with Multi-Caret Edit mode. (case=117499)
```
라고 나와있는것을 확인할 수 있다.

> Reference  
> [forums.wholetomato](https://forums.wholetomato.com/forum/topic.asp?TOPIC_ID=15297)
