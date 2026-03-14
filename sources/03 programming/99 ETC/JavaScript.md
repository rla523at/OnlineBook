# JavaScript

## Proxy
`Proxy`는 자바스크립트에서 객체에 대한 접근을 가로채서 원하는 방식으로 동작을 바꿀 수 있게 해주는 기능이다.

즉, 원래 객체를 직접 쓰는 대신, 그 객체 앞에 “중간 관리자”를 하나 두는 개념으로 보면 된다.

예를 들어 보통 객체는 이렇게 동작한다.

```javascript
const obj = { a: 1 }

console.log(obj.a) // 1
console.log(obj.b) // undefined
```

그런데 `Proxy`를 쓰면 `obj.b`처럼 없는 값을 읽으려 할 때 그냥 `undefined`를 주지 않고, 에러를 던지거나 로그를 남기거나, 다른 값을 대신 반환하게 만들 수 있다.

형태는 대략 이렇다.

```javascript
const proxy = new Proxy(target, handler)
```

각 부분은 다음과 같다.

* `target`

  * 원래 감싸고 싶은 객체이다.
* `handler`

  * 어떤 동작을 가로챌지 정의하는 객체이다.
  * 예를 들어 속성 읽기, 쓰기, 삭제, 함수 호출 등을 가로챌 수 있다.

---

지금 코드에서 핵심은 이것이다.

```javascript
const ROUTE_VALUES = Object.freeze({
  login: '/login',
  wages: '/user/wages',
  changePassword: '/user/change-password',
  uploads: '/admin/upload',
  users: '/admin/users',
  dbTables2: '/admin/db',
})
```

이 객체는 실제 라우트 문자열들을 저장하고 있다.

`Object.freeze(...)`는 이 객체를 수정하지 못하게 만든다.

즉 아래 같은 코드는 막힌다.

```javascript
ROUTE_VALUES.login = '/new-login' // 변경 안 됨
ROUTE_VALUES.newRoute = '/abc'    // 추가 안 됨
```

그래서 `ROUTE_VALUES`는 고정된 상수 테이블 역할을 한다.

---

그 다음이 `Proxy`이다.

```javascript
export const ROUTES = new Proxy(ROUTE_VALUES, {
  get(target, prop) {
    if (typeof prop === 'symbol' || prop in target) {
      return target[prop]
    }
    throw new Error(
      `ROUTES.${String(prop)} is not defined. Available route keys: ${Object.keys(target).join(', ')}`
    )
  },
})
```

여기서

* `target`은 `ROUTE_VALUES`
* `handler`는 `{ get(...) { ... } }`

이다.

즉 `ROUTES`는 `ROUTE_VALUES`를 감싼 프록시 객체이다.

---

`get(target, prop)`는 속성을 읽을 때 호출되는 트랩(trap)이다.

예를 들어 아래 코드가 실행되면

```javascript
ROUTES.login
```

실제로는 내부적으로 대략 이런 식으로 동작한다.

```javascript
get(ROUTE_VALUES, 'login')
```

반대로

```javascript
ROUTES.notFound
```

를 읽으려고 하면 내부적으로

```javascript
get(ROUTE_VALUES, 'notFound')
```

가 호출된다.

---

이 `get` 함수의 로직은 다음과 같다.

```javascript
if (typeof prop === 'symbol' || prop in target) {
  return target[prop]
}
```

의미는 이렇다.

1. `prop`이 `symbol`이면 그냥 허용한다.

   * 자바스크립트 내부 동작이나 일부 내장 기능에서 심볼 프로퍼티를 읽는 경우가 있다.
   * 이런 경우까지 막아버리면 예상치 못한 문제가 생길 수 있어서 통과시키는 것이다.

2. `prop in target`이면 그 값을 반환한다.

   * 즉 요청한 속성이 실제 `ROUTE_VALUES`에 존재하면 정상적으로 값을 꺼내준다.

예를 들어

```javascript
ROUTES.login
```

은 `login in ROUTE_VALUES` 가 참이므로

```javascript
return ROUTE_VALUES.login
```

이 되어 `'/login'`이 반환된다.

---

반대로 존재하지 않는 키면 여기로 간다.

```javascript
throw new Error(
  `ROUTES.${String(prop)} is not defined. Available route keys: ${Object.keys(target).join(', ')}`
)
```

즉 없는 라우트를 잘못 접근하면 조용히 `undefined`가 나오는 대신, 즉시 명확한 에러를 발생시킨다.

예를 들어

```javascript
console.log(ROUTES.logni)
```

처럼 오타를 내면 보통 객체라면 `undefined`가 나오고, 나중에 다른 곳에서 더 큰 버그로 이어질 수 있다.

하지만 지금 코드는 바로 이런 에러를 낸다.

```javascript
Error: ROUTES.logni is not defined. Available route keys: login, wages, changePassword, uploads, users, dbTables2
```

이렇게 하면 오타를 빨리 발견할 수 있다.

---

즉 `ROUTES`는 겉으로 보기엔 그냥 객체처럼 쓰이지만, 실제로는 다음 규칙을 가진 “검사 강화 버전 객체”이다.

* 정의된 라우트 키만 읽을 수 있다
* 없는 키를 읽으면 즉시 에러를 낸다
* 원본 라우트 값은 `freeze`로 보호되어 있다


