# Amazon Lightsail

## 한 줄 요약

Amazon Lightsail은 AWS에서 웹 서비스 운영에 필요한 가상 서버, 관리형 데이터베이스, 고정 IP, 방화벽, DNS, 스냅샷 같은 리소스를 비교적 단순한 화면과 요금 단위로 제공하는 서비스다.

Lightsail은 AWS의 모든 인프라 기능을 세밀하게 조합하는 서비스가 아니라, 작은 웹 사이트나 웹 애플리케이션을 빠르게 운영하기 위한 기본 구성 요소를 묶어서 제공한다. 그래서 EC2, RDS, Route 53, CloudFront 같은 서비스를 처음부터 직접 조합하는 것보다 시작하기 쉽다.

## 먼저 알아야 할 계층

Lightsail을 이해하려면 다음 계층을 구분해야 한다.

```text
AWS 계정
  -> AWS Region
    -> Amazon Lightsail
      -> Lightsail 리소스
        -> 인스턴스
        -> Static IP
        -> DB 리소스
        -> 스냅샷
        -> DNS zone
        -> 로드 밸런서
        -> CDN distribution
        -> block storage disk
        -> object storage bucket
```

`AWS 계정`은 결제, 권한, 리소스 소유의 기준이다.

`AWS Region`은 리소스를 생성하는 지리적 위치다. 예를 들어 서울 리전은 `ap-northeast-2`다. 같은 이름의 리소스라도 리전이 다르면 별개의 리소스다.

`Amazon Lightsail`은 AWS 안의 서비스다. Lightsail 콘솔에서 인스턴스, 데이터베이스, 네트워크, 스토리지, 스냅샷 같은 리소스를 만든다.

`Lightsail 리소스`는 Lightsail에서 생성하고 관리하는 대상 전체를 뜻한다. 인스턴스만 리소스인 것이 아니라 DB, Static IP, 스냅샷도 모두 리소스다.

## Lightsail 리소스란 무엇인가

`리소스`는 클라우드에서 생성해서 사용하는 관리 대상이다. 내 컴퓨터에 직접 설치한 파일이나 프로그램이 아니라, AWS 계정 안에 만들어지고 AWS가 관리하는 객체다.

Lightsail에서 자주 보는 리소스는 다음과 같다.

| 리소스 | 의미 | 예 |
| --- | --- | --- |
| 인스턴스 | 가상 서버 | Ubuntu 서버, WordPress 서버 |
| Static IP | 인스턴스에 붙이는 고정 public IP | `43.200.56.130` |
| DB 리소스 | Lightsail이 관리하는 MySQL 또는 PostgreSQL 서버 | PostgreSQL managed database |
| 스냅샷 | 인스턴스, DB, disk의 백업 이미지 | 인스턴스 snapshot |
| DNS zone | 도메인의 DNS record를 관리하는 영역 | `example.com` zone |
| 로드 밸런서 | 여러 인스턴스로 트래픽을 나누는 리소스 | 웹 트래픽 분산 |
| CDN distribution | 정적 파일을 사용자와 가까운 위치에서 전달하는 리소스 | 이미지, JS, CSS 전송 최적화 |
| block storage disk | 인스턴스에 추가로 붙이는 디스크 | 데이터 저장용 추가 disk |
| object storage bucket | 파일 객체를 저장하는 저장소 | 이미지, 백업 파일 저장 |

이 문서에서 `Lightsail 리소스`라는 말은 위와 같은 관리 대상을 통칭한다.

## Lightsail 인스턴스

`Lightsail 인스턴스`는 Amazon Lightsail에서 만든 가상 프라이빗 서버다. 가상 프라이빗 서버는 물리 서버 한 대를 직접 소유하는 것이 아니라, AWS 데이터센터의 물리 서버 위에서 가상화 기술로 분리되어 실행되는 서버를 뜻한다.

Lightsail 인스턴스를 만들 때는 다음을 선택한다.

- Region과 Availability Zone
- OS 또는 application image
- instance plan 또는 bundle
- SSH key pair
- instance name
- optional launch script
- optional tags

`image`는 인스턴스에 처음 설치될 OS 또는 application stack의 기준 이미지다. Ubuntu, Debian 같은 OS 이미지를 고를 수도 있고, WordPress, LAMP, Node.js 같은 application stack 이미지를 고를 수도 있다.

