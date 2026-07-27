# Intel oneAPI Compiler

## 한 줄 요약

Intel oneAPI DPC++/C++ Compiler는 C, C++와 SYCL program을 compile할 수 있는 Intel compiler toolchain이다. Linux에서는 compiler를 설치한 뒤 현재 shell에 oneAPI environment를 적용해야 `icx`와 `icpx` command를 안정적으로 사용할 수 있다.

## compiler driver

Compiler driver는 source file, compiler option과 library를 받아 compile과 link 과정을 실행하는 command-line program이다.

- `icx`: Linux에서 C source code를 compile할 때 사용하는 기본 driver다.
- `icpx`: Linux에서 C++ source code를 compile할 때 사용하는 기본 driver다.
- `icpx -fsycl`: C++ source code를 SYCL program으로 compile한다.

이 문서에서는 Ubuntu 또는 Debian 계열 Linux distribution에서 APT로 C++ compiler를 설치하고 `icpx`를 검증하는 흐름을 다룬다.

## 설치 전 상태 확인

먼저 현재 shell이 이미 `icpx`를 찾을 수 있는지 확인한다.

```bash
command -v icpx
icpx --version
```

`command -v`가 아무 경로도 출력하지 않거나 `icpx: command not found`가 발생한다면 compiler가 설치되지 않았거나 oneAPI environment가 현재 shell에 적용되지 않은 상태다.

## APT로 설치

### repository 준비 도구 설치

APT package index를 갱신하고 Intel repository의 signing key를 처리하는 데 필요한 program을 설치한다.

```bash
sudo apt update
sudo apt install -y gpg-agent wget
```

`sudo`는 system package 상태를 변경하므로 현재 사용자에게 관리자 권한이 있어야 한다.

### Intel signing key 등록

Intel repository에서 받은 package의 서명을 검증할 수 있도록 public key를 system keyring에 등록한다.

```bash
wget -O- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB \
  | gpg --dearmor \
  | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null
```

명령이 성공하면 `/usr/share/keyrings/oneapi-archive-keyring.gpg`가 생성되거나 갱신된다.

### Intel oneAPI repository 등록

APT가 Intel oneAPI package를 찾을 수 있도록 repository entry를 추가한다.

```bash
echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" \
  | sudo tee /etc/apt/sources.list.d/oneAPI.list
```

이 명령은 `/etc/apt/sources.list.d/oneAPI.list`를 생성하거나 기존 내용을 덮어쓴다. 그다음 새 repository의 package index를 가져온다.

```bash
sudo apt update
```

### compiler 설치

Intel oneAPI DPC++/C++ Compiler package를 설치한다.

```bash
sudo apt install -y intel-oneapi-compiler-dpcpp-cpp
```

C++ standard library header와 GNU build toolchain이 없는 최소 설치 환경이라면 다음 package도 준비한다.

```bash
sudo apt install -y build-essential
```

설치된 APT package 상태는 다음 명령으로 확인할 수 있다.

```bash
dpkg-query -W intel-oneapi-compiler-dpcpp-cpp
```

## oneAPI environment 적용

oneAPI compiler는 설치 layout에 따라 `setvars.sh` 또는 `oneapi-vars.sh`로 필요한 environment variable을 현재 shell에 추가한다.

### Component Directory Layout

System-wide 기본 경로에 설치한 Component Directory Layout은 다음과 같이 적용한다.

```bash
source /opt/intel/oneapi/setvars.sh
```

### Unified Directory Layout

Unified Directory Layout은 설치한 toolkit version directory 안의 `oneapi-vars.sh`를 적용한다. 사용할 수 있는 environment script를 먼저 확인한다.

```bash
find /opt/intel/oneapi -maxdepth 2 \
  \( -name setvars.sh -o -name oneapi-vars.sh \)
```

