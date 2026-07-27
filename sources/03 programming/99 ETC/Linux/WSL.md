# WSL

## 한 줄 요약

Windows Subsystem for Linux(WSL)은 Windows에서 Linux environment를 실행하는 기능이며, WSL 2는 관리되는 경량 virtual machine 안의 Linux kernel 위에서 Linux distribution을 실행한다.

## WSL의 구성 요소

WSL을 하나의 Linux 운영체제 이름으로 보면 서로 다른 버전과 실행 상태를 혼동하기 쉽다.

WSL 2 환경은 다음 계층으로 구분할 수 있다.

```text
Windows host
└─ WSL platform
   └─ WSL 2 managed virtual machine와 Linux kernel
      ├─ Ubuntu distribution
      └─ 다른 Linux distribution
```

각 계층의 역할은 다음과 같다.

| 계층 | 역할 |
|---|---|
| Windows host | 실제 hardware와 Windows filesystem을 관리하고 `wsl.exe`를 실행한다. |
| WSL platform | Linux distribution의 설치, 시작, 종료와 Windows 연동을 관리한다. |
| WSL 1 또는 WSL 2 | 각 distribution이 사용하는 WSL architecture를 나타낸다. |
| Linux kernel | process, memory, filesystem과 system call을 관리한다. WSL 2 distribution은 실제 Linux kernel 위에서 실행된다. |
| Linux distribution | Ubuntu처럼 package와 user space tool의 구성을 제공한다. |
| shell | Bash처럼 사용자의 command를 해석하고 program을 실행한다. |

WSL 2의 여러 distribution은 관리되는 virtual machine의 CPU, kernel, memory와 swap을 공유하지만, distribution별 filesystem과 init process 등은 분리된다. 따라서 Ubuntu release, Linux kernel release, WSL program version과 distribution의 WSL 1/2 mode는 서로 다른 정보다.

Shell과 environment variable의 동작은 [Shell Environment](<./Shell Environment.md>) 문서를 참고한다.

## 명령을 실행하는 위치

WSL에서는 Windows command와 Linux command를 어느 환경에서 실행하는지 구분해야 한다.

| 표기 | 실행 위치 | 예 |
|---|---|---|
| PowerShell | Windows PowerShell 또는 명령 프롬프트 | `wsl --status` |
| WSL 내부 | Ubuntu 같은 Linux distribution의 shell | `uname -r` |

Microsoft의 WSL command 문서는 `wsl` 명령을 PowerShell 또는 명령 프롬프트에서 실행하는 형식으로 설명한다. WSL 내부에서 같은 Windows 실행 파일을 호출해야 한다면 `wsl.exe`처럼 `.exe`를 포함할 수 있지만, 현재 distribution의 Linux 정보를 확인할 때는 Linux command를 직접 실행하는 편이 구분하기 쉽다.

## 버전 정보 구분

### Windows에서 확인

다음 command는 상태를 읽기만 하며 설치 상태를 변경하지 않는다.

```powershell
wsl --version
wsl --status
wsl --list --verbose
```

각 command가 확인하는 대상은 다음과 같다.

| command | 확인 대상 | 확인하지 않는 대상 |
|---|---|---|
| `wsl --version` | WSL program과 WSL component의 version | Ubuntu release |
| `wsl --status` | default distribution, default WSL mode, kernel version 같은 전역 상태 | 각 distribution의 package version |
| `wsl --list --verbose` | 설치된 distribution, 실행 상태, distribution별 WSL 1/2 mode | distribution 내부의 Ubuntu release |

`wsl --list --verbose`에서 `VERSION`이 `2`라는 것은 해당 distribution이 WSL 2 architecture를 사용한다는 뜻이다. Ubuntu 24.04처럼 distribution release가 2라는 뜻이 아니다.

### Linux distribution 내부에서 확인

```bash
cat /etc/os-release
uname -r
```

