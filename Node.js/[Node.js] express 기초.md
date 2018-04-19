# Node.js

## 1. Express.js 기초

위의 http 모듈을 통한 서버 작업을 간편하게 처리하기 위한 라이브러리 중 하나

- 익스프레스 인스턴스는 애플리케이션이라 칭함

```javascript
const express = require('express');
const app = express();
```

- 서버에 필요한 기능인 미들웨어를 어플리케이션에 추가
- 라우팅을 설정할 수 있다.
- 서버를 요청 대기 상태로 만들 수 있다.

```javascript
const express = require('express');
const app = express();

app.listen(3000, () => console.log('running'));
```

### 미들웨어

서버라는 뼈대 사이사이에 들어가는 추가 기능들을 지원해주는 모듈

```javascript
const express = require('express');
const app = express();

const mw = (req, res, next) => {
  console.log('mw!');
  next();
}; // 세번째 인자 다음 미들웨어를 실행시켜주기 위한 콜백

app.use(mw);
```

- 미들웨어는 함수들의 연속이다.


- 미들웨어를 배치하는 순서가 중요하다. 서버 전체의 실행에 영향을 줌

### 라우팅

- 요청 URL에 대해 적절한 핸들러 함수로 연결해 주는 기능을 라우팅이라고 함
- 어플리케이션의 get(), post() 메소드로 구현할 수 있다.
- 라우팅을 위한 전용 Router 클래스를 사용할 수 있다.

### 요청 객체(req)

- 클라이언트의 요청 정보를 담은 객체를 요청(Request) 객체라 함
- http 모듈의 request 객체를 래핑한 것
- param(), query(), body() 메소드를 주로 사용함

### 응답 객체(res)

- 클라이언트 응답 정보를 담은 객체를 응답(Response)객체라 함
- http 모듈의 response 객체를 래핑한 것
- send(), status(), json() 메소드를 주로 사용함