---
title: "[HTTP/네트워크] 기초"
categories:
 - BEB
tags: [HTTP, URL, URI, IP, Port, Domain, DNS, AJAX, SSR, CSR, CORS] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# HTTP를 이용한 클라이언트-서버 통신과 API



## 클라이언트-서버 통신, 주요 프로토콜

프로토콜 : 통신 규약, 약속

- 웹 애플리케이션 아키텍처에서는 클라이언트와 서버가 서로 HTTP라는 프로토콜을 이용하여 통신

주요 프로토콜

- 응용 계층
  - HTTP : 웹에서 HTML, DOM, JSON 등의 정보를 주고 받는 프로토콜
  - HTTPS: HTTP에서 보안이 강화된 프로토콜
  - FTP : 파일 전송 프로토콜
  - SMTP : 메일을 전송하기 위한 프로토콜
  - SSH : CLI 환경의 원격 컴퓨터에 접속하기 위한 프로토콜
  - RDP : Windows 계열의 원격 컴퓨터에 접속하기 위한 프로토콜
  - WebSocket : 실시간 통신, Push 등을 지원하는 프로토콜
- 전송 계층
  - TCP : HTTP, FTP 통신 등의 근간이 되는 인터넷 프로토콜
  - UDP : 양방향의 TCP와 다르게 단방향으로 작동하는 훨씬 더 단순하고 빠르지만 신뢰성이 낮은 인터넷 프로토콜



## API(Application Programming Interface)

API : Application Programming Interface, 의사소통이 가능하도록 만들어진 접점

- 사용자는 이를 이용하여 서버가 제공하는 서비스를 이용할 수 있으며, 이는 인터넷에 있는 데이터를 요청 시, HTTP프로토콜의 주소(URL, URI)를 통해 접근 가능하다.
  - 파라미터로는 &, ?와 같은 기호를 사용함으로써 자세한 질문(Query)을 할 수 있다.



HTTP API 디자인

- 주요 메소드
  - GET : 조회
  - POST : 추가
  - PUT or PATCH : 갱신
  - DELETE : 삭제
- 위와 같은 메소드를 이용하여 이미 기능은 어느 정도 명시가 됨으로, url디자인 시 기능을 제외한 대상을 위주로 명시한다.
  - Ex. 모든 사용자 조회 : /users(O) /viewUsers(X)





# 브라우저 작동 원리(보이지 않는 곳)

---------------------------

## URL과 URI

URL : Uniform Resource Locator, 네트워크 상에서 웹 페이지, 이미지, 동영상 등의 파일이 위치한 정보를 나타낸다.

구분

- Scheme : 통신프로토콜로, file://, http://, https:// 등이 있다.
- Hosts : 웹 페이지, 이미지, 동영상 등의 파일이 위치한 웹 서버, 도메인 또는 IP로 localhosts, www.google.com 등이 있다.
- Port : 웹 서버에 접속하기 위한 통로로, 쓰임새별로 80(http), 443(https), 3000 등이 있다.
- Url-path : 웹 서버의 루트 디렉토리로부터 웹 페이지, 이미지, 동영상 등의 파일이 위치까지의 경로로, /search, /user 등이 있다.



URI : Uniform Resource Identifier, 일반적으로 URL + query, bookmark를 포함한다.

- query : 웹 서버에 전달하는 추가 질문같은 역할로, q=js와 같이 쓰인다.



## IP와 포트

IP : Internet Protocol, 인터넷 상에서 사용하는 주소체계이다. 보통 IPv4로 255.255.255.255와 같은 형태로 사용되었으나, IPv4로 할당할 수 있는 PC가 부족해지면서 IPv6로 확장되고 있다.

- WellKnown IP 
  - Localhost, 127.0.0.1 : 현재 사용 중인 로컬 PC
  - 0.0.0.0, 255.255.255.255 : broadcast address, 로컬 네트워크에 접속된 모든 장치와 소통하는 주소



Port : IP 주소가 가리키는 PC에 접속할 수 있는 채널

- Well Known Port
  - 22 : SSH
  - 80 : HTTP
  - 443 :  HTTPS
  - ...
- 이미 정해진 포트 번호라도, 필요에 따라 자유롭게 사용가능하고, 알려지지 않은 포트의 경우 명시한다.