`/etc/os-release`는 distribution이 제공하는 운영체제 식별 정보다. Ubuntu 이름과 release를 확인할 때 사용한다.

`uname -r`은 현재 실행 중인 Linux kernel의 release를 출력한다. WSL 2의 Linux kernel release와 Ubuntu release는 독립적으로 갱신될 수 있으므로 두 값을 하나의 version으로 합쳐 기록하지 않는다.

## WSL 1과 WSL 2

WSL 1은 Linux system call을 Windows 동작으로 변환하는 architecture이고, WSL 2는 관리되는 virtual machine 안에서 Linux kernel을 실행하는 architecture다.

WSL 2는 Linux system call compatibility와 Linux filesystem 내부의 file I/O에 유리하다. 반면 Windows filesystem에 저장된 파일을 Linux tool로 반복해서 처리하는 성능은 작업 방식에 따라 달라진다. 따라서 WSL 2를 사용한다는 사실만으로 모든 경로에서 같은 filesystem 성능을 기대할 수는 없다.

다음 command는 앞으로 새로 설치할 distribution의 기본 WSL mode를 변경한다.

```powershell
wsl --set-default-version 2
```

이 command는 이미 설치된 distribution의 mode를 바꾸지 않는다. 기존 distribution의 mode는 `wsl --set-version <Distribution Name> 2`로 변경할 수 있지만, 변환에는 시간이 걸리고 실패할 수 있으므로 data를 보존한 뒤 별도 작업으로 수행해야 한다.

## Windows filesystem과 Linux filesystem

WSL 2 distribution의 Linux filesystem은 virtual hard disk(VHD)에 저장된다. Linux 내부에서는 `/home`, `/etc`, `/usr` 같은 일반적인 Linux 경로로 보인다.

Windows의 고정 drive는 기본 설정에서 WSL 내부의 `/mnt` 아래에 mount된다.

```text
Windows: C:\Users\name\project
WSL:     /mnt/c/Users/name/project
```

두 위치는 같은 경로 표기만 다른 것이 아니다.

- `/home/name/project`
  - WSL distribution의 Linux filesystem에 있다.
  - Linux permission, symbolic link와 filename case 규칙을 따른다.
- `/mnt/c/Users/name/project`
  - Windows filesystem을 WSL에 mount한 경로다.
  - Windows와 Linux 사이의 filesystem 변환 계층을 통과한다.

Linux compiler, Git과 build tool을 주로 사용하는 source code는 `/home/<user>/...`처럼 Linux filesystem에 두는 것이 WSL 2의 filesystem 동작과 Microsoft의 성능 권장 사항에 맞는다. Windows tool이 주로 사용하는 파일은 Windows filesystem에 두는 편이 자연스럽다.

현재 경로와 그 경로가 속한 filesystem을 확인하는 command는 다음과 같다.

```bash
pwd
df -h .
```

`pwd`는 현재 directory를 출력한다. `df -h .`는 현재 directory가 속한 filesystem의 사용량을 읽는다. 이 command들은 파일을 이동하거나 filesystem을 변경하지 않는다.

## CPU, memory와 disk

### 두 종류의 resource 상태

WSL 2 resource를 확인할 때는 다음 두 상태를 구분한다.

- Windows host의 실제 resource
- WSL 2 virtual machine에서 Linux가 사용할 수 있는 resource

WSL 2에서 보이는 CPU와 memory는 Windows host의 전체 값과 같을 수도 있지만, `.wslconfig` 제한이나 현재 실행 환경에 따라 다를 수 있다.

PowerShell에서 host 상태를 읽는 예시는 다음과 같다.

```powershell
Get-CimInstance Win32_ComputerSystem |
    Select-Object NumberOfLogicalProcessors, TotalPhysicalMemory

Get-Volume |
    Where-Object DriveType -eq 'Fixed' |
    Select-Object DriveLetter, Size, SizeRemaining
```

