# FastAPI Backend

## 한 줄 요약

FastAPI backend는 HTTP request를 받아 라우팅, 입력 검증, 의존성 주입, 비즈니스 로직 실행, 응답 직렬화를 수행하는 Python ASGI 애플리케이션이다. 실제 네트워크 포트를 열고 요청을 받는 역할은 Uvicorn 같은 ASGI 서버가 담당한다.

## 백엔드 서버란 무엇인가

백엔드 서버는 클라이언트로부터 HTTP request를 받아 처리하고, 결과를 HTTP response로 반환하는 서버 프로그램이다. 웹 서비스에서 화면 렌더링을 프론트엔드가 담당하는 구조라면, 백엔드는 데이터 접근과 비즈니스 로직을 담당하는 쪽을 의미한다. 비즈니스 로직은 서비스 규칙에 따라 데이터를 검증하고 계산하고 변경하는 코드를 뜻한다.

이 문서에서 FastAPI backend는 다음 전체를 포함한다.

- 서버 런타임 레이어
  - Uvicorn 같은 ASGI 서버
- 애플리케이션 레이어
  - FastAPI 앱 객체, 라우트, dependency, middleware
- 데이터/운영 구성
  - DB session, 환경변수, logging, migration, deployment 설정

## 요청 처리 흐름

```text
Client
  -> HTTP request
  -> Uvicorn
  -> FastAPI app
  -> Starlette routing/middleware
  -> dependency와 input validation
  -> endpoint function
  -> response serialization
  -> Uvicorn
  -> HTTP response
```

요청 1건이 처리되는 흐름은 다음과 같다.

1. Uvicorn이 포트를 열고 HTTP 연결을 수신한다.
2. Uvicorn이 request를 ASGI 규약 형태로 만들고 FastAPI 앱을 호출한다.
3. FastAPI/Starlette가 middleware와 router를 통과시킨다.
4. FastAPI가 path/query/body/cookie/header 값을 파싱하고 검증한다.
5. dependency를 실행해 endpoint에 필요한 값을 주입한다.
6. endpoint function이 실행된다.
7. 반환값이 Response로 직렬화된다.
8. Uvicorn이 response를 네트워크로 전송한다.

## ASGI, Uvicorn, Starlette, FastAPI

ASGI는 Python 웹 서버와 웹 애플리케이션이 통신하는 표준 인터페이스다.

- Uvicorn
  - 네트워크 포트를 열고 HTTP/WebSocket 연결을 받아 ASGI 앱을 호출하는 서버 런타임
- Starlette
  - routing, middleware, Request/Response 같은 ASGI 웹 코어
- FastAPI
  - Starlette 위에 타입 힌트 기반 validation, dependency injection, OpenAPI 생성을 얹은 API 프레임워크

FastAPI 자체가 포트를 여는 서버 프로세스는 아니다. FastAPI로 만든 `app` 객체를 Uvicorn이 실행해야 외부 request를 받을 수 있다.

## FastAPI 앱 객체

`app = FastAPI()`는 ASGI 애플리케이션 객체를 만든다. 이 객체는 서버가 요청마다 호출하는 대상이며, 라우트와 middleware, dependency 같은 애플리케이션 규칙을 담는 중앙 컨테이너 역할을 한다.

```python
from fastapi import FastAPI

app = FastAPI()
```

라우트나 middleware는 이 앱 객체에 등록된다.

```python
@app.get('/api/items')
def api_get_items():
    return [{'id': 1, 'name': 'example'}]
```

## Route와 endpoint

라우트는 어떤 HTTP method와 path를 어떤 endpoint function으로 처리할지 정하는 규칙이다.

```python
@app.get('/api/items/{item_id}')
def api_get_item(item_id: int):
    return {'id': item_id}
```

이 라우트의 의미는 다음과 같다.

- method
  - `GET`
- path
  - `/api/items/{item_id}`
