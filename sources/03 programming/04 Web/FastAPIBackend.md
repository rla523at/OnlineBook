# FastAPI Backend

## 개요

### 백엔드 서버란?

백엔드 서버는 클라이언트(웹 브라우저, 모바일 앱, 다른 서버 등)로부터 HTTP 요청을 받아 처리하고, 결과를 HTTP 응답으로 반환하는 서버 프로그램이다. 웹 서비스에서 보통 “데이터와 비즈니스 로직이 있는 쪽”을 의미하며, UI를 담당하는 프론트엔드와 대비된다.

“FastAPI 백엔드 서버”는 보통 다음 전체를 가리킨다.

* 서버 런타임 레이어

  * Uvicorn 같은 ASGI 서버(포트 열기, 요청 수신/전달/응답 반환)
* 애플리케이션 레이어

  * FastAPI로 작성된 API 로직(라우트/검증/응답 등)
* 부가 구성

  * DB 연결, 캐시, 메시지 큐, 로깅/모니터링, 환경 설정, 배포/운영 설정 등

### 요청-응답 처리 흐름(End-to-End)

1. Uvicorn 프로세스 시작

   * 지정한 호스트/포트에서 대기

2. Uvicorn이 앱 객체 로드

   * 예: `uvicorn backend.main:app`

     * `backend.main` 모듈을 import하고 그 안의 `app` 변수를 가져온다.
     * 이 시점에 `app = FastAPI()`가 실행되어 객체가 생성되고 메모리에 올라간다.

3. 요청 수신 및 앱 호출

   * 클라이언트 요청이 들어오면 Uvicorn이 이를 FastAPI 앱 객체에 전달한다.
   * FastAPI는 라우트를 선택하고 라우트 함수를 실행해 응답을 구성한다.

4. 응답 반환

   * FastAPI가 만든 응답을 Uvicorn이 HTTP 응답으로 전송한다.


## 핵심 개념

### HTTP 요청/응답

HTTP 응답은 클라이언트로 돌아가는 “HTTP 메시지”이며 보통 다음으로 구성된다.

* 상태 코드

  * 200, 404, 500 등
* 헤더

  * `Content-Type`, `Set-Cookie` 등
  * 본문이 어떤 형식인지, 캐시 정책은 무엇인지, 쿠키를 설정할지 같은 메타데이터가 들어간다.
* 본문(body)

  * 실제 payload(JSON/HTML/파일 등)

즉, JSON은 “HTTP 응답의 본문에 들어갈 수 있는 형식”이다.

예시

* JSON 응답: `Content-Type: application/json` + body에 JSON
* HTML 응답: `Content-Type: text/html` + body에 HTML

FastAPI에서 라우트가 dict/Pydantic 모델을 반환하면 FastAPI가 JSON 직렬화와 응답 구성을 하고, Uvicorn이 그 응답을 네트워크를 통해 클라이언트로 전송한다.


### ASGI란?

ASGI(Asynchronous Server Gateway Interface)는 “Python 웹 서버(서버 런타임)와 웹 애플리케이션(프레임워크 코드)”이 서로 통신하는 표준 인터페이스(규약)이다. WSGI의 비동기/동시성 확장 버전이라고 보면 된다. HTTP뿐 아니라(구현에 따라) WebSocket 같은 장기 연결도 다루기 쉽도록 설계되어 있다.

#### ASGI 서버(예: Uvicorn)

ASGI 서버는 네트워크 쪽을 맡는 실행 주체다.

* 포트를 열고(소켓 바인딩) HTTP/WebSocket 연결을 받아들인다
* 들어온 요청/이벤트를 ASGI 규약 형태로 만들어
* ASGI 애플리케이션을 호출하고
* 애플리케이션이 보낸 응답/이벤트를 네트워크로 전송한다

요약하면 “네트워크 I/O + 프로토콜 처리 + 애플리케이션 호출”이 ASGI 서버의 역할이다.

#### ASGI 애플리케이션(예: FastAPI 앱 객체)

ASGI 애플리케이션은 요청을 처리하는 로직 덩어리(콜러블)이다.

* 서버가 전달한 요청 정보를 받아
* 라우팅/검증/비즈니스 로직을 수행하고
* 응답을 구성해 서버로 돌려준다

