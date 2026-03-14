# Linux 

## WSL2 설치
powershell 에서 `wsl --install` 입력

> Reference  
> [velog - Window 에서 Linux 사용하기](https://velog.io/@jskim/Windows%EC%97%90%EC%84%9C-Linux-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-with-WSL2)

## Visual Studio Linx 개발 환경 설정

### Linux 워크로드
Visual Studio Installer 를 통해 C++를 사용한 Linux 및 임베디드 개발 워크로드 설치

> Reference  
> [learn.microsoft - download-install-and-setup-the-linux-development-workload](https://learn.microsoft.com/en-us/cpp/linux/download-install-and-setup-the-linux-development-workload?view=msvc-170)  

### Linux 환경 설정
```
sudo apt update
sudo apt-get install g++ gdb rsync zip
```

* g++
  * A compiler - Visual Studio 2019 and later have full support for GCC and Clang.
* gdb 
  * Visual Studio automatically launches gdb on the Linux system, and uses the front end of the Visual Studio debugger to provide a full-fidelity debugging experience on Linux.
* rsync and zip 
  * the inclusion of rsync and zip allows Visual Studio to extract header files from your Linux system to the Windows filesystem for use by IntelliSense.

> Reference  
> [learn.microsoft - download-install-and-setup-the-linux-development-workload](https://learn.microsoft.com/en-us/cpp/linux/download-install-and-setup-the-linux-development-workload?view=msvc-170)  

### Configure Linux MSBuild C++ project in Visual Studio
프로젝트 >> 속성 >> 일반 >> 플랫폼 도구 집합 >> WSL2 GCC Toolset

> Reference  
> [learn.microsoft - Configure a Linux MSBuild C++ project in Visual Studio](https://learn.microsoft.com/en-us/cpp/linux/configure-a-linux-project?view=msvc-170)  

### 참고사항

https://learn.microsoft.com/en-us/cpp/linux/connect-to-your-remote-linux-computer?view=msvc-170
* Starting in Visual Studio 2019 version 16.1, Visual Studio has native support for using C++ with the Windows Subsystem for Linux (WSL). That means you can build and debug on your local WSL installation directly. **You no longer need to add a remote connection or configure SSH.**

https://learn.microsoft.com/en-us/cpp/linux/deploy-run-and-debug-your-linux-project?view=msvc-170#debug-your-linux-project
* Debug your Linux project

https://learn.microsoft.com/en-us/cpp/linux/create-a-new-linux-project?view=msvc-170
* Create a Linux MSBuild C++ project in Visual Studio


## Intel Compiler

### Intel Compiler 설치
```
sudo apt install -y gpg-agent wget
```
* APT 저장소(Repository) 추가를 위한 필수 패키지 설치
* Intel의 패키지를 다운로드하고 검증하는 데 필요한 도구들을 설치하는 과정입니다.

```
wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB | \
gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null
```
* Intel 저장소의 GPG 키 다운로드 및 등록
* Intel에서 제공하는 패키지가 신뢰할 수 있는지 검증하기 위한 GPG 키를 시스템에 추가하는 과정입니다.

```
echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | sudo tee /etc/apt/sources.list.d/oneAPI.list
```
* APT 소스 리스트에 Intel OneAPI 저장소 추가
* APT가 Intel의 공식 패키지 저장소에서 소프트웨어를 다운로드할 수 있도록 설정하는 과정입니다.

```
sudo apt update
```
* 패키지 목록 업데이트
* APT 저장소를 새로 추가했기 때문에, 최신 패키지 목록을 가져와야 합니다. 새롭게 추가된 Intel 저장소에서 제공하는 최신 소프트웨어 정보를 가져오는 과정입니다

> Reference  
> [intel - Download oneAPI DPC++/C++ Compiler - APT](https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compiler-download.html?operatingsystem=linux&distribution-linux=apt)  

### 환경 변수 설정
```
source /opt/intel/oneapi/setvars.sh
```

> Reference  
> [intel - Get Started on Linux](https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/get-started-guide/2025-0/get-started-on-linux.html)  
> [intel - Use the setvars and oneapi-vars Scripts with Linux](https://www.intel.com/content/www/us/en/docs/oneapi/programming-guide/2025-0/use-the-setvars-and-oneapi-vars-scripts-with-linux.html)  

### Visual Studio 에 컴파일러 설정
프로젝트 >> 속성 >> C/C++ >> 일반 >> C++ 컴파일러 >> /opt/intel/oneapi/compiler/latest/bin/icpx

> Reference  
>[intel - Invoke the Compiler from the Command Line](https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/get-started-guide/2025-0/get-started-on-linux.html)  