- path parameter
  - `item_id`
- endpoint function
  - `api_get_item`

실무에서는 라우트와 endpoint를 함께 묶어 부르는 경우가 많다. 하지만 정확히는 라우트가 매핑 규칙이고, endpoint function이 실제 실행되는 함수다.

## Query parameter와 body

Query parameter는 URL의 `?` 뒤에 붙는 값이다.

```python
@app.get('/api/items')
def api_get_items(page: int = 1, size: int = 20):
    return {'page': page, 'size': size}
```

요청:

```text
GET /api/items?page=2&size=50
```

Body는 HTTP request 본문이다. 클라이언트가 JSON 구조의 생성/수정 데이터를 보내는 endpoint라면 JSON body를 Pydantic 모델로 받을 수 있다.

```python
from pydantic import BaseModel

class ItemPayload(BaseModel):
    name: str
    price: float

@app.post('/api/items')
def api_create_item(payload: ItemPayload):
    return {'created': True, 'item': payload}
```

Query parameter는 조회 옵션, 필터, 페이지네이션에 자주 쓰이고 body는 생성/수정할 데이터 본문에 자주 쓰인다.

## FastAPI Request 객체

FastAPI의 `Request` 객체는 현재 처리 중인 HTTP request를 Python 코드에서 읽기 위한 객체다.

```python
from fastapi import Request

@app.get('/api/debug')
def api_debug(request: Request):
    return {'path': request.url.path, 'method': request.method}
```

`Request` 객체로는 method, URL, headers, cookies, query params, path params, client 정보, `request.state` 등을 읽을 수 있다.

Pydantic request schema와는 역할이 다르다.

- `Request` 객체
  - HTTP 요청 전체에 대한 실행 시점 정보
- Pydantic request schema
  - 클라이언트가 body로 보내는 JSON 구조

## Pydantic 모델

Pydantic 모델은 API 요청과 응답의 데이터 모양을 정의한다.

```python
class ItemPayload(BaseModel):
    name: str
    price: float

class ItemOut(BaseModel):
    id: int
    name: str
    price: float
```

- `*Payload`
  - 클라이언트가 보내는 request body
- `*Out`
  - 서버가 돌려주는 response body
- `*Response`
  - 응답 전체 구조를 명시하고 싶을 때

SQLAlchemy 모델은 DB 테이블 구조를 표현하고, Pydantic 모델은 API 계약을 표현한다. 둘을 같은 것으로 보면 안 된다.

## response_model

`response_model`은 endpoint의 응답 계약을 고정한다.

```python
@app.get('/api/items', response_model=list[ItemOut])
def api_get_items():
    ...
```

FastAPI는 반환값을 `ItemOut` 구조에 맞춰 직렬화하고, OpenAPI에도 같은 구조를 기록한다.

## OpenAPI와 generated client

OpenAPI는 HTTP API 계약을 기계가 읽을 수 있는 형식으로 표현하는 표준이다. FastAPI는 route, parameter, Pydantic model 정보를 바탕으로 OpenAPI 스펙을 자동 생성한다.

```text
FastAPI route + Pydantic model
  -> OpenAPI schema
  -> generated TypeScript client
  -> frontend 타입과 API 함수
```

generated client를 쓰면 frontend가 backend API 경로와 request/response 타입을 손으로 맞추는 부담이 줄어든다.

주의할 점은 OpenAPI가 backend 코드에서 자동 생성되더라도, generated client 파일은 별도 생성 절차를 다시 실행해야 최신 상태가 된다는 점이다.

## Dependency

FastAPI dependency는 endpoint 실행 전에 필요한 값이나 검증 로직을 선언적으로 주입하는 기능이다.

```python
from fastapi import Depends


def get_current_user():
    ...

@app.get('/api/me')
def api_get_me(user = Depends(get_current_user)):
    return user
```

Dependency는 다음 용도에 자주 쓰인다.

