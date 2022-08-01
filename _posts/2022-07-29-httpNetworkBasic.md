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



# 브라우저 작동 원리 1



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
- 이미 정해진 포트 번호라도, 필요에 따라 자유롭게 사용가능, 알려지지 않은 포트의 경우 명시



## 도메인과 DNS

Domain name : IP 주소는 ~.~.~.~와 같이 숫자로 이루어져 있기 때문에 사용에 불편이 있다. 따라서 이러한 IP 주소를 google.com과 같이 사용하기 편한 주소로 변환된 주소가 도메인이다.



DNS : Domain Name System의 줄임말로, 호스트의 도메인 이름을 IP 주소로 변환하거나 반대의 경우를 수행할 수 있도록 개발된 DBS이다.



# HTTP

HTTP : HyperText Transfer Protocol, HTML과 같은 문서를 전송하기 위한 응용계층 프로토콜으로, 특정 상태를 유지하지 않는 무상태성(Stateless)를 특징으로 갖는다. 즉, 통신 과정에서 서버나 클라이언트의 상태를 확인하지 않는다.



## HTTP Message

![스크린샷 2022-08-01 오후 6.17.10](../../images/2022-07-29-httpNetworkBasic/스크린샷 2022-08-01 오후 6.17.10.png)

{: .align-center}

HTTP Message는 위와 같은 형식으로 완성되며, Request와 Response의 형식이 좀 다르다.



### Request

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
    - General Headers : 메시지 전체에 적용
    - Request Headers : User-Agent, Accept-Type, Accept-Language과 같은 헤더는 요청을 보다 구체화한다. referer처럼 컨텍스트를 제공하거난 If-None과 같이 조건에 따라 제약을 추가할 수 있다.
    - Entity Headers : Content-Length와 같은 헤더는 body에 적용된다. 따라서 body가 비어있는 경우 전송되지 않는다.
  - Body : message 구조 마지막에 위치하며, GET, HEAD, DELETE, OPTIONS와 같이 서버에 리소스를 요청하는 경우에는 필요없다.
    - 구분
      - single-resource bodies : 헤더 두개(Content-Type과 Content-Length)로 정의된 단일 파일로 구성
      - Multiple-resource bodies : 여러 파트로 구성된 본문에서는 각 파트마다 다른 정보를 지닌다.(HTML Form)

