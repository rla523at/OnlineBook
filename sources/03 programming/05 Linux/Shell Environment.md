# Shell Environment

## 한 줄 요약

Shell environment는 현재 shell process가 command를 해석하고 child process에 전달하기 위해 유지하는 상태이며, 새 shell에서 같은 상태를 얻으려면 올바른 startup file이 그 상태를 다시 구성해야 한다.

이 문서는 Linux의 Bash를 기준으로 설명한다.

## terminal, shell과 program

`terminal`은 사용자의 입력과 program의 출력을 연결하는 interface다.

`shell`은 command line을 해석하고 builtin command 또는 외부 program을 실행하는 command interpreter다. Bash는 Linux에서 사용하는 shell 중 하나다.

`program`은 shell이 실행할 수 있는 외부 실행 파일이나 script다. 예를 들어 `cmake`가 외부 실행 파일이면 Bash는 실행할 파일을 찾은 뒤 child process로 시작한다.

세 대상을 모두 terminal이라고 부르면 설정이 어느 process에 저장되는지 알기 어렵다. 다음 구조에서 environment를 직접 보유하는 대상은 각 process다.

```text
terminal
└─ bash process
   ├─ cmake process
   └─ child bash process
      └─ python3 process
```

Process의 기본 개념은 [Process](<../99 ETC/2 Process.md>) 문서를 참고한다.

## shell state와 environment

Bash가 유지하는 상태에는 shell variable, exported environment variable, function, alias, option, 현재 directory 등이 있다.

### shell variable

다음 assignment는 현재 Bash에 shell variable을 만든다.

```bash
BUILD_MODE=debug
```

이 값은 현재 Bash에서는 확인할 수 있다.

```bash
printf '%s\n' "$BUILD_MODE"
```

하지만 assignment만 한 variable은 외부 command나 child shell의 environment에 자동으로 포함되지 않는다.

### exported environment variable

`export`는 variable을 이후에 실행할 child process의 environment에 포함하도록 표시한다.

```bash
export BUILD_MODE
bash -c 'printf "%s\n" "$BUILD_MODE"'
```

`export BUILD_MODE=debug`처럼 assignment와 export를 한 번에 수행할 수도 있다.

Environment 상속 방향은 parent process에서 child process 방향이다. Child process는 상속받은 값을 변경할 수 있지만 그 변경으로 이미 실행 중인 parent shell의 environment를 바꿀 수는 없다.

특정 command에만 environment variable을 전달할 수도 있다.

```bash
ONE_COMMAND_MODE=release bash -c 'printf "%s\n" "$ONE_COMMAND_MODE"'
printf '%s\n' "${ONE_COMMAND_MODE-unset}"
```

첫 줄의 assignment는 뒤따르는 `bash -c` process의 environment에만 적용된다. 두 번째 줄에서 parent shell에 기존 `ONE_COMMAND_MODE`가 없었다면 `unset`이 출력된다.

### `export`는 영구 저장이 아니다

`export`는 현재 shell과 이후 생성되는 child process에만 영향을 준다. 현재 shell이 종료되면 그 process가 보유한 변경도 사라진다.

새 shell에서도 설정을 사용하려면 Bash가 시작할 때 읽는 startup file에 재실행 가능한 설정을 기록해야 한다.

## `PATH`와 command 선택

`PATH`는 외부 실행 파일을 찾을 directory를 `:`로 구분해 저장한 environment variable이다.

```bash
printf '%s\n' "$PATH"
```

Command 이름에 `/`가 없고 shell function이나 builtin command도 아니라면 Bash는 `PATH`의 directory를 앞에서부터 검색한다. 같은 이름의 실행 파일이 여러 개 있으면 앞에 있는 directory의 파일이 선택된다.

다음 assignment는 사용자 전용 실행 파일 directory를 기존 `PATH` 앞에 둔다.

```bash
export PATH="$HOME/.local/bin:$PATH"
```

이 command는 program을 설치하지 않는다. 현재 Bash의 `PATH` 값과 child process에 전달할 environment만 변경한다.

### 실제로 선택되는 command 확인

```bash
type -a cmake
command -v cmake
cmake --version
```

- `type -a cmake`
  - Bash가 `cmake`라는 이름을 해석할 수 있는 모든 위치와 종류를 보여준다.
- `command -v cmake`
  - 현재 shell이 `cmake`를 command로 사용할 때 선택하는 대상을 보여준다.
- `cmake --version`
  - 선택된 program을 실제로 실행해 그 program이 보고하는 version을 확인한다.