## 도메인과 DNS

Domain name : IP 주소는 ~.~.~.~와 같이 숫자로 이루어져 있기 때문에 사용에 불편이 있다. 따라서 이러한 IP 주소를 google.com과 같이 사용하기 편한 주소로 변환된 주소가 도메인이다.



DNS : Domain Name System의 줄임말로, 호스트의 도메인 이름을 IP 주소로 변환하거나 반대의 경우를 수행할 수 있도록 개발된 DBS이다.





# HTTP

---------------------------

HTTP : HyperText Transfer Protocol, HTML과 같은 문서를 전송하기 위한 응용계층 프로토콜으로, 특정 상태를 유지하지 않는 무상태성(Stateless)를 특징으로 갖는다. 즉, 통신 과정에서 서버나 클라이언트의 상태를 확인하지 않는다.



## HTTP Message

![스크린샷 2022-08-01 오후 6.17.10](../../images/2022-07-29-httpNetworkBasic/스크린샷 2022-08-01 오후 6.17.10.png)

{: .align-center}

HTTP Message는 위와 같은 형식으로 완성되며, Request와 Response의 형식이 좀 다르다.



### Request

-------------------------------

- Start line : 수행할 작업(GET, PUT, POST 등)이나 방식(HEAD, OPTION)을 설명하는 HTTP method를 나타내고 요청 대상 또는 프로토콜, 포트, 도메인의 절대 경로는 요청 컨텍스트에 작성된다.
  - Origin 형식 : ?와 쿼리 문자열이 붙는 절대 경로로, POST, GET, OPTIONS 등의 method와 함께 사용된다.
    - Ex. POST / HTTP 1.1
  - Absolute 형식 : 완전한 URL 형식으로 프록시에 연결하는 경우 대부분 GET method와 함께 사용된다.
    - GET http:// ~
  - Authority 형식 : 도메인 이름과 포트 번호로 이루어진 URL의 authority component이다.
  - Asterisk 형식 : OPTIONS와 함께 * 하나로 서버 전체를 표현한다.
    - OPTIONS * HTTP/1.1
- Header 

![스크린샷 2022-08-01 오후 6.25.22](../../images/2022-07-29-httpNetworkBasic/스크린샷 2022-08-01 오후 6.25.22.png)

{: .align-center}

  - 
    - General Headers : 메시지 전체에 적용한다.
    - Request Headers : User-Agent, Accept-Type, Accept-Language과 같은 헤더는 요청을 보다 구체화한다. referer처럼 컨텍스트를 제공하거난 If-None과 같이 조건에 따라 제약을 추가할 수 있다.
    - Entity Headers : Content-Length와 같은 헤더는 body에 적용된다. 따라서 body가 비어있는 경우 전송되지 않는다.
  - Body : message 구조 마지막에 위치하며, GET, HEAD, DELETE, OPTIONS와 같이 서버에 리소스를 요청하는 경우에는 필요없다.
    - 구분
      - single-resource bodies : 헤더 두개(Content-Type과 Content-Length)로 정의된 단일 파일로 구성되어 있다.
      - Multiple-resource bodies : 여러 파트로 구성된 본문에서는 각 파트마다 다른 정보를 지닌다.(HTML Form)



#### Method

- GET : 특정 리소스의 표시를 요청, 오직 데이터를 받기만 한다.
- HEAD : GET메서드의 요청과 동일한 응답을 요구하지만, body를 포함하지 않는다.
- POST : 특징 리소스에 엔티티를 제출할 때 사용한다.(서버의 상태의 변화나 부작용을 일으킬 수 있다.)
- PUT : 목적 리소스 모든 현재 표시를 요청 payload로 바꾼다.
- DELETE : 특정 리소스를 삭제한다.
- CONNECT : 목적 리소스로 식별되는 서버로의 터널을 연결한다.
- OPTIONS : 목적 리소스의 통신을 설정한다.
- TRACE : 목적 리소스의 경로에 따라 메시지 loop-back 테스트한다.
- PATCH : 리소스의 부분 수정한다. 



### Response

-------------------------------

- Status line : 현재 프로토콜의 버전, 상태코드(요청의 결과 : 200, 302, 404 등), 상태 텍스트(상태 코드에 대한 설명) 의 정보를 포함한다.
  - Ex. HTTP/1.1 404 Not Found