`plan` 또는 `bundle`은 CPU, memory, SSD 용량, 데이터 전송량 같은 서버 크기를 묶은 요금 단위다. 예를 들어 2GB RAM 인스턴스와 4GB RAM 인스턴스는 서로 다른 plan이다.

Lightsail 인스턴스는 실무에서 서버 컴퓨터처럼 다룬다. SSH로 접속하고, 패키지를 설치하고, Nginx를 설정하고, systemd service를 등록하고, 애플리케이션을 배포한다. 다만 실제로는 물리 서버가 아니라 AWS에서 실행되는 가상 서버다.

## 인스턴스와 서버 컴퓨터의 관계

`서버 컴퓨터`라는 표현은 문맥에 따라 두 가지 의미로 쓰인다.

첫 번째 의미는 물리 장비다. 회사나 데이터센터에 실제로 놓여 있는 컴퓨터를 뜻한다.

두 번째 의미는 서버 역할을 하는 실행 환경이다. 웹 요청을 받고 애플리케이션을 실행하는 대상이면 물리 장비가 아니어도 서버라고 부른다.

Lightsail 인스턴스는 두 번째 의미의 서버 컴퓨터에 해당한다. 직접 만질 수 있는 물리 컴퓨터는 아니지만, SSH로 접속해서 OS와 프로그램을 운영한다는 점에서 서비스 운영자에게는 서버처럼 동작한다.

정확히 표현하면 다음과 같다.

```text
Lightsail 인스턴스
  = AWS Cloud에서 실행되는 가상 서버
  = 운영자가 SSH로 접속해서 애플리케이션을 배포하는 서버 환경
  != 사용자가 직접 소유한 물리 서버 컴퓨터
```

## Region과 Availability Zone

`Region`은 AWS 리소스가 위치하는 지리적 영역이다. 서울 리전은 `ap-northeast-2`다.

`Availability Zone`은 하나의 Region 안에 있는 분리된 데이터센터 영역이다. 예를 들어 서울 리전 안에는 `ap-northeast-2a` 같은 zone이 있다.

Lightsail 인스턴스와 DB 리소스는 특정 Region에 생성된다. 같은 Region 안의 리소스끼리는 private network로 통신할 수 있는 경우가 많고, 다른 Region의 리소스는 별도 네트워크 설계가 필요하다.

운영 문서에서 `서울, 영역 A` 같은 표현이 나오면 특정 Region과 Availability Zone에 생성된 Lightsail 리소스를 가리킨다.

## Public IP와 private IP

Lightsail 인스턴스에는 public IP와 private IP가 있다.

`public IP`는 인터넷에서 접근 가능한 주소다. 브라우저, SSH client, 외부 사용자가 인스턴스에 접근할 때 public IP 또는 그 public IP를 가리키는 도메인을 사용한다.

`private IP`는 같은 Lightsail 계정과 같은 Region 안의 리소스에서 접근할 수 있는 내부 주소다. 인터넷에서 직접 접근하는 주소가 아니다.

예를 들면 다음과 같다.

```text
브라우저 사용자
  -> public IP 또는 domain
  -> Lightsail 인스턴스

같은 Region의 내부 리소스
  -> private IP
  -> Lightsail 인스턴스
```

public IP는 인스턴스를 중지했다가 다시 시작하면 바뀔 수 있다. 그래서 운영 서비스에서는 Static IP를 붙인다.

## Static IP 리소스

`Static IP`는 Lightsail 인스턴스에 연결하는 고정 public IPv4 주소다. 기본 public IP는 인스턴스 stop/start 후 바뀔 수 있지만, Static IP는 연결된 인스턴스가 다시 시작되어도 같은 주소를 유지한다.

웹 서비스에서 Static IP가 중요한 이유는 domain과 연결되기 때문이다.

```text
domain
  -> DNS A record
  -> Static IP
  -> Lightsail 인스턴스
```

도메인을 쓰지 않고 IP 자체로 접속하더라도 운영 서버의 public IP가 바뀌면 접속 주소가 달라진다. 따라서 운영 인스턴스에는 Static IP를 붙이는 것이 관리 기준으로 적절하다.

Static IP는 인스턴스와 별개의 Lightsail 리소스다. 그래서 인스턴스를 삭제하기 전에는 Static IP가 필요한지 따로 확인해야 한다.

## IPv4와 IPv6

`IPv4`는 `43.200.56.130`처럼 점으로 구분된 숫자 주소 체계다. 현재 웹 서비스 운영에서 널리 사용된다.

`IPv6`는 더 긴 형식의 주소 체계다. Lightsail의 일부 리소스는 IPv6도 사용할 수 있다.

