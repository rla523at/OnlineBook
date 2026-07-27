# Linux

## 한 줄 요약

Linux 문서군은 Linux 환경에서 프로그램을 실행하고 개발할 때 필요한 운영 환경, shell, service 관리와 Windows 연동 개념을 정리한다.

## 문서 범위

Linux는 엄밀히 말하면 operating system의 핵심인 kernel을 가리킨다. 실제 개발 환경에서는 Ubuntu처럼 Linux kernel과 system program, package manager를 묶어 제공하는 Linux distribution을 사용한다.

이 문서군은 특정 IDE나 compiler의 사용법보다 Linux 환경 자체를 이해하는 데 필요한 내용을 다룬다. Visual Studio와 Intel oneAPI처럼 특정 개발 도구에 종속된 설정은 별도 문서에서 설명한다.

## 관련 문서

- [WSL](./WSL.md): Windows에서 Linux distribution을 실행할 때 필요한 구성 요소와 상태를 설명한다.
- [Shell Environment](<./Shell Environment.md>): shell variable, environment variable, `PATH`와 startup file의 관계를 설명한다.
- [systemd](./systemd.md): Linux service와 system lifecycle을 관리하는 init system을 설명한다.
- [Visual Studio](<../99 ETC/11 Visual Studio.md>): Visual Studio에서 WSL 또는 원격 Linux를 대상으로 C++를 빌드하고 디버깅하는 방법을 설명한다.
- [Intel oneAPI Compiler](<../99 ETC/14 Intel oneAPI Compiler.md>): Intel oneAPI DPC++/C++ Compiler를 Linux에 설치하고 shell 환경을 준비하는 방법을 설명한다.

## WSL

Windows Subsystem for Linux(WSL)은 Windows 안에서 Linux distribution과 Linux command-line tool을 실행할 수 있게 하는 Windows 기능이다.

WSL의 구성 요소, 버전 구분, filesystem, resource와 실행 상태를 확인하는 방법은 [WSL](./WSL.md) 문서를 참고한다.
