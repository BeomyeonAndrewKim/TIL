# HTTP에 기능을 추가한 프로토콜

## 1. HTTP의 병목 현상을 해소하는 SPDY

### 1) HTTP의 병목 현상

- 1개의 커넥션으로 1개의 리퀘스트만 보낼 수 있다.
- 리퀘스트는 클라이언트에서만 시작할 수 있다. 리스폰스만 받는 것은 불가능하다.
- 리퀘스트/리스폰스 헤더를 압축하지 않은 체로 보낸다. 헤더의 정보가 많을수록 지연이 심해진다.
- 장황한 헤더를 보낸다. 매번 같은 헤더를 보내는 것은 낭비다.
- 데이터 압축을 임의로 선택할 수 있다. 압축해서 보내는 것이 강제적이지는 않다.

위와 같은 HTTP 사양이 병목현상의 원인이 됩니다.

#### Ajax에 의한 해결 방법

Ajax(Asynchronous JavaScript+XML)은 JavaScript나 DOM(Document Object Model) 조작 등을 활용하는 방식으로, 웹페이지의 일부분만 고쳐쓸 수 있는 비동기 통신 방법입니다. 기존의 동기식 통신에 비해서 페이지의 일부분만 갱신되기 때문에 리스폰스로 전송되는 데이터 양은 줄어든다는 장점이 있습니다.

Ajax의 핵심 기술은 XMLHttpRequest라는 API로 JavaScript 등의 스크립트 언어로 서버와 HTTP 통신을 할 수 있습니다. Ajax를 사용해서 실시간으로 서버에서 정보를 취득하려고 하면 대량의 리퀘스트가 발생한다는 문제가 있습니다. 또 HTTP의 프로토콜 자신이 가지고 있는 문제가 해결되는 것은 아닙니다.

#### Comet에 의한 해결 방법

Comet은 서버 측의 콘텐츠에 갱신이 있었을 경우, 클라이언트로부터 리퀘스트를 기다리지 않고 클라이언트에 보내기 위한 방법입니다. 응답을 연장시킴으로써, 서버에서 통신을 개시하는 서버 푸시 기능을 유사하게 따르고 있습니다.

통상 리퀘스트가 오면 리스폰스를 바로 반환하지만 Comet에서는 리스폰스를 보류 상태로 해 두고, 서버의 콘텐츠가 갱신되었을 때에 리스폰스를 반환한다는 것입니다. 

콘텐츠를 실시간으로 갱신할 수는 있지만 리스폰스를 보류하기 위해서 커넥션을 유지하는 시간이 길어집니다. 커넥션을 유지하는 동안은 리소스를 소비합니다.

### 2) SPDY 설계와 기능

SPDY는 HTTP를 완전히 바꿔 놓는 것이 아니라 TCP/IP의 애플리케이션 계층과 트랜스포트 계층 사이에 새로운 세션 계층을 추가하는 형태로 동작합니다. 또한, SPDY는 보안을 위해서 표준으로 SSL을 사용하도록 되어 있습니다. SPDY가 세션 계층으로 그 사이에 들어감으로써 데이터의 흐름을 제어하지만, HTTP의 커넥션은 확립되어 있습니다. 그래서 HTTP의 GET이나 POST같은 메소드나 쿠키, HTTP 메시지 등을 그대로 사용할 수 있습니다.

