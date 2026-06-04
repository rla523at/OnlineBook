# React Frontend

## 한 줄 요약

React 프론트엔드는 브라우저에서 실행되는 UI 애플리케이션이다. 개발 중에는 Vite dev server가 소스코드를 빠르게 제공하고, 운영에서는 빌드된 정적 파일을 Nginx 같은 웹 서버가 제공한다.

## 프론트엔드란 무엇인가

프론트엔드는 사용자의 브라우저에서 실행되며 UI를 렌더링하고 사용자 입력을 처리하는 애플리케이션을 의미한다. 웹 서비스에서는 화면과 상호작용을 담당하는 쪽이며, 데이터 저장과 비즈니스 로직을 담당하는 백엔드와 대비된다.

이 문서에서 React 프론트엔드는 다음 전체를 가리킨다.

- 개발 레이어
  - React 컴포넌트, 상태 관리, 라우팅 등 UI 로직
- 빌드 산출물 레이어
  - `index.html`, JS 번들, CSS, 이미지, 폰트 같은 정적 파일 묶음
- 런타임 레이어
  - 브라우저의 렌더링 엔진과 JavaScript 엔진에서 실행되는 SPA

## React, SPA, 정적 파일

React는 UI를 만들기 위한 JavaScript 라이브러리다. React SPA는 React로 만든 Single Page Application이며, 브라우저에서 실행되는 프론트엔드 애플리케이션이다.

정적 프론트엔드 파일은 React SPA를 사용자 브라우저에 전달하기 위한 빌드 산출물이다.

예:

```text
index.html
assets/app.<hash>.js
assets/app.<hash>.css
assets/*.png
assets/*.woff2
```

브라우저가 이 파일을 내려받아 JavaScript를 실행하면 React SPA가 화면을 렌더링한다.

## 브라우저에서 코드가 실행된다는 뜻

React SPA는 서버에서 실행되는 프로그램이 아니라 사용자의 브라우저 안에서 실행되는 프로그램이다. 따라서 React SPA와 FastAPI backend는 같은 프로세스나 메모리를 공유하지 않는다.

둘은 HTTP request와 response로만 상호작용한다.

```text
React SPA in browser
  -> HTTP request
  -> backend API
  -> HTTP response
  -> React state/UI update
```

HTTP, 쿠키, CORS의 기본 동작은 [HTTPAndBrowser.md](./HTTPAndBrowser.md)를 참고한다.

## 정적 파일 request와 API request

React SPA 관점에서 HTTP request는 크게 두 종류로 나뉜다.

- 정적 파일 request
  - 앱을 받는 단계
- API request
  - 앱 실행 중 데이터를 조회하거나 저장하는 단계

정적 파일 request 예:

```text
GET /
GET /assets/app.js
GET /assets/app.css
```

API request 예:

```text
GET /api/items
POST /api/auth/login
POST /api/items
```

정적 파일 request는 브라우저의 페이지 로딩 과정에서 발생하고, API request는 React 코드가 `fetch`, `axios`, generated client 같은 함수를 실행할 때 발생한다.

## Vite dev server

dev server는 로컬 개발 중에 프론트 코드를 브라우저에서 바로 띄우고, 코드를 바꾸면 즉시 반영되게 해 주는 개발용 웹 서버다.

Vite dev server는 React/Vite 프로젝트를 로컬 개발 모드로 실행할 때 다음 기능을 제공한다.

- 개발 중 소스 파일 제공
- 빠른 module 변환
- Hot Module Replacement
- 에러 overlay
- production build와 다른 개발 최적화

Vite 프로젝트에 `dev` script가 정의되어 있다면 개발 중에는 다음처럼 실행한다.

```powershell
npm run dev
```

또는 host와 port를 명시한다.

```powershell
npm run dev -- --host 127.0.0.1 --port 5300
```

## 운영에서는 dev server를 쓰지 않는 이유

운영에서 Nginx가 React/Vite 앱을 정적 파일로 제공하는 구조라면 다음 흐름을 사용한다.

```text
1. npm run build
2. dist/ 정적 파일 생성
3. 정적 파일을 web root로 배치
4. Nginx가 정적 파일을 서빙
```

즉 운영에서는 소스코드를 실시간으로 처리하는 서버보다, 빌드된 결과물을 안정적으로 응답하는 서버가 필요하다.

## SPA 라우팅

SPA에서는 브라우저 주소창의 경로를 서버가 아니라 프론트 라우터가 해석한다.

예:

```text
/admin/users
/reports/daily
/user/change-password
```

브라우저 안에서 링크를 클릭하면 React Router가 경로를 해석하고 화면을 바꾼다. 이때 전체 HTML을 서버에서 다시 받는 것이 아니라, 이미 로드된 React 앱 안에서 화면만 바뀐다.

## 새로고침과 index.html fallback

SPA에서 가장 자주 만나는 배포 문제는 새로고침이다.

사용자가 `/admin/users`를 직접 열거나 새로고침하면 request는 먼저 서버로 간다. 서버 입장에서는 `/admin/users`라는 실제 파일이 없을 수 있다.

그래서 정적 파일 서버는 실제 파일이 없으면 `index.html`을 내려주도록 설정해야 한다.

Nginx 예:

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

이 설정 덕분에 브라우저는 언제나 React 앱의 진입점인 `index.html`을 받고, 그 다음 React Router가 현재 경로에 맞는 화면을 렌더링한다.

## Backend와 통신

React SPA는 backend API를 호출해 데이터를 읽거나 저장한다.

```ts
const response = await fetch('/api/items');
const items = await response.json();
```

로그인처럼 쿠키 기반 인증이 필요한 요청은 브라우저 credential 정책도 함께 고려해야 한다.

```ts
await fetch('/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  credentials: 'include',
  body: JSON.stringify({ login_id: 'alice', password: 'secret' }),
});
```

## 자주 하는 실수

- React 앱이 서버에서 실행된다고 생각하는 경우
- dev server와 운영 Nginx를 같은 역할로 생각하는 경우
- SPA fallback 없이 정적 파일만 서빙해서 새로고침이 404가 되는 경우
- frontend와 backend가 같은 메모리를 공유한다고 생각하는 경우
- 쿠키 기반 API 호출에서 credential 설정을 빠뜨리는 경우

## 관련 문서

- [HTTPAndBrowser.md](./HTTPAndBrowser.md)
- [FastAPIBackend.md](./FastAPIBackend.md)
- [WebDeployment.md](./WebDeployment.md)