IPv4와 IPv6는 별개의 주소 체계다. Lightsail 방화벽도 IPv4 방화벽과 IPv6 방화벽이 따로 관리된다. IPv4에서 443 포트를 열었다고 해서 IPv6에서도 자동으로 같은 규칙이 적용되는 것은 아니다.

웹 서비스를 IPv4만으로 운영한다면 IPv4 방화벽과 IPv4 DNS 설정을 기준으로 확인한다. IPv6까지 사용한다면 IPv6 주소, IPv6 DNS record, IPv6 방화벽 규칙도 따로 확인해야 한다.

## Lightsail 방화벽

Lightsail 인스턴스에는 public IP로 들어오는 트래픽을 제어하는 방화벽이 있다.

방화벽 규칙은 다음 요소로 구성된다.

- protocol
- port 또는 port range
- source IP 또는 source IP range

예를 들어 웹 서비스를 운영한다면 다음 포트가 자주 사용된다.

| 용도 | Protocol | Port |
| --- | --- | --- |
| SSH | TCP | 22 |
| HTTP | TCP | 80 |
| HTTPS | TCP | 443 |

SSH는 서버 관리자가 접속하는 포트다. HTTP와 HTTPS는 브라우저 사용자가 웹 서비스에 접속하는 포트다.

운영 서버의 기본 원칙은 필요한 포트만 여는 것이다. 웹 서비스라면 외부에는 80, 443을 열고, SSH 22는 운영자 IP로 제한하는 구성이 더 안전하다. 모든 IP에 SSH를 열면 인터넷의 모든 위치에서 SSH 접속 시도를 할 수 있다.

Lightsail 방화벽은 public IP로 들어오는 inbound traffic을 제어한다. 서버 내부에서 `127.0.0.1:8300`으로 통신하는 것처럼 loopback 주소를 사용하는 트래픽은 Lightsail 방화벽의 대상이 아니다.

## SSH key pair

`SSH key pair`는 Linux/Unix 인스턴스에 SSH로 접속할 때 사용하는 공개키와 개인키 쌍이다.

```text
public key
  -> Lightsail 또는 인스턴스에 등록

private key
  -> 내 PC에 보관
  -> SSH 접속 시 사용
```

공개키는 서버에 등록해도 된다. 개인키는 외부에 노출되면 안 된다. 개인키를 가진 사용자는 해당 키를 신뢰하는 인스턴스에 접속할 수 있다.

Lightsail에서 key pair를 사용하는 방식은 다음과 같다.

- default SSH key 사용
- Lightsail 콘솔에서 custom key 생성
- 기존 public key 업로드

기존 public key를 업로드하면 내 PC에 이미 있는 private key로 새 인스턴스에 접속할 수 있다.

같은 public key를 여러 인스턴스에 등록하면 같은 private key로 여러 인스턴스에 접속할 수 있다. 이것은 가능하지만, 개인키가 노출되면 여러 인스턴스가 함께 위험해진다. 따라서 운영에서는 private key 보관, passphrase 사용, 접근자 관리가 중요하다.

## 브라우저 기반 SSH와 일반 SSH client

Lightsail 콘솔에는 브라우저에서 바로 SSH 접속하는 기능이 있다. 이 방식은 로컬 PC에 SSH client를 직접 설정하지 않아도 된다.

반면 로컬 터미널에서 접속하는 방식은 다음 형태를 사용한다.

```bash
ssh -i "<private-key-path>" <user>@<public-ip-or-domain>
```

예를 들어 Ubuntu 인스턴스라면 기본 사용자 이름이 `ubuntu`일 수 있다.

```bash
ssh -i "$HOME/.ssh/my_lightsail_key" ubuntu@203.0.113.10
```

여기서 `ubuntu`는 Linux 사용자 계정 이름이고, IP는 접속 대상 인스턴스 주소다. SSH key가 같아도 사용자 이름이 다르면 다른 Linux 계정으로 로그인하는 것이다.

## Linux 사용자 계정과 sudo

SSH 접속에 성공했다는 사실은 서버 관리자 권한을 가진다는 뜻과 같지 않다.

Linux 서버에는 여러 사용자 계정이 있을 수 있다.

- `ubuntu`
- `deploy`
- `ms`
- `root`

각 계정은 서로 다른 home directory, group, sudo 권한을 가질 수 있다.