- Header : 요청 헤더와 동일한 구조

![스크린샷 2022-08-01 오후 10.21.02](../../images/2022-07-29-httpNetworkBasic/스크린샷 2022-08-01 오후 10.21.02.png)

{: .align-center}

- - General Headers : 메시지 전체에 적용한다.
  - Response Headers : Vary, Accept-Ranges와 같이 상태 줄에 넣기에는 공간이 부족했던 추가 정보를 제공한다.
  - Entity Headers : Content-Length와 같은 헤더는 body에 적용, body가 비어있는 경우, entity headers는 전송되지 않는다.

- Body : HTTP messages 구조의 마지막에 위치하며, 201, 204와 같은 상태 코드를 가지는 응답에는 필요없다.
  - Single-resource bodies(단일 리소스 본문) :
    - 길이가 알려진 단일-리소스 본문은 두 개의 헤더(Content-Type, Content-Length)로 정의한다.
    - 길이를 모르는 단일 파일로 구성된 단일-리소스 본문은 Transfer-Encoding이 "chunked"로 설정되어 있으며, 파일은 chunk로 나뉘어 인코딩되어 있다.
  - Multiple-resource bodies(다중 리소스 본문) :  서로 다른 정보를 담고 있다.



#### Status Code(대분류)

- 1XX(Information Response) : 서버가 클라이언트의 요청을 받았고 클라이언트는 프로세스 진행 가능을 알린다.
- 2XX(Successful Response) : 서버가 클라이언트의 요청을 성공적으로 수행했다.
- 3XX(Redirection Response) : 클라이언트가 요청한 리소스의 위치 변화를 알린다.
- 4XX(Client Error Response) : 클라이언트가 서버로 보낸 요청에서의 오류를 알린다.
- 5XX(Server Error Response) : 서버 자체 에러를 알린다.



<br>

# 브라우저의 작동 원리(보이는 곳)

-------------------------------



## SPA : AJAX

AJAX : Asynchronous JavaScript And XMLHttpRequest로, 다양한 기술을 사용하는 웹 개발 기법이다. 가장 큰 특징으로는 웹 페이지에 필요한 부분에 필요한 데이터만 비동기적으로 받아와 화면에 보여줄 수 있다는 것이다.



### 핵심 기술

Fetch : 페이지를 이동하지 않아도 서버로부터 필요한 데이터를 받아올 수 있고, 비동기적인 방식으로 동작하기 때문에 요청을 하는 동안에도 사용자는 페이지를 사용할 수 있다.

JS, DOM : Fetch로 받아온 필요한 데이터를 DOM에 적용시켜 새로운 페이지로 이동하지 않고 가용할 수 있다.



### 장점

- 서버에서 HTML을 완성된 문서로 전송하지 않아도 필요한 데이터를 클라이언트 측에서 요청하여 화면 일부만 업데이트하여 렌더링 할 수 있다.
- XHR이 표준화 되면서 브라우저에 상관없이 사용할 수 있다.
- 유저 중심 어플리케이션 개발 AJAX를 사용하면 필요한 부분만 렌더링하기 때문에 빠르고 더 사용자 친화적인 어플리케이션을 만들 수 있다.
- HTML 파일 대신 필요한 데이터를 텍스트 형태(JSON, XML 등)로 받아오면 되기 때문에 비교적 데이터 크기가 작다.



### 단점

- 검색 엔진은 HTML 파일 정보를 수집하여 검색 결과를 보여주기 때문에 HTML에는 틀만 있는 AJAX에서는 Search Engine Optimization(SEO)에 불리하다.
- AJAX에서는 이전 상태를 기억하지 않기 때문에 뒤로 가기 버튼이 동작하지 않는다.
  - 이를 위해서는 History API를 사용해야 한다.



## SSR vs CSR



### SSR

SSR : Server Side Rendering, 서버는 필요한 데이터를 불러온 다음 완전히 완성된 웹 페이지 파일을 렌더링한다.

- SEO가 우선순위일 경우 사용한다.
- 웹 페이지의 첫 화면 렌더링이 빠르다.
- 단일 파일의 용량이 적을 경우 적합하다.
- 웹 페이지가 사용자와 상호작용이 적은 경우 적합하다.



