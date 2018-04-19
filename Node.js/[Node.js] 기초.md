# Node.js

## 1. Node.js 기초

- 브라우저 밖에서 자바스크립트 코드를 실행
- 크롬에서 사용하는 V8 엔진을 사용
- 이벤트 기반의 비동기 I/O 프레임워크

간단한 이벤트는 이벤트 루프에서 처리 후 바로 클라이언트로 전달

이벤트 루프는 무거운 작업(데이터 가져오기 등)은 Non-blocking Worker로 보낸 후 처리 

작업 완료 후 다시 이벤트 루프로 보내짐

![Node.js 이벤트 루프](https://camo.githubusercontent.com/cb45689d053b03f76965aa02fff3b049ce79e533/68747470733a2f2f7062732e7477696d672e636f6d2f6d656469612f42743579774a72494541414b4a51742e6a7067)

출처: http://stackoverflow.com/questions/10680601/nodejs-event-loop

- CommonJS를 구현한 모듈 시스템

  - 기본 모듈

  ```javascript
  const util = require('util');
  ```

  - 써드파티 모듈

  ex) express.js

  - 사용자 정의 모듈

  ```javascript
  // math.js
  const math = {
      add(a,b){
          return a+b;
      }
  }
  module.exports = Math

  //index.js
  const math = require('./math');
  console.log(math.add(1,2));//3
  ```

## 2. 비동기 세계

```javascript
// test.txt
테스트 파일입니다.

//index.js
const fs = require('fs')
const file = fs.readFile('test.txt', { 
    encoding: 'utf8'
}, (err, data) => console.log(file)) // "undefiend"
```

```javascript
// index.js
const fs = require('fs')
const file = fs.readFileSync('test.txt', { encoding: 'utf8'
})
console.log(file) // "테스트 파일입니다.
```

## 3. Server Basic

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

### 라우팅

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  if (req.url === '/') {
  	res.statusCode = 200;
  	res.setHeader('Content-Type', 'text/plain');
  	res.end('Hello World\n');
  } else if (req.url === '/users') {
    res.statusCode = 200;
  	res.setHeader('Content-Type', 'application/json');
      const userList = [
          {name: 'Alice'},
          {name: 'Beck'},
      ]
  	res.end(JSON.stringify(userList));
  }
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

```bash
$ curl localhost:3000/users -v

*   Trying ::1...
* TCP_NODELAY set
* Connection failed
* connect to ::1 port 3000 failed: Connection refused
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 3000 (#0)
> GET /users HTTP/1.1
> Host: localhost:3000
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Content-Type: application/json
< Date: Thu, 19 Apr 2018 05:18:52 GMT
< Connection: keep-alive
< Content-Length: 34
< 
* Connection #0 to host localhost left intact
[{"name":"Alice"},{"name":"Beck"}]% 
```

