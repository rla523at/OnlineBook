# DB Connection

## 한 줄 요약

DB connection은 애플리케이션과 DBMS 사이에 열린 통신 경로이고, DB 접속 정보는 그 연결을 열기 위해 필요한 값들의 묶음이며, DB 연결 문자열은 그 값을 문자열 하나로 표현한 형식이다.

## 이 문서의 목표

이 문서는 Web 개발 초심자가 DB에 접속한다는 말의 의미와 `DATABASE_URL`, DSN, DB endpoint, DB user, 접속 시작 database의 차이를 이해하는 것을 목표로 한다.

읽은 뒤에는 다음을 설명할 수 있어야 한다.

- DB connection이 SQL query 실행 전에 필요한 이유
- DB 접속 정보와 DB 연결 문자열의 차이
- `DATABASE_URL`과 DSN이 DB 연결 문자열과 어떤 관계인지
- DB endpoint, DB user, 접속 시작 database가 각각 무엇을 가리키는지
- `psql`, DB driver, SQLAlchemy가 DB connection을 만드는 흐름에서 맡는 역할

## 전체 그림

애플리케이션이 PostgreSQL에 query를 보내려면 먼저 DB connection을 열어야 한다.

```text
애플리케이션
  -> DB 접속 정보 준비
  -> DB driver가 DB connection 생성
  -> SQL query 실행
  -> PostgreSQL이 row 반환 또는 변경
```

DB 접속 정보는 여러 값의 묶음이다.

```text
DBMS 종류: PostgreSQL
host: DB 서버 주소
port: 5432
database: 접속 시작 database
user: DB user
password: DB user password
SSL 방식: sslmode
```

이 값을 환경변수 여러 개로 나눠 둘 수도 있고, 연결 문자열 하나로 합쳐 둘 수도 있다.

```text
POSTGRES_HOST=<host>
POSTGRES_PORT=5432
POSTGRES_DB=<database>
POSTGRES_USER=<user>
POSTGRES_PASSWORD=<password>

postgresql://<user>:<password>@<host>:5432/<database>?sslmode=require
```

## 먼저 알아야 할 용어

- DB connection
  - 애플리케이션 또는 DB client와 DBMS 사이에 열린 통신 경로다.
- DB 접속 정보
  - DB connection을 열기 위해 필요한 host, port, database, user, password, SSL 방식 같은 값들의 묶음이다.
- DB 연결 문자열
  - DB 접속 정보를 문자열 하나로 표현한 형식이다.
- `DATABASE_URL`
  - DB 연결 문자열을 담는 환경변수 이름이다.
- DSN
  - Data Source Name의 약자다. DB client 또는 driver 문맥에서 DB 접속 정보를 가리키는 이름이나 입력 형식으로 쓰인다.
- DB endpoint
  - 애플리케이션이 DB 서버에 접속할 때 사용하는 host 이름이다.
- DB user
  - DBMS에 로그인할 때 사용하는 계정 또는 role이다.
- 접속 시작 database
  - DB connection을 처음 열 때 지정하는 PostgreSQL 내부 database 이름이다.

## DB connection

DB connection은 애플리케이션과 DBMS 사이에 열린 통신 경로다. SQL query는 DB connection을 통해 DBMS에 전달된다.

```text
DB connection 생성
  -> SQL query 실행
  -> row 반환 또는 변경
```

`DB에 접속한다`는 DB connection을 여는 것이다. 접속 자체는 table row를 읽거나 쓰는 일이 아니다. table row를 읽거나 쓰려면 connection을 연 뒤 `SELECT`, `INSERT`, `UPDATE`, `DELETE` 같은 SQL query를 실행해야 한다.

| 개념 | 의미 |
| --- | --- |
| DB connection | DBMS와 통신할 수 있는 연결 |
| SQL query | 연결된 DBMS에 보내는 읽기 또는 쓰기 명령 |
| DB client | DBMS에 접속해서 query를 보내는 프로그램 또는 라이브러리 |

## DB 접속 정보

DB 접속 정보는 DB connection을 열기 위해 필요한 값들의 묶음이다.

| 접속 정보 | 의미 | 예 |
| --- | --- | --- |
| DBMS 종류 | 어떤 DBMS에 접속하는지 | `postgresql` |
| host | DB 서버 주소 | `database.example.ap-northeast-2.rds.amazonaws.com` |
| port | DB 서버 port | `5432` |
| database | 접속 시작 database | `automationproject` |
| user | DB user | `automationproject_monitoring` |
| password | DB user password | `<password>` |
| SSL 방식 | 암호화 연결 방식 | `sslmode=require` |

