# Web Deployment

## 한 줄 요약

웹 애플리케이션 배포에서는 앱 코드, 운영 명령, 정적 파일 서빙 경로, 실행 사용자, reverse proxy, systemd service를 분리해서 운영 안정성을 높인다.

## 배포 구조의 기본 모델

예시 배포 구조는 다음 책임을 나눈다.

- app checkout
  - 소스 코드와 `.venv`, `frontend`, `backend`가 있는 위치
- web root
  - Nginx가 읽는 frontend 정적 파일 위치
- service account
  - 앱 파일과 runtime을 소유하는 사용자
- system service
  - backend process를 지속적으로 실행하고 재시작하는 단위
- reverse proxy
  - 공개 HTTP request를 frontend 정적 파일 또는 backend API로 분기하는 계층

예:

```text
/usr/local/bin/deploy      운영 명령
/srv/myapp                 app checkout
/var/www/myapp             frontend web root
myapp-backend.service      backend systemd service
deploy                     service account
nginx                      public web server and reverse proxy
```

## checkout과 web root를 분리하는 이유

React/Vite frontend는 build 후 정적 파일 묶음이 된다.

```text
/srv/myapp/frontend/dist
```

하지만 Nginx가 실제로 읽는 위치는 별도 web root로 두는 경우가 많다.

```text
/var/www/myapp
```

이렇게 나누면 다음 장점이 있다.

- 소스 코드와 공개 정적 파일의 책임이 분리된다.
- Nginx는 필요한 정적 파일만 읽는다.
- 배포 스크립트가 build 결과를 web root로 동기화하는 방식이 명확해진다.

예:

```bash
rsync -av --delete /srv/myapp/frontend/dist/ /var/www/myapp/
```

## deploy service account

운영 서버에서는 앱 코드와 runtime 파일을 root가 아니라 별도 service account가 소유하게 하는 편이 좋다.

예:

```text
deploy:deploy /srv/myapp
deploy:deploy /var/www/myapp
```

이 구조의 목적은 다음과 같다.

- 앱 파일이 root 소유로 꼬이는 것을 방지한다.
- 사람이 로그인하는 운영자 계정과 앱 실행 계정을 분리한다.
- 시스템 영역 작업만 `sudo`로 수행하게 한다.

예:

```bash
sudo chown -R deploy:deploy /srv/myapp
sudo chown -R deploy:deploy /var/www/myapp
```

## `/usr/local/bin/deploy`

`/usr/local/bin`은 서버 관리자가 직접 설치한 실행 파일과 스크립트를 두는 표준 경로다.

배포 명령을 checkout 내부 스크립트로 호출하는 대신 `/usr/local/bin/deploy`에 두면 어느 디렉터리에서든 같은 명령으로 배포할 수 있다.

```bash
deploy production
deploy staging <branch>
```

일반적인 배포 명령은 다음 순서로 동작한다.

1. target에 맞는 checkout, web root, service 이름을 결정한다.
2. Git pull 또는 checkout sync를 수행한다.
3. Python dependency와 frontend dependency를 갱신한다.
4. DB migration을 적용한다.
5. frontend를 build한다.
6. build 결과를 web root로 동기화한다.
7. backend service를 restart한다.

Git, Python, npm 작업은 앱 파일을 소유한 service account 권한으로 실행하고, systemd나 Nginx 같은 시스템 영역 작업만 root 권한으로 수행하는 편이 안전하다.

## Nginx의 역할

Nginx는 웹 서버이자 reverse proxy다.

- frontend 정적 파일 request
  - `/var/www/myapp`에서 파일을 읽어 응답
- backend API request
  - `/api/...` request를 내부 backend port로 전달

예:

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:8300;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

브라우저 입장에서는 같은 host로 요청하지만, Nginx가 내부적으로 정적 파일과 backend API를 나눠 처리한다.

```text
https://app.example.com/          -> /var/www/myapp
https://app.example.com/api/...   -> http://127.0.0.1:8300
```

## SPA fallback

