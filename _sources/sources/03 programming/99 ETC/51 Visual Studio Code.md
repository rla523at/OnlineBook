# Visual Studio Code

## 설치
[Download Visual Studio Code](https://code.visualstudio.com/Download)에서 system installer x64를 다운로드 받는다.

> Reference   
> [User Installer,System Installer 버전 차이는?](https://gocoder.tistory.com/1611)

### 설치 경로
`AppData\Local\Programs\Microsoft VS Code`

## 계정
Github로 로그인 및 sync한다.

```{figure} _image/5101.png
```

## 확장

sync가 제대로 안될 경우 다음 확장들을 설치한다.
* 한국어 패키지  
* Markdown All in One
* Markdown Preview Enhanced

### Markdown Preview Enhanced
#### CSS 적용
* preview 화면 우클릭 >> preview theme >> monokai.css
* F1 >> Markdwon Preview Enhanced: Customize CSS >> 아래 내용 입력

```
.markdown-preview.markdown-preview {
  font-size: 14.5px;     

  h1, h2, h3, h4, h5, h6 
  {        
      color: #e1e1e1;	
  }

  h1	
  {
      margin-top: 80px;
      border-top: 1px solid #e1e1e1;                
      border-bottom: 1px solid #e1e1e1;
      padding: 15px 0px 15px 0px;        
      text-align: center;
      font-size: 40px;
  }
  h2	
  {
      margin: 60px 0px 30px 5px;
      border-left: 8px solid #f79400;
      padding: 0px 0px 0px 10px;       
      text-align: center;
      font-size: 30px;
  }
  h3	
  {
      margin: 40px 0px 10px 10px;
      border-left: 8px solid #32c97d;
      padding: 0px 0px 0px 10px;        
      font-size: 25px;
  }
  h4	
  {        
      margin: 20px 0px 5px 15px;
      border-left: 8px solid #303F9F;
      padding: 0px 0px 0px 10px;        
      font-size: 20px;
  }
  h5	
  {
      margin: 20px 0px 5px 20px;
      border-left: 8px solid #9f11ac;            
      padding: 0px 0px 0px 10px;        
      font-size: 16px;
  }
  h6	
  {
      font-size: 14px;
  }

  code //inline code
  {
      color: #f3dc95;
      font-size: 14.5px !important;
  }

  pre > code //code block ```
  {
      color: #e1e1e1;	        
      font-size: 14.5px !important;
  }
  
  // blockquote // 인용문 >
  // {
  //     border-top: 1px solid white;
  //     border-bottom: 1px solid white;
  //     border-left-style: hidden;
  //     border-style: hidden;    
  //     padding-left: 10px;
     
  //     background: #292929;
  //     color:#e2e2e2;
  // }
}
```

> Reference   
> [stackoverflow - separate-style-for-markdown-in-single-backticks-vs-triple-backticks](https://stackoverflow.com/questions/49703670/separate-style-for-markdown-in-single-backticks-vs-triple-backticks-using-the-m)  

#### 매크로 적용
* F1 >> Markdown Preview Enhanced: Open Config Script (Global)
* KatexConfig에 macro지정

```
  katexConfig: {
  "macros": {
    "\\span"      : "\\text{span}",
    "\\End"       : "\\text{End}",
    "\\C"         : "\\Complex",
    "\\F"         : "\\mathbb{F}",
    "\\R"         : "\\mathbb{R}",
    "\\N"         : "\\mathbb{N}",
    "\\Q"         : "\\mathbb{Q}",
    "\\Z"         : "\\mathbb{Z}",
    "\\Ext"       : "\\text{Ext}",
    "\\Int"       : "\\text{Int}",
    "\\st"        : "\\quad s.t \\quad",
    "\\qed"       : "\\quad\\tiny\\blacksquare",
    "\\preimg"    : "\\text{preimg}",
    "\\img"       : "\\text{img}",
    "\\norm"      : "\\left\\lVert #1 \\right\\rVert",
    "\\sym"       : "\\text{sym}",
    "\\tr"        : "\\text{tr}",
    "\\empty"     : "\\emptyset",
    "\\exist"     : "\\exists",
    "\\Set"       : "\\left \\{ #1 \\right\\}",
    "\\pdiff"     : "\\frac{\\partial #1}{\\partial #2}",
    "\\diff"      : "\\frac{d #1}{d #2}",
    "\\ninf"      : "n \\rightarrow \\infty",
    "\\rank"      : "\\text{rank}",
    "\\nullity"   : "\\text{nullity}",
    "\\lcm"       : "\\text{lcm}"
  }
},
```

> Reference  
> [KaTeX/discussions - Macros with arguments](https://github.com/KaTeX/KaTeX/discussions/2589)  
> [katex - options](https://katex.org/docs/options)  
> [blog - configure the Markdown Preview Enhanced extension](https://shd101wyy.github.io/markdown-preview-enhanced/#/config?id=global)

## 단축키

ctrl , : 설정

ctrl k + ctrl s : 바로가기 키 설정 

ctrl + shift + n : 새 vscode창

ctrl + n : 이름없는 새 파일

## 설정

* 창의 확대 축소 수준
  * 설정 >> zoom 검색 >> Window: Zoom Level >> [-1,1]의 값으로 크기 조절
* 마우스로 편집기 글꼴 확대/축소
  * 설정 >> zoom 검색 >> Mouse wheel zoom
* 맨 밑에 보라색 줄
  * 상태 표시줄
* encoding
  * korean으로 바꿀 수 있음
* Tab Size

## 바로가기 키 설정
* "그룹에서 이전 편집기 열기" 검색 >> alt + $\leftarrow$   
* "그룹에서 다음 편집기 열기" 검색 >> alt + $\rightarrow$ 
* "scrollLineup" 검색 >> alt + $\uparrow$ 
  * "줄 위로 이동" 검색 >> 키 바인딩 제거
* "scrollLineDown" 검색 >> alt + $\downarrow$ 
  * "줄 아래로 이동" 검색 >> 키 바인딩 제거
* 다음 일치 항목 찾기에 선택 항목 추가 >> ctrl + e  
* 일치 항목 찾기의 모든 항목 선택 >> ctrl + d  
* Markdwon: 측면에서 미리보기 열기 >> 키 바인딩 제거
* Markdown: open preview >> 키 바인딩 제거
* Markdown: open preview to the side >> 키 바인딩 제거

## 터미널에서 Shift+Enter 동작하게 하는 방법

### 문제 상황
VS Code 통합 터미널에서는 아래처럼 `Shift+Enter`를 눌러도 줄이 추가되지 않고, 그냥 `Enter`를 누른 것처럼 현재 줄이 바로 실행되었다.

```powershell
PS D:\Study\Code\AutomationProject> digh
digh: The term 'digh' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
```

실제 PowerShell에서 `Shift+Enter`는 현재 줄을 실행하지 않고 다음 줄을 추가하는 동작이다.

```powershell
PS D:\Study\Code\AutomationProject> digh
>>
```

### 해결 방법
`기본 설정: 사용자 설정 열기(JSON)` 에 아래 두 줄을 추가하면 된다.

```json
"terminal.integrated.enableKittyKeyboardProtocol": true,
"terminal.integrated.enableWin32InputMode": true,
```

그 뒤 기존 터미널을 닫고 새 통합 터미널을 열면 된다.
VS Code 공식 설명에 따르면:
* enableKittyKeyboardProtocol은 조합키/수정키 전달 한계를 줄이기 위한 기능이다.
* enableWin32InputMode는 비슷한 목적의 Windows 전용 experimental 입력 모드이며, ConPTY에 맞게 조정되어 있다.

### 원인
원인은 PowerShell 자체가 아니라 VS Code 통합 터미널의 입력 처리 경로에 있다.
VS Code 터미널은 Windows에서 일반 콘솔과 동일한 방식이 아니라 xterm.js 기반 터미널과 ConPTY 같은 pseudoterminal 계층을 사용하므로, 키 입력이 standalone PowerShell/Windows Terminal과 다르게 처리될 수 있다.

특히 Shift+Enter 같은 조합키는 전통적인 터미널 키 인코딩 방식의 한계 때문에 정확히 전달되지 않을 수 있다. VS Code 1.109 릴리스 노트에서도 Kitty keyboard protocol이 이런 “기존 keystroke 인코딩 한계”를 해결하기 위한 기능이라고 설명하고, Windows용으로는 ConPTY에 맞춘 experimental win32 input mode도 추가했다고 밝히고 있다.

실제로 VS Code 이슈에도 “외부 PowerShell/Windows Terminal에서는 되는데 VS Code 통합 터미널에서는 안 된다”는 보고가 오래전부터 있었고, 2025년에도 유사 [보고](https://github.com/microsoft/vscode/issues/74230)가 올라와 있다.

이번 경우에는 shell integration이나 PowerShell 키 바인딩은 이미 정상인데, VS Code 기본 입력 경로가 Shift+Enter를 PowerShell/PSReadLine이 원하는 형태로 제대로 전달하지 못한 것으로 보는 것이 가장 자연스럽다. 이것은 사용자가 설정을 바꾼 뒤 바로 정상 동작한 결과와도 맞는다.

## 터미널에서 ctrl 이나 shift 입력하면 입력 위치로 돌아오는 현상
vscode 버그로 보인다.

https://github.com/microsoft/vscode/issues/288356?utm_source=chatgpt.com