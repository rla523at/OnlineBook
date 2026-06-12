# Database And ORM

## 한 줄 요약

PostgreSQL은 데이터를 저장하는 DBMS이고, SQLAlchemy ORM은 Python 객체와 DB table/row 사이를 연결하는 계층이며, Alembic은 DB schema 변경 이력을 migration 파일로 관리하는 도구다.

## 이 문서의 목표

이 문서는 Python과 FastAPI를 조금 아는 독자가 웹 백엔드에서 database, ORM, migration이 어디에 위치하는지 이해하는 것을 목표로 한다.

읽은 뒤에는 다음을 설명할 수 있어야 한다.

- PostgreSQL이 어떤 역할을 하는지
- `DATABASE_URL`과 DB connection이 왜 필요한지
- SQLAlchemy ORM과 SQLAlchemy Session이 각각 무엇을 맡는지
- Alembic migration이 request 처리 흐름이 아니라 schema 변경 흐름에서 쓰인다는 점

ORM 관계 매핑의 세부 내용은 [SQLAlchemyRelationships.md](./SQLAlchemyRelationships.md)에서 다룬다.

## 전체 그림

FastAPI backend가 DB를 사용하는 요청을 처리할 때 핵심 흐름은 다음과 같다.

```text
HTTP request
  -> FastAPI endpoint
  -> dependency에서 SQLAlchemy Session 준비
  -> SQLAlchemy ORM이 Python 객체 작업을 SQL query로 연결
  -> DB connection을 통해 PostgreSQL에 query 전송
  -> PostgreSQL table row read/write
  -> HTTP response
```

Alembic migration은 위 request 처리 흐름 안에서 매번 실행되는 도구가 아니다. Alembic은 table 추가, column 변경 같은 DB schema 변경이 필요할 때 별도 명령으로 실행한다.

```text
SQLAlchemy model 변경
  -> Alembic migration 파일 생성
  -> migration 적용
  -> PostgreSQL schema 변경
```

## 먼저 알아야 할 용어

- DBMS
  - Database Management System의 약자다. 데이터를 저장하고 query를 처리하며 권한과 동시 접근을 관리하는 데이터베이스 관리 프로그램이다.
- database
  - DBMS 안에서 애플리케이션 데이터를 담는 논리적인 저장 공간이다.
- schema
  - DB에서 table, column, 제약 조건 같은 구조 정보를 뜻한다. API 입출력 구조를 뜻하는 Pydantic schema와는 역할이 다르다.
- table과 row
  - table은 같은 구조의 데이터를 모아 두는 저장 단위이고, row는 table 안의 데이터 한 건이다.
- query
  - DB에 데이터를 읽거나 쓰라고 요청하는 명령이다. 관계형 DB에서는 SQL query를 사용한다.
- ORM
  - Object Relational Mapper의 약자다. Python 객체와 관계형 DB table/row 사이를 연결해 주는 계층이다.
- migration
  - DB schema 변경을 파일로 기록하고 순서대로 적용하는 절차다.

## PostgreSQL은 무엇인가

PostgreSQL은 관계형 데이터베이스 DBMS다. 사용자, 게시글, 주문, 파일 메타데이터처럼 HTTP request가 끝난 뒤에도 남아야 하는 데이터를 저장한다.

DBMS는 DB를 관리하는 프로그램 범주이고, PostgreSQL은 그 범주에 속하는 구체적인 제품이다.

애플리케이션은 PostgreSQL data file을 직접 수정하지 않는다. FastAPI backend는 DB connection을 통해 PostgreSQL에 query를 보내고, PostgreSQL은 table row를 읽거나 변경한 결과를 돌려준다.

## DB connection과 DATABASE_URL

DB connection은 애플리케이션과 DBMS 사이에 열린 통신 경로다. FastAPI 코드가 DB를 사용하려면 SQLAlchemy나 DB driver가 PostgreSQL에 접속할 수 있어야 한다.

`DATABASE_URL`은 DB 접속 정보를 한 문자열로 표현한 연결 문자열이다. 연결 문자열에는 DB 종류, driver, 사용자, 비밀번호, host, port, database 이름이 들어간다.

```text
postgresql+psycopg2://<user>:<password>@<host>:<port>/<db>
```