Version 출력만 기록하면 의도하지 않은 경로의 program을 검사했을 수 있다. 실행 경로와 version을 함께 확인해야 `PATH` 우선순위까지 검증할 수 있다.

Bash는 이미 찾은 외부 command 경로를 hash table에 기억할 수 있다. 설치나 파일 이동 뒤 현재 shell이 이전 경로를 계속 기억한다고 의심되면 다음 command로 기억한 경로를 지울 수 있다.

```bash
hash -r
```

이 command는 현재 Bash의 command path cache만 비우며 파일을 삭제하지 않는다.

## interactive shell과 login shell

`interactive shell`은 terminal에서 사용자의 command를 반복해서 읽는 shell이다. Script를 실행하기 위해 입력 없이 동작하는 shell은 non-interactive shell일 수 있다.

`login shell`은 login 과정으로 시작되었거나 `--login` option으로 시작된 shell이다. Interactive 여부와 login 여부는 서로 다른 분류이므로 interactive login shell과 interactive non-login shell이 모두 가능하다.

현재 Bash 상태를 확인하는 예시는 다음과 같다.

```bash
printf 'Bash executable: %s\n' "$BASH"

case "$-" in
    *i*) echo 'interactive shell' ;;
    *)   echo 'non-interactive shell' ;;
esac

if shopt -q login_shell; then
    echo 'login shell'
else
    echo 'non-login shell'
fi
```

`$SHELL`은 사용자의 login shell 경로를 나타낼 수 있지만 현재 실행 중인 process가 반드시 그 shell이라는 뜻은 아니다. 현재 Bash executable은 `$BASH`로 확인한다.

Terminal program과 실행 방식에 따라 새 tab이 login shell을 만들 수도 있고 non-login shell을 만들 수도 있다. Startup file을 고르기 전에 실제 상태를 확인하고, 특정 terminal의 동작을 모든 Bash 환경의 규칙으로 일반화하지 않는다.

## Bash startup file

Bash가 읽는 startup file은 실행 방식에 따라 달라진다.

| Bash 실행 방식 | 읽는 주요 startup file |
|---|---|
| interactive login shell | `/etc/profile` 이후 `~/.bash_profile`, `~/.bash_login`, `~/.profile` 중 처음 발견한 파일 하나 |
| interactive non-login shell | `~/.bashrc` |
| `--login` option이 없는 non-interactive shell | `BASH_ENV`가 지정한 파일 |

Interactive login shell은 세 사용자 파일을 전부 읽지 않는다. `~/.bash_profile`, `~/.bash_login`, `~/.profile` 순서로 찾아 처음 존재하고 읽을 수 있는 파일 하나만 읽는다.

Login startup file에서 `~/.bashrc`를 명시적으로 읽도록 구성할 수 있다.

```bash
if [ -f "$HOME/.bashrc" ]; then
    . "$HOME/.bashrc"
fi
```

이 연결은 사용자의 startup file에 실제로 작성되어 있을 때만 동작한다. Bash가 모든 login shell에서 자동으로 `.bashrc`까지 읽는다고 가정하면 안 된다.

### 설정 위치 선택

- Interactive Bash에서 사용할 alias, prompt와 interactive tool 설정
  - `~/.bashrc`에 둔다.
- Login 시 한 번 구성할 environment
  - 실제 login startup file chain을 확인한 뒤 `~/.profile` 또는 `~/.bash_profile`에 둔다.
- Non-interactive script
  - Interactive startup file이 자동으로 실행된다고 가정하지 말고 script의 dependency와 environment를 명시한다.

같은 `PATH` 변경을 여러 startup file에 중복해서 넣으면 file 간 연결 방식에 따라 같은 directory가 반복 추가될 수 있다. 한 위치를 기준으로 삼고 다른 startup file에서는 필요한 경우 그 파일을 한 번 연결한다.

`~/.bashrc`가 반복해서 실행될 수 있는 환경이라면 다음처럼 동일한 directory를 중복 추가하지 않게 할 수 있다.

```bash
if [[ ":$PATH:" != *":$HOME/.local/bin:"* ]]; then
    export PATH="$HOME/.local/bin:$PATH"
fi
```

이 예시는 Bash 문법인 `[[ ... ]]`을 사용하므로 Bash가 아닌 shell이 읽는 범용 `~/.profile`에 그대로 복사하지 않는다.

## `source`와 별도 실행

`source`와 `.`은 파일의 command를 현재 shell context에서 읽고 실행하는 Bash builtin이다.

```bash
source "$HOME/.bashrc"
# 다음과 같은 의미다.
. "$HOME/.bashrc"
```