`sudo`는 일반 사용자가 관리자 권한으로 명령을 실행할 때 사용하는 도구다. 어떤 사용자가 어떤 명령을 sudo로 실행할 수 있는지는 `/etc/sudoers`와 `/etc/sudoers.d/` 설정으로 결정된다.

따라서 같은 SSH key로 접속하더라도 다음은 서로 다른 의미다.

```bash
ssh -i key ubuntu@server
ssh -i key ms@server
```

첫 번째는 `ubuntu` Linux 계정으로 로그인한다. 두 번째는 `ms` Linux 계정으로 로그인한다. 두 계정의 sudo 권한이 다르면 실행 가능한 관리 작업도 달라진다.

## Lightsail DB 리소스

`Lightsail DB 리소스`는 Lightsail에서 생성하고 관리하는 MySQL 또는 PostgreSQL 서버다. 사용자가 직접 인스턴스에 PostgreSQL을 설치해서 관리하는 방식과 다르게, Lightsail이 DB 서버 운영의 일부를 관리한다.

Lightsail DB 리소스에는 다음 요소가 있다.

- DB engine
- DB endpoint
- port
- master user
- password
- plan
- storage
- backup과 snapshot
- metrics와 logs

`DB endpoint`는 애플리케이션이 DB 서버에 접속할 때 사용하는 host 이름이다.

```text
application
  -> DB endpoint:5432
  -> Lightsail DB 리소스
```

Lightsail DB 리소스는 서버 컴퓨터인 Lightsail 인스턴스와 별개의 리소스다. 인스턴스를 삭제해도 DB 리소스가 자동으로 삭제되는 구조로 이해하면 안 된다. 반대로 DB 리소스를 삭제하면 인스턴스는 남아 있어도 애플리케이션이 DB에 접속하지 못할 수 있다.

## DB 리소스와 논리 DB

DB 리소스와 논리 DB를 구분해야 한다.

`DB 리소스`는 Lightsail 콘솔의 데이터베이스 탭에서 보이는 관리형 DB 서버다.

`논리 DB`는 PostgreSQL 서버 안에 생성된 개별 database다.

관계는 다음과 같다.

```text
Lightsail DB 리소스
  -> PostgreSQL 서버
    -> postgres
    -> automationproject
    -> automationproject_staging
    -> const_production
```

이 구조에서 `automationproject`와 `const_production`은 Lightsail 리소스가 아니라 PostgreSQL 내부의 논리 DB다.

새 Lightsail DB 리소스를 만든다는 것은 관리형 PostgreSQL 서버를 하나 더 만드는 일이다. 새 논리 DB를 만든다는 것은 기존 PostgreSQL 서버 안에 database를 하나 더 만드는 일이다.

두 작업은 비용, 운영 범위, 격리 수준이 다르다.

| 작업 | 의미 | 비용 영향 | 격리 수준 |
| --- | --- | --- | --- |
| 새 Lightsail DB 리소스 생성 | 관리형 DB 서버를 하나 더 생성 | 새 DB 리소스 비용 발생 | 서버 수준 분리 |
| 기존 DB 리소스에 논리 DB 생성 | 같은 PostgreSQL 서버 안에 database 추가 | 기존 DB 리소스 비용 안에서 사용 | DB 이름 수준 분리 |

운영 환경에서는 이 차이를 명확히 해야 한다. `const_production` 논리 DB를 만들었다고 해서 새 Lightsail DB 리소스가 생긴 것은 아니다.

## DB user와 권한

PostgreSQL에는 DB user 또는 role이 있다. 애플리케이션은 특정 user와 password로 DB에 접속한다.

이 문서의 웹 애플리케이션 구성에서는 `.env`의 다음 값들이 DB 연결을 구성한다.

```text
POSTGRES_HOST=<DB endpoint>
POSTGRES_PORT=5432
POSTGRES_DB=<논리 DB 이름>
POSTGRES_USER=<DB user>
POSTGRES_PASSWORD=<DB user password>
```

`POSTGRES_HOST`는 DB 서버를 가리킨다. Lightsail 관리형 DB를 쓰면 DB resource endpoint가 들어간다.

`POSTGRES_DB`는 접속할 논리 DB를 가리킨다.

`POSTGRES_USER`는 접속 계정을 가리킨다.

따라서 같은 Lightsail DB 리소스를 쓰더라도 `POSTGRES_DB`만 바꾸면 다른 논리 DB에 접속할 수 있다. 단, 해당 user가 그 논리 DB에 접근할 권한을 가지고 있어야 한다.

