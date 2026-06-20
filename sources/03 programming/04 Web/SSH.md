# SSH

## 한 줄 요약

SSH는 원격 서버에 안전하게 접속하고 명령을 실행하기 위한 protocol이다. SSH 보안은 두 방향의 신원 확인으로 구성된다.

- 사용자 인증: 접속하는 사람이 서버에 자신을 증명한다.
- 서버 인증: 접속 대상 서버가 client에 자기 신원을 증명한다.

`authorized_keys`는 사용자 인증에 쓰이고, `known_hosts`는 서버 인증에 쓰인다. 두 파일은 모두 SSH와 관련되지만 인증 방향이 반대다.

## 먼저 구분할 용어

| 용어 | 의미 |
|---|---|
| SSH client | 접속을 시작하는 쪽이다. 로컬 PC, 운영자 노트북, GitHub Actions runner가 해당한다. |
| SSH server | 접속을 받는 쪽이다. Linux instance, production server, staging server가 해당한다. |
| SSH key pair | public key와 private key 한 쌍이다. 보통 사용자 인증에 사용한다. |
| SSH host key | SSH server가 자기 신원을 증명하기 위해 가진 key pair이다. |
| `authorized_keys` | 특정 Linux 계정으로 로그인할 수 있는 사용자 public key 목록이다. |
| `known_hosts` | SSH client가 신뢰하는 SSH server host public key 목록이다. |
| `StrictHostKeyChecking` | SSH client가 서버 host key 검증을 얼마나 엄격하게 적용할지 정하는 option이다. |

## 사용자 인증: SSH key pair와 authorized_keys

사용자 인증은 client가 서버에 “나는 이 Linux 계정으로 접속할 권한이 있다”는 사실을 증명하는 과정이다.

```text
private key
  -> 접속하는 사람 또는 automation runner가 보관한다.
  -> 외부에 노출하면 안 된다.

public key
  -> 접속을 허용할 서버 Linux 계정의 authorized_keys에 등록한다.
  -> 공개되어도 private key 없이 로그인 권한이 생기지는 않는다.
```

예를 들어 다음 명령은 `ubuntu` Linux 계정으로 서버에 접속한다.

```bash
ssh -i ~/.ssh/admin_key ubuntu@203.0.113.10
```

서버는 `/home/ubuntu/.ssh/authorized_keys`에 등록된 public key 중 하나와 client가 제시한 private key가 짝인지 확인한다. 같은 private key를 사용하더라도 사용자 이름이 다르면 서버가 확인하는 `authorized_keys` 파일도 달라진다.

```bash
ssh -i ~/.ssh/admin_key ubuntu@server
ssh -i ~/.ssh/admin_key deploy@server
```

첫 번째 명령은 `/home/ubuntu/.ssh/authorized_keys`를 기준으로 검증하고, 두 번째 명령은 `/home/deploy/.ssh/authorized_keys`를 기준으로 검증한다.

## 서버 인증: SSH host key와 known_hosts

서버 인증은 server가 client에 “나는 네가 접속하려는 그 서버가 맞다”는 사실을 증명하는 과정이다.

SSH server는 설치 시점에 host key pair를 가진다. 일반적으로 public key 파일은 다음 위치에 있다.

```text
/etc/ssh/ssh_host_ed25519_key.pub
/etc/ssh/ssh_host_ecdsa_key.pub
/etc/ssh/ssh_host_rsa_key.pub
```

SSH client는 서버가 제시한 host public key를 자기 `known_hosts` 파일에 저장된 값과 비교한다.

```text
server host public key
  -> 서버가 client에 제시한다.

known_hosts
  -> client가 신뢰하는 server host public key 목록이다.
```

`known_hosts`의 한 줄은 다음 형태다.

```text
203.0.113.10 ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA...
```

이 줄은 `203.0.113.10` 서버가 `ssh-ed25519` host key를 사용하며, public key 값이 뒤의 문자열과 같아야 한다는 뜻이다.

## authorized_keys와 known_hosts의 차이

| 파일 | 위치 | 누가 사용하나 | 목적 |
|---|---|---|---|
| `authorized_keys` | SSH server의 Linux 계정 home 아래 | server | 이 Linux 계정으로 로그인할 수 있는 사용자 public key를 확인한다. |
| `known_hosts` | SSH client의 `.ssh` 아래 또는 automation runner의 임시 파일 | client | 접속 대상 server의 host public key가 기대한 값인지 확인한다. |

두 파일을 혼동하면 SSH 보안 모델을 잘못 이해하게 된다.

- `authorized_keys`에 key를 등록하면 사용자가 서버에 로그인할 수 있다.
- `known_hosts`에 key를 등록하면 client가 서버의 신원을 검증할 수 있다.
- `known_hosts`만으로는 서버 로그인 권한이 생기지 않는다.
- `authorized_keys`만으로는 client가 잘못된 서버에 접속하는 문제를 막을 수 없다.

## StrictHostKeyChecking

`StrictHostKeyChecking`은 SSH client가 서버 host key 검증을 어떻게 처리할지 정하는 option이다.

| 값 | 동작 |
|---|---|
| `yes` | `known_hosts`에 없는 서버나 host key가 바뀐 서버 접속을 거부한다. |
| `accept-new` | 처음 보는 서버는 `known_hosts`에 추가하고, 이후 key가 바뀌면 거부한다. |
| `no` | host key 확인을 약하게 처리한다. automation에서는 기본 선택지로 사용하지 않는다. |

운영 automation에서는 `StrictHostKeyChecking=yes`를 기준으로 삼고, workflow 시작 시점에 신뢰할 `known_hosts` 값을 미리 준비하는 방식이 안전하다.