첫 command는 Windows가 인식한 logical processor 수와 물리 memory 크기를 읽는다. 두 번째 command는 Windows의 fixed volume별 전체 크기와 남은 공간을 읽는다. WSL VHD가 있는 volume의 남은 공간을 판단할 때 특정 drive 하나를 가정하지 않고 실제 저장 위치와 함께 확인해야 한다.

WSL 내부에서 Linux에 보이는 resource를 읽는 예시는 다음과 같다.

```bash
lscpu
nproc
free -h
df -h /
```

- `lscpu`는 Linux가 인식한 CPU architecture와 CPU 정보를 출력한다.
- `nproc`은 현재 process가 사용할 수 있는 processing unit 수를 출력한다.
- `free -h`는 Linux가 인식한 memory와 swap을 출력한다. 새 process가 사용할 수 있는 여유를 판단할 때는 cache를 제외한 `free`만 보지 말고 회수 가능한 cache를 고려한 `available`도 확인한다.
- `df -h /`는 distribution root filesystem의 전체, 사용, 가용 공간을 출력한다.

WSL 2 VHD는 동적으로 증가할 수 있다. `df -h /`의 가용 공간은 VHD filesystem 관점의 값이며, VHD를 저장하는 Windows volume의 실제 남은 공간보다 클 수 있다. Disk 여유를 판단할 때는 Linux의 `df`와 Windows volume의 `SizeRemaining`을 함께 확인한다.

### `.wslconfig`와 `wsl.conf`

두 설정 파일은 이름이 비슷하지만 범위와 저장 위치가 다르다.

| 파일 | 위치 | 적용 범위 | 대표 설정 |
|---|---|---|---|
| `.wslconfig` | Windows의 `%UserProfile%\.wslconfig` | 모든 WSL 2 distribution에 사용하는 virtual machine | memory, processor, swap |
| `wsl.conf` | distribution 내부의 `/etc/wsl.conf` | 해당 distribution | systemd, automount, Windows interoperability, default user |

`.wslconfig`에서 resource limit을 바꾸거나 `wsl.conf`의 boot 설정을 바꾼 뒤 새 Bash process만 실행해서는 WSL virtual machine이 재시작되지 않는다. 실행 중인 작업을 종료한 뒤 PowerShell에서 `wsl --shutdown`으로 WSL 전체를 종료하고 다시 시작해야 변경 사항이 확실히 반영된다.

## systemd와 WSL lifecycle

`systemd`는 Linux에서 PID 1로 실행되며 service와 system resource를 관리하는 init system이다. 자세한 unit과 service 관리 방법은 [systemd](./systemd.md) 문서를 참고한다.

현재 distribution에서 PID 1과 systemd 상태를 확인한다.

```bash
ps -p 1 -o comm=
systemctl status
```

첫 command는 PID 1 process 이름을 읽는다. 두 번째 command가 system 상태를 반환하면 systemd manager와 통신할 수 있다는 뜻이다. `degraded` 상태는 systemd가 없다는 뜻이 아니라 하나 이상의 unit이 실패한 상태이므로 실패 unit을 별도로 확인해야 한다.

최근 WSL에서 기본으로 설치되는 Ubuntu는 systemd를 사용하지만, 다른 distribution이나 기존 설치 상태까지 같다고 가정할 수는 없다. Systemd가 필요한데 비활성화된 distribution에서는 Microsoft가 안내하는 조건을 확인한 뒤 `/etc/wsl.conf`에 다음 설정을 사용할 수 있다.

```ini
[boot]
systemd=true
```

이 설정은 해당 distribution의 boot 방식을 변경한다. 저장 후 실행 중인 작업을 종료하고 PowerShell에서 `wsl --shutdown`을 실행한 다음 distribution을 다시 시작해야 한다.

Systemd service가 실행 중이라는 사실만으로 WSL instance가 계속 살아 있는 것은 아니다. WSL의 실행과 종료 lifecycle은 일반적인 상시 실행 Linux host와 다르므로 장기 실행 service의 동작을 설계할 때 이 차이를 고려해야 한다.

