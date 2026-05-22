# HTTP And Browser

## 한 줄 요약

웹 애플리케이션은 클라이언트가 HTTP request를 보내고 서버가 HTTP response를 돌려주는 구조 위에서 동작한다.
브라우저는 HTML/CSS/JavaScript를 실행할 뿐 아니라, 쿠키 저장, CORS 검사, API request 전송 같은 웹 보안과 네트워크 동작도 함께 담당한다.

## HTTP request와 response

HTTP request는 HTTP client가 HTTP server에 보내는 요청 메시지다.

예:

```text
GET /api/items
POST /api/auth/login
POST /api/items/import
```

HTTP request에는 보통 다음 정보가 포함된다.

- method
  - `GET`, `POST`, `PUT`, `DELETE` 같은 요청 방식
- path
  - `/api/items`, `/api/auth/login` 같은 요청 경로
- query parameter
  - `/api/items?page=1`의 `page=1` 같은 URL 파라미터
- header
  - content type, 인증 정보, 쿠키 등 요청 메타데이터
- body
  - JSON, form data, 파일 등 클라이언트가 서버에 전달하는 본문

모든 request가 body를 가지는 것은 아니다. 일반적인 `GET` 요청은 query parameter와 header만으로 필요한 정보를 전달하는 경우가 많다.

HTTP response는 서버가 클라이언트에 돌려주는 응답 메시지다.

- status code
  - `200`, `404`, `500` 같은 처리 결과
- header
  - `Content-Type`, `Set-Cookie` 같은 응답 메타데이터
- body
  - JSON, HTML, 파일 등 실제 응답 본문

JSON은 HTTP 자체가 아니라 HTTP body에 담을 수 있는 데이터 형식이다.

## 브라우저, 프론트엔드 앱, API client

`브라우저`, `프론트엔드 앱`, `API client`는 같은 뜻이 아니다.

- 브라우저
  - Chrome, Edge, Safari처럼 웹 페이지를 열고 JavaScript를 실행하는 프로그램
- 프론트엔드 앱
  - 브라우저 안에서 실행되는 React/Vite 같은 애플리케이션 코드
- API client
  - 서버 API를 호출하는 클라이언트 프로그램을 넓게 부르는 말

프론트엔드 앱이 브라우저 안에서 `fetch`나 generated API client 함수를 실행하면, 실제 HTTP request는 브라우저가 서버로 전송한다.

Postman, curl, Python script, 모바일 앱처럼 브라우저가 아닌 프로그램도 API client가 될 수 있다.

## request 단위와 API 호출 1회

`request 단위`는 HTTP request 1개가 서버 앱에 들어와 response가 만들어질 때까지의 처리 범위를 의미한다.

CORS preflight 같은 추가 HTTP request가 없으면 API endpoint 호출 1회는 HTTP request 1회와 대응한다.

```text
POST /api/auth/login HTTP request 1개
  -> backend endpoint 실행
  -> HTTP response 반환
  -> request 처리 종료
```

하지만 `API 호출 1회`는 코드나 사람이 부르는 작업 단위이고, `HTTP request 1회`는 서버가 실제로 받는 프로토콜 요청 단위다. 브라우저나 API client 동작 때문에 하나의 API 호출처럼 보이는 작업에서 HTTP request가 2개 이상 발생할 수 있다.

대표적인 예는 CORS preflight다.

```text
OPTIONS /api/auth/login  -> CORS preflight HTTP request
POST /api/auth/login     -> 실제 로그인 처리 HTTP request
```

서버 입장에서는 `OPTIONS` request와 `POST` request가 별도의 request 생명주기를 가진다. DB session, dependency cache, request-local state 같은 동작은 HTTP request 사이에서 공유되지 않는다.

## 쿠키

쿠키는 서버가 브라우저에 저장하게 하는 작은 문자열 데이터다. 이후 브라우저는 같은 사이트에 요청할 때 그 쿠키를 자동으로 함께 보낼 수 있다.

대표적인 용도는 로그인 상태 유지다.

