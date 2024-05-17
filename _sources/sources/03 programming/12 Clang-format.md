# Clang-format 

## 최신 clang-foramt.exe 설치하기
* [LLVM Download Page](https://releases.llvm.org/download.html)에 접속한다.
* GItHub release page에 접근한다.
* LLVM-17.0.1-win64.exe 찾아서 다운로드 후 설치
  * 일반적으로 C:\Program Files\LLVM\bin 설치된다.
* Visual studio에서 사용하고 있는 clang-format.exe를 교체해준다.
  * 2022 
    * C:\Program Files\Microsoft Visual Studio\2022\Professional\VC\Tools\Llvm\bin
    * C:\Program Files\Microsoft Visual Studio\2022\Professional\VC\Tools\Llvm\x64\bin
    * C:\Program Files\Microsoft Visual Studio\2022\Professional\VC\Tools\Llvm\ARM64\bin
  * 2017
    * C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\VC\vcpackages

## Bash로 사용하기
* [documentation](https://clang.llvm.org/docs/ClangFormat.html)
* `clang-format -style=file -i main.cpp`