이 값들은 SQL query가 아니다. 이 값들은 SQL query를 보낼 수 있는 DB connection을 만들기 위한 입력이다.

## DB 연결 문자열

DB 연결 문자열은 DB 접속 정보를 문자열 하나로 표현한 형식이다.

```text
postgresql://<user>:<password>@<host>:<port>/<database>?sslmode=require
```

각 부분의 의미는 다음과 같다.

| 부분 | 의미 |
| --- | --- |
| `postgresql://` | PostgreSQL에 접속한다는 scheme |
| `<user>` | DB user |
| `<password>` | DB user password |
| `<host>` | DB 서버 주소 |
| `<port>` | DB 서버 port |
| `<database>` | 접속 시작 database |
| `sslmode=require` | SSL 연결 방식 |

DB 연결 문자열은 비밀번호를 포함할 수 있다. 그래서 실제 운영 password가 들어간 연결 문자열은 문서와 repository에 저장하지 않는다.

## DATABASE_URL

`DATABASE_URL`은 DB 연결 문자열을 담는 환경변수 이름이다.

```text
DATABASE_URL=postgresql+psycopg2://<user>:<password>@<host>:5432/<database>
```

`DATABASE_URL` 자체가 DB connection은 아니다. `DATABASE_URL`은 DB driver나 SQLAlchemy가 DB connection을 만들 때 읽는 설정값이다.

관계는 다음과 같다.

```text
DATABASE_URL 환경변수
  -> DB 연결 문자열
  -> DB driver가 해석
  -> DB connection 생성
```

SQLAlchemy를 사용할 때는 연결 문자열에 driver 이름이 포함될 수 있다.

```text
postgresql+psycopg2://<user>:<password>@<host>:5432/<database>
```

여기서 `psycopg2`는 Python 코드가 PostgreSQL에 접속할 때 사용하는 DB driver다.

## DSN

DSN은 Data Source Name의 약자다. Data source는 애플리케이션이 데이터를 읽거나 쓰는 대상이고, PostgreSQL을 쓰는 문맥에서는 접속 대상 DB를 가리킨다.

DSN은 문맥에 따라 두 방식으로 쓰인다.

| 문맥 | DSN의 의미 |
| --- | --- |
| ODBC 같은 설정 기반 client | 미리 등록된 데이터 원본 설정의 이름 |
| PostgreSQL driver 또는 Python library | DB 접속 정보를 담은 문자열 또는 conninfo |

이 문서의 Web backend 문맥에서는 DSN을 `DB 접속 정보를 driver에 넘기기 위한 값`으로 이해하면 된다.

예를 들어 다음 값은 monitoring user로 PostgreSQL에 접속하기 위한 DSN이다.

```text
postgresql://automationproject_monitoring:<password>@<host>:5432/automationproject?sslmode=require
```

따라서 이 문맥에서는 DSN과 DB 연결 문자열이 같은 역할을 한다. 다만 두 용어의 초점은 다르다.

| 용어 | 초점 |
| --- | --- |
| DB 연결 문자열 | DB 접속 정보를 문자열 하나로 표현한 형식 |
| DSN | DB client 또는 driver가 DB 접속 정보를 부르는 이름이나 입력 형식 |

## DB endpoint

DB endpoint는 애플리케이션이 DB 서버에 접속할 때 사용하는 host 이름이다.

```text
application
  -> DB endpoint:5432
  -> PostgreSQL 서버
```

DB endpoint는 DB 접속 정보 전체가 아니다. DB endpoint는 DB 접속 정보 중 host에 해당한다.

```text
POSTGRES_HOST=<DB endpoint>
POSTGRES_PORT=5432
POSTGRES_DB=<접속 시작 database>
POSTGRES_USER=<DB user>
POSTGRES_PASSWORD=<DB user password>
```

Lightsail 관리형 DB를 쓰면 `POSTGRES_HOST`에는 Lightsail DB resource endpoint가 들어간다.

## 접속 시작 database

PostgreSQL 서버 안에는 여러 database가 있을 수 있다.

```text
PostgreSQL 서버
  -> postgres
  -> automationproject
  -> automationproject_staging
```

접속 시작 database는 DB connection을 처음 열 때 지정하는 database다.

```text
psql -h '<host>' -p 5432 -U '<user>' -d 'automationproject' -W
```

