# 5. HTTP와 연계하는 웹서버

웹 서버는 1대의 서버에서 멀티 도메인으로 웹사이트를 실행하거나 중계 서버를 두어 통신 중에 효율을 높일 수 있습니다.

## 1. 1대로 멀티 도메인을 가능하게 하는 가상 호스트

HTTP/1.1에서는 하나의 HTTP 서버에 여러 개의 웹사이트를 실행할 수 있습니다. 예를 들면, 웹 호스팅을 제공하고 있는 사업자는 1대의 서버에 여러 고객의 웹사이트를 넣을 수 있습니다. 고객마다 다른 도메인을 가지고 다른 웹사이트를 실행할 수 있습니다. 이를 위해 가상 호스트(Virtual Host)라는 기능을 사용하고 있습니다.

인터넷에서 도메인명은 DNS에 의해서 IP주소로 변환되고 나서 액세스하게 됩니다. 결국 리퀘스트가 서버에 도착한 시점에는 IP주소를 기준으로 액세스하게 됩니다. 같은 IP주소에서 다른 호스트명과 도메인 명을 가진 여러 개의 웹사이트가 실행되게 하는 가상 호스트가 있습니다. 이 때문에 HTTP 리퀘스트를 보내는 경우에는 호스트명과 도메인 명을 완전하게 포함한 URI를 저장하거나, 반드시 Host 헤더 필드에서 지정해야 합니다.

## 2. 통신을 중계하는 프로그램: 프록시, 게이트웨이, 터널

### 1) 프록시

서버와 클라이언트 양쪽 역할을 중계하는 프로그램으로 기본적인 동작은 클라이언트로부터 받은 리퀘스트를 다른 서버에 전송하는 것입니다. 클라이언트로부터 받은 리퀘스트 URI를 변경하지 않고 그 다음의 리소스를 가지고 있는 서버에 보냅니다.

리소스 본체를 가진 서버를 오리진 서버(Origin Server)라고 부릅니다. 오리진 서버로부터 되돌아온 리스폰스는 프록시 서버를 경유해서 클라이언트에 돌아옵니다.

HTTP 통신을 할 때, 프록시 서버를 여러 대 경유하는 것도 가능합니다. 체인과 같이 여러 대 경유해서 리퀘스트랑 리스폰스를 중계해 갑니다. 중계할 때에는 Via 헤더 필드에 경유한 호스트 정보를 추가해야합니다.

프록시 서버를 사용하는 이유는 캐시를 사용해서 네트워크 대역 등으 ㄹ효율적으로 사용하는 것과 조직 내에 특정 웹 사이트에 대한 액세스 제한, 액세스 로그를 획득하는 정책을 철저하게 지키려는 목적으로 사용하는 경우도 있습니다.

프록시의 사용 방법은 두 가지 기준으로 분류합니다.

#### 캐싱 프록시(Cashing Proxy)

프록시로 리스폰스를 중계하는 때에는 프록시 서버 상에 리소스 캐시를 보존해 두는 타입의 프록시입니다. 프록시에 다시 같은 리소스에 리퀘스트가 온 경우, 오리진 서버로부터 리소스를 획득하는 것이 아니라 캐시를 리스폰스로 되돌려 줍니다.

#### 투명 프록시(Transparent Proxy)

프록시로 리퀘스트와 리스폰스를 중계를 할 때 메시지 변경을 하지 않는 타입의 프록시를 투명 프록시라고 합니다. 반대로 메시지에 변경을 가하는 타입의 프록시를 비투과 프록시라고 합니다.

### 2) 게이트웨이

게이트웨이의 동작은 프록시와 매우 유사합니다. 다만 게이트웨이는 클라이언트가 HTTP프로토콜 이외에 다른 프로토콜끼리 통신하도록 도와주는 역할을 합니다. 클라이언트와 게이트웨이 사이를 암호화하는 등으로 안전하게 접속함으로써 통신의 안정성을 높이는 역할도 합니다.

예를 들면, 게이트웨이는 데이터베이스에 접속해 SQL 쿼리를 사용해서 데이터를 얻는 곳에 이용할 수 있습니다. 그 밖에도 쇼핑 사이트 등에서 신용 카드 결제 시스템 등과 연계할 때 사용되기도 합니다.

### 3) (IP)터널

터널은 요구에 따라서 다른 서버와의 통신 경로를 확립합니다. 네트워크의 패킷을 캡슐화여 또 다른 네트워크로 전달한다는 의미도 됩니다. 이 때 클라이언트는 SSL 같은 암호화 통신을 통해 서버와 통신을 안전하게 해줍니다. 터널 자체는 HTTP 리퀘스트를 해석하려고 하지 않습니다. 결국 리퀘스트를 그대로 다음 서버에 중계합니다. 그리고 터널은 통신하고 있는 양쪽 끝의 접속이 끊어질 때에 종료합니다. 

## 3. 리소스를 보관하는 캐시

캐시(Cache)는 프록시 서버와 클라이언트의 로컬 디스크에 보관된 리소스의 사본을 가리킵니다. 캐시를 사용하면 리소스를 가진 서버에의 액세스를 줄이는 것이 가능하기 때문에 통신량과 통신 시간을 절약할 수 있습니다.

캐시 서버는 프록시 서버의 하나로 캐싱 프록시로 분류됩니다. 결국, 프록시가 서버로부터의 리스폰스를 중계하는 때에 프록시 서버 상에 리소스의 사본을 보관하게 됩니다. 

캐시 서버의 장점은 캐시를 이용함으로써 같은 데이터를 몇 번이고 오리진 서버에 전송할 필요가 없다는 것입니다. 그래서 클라이언트는 네트워크에서 가까운 서버로부터 리소스를 얻을 수 있게 되어 서버는 같은 리퀘스트를 매번 처리 하지 않아도 됩니다.

### 1) 캐시는 유효기간이 있다.

캐시 서버에 캐시가 있는 경우라도 같은 리소스의 리퀘스트에 대해서 항상 캐시를 돌려 준다고 할 수 없습니다. 이것은 캐시되어 있는 리소스의 유효성과 관계가 있습니다.

언제까지나 같은 캐시를 계속해서 사용하고 있다보면 오리진 서버에 있는 원래 리소스가 갱신되는 경우가 있는데, 이 때 캐시 서버는 갱신되기 전의낡은 리소스를 그대로 보내게 됩니다.

그래서 캐시를 가지고 있더라고 클라이언트의 요구나 캐시의 유효 기간 등에 의해서 오리진 서버에 리소스의 유효성을 확인하거나 새로운 리소스를 다시 획득하러 가게 되는 경우가 있습니다.

### 3) 클라이언트 측에도 캐시가 있다.

브라우저에서 클라이언트가 보관하고 있는 캐시를 인터넷 임시 파일이라고 부릅니다. 브라우저가 유효한 캐시를 가지고 있는 경우, 같은 리소스의 액세스는 서버에 액세스하지 않고 로컬 디스크로부터 불러 옵니다.

또한 캐시 서버와 마찬가지로 리소스가 오래된 것으로 판단된 경우에는 오리진 서버에 리소스의 유효성을 확인하러 가거나 새로운 리소스를 다시 획득합니다.

Reference [그림으로 배우는 HTTP & Network Basic](http://www.youngjin.com/book/book_detail.asp?prod_cd=9788931447897&seq=5470&cate_cd=1&child_cate_cd=10&goPage=1&orderByCd=1&searchType=Y&keyword1=%B1%D7%B8%B2%C0%B8%B7%CE%20%B9%E8%BF%EC%B4%C2%20http)