## DB snapshot과 pg_dump의 차이

Lightsail DB snapshot과 `pg_dump`는 모두 백업 또는 복구에 사용될 수 있지만 계층이 다르다.

`Lightsail DB snapshot`은 Lightsail DB 리소스를 기준으로 하는 백업이다. snapshot에서 새 DB 리소스를 만들 수 있다.

`pg_dump`는 PostgreSQL 논리 DB 또는 schema 내용을 파일로 내보내는 PostgreSQL 도구다. 이 파일은 기존 PostgreSQL 서버 안의 다른 논리 DB로 복원할 수 있다.

관계는 다음과 같다.

```text
Lightsail DB snapshot
  -> DB 리소스 수준 백업
  -> 새 Lightsail DB 리소스 생성에 사용

pg_dump / pg_restore
  -> PostgreSQL 논리 DB 수준 백업과 복원
  -> 같은 DB 리소스 안의 다른 논리 DB로 복사 가능
```

기존 DB 리소스를 유지하면서 `automationproject` 내용을 `const_production`으로 복사하려면 `pg_dump`와 `pg_restore`가 적합하다.

새 Lightsail DB 리소스를 만들어 DB 서버 자체를 분리하려면 Lightsail DB snapshot 또는 별도 dump/restore 절차가 필요하다.

## 스냅샷

`스냅샷`은 특정 시점의 리소스 상태를 저장한 백업 이미지다. Lightsail에서는 인스턴스, DB, block storage disk의 snapshot을 만들 수 있다.

인스턴스 snapshot은 인스턴스의 system disk와 연결된 block storage disk 데이터를 포함한다. snapshot에서 새 인스턴스를 만들면 원본 인스턴스와 같은 파일 시스템 상태에서 시작한다.

하지만 snapshot에서 새 인스턴스를 만들었다고 모든 운영 설정이 현재 서비스에 바로 맞는 것은 아니다. 새 인스턴스에는 새 public IP가 생길 수 있고, domain, SSL 인증서, firewall, application 환경 변수, systemd service 활성화 상태를 확인해야 한다.

Lightsail 공식 문서 기준으로, snapshot에서 인스턴스를 만들 때 원본 인스턴스의 custom firewall rule은 새 인스턴스에 그대로 복사되지 않고 기본 rule만 복사된다. 그래서 snapshot 복원 후에는 방화벽을 반드시 확인해야 한다.

## 수동 스냅샷과 자동 스냅샷

`수동 스냅샷`은 운영자가 직접 생성하는 snapshot이다. 삭제하기 전까지 계정에 남는다.

`자동 스냅샷`은 Lightsail이 설정된 시간에 생성하는 snapshot이다. 운영 중인 리소스의 정기 백업에 사용한다.

snapshot은 저장 공간 비용이 발생할 수 있다. 그래서 더 이상 필요 없는 snapshot은 삭제 여부를 검토해야 한다. 단, 삭제 전에 롤백이나 감사 목적에 필요한 백업인지 확인해야 한다.

## DNS와 도메인

`domain`은 사람이 읽기 쉬운 주소다. 예를 들어 `example.com` 같은 이름이다.

`DNS`는 domain을 IP 주소나 다른 host 이름으로 연결하는 시스템이다.

웹 서비스 접속 흐름은 다음과 같다.

```text
사용자 브라우저
  -> domain 조회
  -> DNS가 IP 반환
  -> 해당 IP의 서버로 HTTPS 요청
```

Lightsail은 DNS zone을 생성하고 DNS record를 관리하는 기능을 제공한다. 도메인을 Lightsail DNS로 관리하면 Lightsail 콘솔에서 A record, CNAME record 같은 record를 설정할 수 있다.

`A record`는 domain을 IPv4 주소에 연결한다.

```text
app.example.com
  -> A record
  -> 43.200.56.130
```

도메인이 없고 `nip.io` 같은 IP 기반 host 이름을 사용한다면 별도 DNS zone을 만들지 않아도 된다. 예를 들어 `prod.43.200.56.130.nip.io`는 host 이름 안의 IP를 사용해 해당 IP로 해석되는 방식이다.

## HTTPS와 인증서

HTTPS는 HTTP 통신을 TLS로 암호화한 방식이다. 사용자가 웹 사이트에 로그인하거나 데이터를 입력하는 서비스에서는 HTTPS를 사용해야 한다.