- DB session 주입
- 인증 사용자 조회
- 권한 검사
- 공통 query/header 처리
- resource cleanup

## request-scoped DB session

FastAPI dependency로 DB를 주입하는 구조에서는 DB session을 HTTP request 1회 처리 동안만 유지하고, 요청이 끝나면 닫는 방식을 사용할 수 있다.

```python
def get_db() -> Iterator[Session]:
    with create_db_session(DATABASE_URL) as db:
        yield db
```

endpoint에서는 DB 사용 사실이 함수 signature에 드러나도록 dependency로 받는다.

```python
@app.get('/api/items')
def api_get_items(db: Session = Depends(get_db)):
    ...
```

같은 HTTP request 안에서 같은 dependency callable을 여러 번 요구하면 FastAPI는 기본적으로 한 번만 실행하고 결과를 재사용한다. `Depends(..., use_cache=True)`가 기본값이다.

```text
request start
  get_db() 실행 -> Session A 생성
  auth dependency(db=Session A) 실행
  endpoint(db=Session A) 실행
request end
  get_db cleanup -> Session A close
```

`create_db_session(...)`을 직접 호출하면 호출할 때마다 새 SQLAlchemy `Session` 객체를 만든다. 반면 FastAPI endpoint와 인증/권한 검사 dependency가 모두 `Depends(get_db)`를 사용하면, 같은 HTTP request 안에서는 FastAPI dependency cache 때문에 같은 `Session` 객체를 공유한다.

`Depends(get_db, use_cache=False)`를 쓰거나 endpoint 내부에서 별도 DB session을 직접 열면 session 공유가 깨질 수 있다.

## request.state

`request.state`는 현재 HTTP request를 처리하는 동안만 필요한 값을 저장하는 공간이다.

```python
def require_login(request: Request, db: Session = Depends(get_db)):
    user = get_current_user(db, request)
    request.state.current_user = user
```

같은 request 안의 endpoint나 helper에서 다시 꺼내 쓸 수 있다.

```python
def get_current_user_from_request(request: Request):
    return request.state.current_user
```

`request.state`는 전역 저장소가 아니다.

- 현재 HTTP request 처리 중에만 의미가 있다.
- 다른 사용자의 request와 공유되지 않는다.
- 같은 사용자의 다음 request와도 자동 공유되지 않는다.
- background task, thread, queue job으로 ORM 객체를 그대로 넘기는 용도로 쓰지 않는다.

## Transaction boundary

DB session lifecycle과 transaction boundary는 같은 개념이 아니다.

- session lifecycle
  - DB 연결과 ORM 객체 상태를 관리하는 범위
- transaction boundary
  - 쓰기 작업을 commit 또는 rollback으로 확정하는 범위

여러 DB 쓰기 작업이 함께 성공하거나 함께 실패해야 한다면 명시적인 transaction helper로 감싼다.

```python
with db_transaction(db):
    create_item_without_commit(db, payload)
```

DB row 생성/수정/삭제 또는 `db.flush()`를 수행하지만 commit은 호출하지 않는 helper라면, 호출자가 transaction boundary를 헷갈리지 않도록 이름에서 그 사실을 드러낸다.

## 자주 하는 실수

- FastAPI가 직접 포트를 여는 서버라고 생각하는 경우
- route와 endpoint function을 구분하지 않는 경우
- query parameter와 request body를 같은 입력 방식으로 생각하는 경우
- ORM 모델을 API response schema로 그대로 노출하는 경우
- request-scoped DB session과 별도 session을 한 request 안에서 섞는 경우
- `request.state`에 넣은 ORM 객체를 background task로 넘기는 경우

## 관련 문서

- [HTTPAndBrowser.md](./HTTPAndBrowser.md)
- [DatabaseAndORM.md](./DatabaseAndORM.md)
- [ReactFrontend.md](./ReactFrontend.md)