Startup file이 environment variable이나 alias를 설정하면 현재 Bash에서 즉시 확인할 수 있다.

반면 다음 command는 새로운 Bash process에서 script를 실행한다.

```bash
bash setup.sh
```

`setup.sh`가 environment를 변경해도 그 child Bash가 종료된 뒤 parent shell의 environment에는 반영되지 않는다.

`source ~/.bashrc`는 현재 shell에서 설정의 즉시 효과를 확인하는 데 유용하지만, 새 shell이 올바른 startup file을 읽는지 증명하지는 않는다.

## 새 shell에서 검증

### 중첩 shell의 한계

현재 shell에서 다음처럼 child Bash를 시작하면 exported environment variable을 그대로 상속한다.

```bash
bash
```

따라서 현재 shell에서 수동으로 `export`한 뒤 child Bash에서 값이 보인다는 사실만으로 startup file에 설정이 영구적으로 기록되었다고 판단할 수 없다.

### 검증 절차

1. 현재 shell에서 command의 경로와 version을 기록한다.

   ```bash
   type -a cmake
   command -v cmake
   cmake --version
   ```

2. 필요한 installation이나 startup file 변경을 수행한다.

3. `source`로 현재 shell의 문법과 즉시 효과를 먼저 확인한다.

   ```bash
   source "$HOME/.bashrc"
   ```

4. 현재 session을 종료한다.

   ```bash
   exit
   ```

5. 원래 terminal 또는 WSL launcher에서 새 session을 시작한다. 변경한 environment를 가진 기존 Bash 아래에서 중첩 Bash만 시작하지 않는다.

6. 새 shell의 종류와 command 선택을 다시 확인한다.

   ```bash
   case "$-" in *i*) echo interactive;; *) echo non-interactive;; esac
   shopt -q login_shell && echo login || echo non-login
   type -a cmake
   command -v cmake
   cmake --version
   ```

Compiler, CMake, Git과 Python을 함께 확인하는 예시는 다음과 같다.

```bash
type -a gcc cmake git python3
gcc --version
cmake --version
git --version
python3 --version
```

이 command들은 설치 상태를 바꾸지 않는다. `type -a` 결과와 각 program의 version 출력을 함께 저장하면 어떤 실행 파일을 검증했는지 추적할 수 있다.

WSL의 `.wslconfig`, `/etc/wsl.conf` 또는 systemd boot 설정은 Bash startup file보다 바깥 계층의 상태다. 이런 설정은 새 shell만 열어서는 반영되지 않을 수 있으므로 [WSL](./WSL.md)의 lifecycle 구분에 따라 distribution 또는 WSL virtual machine을 재시작한다.

## 자주 혼동하는 상태

| 관찰 | 실제 의미 |
|---|---|
| 현재 shell에서 variable이 보인다. | 현재 process에 값이 있다는 뜻이며 startup file에 저장되었다는 뜻은 아니다. |
| Child shell에서 exported variable이 보인다. | Parent의 environment를 상속했다는 뜻일 수 있으며 영속성 검증은 아니다. |
| `PATH`에 directory를 추가했다. | 검색 경로를 바꾼 것이며 program을 설치한 것은 아니다. |
| `program --version`이 성공했다. | 어떤 program 하나를 실행했다는 뜻이며 의도한 경로인지는 `type`이나 `command -v`로 따로 확인해야 한다. |
| `source ~/.bashrc` 후 설정이 동작한다. | 현재 Bash에 적용된 것이며 새 Bash가 그 파일을 자동으로 읽는지는 별도 검증이 필요하다. |
| 새 terminal을 열었다. | 새 shell은 시작되지만 login 여부와 읽는 startup file은 launcher 설정에 따라 달라질 수 있다. |

## 관련 문서

- [Linux](./Linux.md)
- [WSL](./WSL.md)
- [Process](<../99 ETC/2 Process.md>)

## References

- [GNU Bash Reference Manual - Shell Parameters](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameters.html)
- [GNU Bash Reference Manual - Environment](https://www.gnu.org/software/bash/manual/html_node/Environment.html)
- [GNU Bash Reference Manual - Command Execution Environment](https://www.gnu.org/software/bash/manual/html_node/Command-Execution-Environment.html)
- [GNU Bash Reference Manual - Command Search and Execution](https://www.gnu.org/software/bash/manual/html_node/Command-Search-and-Execution.html)
- [GNU Bash Reference Manual - Bash Startup Files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)
- [GNU Bash Reference Manual - Bourne Shell Builtins](https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html)
- [GNU Bash Reference Manual - Bash Builtins](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html)