Lightsail 자체가 모든 인스턴스 웹 서버의 HTTPS 설정을 자동으로 끝내주는 것은 아니다. 인스턴스 안에서 Nginx, Apache, Certbot 같은 도구를 사용해 인증서를 발급하고 웹 서버 설정을 적용할 수 있다.

Lightsail load balancer나 CDN distribution을 사용하는 경우에는 해당 리소스에서 certificate를 관리하는 구성이 가능하다. 단, 인스턴스에 직접 Nginx를 두고 운영한다면 인스턴스 안의 웹 서버 설정과 인증서 갱신 상태를 확인해야 한다.

## 로드 밸런서

`로드 밸런서`는 여러 인스턴스로 들어오는 트래픽을 나누는 리소스다.

한 대의 인스턴스만 운영할 때는 사용자가 직접 그 인스턴스의 public IP 또는 domain으로 접속한다.

여러 인스턴스로 서비스를 운영할 때는 사용자가 로드 밸런서로 접속하고, 로드 밸런서가 정상 상태인 인스턴스로 요청을 전달한다.

```text
사용자
  -> 로드 밸런서
    -> 인스턴스 A
    -> 인스턴스 B
```

로드 밸런서는 가용성을 높이는 데 사용할 수 있지만, 애플리케이션이 여러 인스턴스에서 동시에 실행되어도 안전한 구조인지 확인해야 한다. session, file upload, background worker, DB migration 같은 요소가 단일 인스턴스 기준으로 작성되어 있다면 추가 설계가 필요하다.

## CDN distribution

`CDN`은 Content Delivery Network의 약자다. 정적 파일을 사용자와 가까운 위치에서 전달해 응답 속도를 개선하는 데 사용한다.

Lightsail CDN distribution은 Amazon CloudFront 기반으로 제공된다. 이미지, CSS, JavaScript 같은 정적 파일 전달에 사용할 수 있다.

동적 API 응답이 많은 서비스에서는 CDN 적용 범위를 조심해야 한다. 로그인 사용자별로 달라지는 응답을 잘못 캐시하면 보안 문제가 생길 수 있다.

## Block storage와 object storage

`block storage disk`는 인스턴스에 추가로 연결하는 디스크다. Linux 서버에서는 마운트해서 파일 시스템처럼 사용할 수 있다. 데이터 저장 공간이 부족하거나 애플리케이션 파일과 데이터 파일을 분리하고 싶을 때 사용한다.

`object storage bucket`은 파일을 객체 단위로 저장하는 저장소다. 이미지, 첨부파일, 백업 파일처럼 애플리케이션 서버의 로컬 디스크에 묶어 두지 않아도 되는 파일을 저장할 때 사용한다.

두 저장소는 사용 방식이 다르다.

| 저장소 | 접근 방식 | 주요 용도 |
| --- | --- | --- |
| block storage disk | 인스턴스에 디스크처럼 연결 | 서버 내부 파일 시스템 확장 |
| object storage bucket | API 또는 HTTP 기반 객체 접근 | 이미지, 첨부파일, 백업 저장 |

## Lightsail과 EC2의 차이

Lightsail과 EC2는 모두 AWS에서 가상 서버를 만들 수 있는 서비스다. 하지만 목적이 다르다.

Lightsail은 웹 사이트나 작은 웹 애플리케이션을 빠르게 시작하기 위한 단순한 리소스 모델을 제공한다. 콘솔이 단순하고 요금 단위도 예측하기 쉽다.

EC2는 더 세밀한 서버, 네트워크, 보안, 스토리지 구성을 제공한다. 인스턴스 타입, VPC, subnet, security group, IAM role, Auto Scaling 같은 AWS 구성 요소를 직접 조합해야 한다.

정리하면 다음과 같다.

| 항목 | Lightsail | EC2 |
| --- | --- | --- |
| 목적 | 빠른 시작과 단순 운영 | 세밀한 인프라 설계 |
| 서버 크기 선택 | 정해진 plan 중심 | 다양한 instance type |
| 네트워크 | Lightsail 콘솔 중심 | VPC, subnet, security group 직접 설계 |
| 학습 난이도 | 낮음 | 높음 |
| 확장성 | 제한된 선택지 | 넓은 선택지 |

작은 서비스, 학습, 개인 프로젝트, 단일 서버 운영에는 Lightsail이 적합하다. 복잡한 네트워크, Auto Scaling, 세밀한 권한 제어, 다양한 AWS 서비스 연동이 요구사항이면 EC2 기반 구성을 검토한다.