### CSR

CSR : Client Side Rendering, 서버는 웹 페이지 틀과 JS파일을 보내고 이를 활용하여 클라이언트에서는 API, JS를 통해 데이터를 가져와서 완전한 페이지를 구성한다.

- SEO가 우선순위가 아닌 경우 활용한다.
- 사이트에 많은 상호 작용이 요구되는 경우, 사용한다.
- 웹 애플리케이션을 제작하는 경우, 빠른 동적 렌더링을 제공할 수 있다.



## CORS

CORS :   Cross-Origin Resource Sharing, SOP(Same-Origin Policy. : 동일 출처 정책)을 잠시 풀어주어 다른 출처간에 리소스를 공유할 수 있도록 하는 것이다.

- 출처 : Protocol, Host, Port 의 URL
- 리소스 : 요청 데이터
- SOP : 동일한 출처끼리만 데이터를 주고 받게 함으로써 사용자의 웹 브라우저 상에 있는 정보를 지키 도록 한다.



### CORS 표준

---------------------------------

- Fetch API와 XMLHttpRequest
- Web CSS
- WebGL texture
- drawImage
- CSS Shapes from images

는 CORS를 허용한다.



### CORS 접근 제어 시나리오

---------------------------------

#### Simple Requests

- Method
  - GET, HEAD, POST
- 유저 에이전트가 자동으로 설정한 헤더
- Content-Type
  - Application/x-www-form-urlencoded
  - Multipart/form-data
  - Text/plain
- XMLHttpRequestUpload 객체에 이벤트 리스너 등록 X
  - 접근하려면 XMLHttpRequest.upload 사용
- ReadableStream 객체 X
- Ex.

```js
const xhr = new XMLHttpRequest();
const url = 'https://bar.other/resources/public-data/';

xhr.open('GET', url);
xhr.onreadystatechange = someHandler;
xhr.send();
```

조건 하에 서버가 "Access-Control-Allow-Origin" 헤더에 *(모두 허용)이나 요청 url을 반환하면 CORS 허용



#### Preflighted Request

- simple requests와 달리, cross-origin 요청은 사용자 데이터에 영향을 줄 수 있기 때문에 먼저 OPTIONS 메소드를 통해 다른 도메인 리소스로 HTTP 요청을 보내 실제 요청이 안전한지 확인(미리 전송)
- Ex.

```js
const xhr = new XMLHttpRequest();
xhr.open('POST', 'https://bar.other/resources/post-here/');
xhr.setRequestHeader('Ping-Other', 'pingpong'); // 요청 헤더 설정
xhr.setRequestHeader('Content-Type', 'application/xml'); // 사용자 정의 헤더 설정으로 preflighted
xhr.onreadystatechange = handler;
xhr.send('<person><name>Arun</name></person>');
```

- Request에 따라 Response로 허가가 된다.
  - Request
    - Origin : 요청 출처
    - Access-Control-Request-Method : 요청 메서드
    - Access-Control-Request-Headers : 요청 헤더
  - Response
    - Access-Control-Allow-Origin : 허가 출처
    - Access-Control-Allow-Methods : 허가 메서드
    - Access-Control-Allow-Headers : 허가 헤더
    - Access-Control-Max-Age : 허가 유효 시간



#### Requests with credential

- HTTP cookies와 HTTP Authentication 정보를 인식하여 CORS 허용
  - 일반적으로 XMLHttpRequest나 Fetch에서 브라우저는 자격증명을 보내지 않기 때문에 특정 플래그 설정
- Ex.

```js
const invocation = new XMLHttpRequest();
const url = 'http://bar.other/resources/credentialed-content/';

function callOtherDomain() {
  if (invocation) {
    invocation.open('GET', url, true);
    invocation.withCredentials = true; //쿠키와 함께 호출
    invocation.onreadystatechange = handler;
    invocation.send();
  }
}
```

- Preflighted 가 아닌 simple request로 응답에 Access-Control-Allow-Credentials가 true가 아닐 경우 CORS 허용 되지 않음
- 또한, 요청 헤더에 쿠키가 포함되기 때문에 Access-Control-Allow-Origin의 경우 *이 아닌 요청 URL로 지정해야 한다.





