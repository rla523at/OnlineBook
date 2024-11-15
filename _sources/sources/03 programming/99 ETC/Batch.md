# Batch

## for

Syntax 는 다음과 같다.
```
for {%% | %}<variable> in (<set>) do <command> [<commandlineoptions>]
```

### for Extension
for 는 다음과 같은 extension 을 제공한다.

* directory only
```
for /d {%%|%}<variable> in (<set>) do <command> [<commandlineoptions>]
```

* recursive
```
for /r [[<drive>:]<path>] {%%|%}<variable> in (<set>) do <command> [<commandlineoptions>]
```

* iterating a range of values
```
for /l {%%|%}<variable> in (<start#>,<step#>,<end#>) do <command> [<commandlineoptions>]
```

* iterating and file parsing
```
for /f [<parsingkeywords>] {%%|%}<variable> in (<set>) do <command> [<commandlineoptions>]
for /f [<parsingkeywords>] {%%|%}<variable> in (<literalstring>) do <command> [<commandlineoptions>]
for /f [<parsingkeywords>] {%%|%}<variable> in ('<command>') do <command> [<commandlineoptions>]
```

참고로, command 자리에 dir 명령어를 사용할 수 있다.

> Reference    
> [learn.microsoft - windows-commands/for](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/for)  

## if

Syntax 는 다음과 같다.
```
if [not] ERRORLEVEL <number> <command> [else <expression>]
if [not] <string1>==<string2> <command> [else <expression>]
if [not] exist <filename> <command> [else <expression>]
```

만약 여러줄로 if 문을 사용하고 싶으면 다음과 같이 쓰면된다.
```
if condition (
  command
)
```

이 떄, condition 과 ( 사이에 공백이 반드시 있어야 한다.

> Reference  
> [learn.microsoft - windows-commands/if](https://learn.microsoft.com/ko-kr/windows-server/administration/windows-commands/if)  

## dir
Syntax 는 다음과 같다
```
dir [<drive>:][<path>][<filename>] [...] [/p] [/q] [/w] [/d] [/a[[:]<attributes>]][/o[[:]<sortorder>]] [/t[[:]<timefield>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4] [/r]
```

| 매개변수                    | 설명                                                                                                                       |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `[<drive>:][<path>]`       | 표시할 디렉터리 및 드라이브를 지정합니다.                                                                                  |
| `[<filename>]`             | 표시할 특정 파일이나 파일 그룹을 지정합니다.                                                                               |
| `/p`                        | 한 화면씩 목록을 표시합니다. 다음 화면을 보려면 아무 키나 누르십시오.                                                     |
| `/q`                        | 파일 소유자 정보를 표시합니다.                                                                                            |
| `/w`                        | 넓은 형식으로 표시하며, 각 줄에 최대 5개의 파일 이름 또는 디렉터리 이름을 표시합니다.                                      |
| `/d`                        | `/w`와 동일한 형식으로 표시하되, 파일을 열의 순서로 정렬합니다.                                                           |
| `/a[[:]<속성>]`             | 지정한 속성을 가진 디렉터리와 파일만 표시합니다. 속성을 지정하지 않으면 숨김 및 시스템 파일을 포함한 모든 파일을 표시합니다. 속성 값은 아래와 같습니다: <br> `d` - 디렉터리 <br> `h` - 숨김 파일 <br> `s` - 시스템 파일 <br> `l` - 재분석 지점 <br> `r` - 읽기 전용 파일 <br> `a` - 아카이브 준비된 파일 <br> `i` - 내용 인덱싱되지 않은 파일<br> `-`를 사용하여 해당 속성이 없는 파일을 표시할 수 있습니다. 예를 들어, `-s`는 시스템 파일을 제외하고 표시합니다. |
| `/o[[:]<정렬순서>]`         | 지정한 정렬 순서에 따라 출력 내용을 정렬합니다. 사용할 수 있는 값은 다음과 같습니다: <br> `n` - 이름순 <br> `e` - 확장자순 <br> `g` - 디렉터리 우선 <br> `s` - 크기순 (작은 것부터) <br> `d` - 날짜/시간순 (오래된 것부터) <br> `-`를 사용하여 정렬 순서를 반대로 할 수 있습니다. 여러 값을 나열할 수 있으며, 값은 공백 없이 작성합니다. |
| `/t[[:]<시간필드>]`         | 표시할 시간 필드를 지정하거나 정렬에 사용할 시간을 지정합니다. 사용할 수 있는 값은 다음과 같습니다: <br> `c` - 생성 시간 <br> `a` - 마지막 접근 시간 <br> `w` - 마지막 수정 시간 |
| `/s`                        | 지정한 디렉터리 및 모든 하위 디렉터리에서 지정한 파일 이름의 모든 발생을 나열합니다.                                     |
| `/b`                        | 디렉터리 및 파일 이름만 표시하며 추가 정보를 제공하지 않습니다. `/b`는 `/w`를 무시합니다.                                |
| `/l`                        | 디렉터리 및 파일 이름을 소문자로 표시하며, 정렬하지 않습니다.                                                            |
| `/n`                        | 긴 목록 형식으로 표시하며, 파일 이름을 화면 오른쪽 끝에 표시합니다.                                                       |
| `/x`                        | 8.3 형식의 짧은 이름을 생성하여 표시합니다. `/n`과 동일한 형식이지만 짧은 이름이 긴 이름 앞에 표시됩니다.                 |
| `/c`                        | 파일 크기에 천 단위 구분 기호를 표시합니다. 기본적으로 활성화되어 있으며, `/–c`를 사용하여 구분 기호를 숨길 수 있습니다. |
| `/4`                        | 연도를 네 자리로 표시합니다.                                                                                             |
| `/r`                        | 파일의 대체 데이터 스트림을 표시합니다.                                                                                   |
| `/?`                        | 명령 프롬프트에 도움말을 표시합니다.                                                                                       |

> Reference  
> [learn.microsoft - windows-commands/dir](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/dir)  


## 주의사항

### Path 
batch 파일 내에서 Path 라는 변수를 설정하는걸 조심해야한다.

왜냐하면 Windows의 Path는 시스템 환경 변수 중 하나로, 이를 설정하게 되면 시스템의 경로 환경 변수가 변경되어 경로를 인식하지 못하는 문제가 발생할 수 있기 때문이다.