React SPA는 브라우저 경로를 프론트 라우터가 해석한다. 따라서 사용자가 `/admin/users` 같은 경로를 직접 열어도 서버는 `index.html`을 내려줘야 한다.

Nginx 예:

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

이 설정이 없으면 실제 파일이 없는 SPA route에서 새로고침 시 404가 발생할 수 있다.

## host 분기와 port 분기

운영과 staging을 나누는 방법은 크게 두 가지다.

- host 분기
  - `app.example.com`, `staging.example.com`
- port 분기
  - `example.com:80`, `example.com:8080`

공개 웹 서비스에서는 보통 host 분기가 더 자연스럽다.

- 사용자가 port를 직접 붙이지 않아도 된다.
- 쿠키와 CORS 구성이 단순해진다.
- HTTPS 인증서와 Nginx server block을 환경별로 나누기 쉽다.

## systemd service

웹 backend는 장기 실행 process이므로 운영 서버에서는 systemd service로 등록해 실행할 수 있다.

systemd는 backend 전용 기술이 아니라 Linux의 init system이자 service manager다. unit, service, target, `systemctl`, `journalctl` 개념은 [systemd](<../99 ETC/Linux/systemd.md>)를 참고한다.

웹 배포에서 systemd service는 다음 역할을 담당한다.

- backend process 시작, 중지, 재시작
- 서버 부팅 시 backend 자동 시작
- 실패 시 restart policy 적용
- journal에 backend log 기록

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

반영과 확인:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now myapp-backend.service
systemctl status myapp-backend.service
sudo journalctl -u myapp-backend.service -f
```

## SSH와 운영자 계정

SSH는 원격 서버에 안전하게 접속하고 명령을 실행하기 위한 표준 방식이다.

운영 서버에서는 보통 비밀번호 로그인보다 공개키 로그인을 권장한다.

```powershell
ssh -i "$HOME\.ssh\<user>_server" <user>@<server-host>
```

구조는 다음과 같다.

- 개인키
  - 접속하는 사람의 PC에만 둔다.
- 공개키
  - 서버의 `~/.ssh/authorized_keys`에 등록한다.

서버에서 실제 접속 허용 기준은 클라우드 콘솔에 보이는 키 목록이 아니라 각 계정의 `authorized_keys` 파일이다.

## 파일 소유권과 권한

Linux 파일 권한은 owner, group, other 기준으로 나뉜다.

```text
-rwxr-x---
```

- owner 권한
- group 권한
- other 권한

SSH 키 인증에서는 권한이 너무 넓으면 보안상 키가 무시될 수 있다.

일반적인 기준:

```text
~/.ssh                 700
~/.ssh/authorized_keys 600
```

예:

```bash
sudo chmod 700 /home/<user>/.ssh
sudo chmod 600 /home/<user>/.ssh/authorized_keys
sudo chown -R <user>:<user> /home/<user>/.ssh
```

운영 스크립트는 보통 root가 소유하고 누구나 실행할 수 있게 둔다.

```bash
sudo chown root:root /usr/local/bin/deploy
sudo chmod 755 /usr/local/bin/deploy
```

## 자주 하는 실수

- checkout과 web root를 같은 역할로 생각하는 경우
- root로 `npm install`이나 `pip install`을 실행해 앱 파일 소유권이 꼬이는 경우
- SPA fallback 없이 정적 파일만 서빙하는 경우
- Nginx 설정 수정 후 `nginx -t` 없이 reload하는 경우
- service unit을 수정하고 `systemctl daemon-reload`를 빼먹는 경우
- `.ssh`와 `authorized_keys` 권한을 너무 넓게 설정하는 경우

## 관련 문서

- [ReactFrontend.md](./ReactFrontend.md)
- [FastAPIBackend.md](./FastAPIBackend.md)
- [HTTPAndBrowser.md](./HTTPAndBrowser.md)
- [Cloud.md](./Cloud.md)
- [systemd](<../99 ETC/Linux/systemd.md>)