![SPDY](http://d2.naver.com/content/images/2015/06/helloworld-140351-1.png)

#### 다중화 스트림

단일 TCP 접속을 통해서 복수의 HTTP 리퀘스트를 무제한으로 처리할 수 있습니다. 한 번의 TCP 접속으로 리퀘스트를 주고 받는 것이 가능하기 때문에 TCP의 효율이 높아집니다.

#### 리퀘스트의 우선 순위 부여

SPDY는 무제한으로 리퀘스트를 병렬 처리할 수 있지만, 각 리퀘스트에 우선순위를 할당할 수 있습니다. 이것은 복수의 리퀘스트를 보낼 때 대역폭이 좁으면 처리가 늦어지는 현상을 해결하기 위해서입니다.

#### HTTP 헤더 압축

리퀘스트와 리스폰스의 HTTP 헤더를 압축합니다. 이로 인해 보다 적은 패킷수와 송신 바이트 수로 통신할 수 있습니다.

#### 서버 푸시 기능

서버에서 클라이언트로 데이터를 푸쉬하는 서버 푸시 기능을 지원합니다.

#### 서버 힌트 기능

서버가 클라이언트에게 리퀘스트 해야 할 리소스를 제안할 수 있습니다. 클라이언트가 자원을 발견하기 전에 리소스의 존재를 알 수 있기 때문에 이미 캐시를 가지고 있는 상태라면 불필요한 리퀘스트를 보내지 않아도 됩니다.

#### 한계

SPDY는 기본적으로 한 개의 도메인(IP 주소)과의 통신을 다중화할 뿐이기 때문에 하나의 웹 사이트에서 복수의 도메인으로 리소스를 사용하고 있는 경우에는 그 효과는 한정적이게 됩니다.

## 2. 브라우저에서 양방향 통신을 하는 WebSocket

### 1) WebSocket의 설계와 기능

WebSocket은 웹 브라우저와 웹 서버를 위한 양방향 통신 규격입니다. 주로 Ajax나 Comet에서 사용하는 XMLHttpRequest의 결점을 해결하기 위한 기술로서 개발이 진행되고 있습니다.

### 2) WebSocket 프로토콜

WebSocket은 웹 서버와 클라이언트가 한 번 접속을 확립하면 그 뒤의 통신을 모두 전용 프로토콜로 하는 방식으로 JSON이나 XML, HTML이나 이미지 등 임의 형식의 데이터를 보내게 됩니다.

HTTP에 의한 접속의 출발점이 클라이언트에 있다는 것에는 변함이 없지만 한 번 접속을 확립하면 WebSocket을 사용하여 서버와 클라이언트 어느쪽에서도 송신을 할 수 있게 됩니다.

#### 서버 푸시 기능

서버에서 클라이언트에 데이터를 푸시하는 서버 푸시 기능을 제공합니다.

#### 통신량의 감소

WebSocket은 접속을 한 번 확립하면 접속을 유지하려고 합니다. HTTP에 비해서 자주 접속을 하는 오버헤더가 적어지고, 또 헤더의 사이즈도 작기 때문에 통신량을 줄일 수 있습니다. 

WebSocket으로 통신을 하려면 한 번 HTTP에 접속을 하고 핸드쉐이크 절차를 밟을 필요가 있습니다.

#### 핸드쉐이크/리퀘스트

```http
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: asdfasdf==
Origin: http://example.com
Sec-WebSocket-Protocal: chat, superchat
Sec-WebSocket-Version: 13
```

Sec-WebSocket-Key에는 핸드쉐이크에 필요한 키가 저장되어 Set-WebSocket-Protocol에는 사용하는 서브 프로토콜이 저장되어 있습니다. 서브 프로토콜은 WebSocket 프로토콜에 의한 커넥션을 여러 개로 구분하고 싶을 때에 이름을 붙여서 정의합니다.

#### 핸드쉐이크/리스폰스

```http
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: asdfasdf=
Sec-WebSocket-Protocol: chat
```

Sec-WebSocket-Accept는 Sec-WebSocket-Key의 값에서 생성된 값이 저장됩니다. 핸드쉐이크에 의해 WebSocket 커넥션이 확립된 후에는 HTTP가 아닌, WebSocket 독자적인 데이터 프레임을 이용해 통신을 합니다.

#### WebSocket API

```javascript
const socket = new WebSocket('ws://game.example.com:12010/updates');
socket.onopen = function(){
    setInterval(function(){
        if(socket.bufferedAmount === 0){
            socket.send(getUpdateData());
        }
    },50);
};
```