출력된 실제 `oneapi-vars.sh` 경로를 `source`의 인자로 사용한다. 경로 형식은 `/opt/intel/oneapi/<toolkit-version>/oneapi-vars.sh`이며 `<toolkit-version>` 부분은 설치된 version directory name을 뜻한다.

`source`는 script를 별도 process에서 실행하지 않고 현재 shell에서 실행한다. 따라서 environment 변경은 기본적으로 해당 terminal session에만 적용된다. 새 terminal에서도 자동으로 적용할지 결정하려면 [Shell Environment](<../05 Linux/Shell Environment.md>)의 Bash startup file과 검증 절차를 참고한다.

## 설치 결과 검증

Environment를 적용한 shell에서 compiler 경로와 version을 다시 확인한다.

```bash
command -v icpx
icpx --version
```

System-wide Component Directory Layout을 사용했다면 `command -v icpx`는 일반적으로 `/opt/intel/oneapi/.../bin/icpx` 형태의 경로를 출력한다. 정확한 중간 directory는 설치한 version과 layout에 따라 달라질 수 있으므로 특정 경로를 가정하지 않고 실제 출력값을 사용한다.

### 최소 C++ program compile

`hello-world.cpp`를 다음과 같이 작성한다.

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello, world!\n";
    return 0;
}
```

`icpx`로 compile과 link를 수행한 뒤 생성된 executable을 실행한다.

```bash
icpx hello-world.cpp -o hello-world
./hello-world
```

정상적으로 실행되면 다음 결과가 출력된다.

```text
Hello, world!
```

이 검증은 세 가지 상태를 함께 확인한다.

1. 현재 shell이 `icpx`를 찾을 수 있다.
2. compiler가 C++ standard library를 사용해 source code를 compile하고 link할 수 있다.
3. 생성된 executable을 현재 Linux 환경에서 실행할 수 있다.

## Visual Studio에서 사용

Visual Studio에서 WSL 또는 원격 Linux의 `icpx`를 compiler로 선택하는 방법은 [Visual Studio](<./11 Visual Studio.md>)의 Linux C++ 개발 환경 절을 참고한다.

Visual Studio가 실행하는 Linux build process도 compiler 경로와 environment variable을 확인할 수 있어야 한다. Terminal에서 `source`한 상태가 Visual Studio의 build environment에 자동으로 전달된다고 가정하지 말고, 실제 build output에 표시되는 compiler와 경로를 확인한다.

## 자주 발생하는 문제

### 새 terminal에서 `icpx`를 찾지 못한다

`source`로 적용한 environment는 현재 shell state이므로 새 terminal에는 자동으로 상속되지 않을 수 있다. 새 terminal에서 environment script를 다시 적용한 뒤 `command -v icpx`와 `icpx --version`을 확인한다.

### package는 설치됐지만 `icpx`를 찾지 못한다

APT package 상태와 environment script 위치를 각각 확인한다.

```bash
dpkg-query -W intel-oneapi-compiler-dpcpp-cpp
find /opt/intel/oneapi -maxdepth 2 \
  \( -name setvars.sh -o -name oneapi-vars.sh \)
```

Package 설치와 현재 shell의 `PATH` 설정은 서로 다른 상태다. Package가 설치되어 있어도 environment script를 적용하지 않았다면 shell이 `icpx`를 찾지 못할 수 있다.

## 관련 문서

- [Linux](<../05 Linux/Linux.md>)
- [Shell Environment](<../05 Linux/Shell Environment.md>)
- [Visual Studio](<./11 Visual Studio.md>)

## References

- [Intel - Get Intel oneAPI DPC++/C++ Compiler](https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compiler-download.html?distribution-linux=apt&operatingsystem=linux)
- [Intel - Get Started on Linux](https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/get-started-guide/current/get-started-on-linux.html)
- [Intel - Use the setvars and oneapi-vars Scripts with Linux](https://www.intel.com/content/www/us/en/docs/oneapi/programming-guide/current/use-the-setvars-and-oneapi-vars-scripts-with-linux.html)
