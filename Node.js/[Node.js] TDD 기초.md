# Node.js

## 1. TDD

### mocha

```javascript
//app.spec.js
const assert = require('assert');

describe('GET /users', () => {
  it('배열을 반환한다', () => {
    assert.equal(1,1);
  });
});

//mocha app.spec.js

  GET /users
    ✓ 배열을 반환한다


  1 passing (5ms)

```

### should

- 슈드(should)는 검증(assertion) 라이브러리다.
- 가독성 높은 테스트 코드를 만들 수 있다.

```javascript
const assert = require('assert');
const should = require('should');

describe('GET /users', () => {
  it('배열을 반환한다', (done) => {
    (1).should.equal(1);
  });
});
```

### supertest

- 단위 테스트: 함수의 기능 테스트
- 통합 테스트: API의 기능 테스트
- 슈퍼 테스트는 익스프레스 통합 테스트용 라이브러리다
- 내부적으로 익스프레스 서버를 구동시켜 실제 요청을 보낸 뒤 결과를 검증한다

```javascript
// app.js
const express = require('express');
const logger = require('morgan');

const app = express();

const users = [{name: 'Alice'}];

app.get('/', (req, res) => res.send('Hello World!'));

app.get('/users', (req, res) => res.json(users));

app.listen(3000, () => console.log('Example app listening on port 3000!'));

module.exports = app
```

```javascript
// app.spec.js
const request = require('supertest');
const express = require('express');
const app = require('./app');

describe('GET /users', () => {
  it('배열을 반환한다', (done) => {
    request(app)
      .get('/users')
      .end((err, res) => {
        console.log(res.body);
        done();
      })
  });
});

// mocha app.spec.js
Example app listening on port 3000!
  GET /users
[ { name: 'Alice' } ]
    ✓ 배열을 반환한다


  1 passing (28ms)
```

```javascript
// app.spec.js
const request = require('supertest');
const should = require('should');
const express = require('express');
const app = require('./app');

describe('GET /users', () => {
  it('배열을 반환한다', (done) => {
    request(app)
      .get('/users')
      .end((err, res) => {
        res.body.should.be.instanceof(Array);
        res.body.forEach(user => {
          user.should.have.property('name');
        })
        done();
      })
  });
});

// mocha app.spec.js

Example app listening on port 3000!
  GET /users
    ✓ 배열을 반환한다


  1 passing (28ms)
```

## 2. API 테스트 만들기

```javascript
// app.js
const express = require('express');
const logger = require('morgan');

const app = express();

const users = [
  {id: 1, name: 'Alice'},
  {id: 2, name: 'Bek'},
  {id: 3, name: 'Chris'},
];

app.get('/', (req, res) => res.send('Hello World!'));

app.get('/users', (req, res) => {
  req.query.limit = req.query.limit || 10;
  const limit = parseInt(req.query.limit, 10);
  res.json(users.slice(0, limit));
});

module.exports = app
```

```javascript
// app.spec.js
const request = require('supertest');
const should = require('should');
const express = require('express');
const app = require('./app');

describe('GET /users', () => {
  it('최대 limit 갯수만큼 응답한다', done => {
    request(app)
      .get('/users?limit=2')
      .end((err, res) => {
        res.body.should.have.lengthOf(2);
        done();
      })
  })
});
```

### 에러 처리

```javascript
// app.js
const express = require('express');
const logger = require('morgan');

const app = express();

const users = [
  {id: 1, name: 'Alice'},
  {id: 2, name: 'Bek'},
  {id: 3, name: 'Chris'},
];

app.get('/', (req, res) => res.send('Hello World!'));

app.get('/users', (req, res) => {
  req.query.limit = req.query.limit || 10;
  const limit = parseInt(req.query.limit, 10);
  Number.isNaN(limit)
    ? res.status(400).send('400!')
    : res.json(users.slice(0, limit))
});

module.exports = app
```

```javascript
// app.spec.js
const request = require('supertest');
const should = require('should');
const express = require('express');
const app = require('./app');

describe('GET /users', () => {
  describe('성공', () => {
    it('배열을 반환한다', (done) => {
      request(app)
        .get('/users')
        .end((err, res) => {
          res.body.should.be.instanceof(Array);
          res.body.forEach(user => {
            user.should.have.property('name');
          })
          done();
        })
    });
    it('최대 limit 갯수만큼 응답한다', done => {
      request(app)
        .get('/users?limit=2')
        .end((err, res) => {
          res.body.should.have.lengthOf(2);
          done();
        })
    })
  });
  describe('실패', () => {
    it('limit이 정수가 아니면 400을 응답한다', done => {
      request(app)
        .get('/users?limit=two')
        .expect(400)
        .end(done)
    })
  })
});
```