```bash
ssh -o StrictHostKeyChecking=yes -o UserKnownHostsFile=./known_hosts user@server
```

`StrictHostKeyChecking=no`는 편하지만 잘못된 서버에 접속하는 위험을 키운다. 임시 진단을 제외하고 운영 workflow의 기본값으로 두지 않는다.

## GitHub Actions에서 known_hosts가 필요한 이유

GitHub Actions runner는 매 실행마다 새 환경으로 시작한다. 로컬 PC처럼 `~/.ssh/known_hosts`가 누적되어 있다고 가정할 수 없다.

따라서 workflow는 다음 값을 직접 준비해야 한다.

- 접속할 server host
- 접속할 Linux user
- SSH private key
- `known_hosts`에 넣을 server host public key

이 중 SSH private key는 사용자 인증에 필요하고, `known_hosts`는 서버 인증에 필요하다.

`known_hosts` 검증이 없으면 다음 문제가 생길 수 있다.

- 잘못된 IP나 DNS로 접속해도 workflow가 감지하지 못한다.
- 네트워크 중간에서 다른 서버가 응답하는 상황을 막기 어렵다.
- automation private key가 의도하지 않은 서버에 사용될 수 있다.

운영 workflow에서는 서버별 host public key를 미리 수집하고, GitHub Actions secret 또는 variable로 전달한 뒤 workflow 안에서 `~/.ssh/known_hosts` 파일로 써서 사용한다.

## .ssh 파일 권한

OpenSSH server는 key 관련 파일 권한이 너무 넓으면 보안상 로그인을 거부할 수 있다. 일반 기준은 다음과 같다.

```text
~/.ssh                 700
~/.ssh/authorized_keys 600
private key            600
known_hosts            644 또는 600
```

예:

```bash
sudo install -d -o <user> -g <user> -m 700 /home/<user>/.ssh
sudo install -o <user> -g <user> -m 600 authorized_keys /home/<user>/.ssh/authorized_keys
chmod 600 ~/.ssh/my_private_key
```

`known_hosts`는 private key가 아니므로 공개되어도 로그인 권한이 생기지 않는다. 다만 운영 환경에서는 workflow log에 불필요하게 출력하지 않는 편이 좋다.

## Linux 사용자 계정과 sudo

SSH 접속에 성공했다는 사실은 관리자 권한을 가진다는 뜻이 아니다. SSH는 특정 Linux 계정으로 로그인하는 수단이고, 관리자 권한은 `sudo` 설정으로 별도 결정된다.

```bash
ssh -i key ms@server
ssh -i key deploy@server
```

위 두 명령은 같은 서버와 같은 private key를 사용하더라도 서로 다른 Linux 계정으로 로그인한다. 두 계정의 group, home directory, sudoers rule이 다르면 실행할 수 있는 작업도 다르다.

`sudo` 권한은 `/etc/sudoers`와 `/etc/sudoers.d/`에서 결정된다. 사람별 관리자 계정은 `sudo` group membership으로 관리자 권한을 얻을 수 있고, automation 계정은 필요한 command만 `NOPASSWD`로 허용하는 방식이 안전하다.

## 사람 계정과 자동화 계정

운영 서버에서는 사람 계정과 automation 계정을 분리한다.

| 계정 유형 | 목적 | key 기준 | sudo 기준 |
|---|---|---|---|
| 사람별 관리자 계정 | 수동 점검, 장애 대응, 권한 정리 | 개인 public key | 관리자 권한 또는 조직 정책에 따른 sudo |
| 비상 관리자 계정 | 사람 계정 접근 장애 시 복구 | break-glass key | 관리자 권한 |
| application runtime 계정 | systemd service 실행, repository checkout 소유 | SSH key 없음 | 없음 |
| automation 계정 | GitHub Actions, 배포, 검증 | automation 전용 key | 필요한 command만 `NOPASSWD` |

개인 private key를 automation secret으로 재사용하지 않는다. production automation key와 staging automation key도 분리한다.

## Host key 변경 시 처리

서버를 재설치하거나 SSH host key를 재생성하면 기존 `known_hosts` 값과 서버가 제시하는 host key가 달라진다. 이때 SSH client는 경고를 내고 접속을 거부할 수 있다.

처리 순서는 다음과 같다.

1. 서버 재설치 또는 host key 변경이 의도된 작업인지 확인한다.
2. 서버 console 또는 신뢰할 수 있는 관리자 접속 경로로 새 host public key fingerprint를 확인한다.
3. 기존 `known_hosts` 값을 새 값으로 교체한다.
4. workflow 또는 client 접속을 다시 검증한다.

경고를 없애기 위해 `StrictHostKeyChecking=no`로 우회하는 것은 운영 기준으로 사용하지 않는다.

## 자주 헷갈리는 표현

| 표현 | 정확한 의미 |
|---|---|
| SSH key | 보통 사용자 인증용 key pair를 가리킨다. 문맥에 따라 host key와 구분해야 한다. |
| public key를 서버에 등록한다 | 대부분 `authorized_keys`에 사용자 public key를 등록한다는 뜻이다. |
| known_hosts에 등록한다 | client가 server host public key를 신뢰하도록 등록한다는 뜻이다. |
| SSH 접속 가능 | 특정 Linux 계정으로 로그인할 수 있다는 뜻이다. sudo 권한까지 보장하지 않는다. |
| server fingerprint | server host public key를 사람이 비교하기 쉽게 줄인 값이다. |

## 관련 문서

- [AmazonLightsail.md](./AmazonLightsail.md): Lightsail instance에서 SSH 접속을 사용하는 맥락을 설명한다.
- [WebDeployment.md](./WebDeployment.md): 웹 애플리케이션 배포에서 SSH 계정과 배포 command를 사용하는 맥락을 설명한다.