이 값을 코드에 직접 적으면 dev, test, staging, production 환경을 분리하기 어렵고 비밀번호가 코드에 남을 수 있다. 그래서 운영 서버나 여러 개발 환경에서 같은 코드를 다른 DB에 연결해야 한다면 환경변수나 `.env` 파일에서 읽는다.

```text
POSTGRES_USER=myapp
POSTGRES_PASSWORD=<password>
POSTGRES_HOST=127.0.0.1
POSTGRES_PORT=5432
POSTGRES_DB=myapp
```

이 값들은 최종적으로 `DATABASE_URL`을 만들거나, DB driver 설정에 전달되는 접속 정보가 된다.

### DB에 접속한다는 말의 의미

`DB에 접속한다`는 PostgreSQL 서버와 통신할 수 있는 DB connection을 여는 것이다. 접속 자체는 값을 읽는 일이 아니다. 값을 읽으려면 connection을 연 뒤 SQL query를 실행해야 한다.

| 개념 | 의미 |
| --- | --- |
| DB connection | PostgreSQL 서버와 통신할 수 있는 연결 |
| query | 연결된 DB에 `SELECT`, `INSERT` 같은 SQL 명령을 보내는 것 |
| client | DB에 접속해서 query를 보내는 프로그램 또는 라이브러리 |

흐름은 다음과 같다.

```text
DB connection 생성
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
| 접속 시작 DB | `-d <database>` | `PGDATABASE` | 서버 안의 어떤 database에 먼저 접속할지 |
| 비밀번호 | `-W`로 입력 요청 | `PGPASSWORD` | DB user 비밀번호 |
| SSL 방식 | 연결 문자열 또는 별도 옵션 | `PGSSLMODE` | SSL 연결 방식을 정함 |

`PGPASSWORD`는 비밀번호를 환경변수에 미리 넣는 방식이고, `-W`는 `psql` 실행 중 비밀번호를 직접 입력하게 하는 방식이다. 비밀번호 원문은 문서와 repository에 저장하지 않는다.

### psql, psycopg2, SQLAlchemy의 관계

같은 PostgreSQL에 접속하더라도 실행 주체에 따라 사용하는 client가 다르다.

```text
사람이 직접 확인:
psql
  -> DB connection
  -> SQL query 입력
  -> row 반환

FastAPI 코드 실행:
FastAPI endpoint
  -> SQLAlchemy Session
  -> SQLAlchemy Engine
  -> psycopg2
  -> DB connection
  -> SQL query 실행
  -> row 반환
```

`psycopg2`는 Python 코드가 PostgreSQL에 접속할 때 사용하는 DB driver다. SQLAlchemy는 `DATABASE_URL`에 적힌 driver 이름을 보고 `psycopg2`를 통해 DB connection을 만든다.

```text
postgresql+psycopg2://<user>:<password>@<host>:<port>/<database>
```

이 프로젝트에서는 `backend/config/.env`의 `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_HOST`, `POSTGRES_PORT`, `POSTGRES_DB`를 합쳐 `DATABASE_URL`을 만든다. 이 값들은 SQL query 자체가 아니라 DB connection을 만들기 위한 접속 정보다.

## SQLAlchemy ORM은 무엇인가

SQLAlchemy ORM은 Python 코드에서 DB table과 row를 객체처럼 다루게 해 주는 계층이다.

ORM을 쓰지 않으면 애플리케이션 코드는 SQL query 문자열을 직접 만들고 실행해야 한다. ORM을 쓰면 Python class와 객체를 통해 table 구조와 row 데이터를 다룰 수 있다.

```text
Python 객체 작업
  -> SQLAlchemy ORM
  -> SQL query
  -> PostgreSQL table row
```

ORM은 DB를 없애는 도구가 아니다. 실제 데이터 저장, query 실행, 제약 조건 검사는 PostgreSQL이 맡는다. SQLAlchemy ORM은 Python 코드와 PostgreSQL 사이에서 매핑과 작업 추적을 담당한다.

## SQLAlchemy 모델과 Pydantic 모델

SQLAlchemy 모델은 DB table 구조를 Python class로 표현한다. 모델 class는 어떤 table에 어떤 column이 있는지 ORM에게 알려준다.

```python
class Article(DbBaseModel):
    __tablename__ = 'articles'

    id = Column(Integer, primary_key=True)
    title = Column(String, nullable=False)