여기서 `-d automationproject`가 접속 시작 database를 지정한다. 같은 PostgreSQL 서버라도 접속 시작 database가 다르면 처음 연결되는 논리 DB가 달라진다. 단, DB user가 그 database에 접속할 권한을 가지고 있어야 한다.

## DB user와 권한

DB user는 DBMS에 로그인할 때 사용하는 계정 또는 role이다. 같은 host와 database를 사용하더라도 user가 다르면 접근 가능한 table, schema, function이 달라질 수 있다.

```text
DB user
  -> DBMS 로그인
  -> 권한 확인
  -> 허용된 database/schema/table에 query 실행
```

예를 들어 monitoring 용도라면 table row data를 읽는 user가 아니라 size metadata만 조회할 수 있는 read-only monitoring user를 둘 수 있다.

## psql, DB driver, SQLAlchemy

같은 PostgreSQL 서버에 접속하더라도 실행 주체에 따라 사용하는 client가 다르다.

```text
사람이 직접 확인:
psql
  -> DB connection
  -> SQL query 입력
  -> row 반환

Python 코드 실행:
SQLAlchemy Engine
  -> DB driver
  -> DB connection
  -> SQL query 실행
  -> row 반환
```

`psql`은 사람이 터미널에서 쓰는 PostgreSQL client다. 다음 명령은 DB connection을 여는 구체적인 방법이다.

```powershell
psql -h '<host>' -p 5432 -U '<user>' -d '<database>' -W
```

| 접속 정보 | `psql` 옵션 | PostgreSQL 환경변수 | 의미 |
| --- | --- | --- | --- |
| DB 서버 주소 | `-h <host>` | `PGHOST` | 어느 PostgreSQL 서버로 갈지 |
| DB 서버 port | `-p 5432` | `PGPORT` | 그 서버의 어느 port로 갈지 |
| DB user | `-U <user>` | `PGUSER` | 어떤 DB 계정으로 로그인할지 |
| 접속 시작 database | `-d <database>` | `PGDATABASE` | 서버 안의 어떤 database에 먼저 접속할지 |
| 비밀번호 | `-W`로 입력 요청 | `PGPASSWORD` | DB user password |
| SSL 방식 | 연결 문자열 또는 별도 옵션 | `PGSSLMODE` | SSL 연결 방식을 정함 |

`PGPASSWORD`는 비밀번호를 환경변수에 미리 넣는 방식이고, `-W`는 `psql` 실행 중 비밀번호를 직접 입력하게 하는 방식이다. 비밀번호 원문은 문서와 repository에 저장하지 않는다.

SQLAlchemy는 Python 코드에서 DB 작업을 관리하는 라이브러리다. SQLAlchemy 자체가 PostgreSQL wire protocol을 직접 처리하는 것이 아니라, 연결 문자열에 지정된 DB driver를 통해 DB connection을 만든다.

```text
FastAPI endpoint
  -> SQLAlchemy Session
  -> SQLAlchemy Engine
  -> DB driver
  -> DB connection
  -> SQL query 실행
```

## 자주 헷갈리는 표현

| 표현 | 정확한 의미 |
| --- | --- |
| DB connection | 애플리케이션과 DBMS 사이에 열린 통신 경로 |
| DB 접속 정보 | DB connection을 열기 위해 필요한 값들의 묶음 |
| DB 연결 문자열 | DB 접속 정보를 문자열 하나로 표현한 형식 |
| `DATABASE_URL` | DB 연결 문자열을 담는 환경변수 이름 |
| DSN | DB client 또는 driver가 DB 접속 정보를 부르는 이름이나 입력 형식 |
| DB endpoint | DB 서버에 접속할 때 사용하는 host 이름 |
| DB user | DBMS에 로그인하는 계정 또는 role |
| 접속 시작 database | connection을 처음 열 때 지정하는 PostgreSQL 내부 database |
| SQL query | 열린 DB connection을 통해 DBMS에 보내는 명령 |

## 관련 문서

- [DatabaseAndORM.md](./DatabaseAndORM.md): DB connection 위에서 SQLAlchemy ORM, Session, Alembic migration이 어떤 역할을 하는지 설명한다.
- [AmazonLightsail.md](./AmazonLightsail.md): Lightsail 관리형 DB 리소스, DB endpoint, 논리 DB의 차이를 설명한다.
- [FastAPIBackend.md](./FastAPIBackend.md): FastAPI request 처리에서 DB session을 사용하는 흐름을 설명한다.

## 참고 문서

- PostgreSQL libpq connection strings: https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING
- Psycopg conninfo: https://www.psycopg.org/psycopg3/docs/api/conninfo.html