FastAPI에서 `app = FastAPI()`로 만든 객체가 대표적인 ASGI 애플리케이션이다(정확히는 ASGI 호출 규약을 만족하는 callable 객체).


### Uvicorn이란?

Uvicorn은 Python에서 사용되는 ASGI(Asynchronous Server Gateway Interface) 서버 구현체다. 운영체제 관점에서는 Uvicorn이 실행될 때 기본적으로 하나의 프로세스가 생성되거나 workers 설정에 따라 여러 프로세스가 생성되며, 그 프로세스가 네트워크 포트를 열고 외부의 HTTP 요청을 수신한다.

즉, Uvicorn은 FastAPI로 작성된 ASGI 애플리케이션을 네트워크에서 서비스 가능하게 만드는 서버 런타임이다.

#### 역할

* 포트 바인딩 및 요청 수신

  * 지정한 호스트/포트에서 소켓을 열고 HTTP 연결을 받아들인다.
* 프로토콜 처리 및 변환

  * 들어온 HTTP 요청을 해석하고, ASGI 규약에 맞는 형태로 ASGI 애플리케이션에 전달할 준비를 한다.
* 애플리케이션 호출 및 응답 전달

  * ASGI 애플리케이션(FastAPI 앱 객체)을 호출하여 요청 처리를 맡기고, 애플리케이션이 send로 보낸 ASGI 응답 메시지를 실제 HTTP 바이트 스트림으로 직렬화해 클라이언트로 전송한다.
* 동시성 처리(이벤트 루프 기반)

  * 비동기 요청 처리를 위한 이벤트 루프를 운용하고 여러 연결을 효율적으로 처리한다.


### FastAPI란?

FastAPI는 Python으로 웹 API(백엔드)를 만들기 위한 웹 프레임워크다. 라우팅, 요청/응답 처리, 입력 검증(Pydantic), 의존성 주입(Dependencies), 미들웨어, 예외 처리, OpenAPI 문서 자동 생성 같은 기능을 제공한다.

중요한 점은 FastAPI 자체가 네트워크 포트를 열어 HTTP 요청을 직접 받는 “서버 프로세스”가 아니라는 것이다. FastAPI는 “요청을 어떻게 처리할지”를 정의하는 애플리케이션 코드를 작성하기 위한 도구(프레임워크)이고, 실제로 포트를 열고 외부 요청을 수신하는 역할은 ASGI 서버(Uvicorn 등)가 담당한다.


### Starlette란?

Starlette는 Python의 ASGI(Asynchronous Server Gateway Interface) 기반 경량 웹 프레임워크(또는 웹 툴킷)다. 웹 애플리케이션을 만들 때 필요한 핵심 뼈대(라우팅, 미들웨어, Request/Response, WebSocket 등)를 제공한다.

FastAPI는 이 Starlette 코어 위에 ‘API 개발 편의 기능’을 얹은 프레임워크다. 그래서 FastAPI를 쓰더라도 내부적으로는 Starlette의 라우팅/미들웨어 체인과 Request/Response 모델이 그대로 사용되고, 실무에서도 `starlette.middleware.*`, `starlette.responses.*` 같은 컴포넌트를 함께 쓰는 경우가 흔하다.

Starlette가 주로 담당하는 범주는 다음과 같다.

* 라우팅(경로/메서드 매칭)
* 미들웨어 체인(요청 전/응답 후 공통 처리)
* Request/Response 추상화
* WebSocket 지원
* 정적 파일 서빙, 템플릿, 백그라운드 태스크 같은 웹 기본 부품


### FastAPI / Starlette / Uvicorn 역할 분담

아래는 요청이 들어와서 응답이 나가기까지의 계층 구조를 단순화해 그린 그림이다.

```
[Client] (browser / app / curl)
   |
   |  HTTP / WebSocket
   v
[Uvicorn] (ASGI Server)
   |
   |  ASGI scope + receive/send으로 앱 호출
   v
[FastAPI App] (ASGI application)
   |
   |  내부적으로 Starlette 코어를 사용
   v
[Starlette Core] (routing / middleware / request/response / websocket)
   |
   v
[Your endpoint code] (path operation function)
   |
   v
[Response]
   |
   v
[Uvicorn] -> 네트워크 전송 -> [Client]
```

요청 1건이 처리되는 흐름을 단계로 보면 다음과 같다.