## shell 재시작과 WSL 재시작

재시작 대상에 따라 검증하는 상태가 다르다.

| 동작 | 바뀌는 범위 | 적합한 검증 |
|---|---|---|
| `source ~/.bashrc` | 현재 Bash의 상태 | 현재 shell에서 설정 문법과 즉시 효과 확인 |
| 현재 shell을 종료하고 새 shell 시작 | 새 Bash process와 startup file | `PATH`, environment variable, alias의 지속 여부 확인 |
| `wsl --terminate <Distribution Name>` | 지정한 distribution | distribution 단위의 종료와 재시작 확인 |
| `wsl --shutdown` | 실행 중인 모든 distribution과 WSL 2 VM | `.wslconfig`, boot, systemd 설정 반영 확인 |

`wsl --shutdown`은 실행 중인 모든 WSL distribution과 그 안의 process를 즉시 종료한다. 저장하지 않은 작업이 없는지 확인한 뒤 사용한다.

새 shell 검증의 자세한 절차는 [Shell Environment](<./Shell Environment.md>) 문서를 참고한다.

## 최소 환경 확인 흐름

PowerShell에서 WSL 계층을 확인한다.

```powershell
wsl --version
wsl --status
wsl --list --verbose
```

대상 distribution을 시작한 뒤 Linux 계층을 확인한다.

```bash
cat /etc/os-release
uname -r
ps -p 1 -o comm=
lscpu
nproc
free -h
df -h /
```

각 command의 원본 출력은 실행한 날짜, host와 distribution 이름을 함께 환경 기록에 저장한다. Book 문서에는 특정 computer의 값 대신 각 command가 어느 계층을 확인하는지만 남긴다.

Compiler, CMake, Git과 Python처럼 `PATH`를 통해 선택되는 program은 경로와 version을 함께 확인해야 한다. 그 이유와 새 shell 검증 방법은 [Shell Environment](<./Shell Environment.md>)에서 설명한다.

## 관련 문서

- [Linux](./Linux.md)
- [Shell Environment](<./Shell Environment.md>)
- [systemd](./systemd.md)

## References

- [Microsoft Learn - What is the Windows Subsystem for Linux?](https://learn.microsoft.com/en-us/windows/wsl/about)
- [Microsoft Learn - Basic commands for WSL](https://learn.microsoft.com/en-us/windows/wsl/basic-commands)
- [Microsoft Learn - Comparing WSL versions](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)
- [Microsoft Learn - Advanced settings configuration in WSL](https://learn.microsoft.com/en-us/windows/wsl/wsl-config)
- [Microsoft Learn - Working across Windows and Linux file systems](https://learn.microsoft.com/en-us/windows/wsl/filesystems)
- [Microsoft Learn - How to manage WSL disk space](https://learn.microsoft.com/en-us/windows/wsl/disk-space)
- [Microsoft Learn - Use systemd to manage Linux services with WSL](https://learn.microsoft.com/en-us/windows/wsl/systemd)
- [GNU Coreutils - `uname`: Print system information](https://www.gnu.org/software/coreutils/manual/html_node/uname-invocation.html)
- [GNU Coreutils - `nproc`: Print the number of available processors](https://www.gnu.org/software/coreutils/manual/html_node/nproc-invocation.html)
- [GNU Coreutils - `df`: Report file system space usage](https://www.gnu.org/software/coreutils/manual/html_node/df-invocation.html)
- [Linux manual page - `lscpu`](https://man7.org/linux/man-pages/man1/lscpu.1.html)
- [Linux manual page - `free`](https://man7.org/linux/man-pages/man1/free.1.html)
- [Microsoft Learn - Get-CimInstance](https://learn.microsoft.com/en-us/powershell/module/cimcmdlets/get-ciminstance)
- [Microsoft Learn - Get-Volume](https://learn.microsoft.com/en-us/powershell/module/storage/get-volume)
