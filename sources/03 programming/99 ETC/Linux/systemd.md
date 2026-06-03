# systemd

## 개요

`systemd`는 Linux에서 PID 1로 실행되는 init system이자 service manager다.

Linux kernel이 부팅된 뒤 첫 user space process를 시작하면, 그 process가 system의 다른 process와 service를 관리한다. systemd를 사용하는 Linux에서는 이 역할을 systemd가 담당한다.

systemd는 process만 직접 실행하는 도구가 아니라 system resource를 `unit` 단위로 관리한다.

대표적인 unit type은 다음과 같다.

- `service`
  - daemon이나 backend server처럼 실행할 process를 관리한다.
- `target`
  - 여러 unit을 묶어 system state를 표현한다.
- `socket`
  - socket activation에 사용할 socket을 관리한다.
- `timer`
  - 정해진 시간이나 주기에 unit을 실행한다.

## systemd service

`systemd service`는 `.service` 확장자를 갖는 systemd unit이다.

service unit은 process를 어떻게 시작하고, 어떤 사용자 권한으로 실행하며, 실패했을 때 어떻게 처리할지를 정의한다. 장기 실행 process를 terminal에서 직접 실행하지 않고 service로 등록하면 systemd가 process lifecycle을 관리한다.

service로 등록하면 다음을 얻을 수 있다.

- 서버 부팅 시 자동 시작
- 실패 시 재시작 정책
- `systemctl`을 통한 start, stop, restart, status 관리
- `journalctl`을 통한 표준화된 로그 확인
- 실행 사용자, 작업 디렉터리, 환경 변수의 명시적 관리

## service unit 파일

system service unit은 주로 `/etc/systemd/system/<name>.service`에 둔다.

예:

```ini
[Unit]
Description=MyApp Backend
After=network.target

[Service]
User=deploy
Group=deploy
WorkingDirectory=/srv/myapp
ExecStart=/srv/myapp/.venv/bin/python -m scripts.backend_manager --run-prod
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

각 section의 의미는 다음과 같다.

- `[Unit]`
  - unit의 설명과 다른 unit과의 관계를 정의한다.
- `[Service]`
  - service process를 어떻게 실행하고 관리할지 정의한다.
- `[Install]`
  - `systemctl enable` 시 어떤 target에 연결할지 정의한다.

주요 directive는 다음과 같다.

- `Description`
  - unit의 사람이 읽을 수 있는 설명이다.
- `After`
  - 다른 unit 이후에 시작되도록 순서만 지정한다.
- `User`, `Group`
  - service process를 실행할 계정과 group이다.
- `WorkingDirectory`
  - process의 작업 디렉터리다.
- `ExecStart`
  - service를 시작할 command다.
- `Restart`
  - process 종료 시 재시작 조건이다.
- `WantedBy`
  - enable 시 연결될 target이다.

`After=network.target`은 시작 순서를 지정할 뿐, 해당 unit을 자동으로 끌어오지는 않는다. 의존 unit을 함께 시작해야 한다면 `Wants`나 `Requires` 같은 의존 관계 directive가 필요하다.

## systemctl

`systemctl`은 systemd unit을 제어하는 command다.

unit 파일을 새로 만들거나 수정한 뒤에는 systemd manager가 unit 파일을 다시 읽게 해야 한다.

```bash
sudo systemctl daemon-reload
```

service를 지금 시작한다.

```bash
sudo systemctl start myapp-backend.service
```

service를 중지한다.

```bash
sudo systemctl stop myapp-backend.service
```

service를 재시작한다.

```bash
sudo systemctl restart myapp-backend.service
```

service 상태를 확인한다.

```bash
systemctl status myapp-backend.service
```

부팅 시 자동 시작되도록 등록한다.

```bash
sudo systemctl enable myapp-backend.service
```

자동 시작 등록과 즉시 시작을 한 번에 수행한다.

```bash
sudo systemctl enable --now myapp-backend.service
```

## journalctl

systemd는 service의 stdout, stderr와 system log를 journal에 기록한다.

특정 service log를 확인한다.

```bash
sudo journalctl -u myapp-backend.service
```

새 log를 계속 따라간다.

```bash
sudo journalctl -u myapp-backend.service -f
```

최근 log만 확인한다.

```bash
sudo journalctl -u myapp-backend.service -n 100
```

## 관련 문서

- [WebDeployment](<../../04 Web/WebDeployment.md>)
