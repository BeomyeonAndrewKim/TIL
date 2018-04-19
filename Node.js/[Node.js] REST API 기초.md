# Node.js

## 1. REST API

- 모든 자원은 명사로 명시한다.
- HTTP 경로로 자원을 요청한다.

```http
GET localhost:3000/users
DELETE localhost:3000/users/1
```

- 서버 자원에 대한 행동을 나타낸다. (동사로 표현)
  - GET
  - POST
  - PUT
  - DELETE
- exprss 메소드로 지원함

### HTTP 상태코드

- 1xx: 아직 처리중
- 2xx: 성공
  - 200: 성공(success), GET, PUT
  - 201: 작성됨(created), POST
  - 204: 내용 없음(No Content), DELETE
- 3xx: 리다이렉트
- 4xx: 클라이언트 문제
  - 400: 잘못된 요청(Bad Request)
  - 401: 권한 없음(Unauthorized)
  - 409: 충돌(Conflict)
- 5xx: 서버 문제
  - 500: 서버 에러(Internal server error)