```

Pydantic 모델은 API request/response의 데이터 모양을 표현한다.

SQLAlchemy 모델과 Pydantic 모델은 둘 다 Python class로 작성될 수 있지만 역할이 다르다.

| 구분 | 역할 |
|------|------|
| SQLAlchemy 모델 | DB table 구조와 row 매핑 |
| Pydantic 모델 | API request/response 구조와 validation |

DB 내부 column과 외부 API 계약은 서로 다른 이유로 바뀔 수 있다. 그래서 API 입출력 구조를 SQLAlchemy 모델에 그대로 의존시키지 않고 Pydantic 모델로 따로 정의한다.

## SQLAlchemy Session은 무엇인가

SQLAlchemy Session은 DB 작업을 모아 실행하고 ORM 객체 상태를 추적하는 작업 단위다. 여기서 session은 로그인 세션이 아니라 DB 작업 범위를 뜻한다.

Session은 다음 일을 담당한다.

- 새 객체를 DB에 저장할 대상으로 등록한다.
- 수정된 ORM 객체를 추적한다.
- 필요한 시점에 SQL query를 실행한다.
- transaction 안에서 commit 또는 rollback이 일어나도록 돕는다.

FastAPI dependency로 DB를 주입하는 구조에서는 HTTP request 1회 동안 Session 하나를 만들고, request가 끝나면 닫는 방식을 사용할 수 있다.

```text
request start
  -> Session 생성
  -> endpoint와 helper에서 DB 작업 수행
  -> commit 또는 rollback
  -> Session close
request end
```

request-scoped DB session의 자세한 lifecycle은 [FastAPIBackend.md](./FastAPIBackend.md)의 request-scoped DB session 설명을 참고한다.

## Alembic migration은 무엇인가

Alembic은 DB schema 변경 이력을 migration 파일로 관리하는 도구다.

예를 들어 SQLAlchemy 모델에 새 table을 추가하거나 column을 변경했다면, PostgreSQL schema도 같은 구조로 바뀌어야 한다. 이 변경을 개발자 PC, test DB, staging DB, production DB에 같은 순서로 적용하려면 변경 이력이 파일로 남아 있어야 한다.

```text
모델 변경
  -> migration revision 생성
  -> migration 파일 검토
  -> DB에 migration 적용
```

revision은 특정 시점의 schema 변경 내용을 담는 migration 파일 한 개를 뜻한다.

```powershell
python -m scripts.db_manager --make-migration -m "add article table"
python -m scripts.db_manager --migrate
```

Alembic migration을 관리하는 이유는 다음과 같다.

- DB schema 변경 이력을 코드로 남길 수 있다.
- 여러 환경의 DB schema를 같은 순서로 맞출 수 있다.
- 배포 시 어떤 schema 변경이 적용됐는지 추적할 수 있다.

autogenerate는 SQLAlchemy 모델과 현재 DB schema를 비교해 migration 초안을 만드는 기능이다. rename, constraint 변경, nullable 변경, data migration은 자동 생성 결과가 의도와 다를 수 있으므로 migration 파일을 적용하기 전에 검토한다.

## 웹 요청 처리 흐름에서 다시 보기

DB를 읽는 API는 다음처럼 이해할 수 있다.

```text
GET /api/articles
  -> FastAPI endpoint 실행
  -> dependency가 SQLAlchemy Session 제공
  -> Session이 SQLAlchemy ORM을 통해 query 실행
  -> DB connection을 통해 PostgreSQL에서 row 조회
  -> Pydantic response model에 맞춰 응답 반환
```

DB를 변경하는 API는 transaction 경계가 추가된다.

```text
POST /api/articles
  -> request body validation
  -> SQLAlchemy 모델 객체 생성
  -> Session에 저장 작업 등록
  -> commit으로 transaction 확정
  -> response 반환
```

schema를 변경하는 작업은 request 처리 흐름과 분리해서 생각한다.

```text
새 column이 필요함
  -> SQLAlchemy 모델 수정
  -> Alembic migration 생성과 검토
  -> 배포 과정에서 migration 적용
  -> 이후 request가 새 schema를 사용
```

## 관련 문서

- [FastAPIBackend.md](./FastAPIBackend.md)
- [WebDeployment.md](./WebDeployment.md)
- [SQLAlchemyRelationships.md](./SQLAlchemyRelationships.md)