## Lightsail 비용을 볼 때 구분할 항목

Lightsail은 예측 가능한 월 요금 단위를 제공하지만, 모든 비용이 인스턴스 plan 하나로 끝나는 것은 아니다.

비용을 볼 때는 다음 항목을 구분한다.

- 인스턴스 plan 비용
- DB 리소스 plan 비용
- snapshot storage 비용
- Static IP 비용 조건
- block storage disk 비용
- object storage 비용
- CDN distribution 비용
- 데이터 전송량
- 도메인 등록 비용

서버를 중지해도 연결된 disk, snapshot, DB 리소스 같은 항목은 비용이 계속 발생할 수 있다. 리소스를 삭제하기 전에는 백업과 롤백 필요 여부를 확인해야 한다.

## 웹 애플리케이션 배포 관점의 전체 흐름

Lightsail로 단일 웹 애플리케이션을 운영하면 흐름은 다음처럼 볼 수 있다.

```text
사용자 브라우저
  -> domain
  -> DNS
  -> Static IP
  -> Lightsail 인스턴스
  -> Lightsail 방화벽에서 443 허용
  -> Nginx
    -> frontend 정적 파일
    -> backend API reverse proxy
  -> backend process
  -> Lightsail DB 리소스
  -> PostgreSQL 논리 DB
```

이 흐름에서 장애가 생기면 어느 계층이 문제인지 나눠서 봐야 한다.

- domain이 잘못됐는가
- DNS record가 잘못됐는가
- Static IP가 올바른 인스턴스에 붙었는가
- Lightsail 방화벽에서 80 또는 443이 열려 있는가
- Nginx 설정이 올바른 host를 받고 있는가
- backend process가 실행 중인가
- `.env`의 DB 연결값이 올바른가
- DB user가 해당 논리 DB에 접근할 수 있는가

이 계층을 구분하면 `서버가 안 된다`라는 큰 문제를 더 작은 확인 항목으로 나눌 수 있다.

## AutomationProject 기준 예시

AutomationProject의 Lightsail 구성을 예로 들면 다음처럼 매핑할 수 있다.

| 실제 이름 | 계층 | 의미 |
| --- | --- | --- |
| `const-production` | Lightsail 인스턴스 | Production 애플리케이션을 실행하는 가상 서버 |
| `const-staging` | Lightsail 인스턴스 | Staging 애플리케이션을 실행하는 가상 서버 |
| `const-production-ip` | Static IP 리소스 | Production 인스턴스에 붙인 고정 public IP |
| `const-staging-ip` | Static IP 리소스 | Staging 인스턴스에 붙일 고정 public IP |
| Lightsail 데이터베이스 탭의 PostgreSQL | Lightsail DB 리소스 | PostgreSQL 서버 자체 |
| `automationproject` | 논리 DB | 기존 Production 데이터베이스 |
| `automationproject_staging` | 논리 DB | Staging 데이터베이스 |
| `const_production` | 논리 DB | 새 Production 인스턴스가 사용할 Production 데이터베이스 |
| `AutomationProject-1780659660` | 인스턴스 스냅샷 | 기존 인스턴스 파일 시스템을 복원하기 위한 백업 이미지 |

이 예시에서 중요한 점은 `const_production`이 Lightsail DB 리소스가 아니라 PostgreSQL 내부의 논리 DB라는 점이다.

즉 다음 두 문장은 서로 다르다.

```text
새 Lightsail DB 리소스를 만든다.
기존 Lightsail DB 리소스 안에 const_production 논리 DB를 만든다.
```

첫 번째는 관리형 DB 서버를 새로 만드는 작업이다. 두 번째는 기존 PostgreSQL 서버 안에 database를 하나 더 만드는 작업이다.

## 자주 헷갈리는 표현

| 표현 | 정확한 의미 |
| --- | --- |
| Lightsail | AWS의 간단한 클라우드 리소스 관리 서비스 |
| Lightsail 인스턴스 | AWS Cloud에서 실행되는 가상 서버 |
| 서버 | 문맥에 따라 물리 서버 또는 서버 역할을 하는 실행 환경 |
| Lightsail 리소스 | Lightsail에서 생성하고 관리하는 대상 전체 |
| DB 리소스 | Lightsail이 관리하는 DB 서버 |
| 논리 DB | PostgreSQL 서버 안의 개별 database |
| Static IP | 인스턴스에 연결하는 고정 public IPv4 |
| 스냅샷 | 특정 시점의 리소스 백업 이미지 |
| SSH key pair | SSH 접속 신원 확인에 사용하는 공개키와 개인키 쌍 |
| 방화벽 규칙 | public IP로 들어오는 트래픽 허용 조건 |