1. Uvicorn이 포트를 열고 HTTP/WebSocket 연결을 수신한다.
2. Uvicorn이 들어온 요청을 ASGI 규약 형태(scope, receive, send)로 만들고 `app(scope, receive, send)`를 호출한다.
3. FastAPI 앱은 Starlette 기반 미들웨어 체인을 통과시키고, Starlette 라우터로 엔드포인트를 선택한다.
4. FastAPI가 선택된 엔드포인트를 실행하기 전에 입력 파싱/검증(Pydantic), 의존성 주입(Depends) 같은 API 친화 기능을 수행한다.
5. 사용자 엔드포인트 코드가 실행되어 결과를 반환한다.
6. FastAPI/Starlette가 반환값을 Response(JSONResponse 등)로 구성한다.
7. 미들웨어가 응답 후처리를 수행한 뒤, 최종 응답이 Uvicorn을 통해 네트워크로 전송된다.

역할을 한 줄로 요약하면 다음과 같다.

* Uvicorn: 네트워크/프로토콜 처리 + ASGI 앱 호출(서버 런타임)
* Starlette: 라우팅/미들웨어/Request-Response/WebSocket 같은 웹 코어
* FastAPI: 타입 힌트 기반 입력 검증/직렬화, 의존성 주입, OpenAPI 문서화 같은 API 개발 편의


## API 문서화

### OpenAPI란?

OpenAPI(정확히는 OpenAPI Specification, OAS)는 **HTTP API를 “기계가 읽을 수 있는 형식”으로 정의하는 표준 규격**이다.

OpenAPI 문서(보통 `openapi.json` 또는 `openapi.yaml`)에는 이런 정보가 들어간다.

* 어떤 URL 경로(path)가 있는지: `/auth/login`, `/uploads` …
* 각 경로에서 어떤 HTTP 메서드를 받는지: GET/POST/PUT/DELETE …
* 요청 파라미터/바디 스키마: 어떤 필드가 필수인지, 타입이 뭔지
* 응답 스키마: 200이면 어떤 JSON 구조가 오는지, 에러면 어떤 구조인지
* 인증 방식: Bearer 토큰, API 키, OAuth2 등
* operationId 같은 “엔드포인트 식별자”(클라이언트 생성 시 함수명 기반이 되기도 함)

즉, OpenAPI는 **“API의 계약서(Contract)”** 역할을 한다.
사람이 보기 위한 문서이기도 하지만, 더 중요한 목적은 **도구가 이걸 읽고 자동 생성/검증을 하게 하는 것**이다.

### Swagger란?

Swagger는 원래 **REST API 문서화/테스팅 도구 및 명세 포맷의 이름**으로 시작했다.
그런데 역사적으로 “Swagger 명세”가 발전해서 **OpenAPI로 표준화**되었다.

그래서 요즘은 보통 이렇게 구분해서 쓰는 게 정확하다.

* **OpenAPI**: 표준 “명세 규격” 자체 (스펙)
* **Swagger**: OpenAPI를 다루는 “도구/브랜드/생태계” (예: Swagger UI, Swagger Editor)

현장에서 “Swagger 쓴다”라고 하면 대개

* OpenAPI 스펙을 만들거나(자동 생성 포함)
* 그걸 Swagger UI로 보여주거나
* Swagger Editor로 편집하거나
  같은 흐름을 통칭하는 경우가 많다.

## FastAPI 애플리케이션 구성 요소

### FastAPI 앱 객체(`app = FastAPI()`)란?

`app = FastAPI()`는 FastAPI 프레임워크가 제공하는 “ASGI 애플리케이션 객체(ASGI callable)”를 생성하는 코드다. 이 객체는 단순한 설정값이 아니라, 서버(Uvicorn)가 요청마다 호출하는 대상이며, 동시에 애플리케이션의 규칙과 로직을 담는 중앙 컨테이너 역할을 한다.

#### 앱 객체로 백엔드 애플리케이션이 “구성되는” 흐름

FastAPI에서 백엔드 애플리케이션은 “코드를 다 써놓고 마지막에 객체로 변환”되는 느낌이라기보다, 먼저 앱 객체(ASGI 애플리케이션 객체)를 만들고 그 객체에 기능을 계속 ‘등록’하면서 완성되는 구조다.