```text
1. 로그인 성공
2. 서버가 Set-Cookie 응답 헤더로 세션 쿠키를 내려줌
3. 브라우저가 쿠키를 저장
4. 이후 같은 사이트 요청에 Cookie 헤더를 자동 포함
5. 서버가 쿠키를 검증해 로그인 상태 판단
```

예:

```text
Set-Cookie: session=...; HttpOnly; Secure; Path=/; SameSite=Lax
Cookie: session=...
```

쿠키는 브라우저가 관리한다. 프론트엔드 JavaScript가 매번 직접 header에 넣는 값이 아니라, 브라우저의 쿠키 정책과 request 옵션에 따라 자동 전송되는 값이다.

## 세션과 쿠키 검증

세션은 사용자와 서버 사이의 연속적인 상태를 뜻한다. 로그인 세션은 그중에서도 서버가 사용자를 로그인 상태로 인정하는 기간과 상태를 의미한다.

쿠키 기반 세션에서는 서버가 쿠키를 검증해서 이 요청이 어떤 사용자와 연결되는지 판단한다. 쿠키에 서명을 붙이면 서버가 만든 값인지, 중간에 변조되지 않았는지 확인할 수 있다.

서명 키가 바뀌면 기존 쿠키는 검증에 실패할 수 있다. 그래서 session secret은 운영 환경에서 안전하게 관리해야 한다.

## CORS

CORS는 Cross-Origin Resource Sharing의 약자다. 브라우저가 현재 페이지를 내려 준 origin과 다른 origin으로 요청을 보내려 할 때, 그 요청을 허용할지 판단하는 보안 규칙이다.

origin은 다음 3개 조합이다.

- scheme
  - `http`, `https`
- host
  - `127.0.0.1`, `staging.example.com`
- port
  - `5300`, `8300`, `80`

예:

```text
http://127.0.0.1:5300
http://127.0.0.1:8300
```

두 주소는 host가 같아 보여도 port가 다르므로 다른 origin이다.

브라우저는 다른 origin으로 request를 보낼 때 서버가 명시적으로 허용했는지 확인한다. 서버는 `Access-Control-Allow-Origin`, `Access-Control-Allow-Credentials` 같은 응답 헤더로 허용 여부를 알려준다.

## credentials: include

프론트엔드에서 쿠키 기반 인증을 사용할 때는 API request에 쿠키를 함께 보내야 한다.

```ts
fetch('/api/me', {
  credentials: 'include',
})
```

`credentials: 'include'`는 브라우저에게 이 request에 쿠키 같은 credential을 포함하라고 지시한다. 특히 frontend와 backend origin이 다르면 이 설정이 없을 때 로그인 쿠키가 API request에 붙지 않을 수 있다.

## 같은 host 아래에서 `/api`를 쓰는 구조

운영 배포에서는 보통 Nginx 같은 reverse proxy를 사용해 프론트 정적 파일과 backend API를 같은 공개 host 아래에 둔다.

```text
https://app.example.com/          -> frontend 정적 파일
https://app.example.com/api/...   -> backend API로 proxy
```

이 구조는 다음 장점이 있다.

- 브라우저 입장에서 같은 origin으로 보이기 쉽다.
- 쿠키와 CORS 설정이 단순해진다.
- frontend 코드가 `/api/...` 같은 상대 경로를 사용할 수 있다.

## 자주 하는 실수

- HTTP request와 API 호출이라는 표현을 항상 같은 뜻으로 쓰는 경우
- host만 같으면 같은 origin이라고 생각하고 port 차이를 놓치는 경우
- 쿠키 기반 인증인데 `credentials: 'include'`를 빠뜨리는 경우
- CORS preflight를 실제 API endpoint 호출과 같은 request 생명주기로 생각하는 경우
- session cookie에 비밀번호나 민감한 원문 데이터를 넣는 경우

## 관련 문서

- [ReactFrontend.md](./ReactFrontend.md)
- [FastAPIBackend.md](./FastAPIBackend.md)
- [WebDeployment.md](./WebDeployment.md)