## 운영할 때의 기본 점검 순서

Lightsail에서 웹 서비스가 열리지 않으면 다음 순서로 확인한다.

1. 인스턴스가 `Running` 상태인지 확인한다.
2. Static IP가 올바른 인스턴스에 연결되어 있는지 확인한다.
3. DNS record가 올바른 Static IP를 가리키는지 확인한다.
4. Lightsail 방화벽에서 필요한 port가 열려 있는지 확인한다.
5. SSH로 인스턴스에 접속되는지 확인한다.
6. Nginx 설정 검사를 실행한다.
7. Nginx service 상태를 확인한다.
8. backend systemd service 상태를 확인한다.
9. backend log를 확인한다.
10. `.env`의 DB 연결값을 확인한다.
11. DB endpoint와 논리 DB 접속을 확인한다.
12. HTTPS 인증서와 redirect 설정을 확인한다.

이 순서는 네트워크 바깥쪽에서 애플리케이션 내부로 들어오는 순서다. 이렇게 확인하면 문제 위치를 좁히기 쉽다.

## Lightsail을 학습할 때의 핵심 관점

Lightsail을 처음 학습할 때는 기능 목록을 외우는 것보다 계층을 구분하는 것이 중요하다.

먼저 `인스턴스`와 `DB 리소스`를 구분한다. 인스턴스는 애플리케이션이 실행되는 가상 서버이고, DB 리소스는 데이터베이스 서버다.

다음으로 `DB 리소스`와 `논리 DB`를 구분한다. DB 리소스는 Lightsail 콘솔에서 보이는 관리형 서버이고, 논리 DB는 PostgreSQL 내부의 database다.

그 다음 `public IP`, `Static IP`, `domain`, `DNS`를 구분한다. public IP는 인터넷 주소이고, Static IP는 고정 public IP이며, domain은 사람이 읽는 이름이고, DNS는 domain을 IP로 연결하는 시스템이다.

마지막으로 `snapshot`과 `dump`를 구분한다. snapshot은 Lightsail 리소스 수준의 백업이고, `pg_dump`는 PostgreSQL 논리 DB 수준의 백업이다.

이 네 가지 구분이 잡히면 Lightsail 콘솔에서 보이는 대부분의 용어를 운영 문서와 연결해서 이해할 수 있다.

## 관련 문서

- [Cloud.md](./Cloud.md): 클라우드 컴퓨팅, 가상 서버, managed database의 기본 개념을 설명한다.
- [NetworkBasics.md](./NetworkBasics.md): IP, port, DNS 같은 네트워크 기초를 설명한다.
- [WebDeployment.md](./WebDeployment.md): 웹 애플리케이션 배포에서 Nginx, backend, DB가 어떻게 연결되는지 설명한다.
- [Monitoring.md](./Monitoring.md): 운영 중인 서버와 애플리케이션 상태를 관찰하는 기본 개념을 설명한다.

## 참고 문서

- Amazon Lightsail 개요: https://docs.aws.amazon.com/lightsail/latest/userguide/what-is-amazon-lightsail.html
- Lightsail 인스턴스: https://docs.aws.amazon.com/lightsail/latest/userguide/understanding-instances-virtual-private-servers-in-amazon-lightsail.html
- Lightsail 관리형 데이터베이스: https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-databases.html
- Lightsail IP 주소: https://docs.aws.amazon.com/lightsail/latest/userguide/understanding-public-ip-and-private-ip-addresses-in-amazon-lightsail.html
- Lightsail 방화벽: https://docs.aws.amazon.com/lightsail/latest/userguide/understanding-firewall-and-port-mappings-in-amazon-lightsail.html
- Lightsail SSH key pair: https://docs.aws.amazon.com/lightsail/latest/userguide/understanding-ssh-in-amazon-lightsail.html
- Lightsail 스냅샷: https://docs.aws.amazon.com/en_us/lightsail/latest/userguide/understanding-snapshots-in-amazon-lightsail.html
- snapshot에서 인스턴스 생성: https://docs.aws.amazon.com/lightsail/latest/userguide/lightsail-how-to-create-instance-from-snapshot.html
- Lightsail DNS: https://docs.aws.amazon.com/lightsail/latest/userguide/understanding-dns-in-amazon-lightsail.html