1. `app = FastAPI()`로 앱 객체를 만든다

   * 이 한 줄이 “서버(Uvicorn)가 호출할 ASGI 애플리케이션 엔트리포인트”를 만든다.
   * 아직 이 시점에는 라우트나 미들웨어가 거의 없어서, “처리 규칙이 빈 컨테이너”에 가까운 상태다.

2. 앱 객체 위에 라우트/미들웨어/의존성/이벤트를 등록한다

   * 라우트: 어떤 요청을 어떤 함수로 처리할지(메서드+경로 → 함수)
   * 미들웨어: 요청/응답을 가로채서 공통 처리(로깅, CORS, 인증 전처리 등)
   * 의존성: 라우트 실행 전에 필요한 공통 자원/로직(DB 세션, 인증 정보 등)
   * 이벤트: startup/shutdown 같은 수명주기 훅에서 초기화/정리 로직 실행

3. 최종적으로 `app`이 “이 프로젝트의 ASGI 애플리케이션”이 된다

   * 등록이 끝난 `app` 객체가 곧 “내 API 서버의 동작 규칙 전체”이며, Uvicorn은 요청이 올 때마다 이 `app`을 호출한다.
   * 그래서 `uvicorn backend.main:app`에서 뒤의 `app`이 바로 그 엔트리포인트 객체다.

#### 앱 객체가 요청을 처리하는 흐름

클라이언트 요청이 들어오면, Uvicorn이 `app`(FastAPI 앱 객체)을 ASGI 규약에 따라 호출하고, `app`은 아래 흐름으로 처리한다.

* 라우트 선택: 요청의 path와 method를 기준으로 처리할 엔드포인트를 고른다.
* 입력 처리: 필요한 값을 추출하고 타입 변환/검증을 수행한다.
* 의존성 주입: 공통 로직(인증, DB 세션 등)을 준비해 엔드포인트 함수에 주입한다.
* 핸들러 실행: 선택된 엔드포인트 함수를 실행해 비즈니스 로직을 수행한다.
* 응답 생성:

  * 엔드포인트 함수 반환 값(dict/Pydantic 등)을 FastAPI/Starlette가 JSON으로 직렬화해 `Response`를 만든다.
  * `Response`는 `http.response.start`, `http.response.body` 같은 ASGI 메시지를 `send` 콜백을 통해 서버(Uvicorn)로 보낸다.
* 문서화(OpenAPI): 라우트/모델 정보를 기반으로 API 문서를 자동 생성한다(기능적으로는 항상 유지되는 메타데이터).

### 라우트(route)란?

라우트(route)는 어떤 HTTP 요청을 어떤 코드로 처리할지 정해두는 규칙이다. FastAPI에서는 보통 경로(path)와 HTTP 메서드(method)의 조합이 특정 엔드포인트 함수(핸들러)로 연결되는 것을 의미한다.

예를 들어 `GET /users/1` 요청이 들어오면 FastAPI는 등록된 라우트들 중 `GET /users/{user_id}` 같은 규칙과 매칭되는 항목을 찾고, 그 라우트에 연결된 함수를 실행한다.

실무에서 “라우트”라는 표현은 엄밀히는 라우팅 규칙(메서드+경로 매핑)과 그 규칙에 매핑된 엔드포인트 함수를 함께 묶어 부르는 경우가 많다. 그래서 “라우트가 dict를 반환한다”는 말은 정확히는 “그 라우트에 연결된 엔드포인트 함수가 dict를 반환한다”는 뜻으로 이해하면 된다.

참고로 `{user_id}`처럼 URL 경로 일부를 변수로 받는 값은 path parameter이고, `?page=1`처럼 URL 뒤에 붙는 값은 query parameter다.

#### 라우트를 구성하는 요소

라우트는 대체로 다음 요소들로 이루어진다.

* HTTP 메서드

  * GET, POST, PUT, PATCH, DELETE 등
* 경로(path)

  * `/users`, `/users/{user_id}` 같은 URL 패턴
* 경로 파라미터(path parameter)

  * `{user_id}`처럼 경로 일부를 변수로 받는 값
* 쿼리 파라미터(query parameter)

  * `?page=1&size=20`처럼 URL 뒤에 붙는 값
* 요청 본문(body)

  * POST/PUT/PATCH에서 JSON 등의 본문 데이터
* 헤더(header)/쿠키(cookie)

  * 인증 토큰, 세션 쿠키 같은 부가 정보
* 엔드포인트 함수(endpoint handler)

  * 실제 비즈니스 로직을 수행하는 함수
* (선택) 의존성 주입(dependencies)

  * 인증/인가, DB 세션 주입 같은 공통 로직
* (선택) 응답 설정

  * 응답 모델(response_model), 상태 코드(status_code), 헤더, 태그(tags) 등


### 라우트 등록 방식

#### 데코레이터로 라우트 등록 (`@app.get`, `@app.post` 등)

`@app.get("/path")` 같은 데코레이터를 함수 위에 붙이면 FastAPI가 그 함수와 “메서드+경로”를 묶어서 라우트로 등록한다.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class UserOut(BaseModel):
    id: int
    name: str

@app.get("/users/{user_id}", response_model=UserOut)
def read_user(user_id: int):
    return {"id": user_id, "name": "minseok"}
```

이 라우트가 의미하는 바는 다음과 같다.

* 메서드: GET
* 경로: `/users/{user_id}`
* path parameter: `user_id` (요청에서 추출 후 `int`로 변환/검증)
* 엔드포인트 함수: `read_user`
* 응답: dict 반환 → JSON 직렬화 → `UserOut` 스키마에 맞춰 응답 구성

#### APIRouter로 등록 후 앱에 포함하여 라우트 등록 (`include_router`)

프로젝트가 커지면 파일을 나누기 위해 `APIRouter`를 사용한다. 이 경우 라우트 묶음을 router에 등록하고, 마지막에 app에 포함시키는 방식으로 구성한다.

```python
from fastapi import FastAPI, APIRouter

router = APIRouter(prefix="/users")

@router.get("/{user_id}")
def read_user(user_id: int):
    return {"id": user_id, "name": "minseok"}

app = FastAPI()
app.include_router(router)
```

* `router`에 라우트를 등록한다.
* `app.include_router(router)`로 라우트 묶음을 앱에 붙인다.
* 결과적으로 `GET /users/{user_id}` 라우트가 앱에 등록된다.


### 라우트 매칭과 요청 처리 흐름

라우트 기준으로 요청 처리는 보통 다음 순서로 진행된다.

1. 클라이언트가 HTTP 요청을 보낸다. (예: `GET /users/1`)
2. 서버(Uvicorn)가 요청을 받고 FastAPI 앱 객체(`app`)를 호출한다.
3. FastAPI/Starlette 라우터가 등록된 라우트들 중에서

   * method가 일치하는지 확인하고
   * path가 패턴과 매칭되는지 확인한다. (예: `/users/{user_id}`)
4. 매칭되면 path parameter를 추출한다. (예: `"1"`)
5. 타입 힌트에 따라 변환/검증한다. (예: `user_id: int`)
6. 의존성이 있으면 의존성 주입을 수행한다.
7. 엔드포인트 함수를 실행한다.
8. 반환값을 기반으로 응답을 구성한다. (JSON 직렬화, 상태 코드, 헤더)
9. Uvicorn이 응답을 네트워크로 전송한다.


### 요청 파라미터

#### Query parameter 예시 (`?page=1&size=20`)

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items")
def list_items(page: int = 1, size: int = 20):
    return {"page": page, "size": size}
```

* `GET /items?page=2&size=50` 요청이면
* `page=2`, `size=50`으로 바인딩되고 검증된다.

#### Body(Pydantic 모델) 예시 (POST JSON)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class ItemIn(BaseModel):
    name: str
    price: float

@app.post("/items")
def create_item(item: ItemIn):
    return {"created": True, "item": item}
```

* `POST /items`로 JSON body를 보내면
* FastAPI가 `ItemIn`으로 파싱/검증한 뒤 `create_item`에 주입한다.


#### Query parameter와 Body의 차이

둘 다 “라우트(엔드포인트) 함수의 입력”으로 들어오는 건 맞다. 하지만 같은 것은 아니다. 둘은 HTTP에서 위치도 다르고, 용도/제약도 다르고, FastAPI에서 처리 방식도 다르다.

FastAPI에서는 요청에서 값을 꺼내서(바인딩해서) 라우트 함수 인자에 넣어준다.

* Query parameter → 함수 인자 `page`, `size` 같은 “스칼라/단순값”으로 들어오는 경우가 흔함
* Body → 함수 인자 `item: ItemIn` 같은 “요청 본문(JSON)을 파싱한 객체”로 들어오는 경우가 흔함

하지만 이 둘의 가장 큰 차이는 값이 오는 위치다.

* Query parameter: URL의 `?` 뒤에 붙는 값

  * 예: `GET /items?page=2&size=50`
* Body: HTTP 메시지의 본문(payload)

  * 예: `POST /items` + body에 JSON `{ "name": "...", "price": ... }`

즉, 둘은 HTTP 프로토콜 레벨에서 완전히 다른 부분이다.

일반적으로 이 둘은 다음과 같이 서로 다른 상황에서 사용한다.

* Query parameter: 조회/필터/정렬/페이지네이션처럼 “요청을 설명하는 옵션”에 적합

  * 예: `page`, `size`, `sort`, `keyword`, `from`, `to`
* Body: 생성/수정처럼 “리소스의 내용(데이터)”을 전달할 때 적합

  * 예: 상품 생성 시 `{name, price, description, ...}`

이 둘은 FastAPI에서도 바인딩 규칙에 중요한 차이가 있다. FastAPI는 함수 시그니처를 보고 “이 인자는 어디서 꺼낼지”를 결정한다.

* 단순 타입(int/str/bool/float 등) + 기본값 존재 → 기본적으로 Query로 간주되는 경우가 많음
* Pydantic 모델 타입 → 기본적으로 Body로 간주됨

예를 들어:

```python
@app.post("/items")
def create_item(page: int = 1, item: ItemIn = ...):
    ...
```

* `page`는 query에서,
* `item`은 body에서 가져오게 된다.

### 미들웨어/세션

#### SessionMiddleware란?

`SessionMiddleware`는 Starlette가 제공하는 미들웨어 중 하나로, 요청/응답 사이에 세션 데이터를 유지할 수 있게 해준다. 라우트(엔드포인트)에서는 `request.session`을 dict처럼 다뤄 세션 값을 읽고 쓸 수 있다.

동작 방식의 핵심은 다음과 같다.

* 요청 시

  * 세션 쿠키(기본 이름 `session`)가 있으면 서명 검증 후 내용을 디코드해 `request.session`에 올린다.
  * 세션 쿠키가 없으면 빈 세션으로 시작한다.
* 응답 시

  * 세션 내용이 변경되면, 변경된 세션을 다시 쿠키로 내려주기 위해 응답에 `Set-Cookie`를 붙인다.

주의점도 같이 이해하는 편이 좋다.

* 기본 구현은 ‘서버 저장소 기반 세션’이 아니라 ‘클라이언트 쿠키 기반(서명된) 세션’에 가깝다.

  * 즉, 세션 내용이 서버 DB에 저장되는 구조가 아니라, 세션 데이터가 쿠키에 실려 왕복하는 형태다.
* 서명(signing)으로 ‘변조’를 막는 것이 핵심이고, 암호화(encryption)는 별개다.

  * 민감한 정보(비밀번호, 토큰 원문, 개인식별정보 등)는 세션에 넣지 않는 것이 원칙이다.
* 쿠키 용량 제한이 있어 큰 데이터를 넣으면 문제가 된다.
* `secret_key`가 필수다.

  * 서명에 사용하는 키가 바뀌면 기존 세션 쿠키는 전부 무효가 된다.
* 보안 옵션을 환경에 맞게 설정하는 것이 일반적이다.

  * `https_only`, `same_site`, `max_age`, `path`, `domain` 등

FastAPI 프로젝트에서의 전형적인 사용 형태는 다음과 같다.

```python
from starlette.middleware.sessions import SessionMiddleware

app.add_middleware(
    SessionMiddleware,
    secret_key="<your-secret>",
    https_only=True,
    same_site="lax",
)
```

세션에 ‘큰 데이터’나 ‘서버에서 강제로 폐기(revoke)해야 하는 상태’가 들어가야 한다면, 보통은 Redis/DB 같은 서버 저장소에 세션을 두고 쿠키에는 세션 id만 두는 구조로 설계하는 편이 운영 상 안전하다.
