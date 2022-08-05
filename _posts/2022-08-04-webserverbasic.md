---
title: "[Web Server] 기초"
categories:
 - BEB
tags: [NodeJs, Express, HTTP Server, HTTP Transaction] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---



# http 모듈을 활용한 웹 서버 구축

-------------------------



## HTTP 트랜잭션 해부



### 서버 생성

-------------------------

```js
const http = require('http')

const server = http.createServer((request,response) => {
	// 서버 작업
  // request = 클라이언트 -> 서버 요청
  // response = 서버 -> 클라이언트 응답
})
```

위 코드와 같이 http모듈의 createServer로 NodeJs를 통해 서버를 구축할 수 있다.



### Request

-------------------------



#### Method, URL, Headers

NodeJs에서 request안에 객체로 method, url, headers의 프로퍼티는 기본적으로 제공하기 때문에 이를 통해 핸들링이 가능하다.

```js
const http = require('http')
//const {method, url, headers} = request

const server = http.createServer((request,response) => {
	console.log(request.method) // -> ex. post
  console.log(request.url) // -> /post
  console.log(request.headers)
  // ->
  //host: 'localhost:4999',
  //connection: 'keep-alive',
  //accept: '*/*',
  //'access-control-request-method': 'POST',
  //'access-control-request-headers': 'content-type',
  //origin: 'null',
  // 'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36',
  //'sec-fetch-mode': 'cors',
  //'sec-fetch-site': 'cross-site',
  //'sec-fetch-dest': 'empty',
  //'accept-encoding': 'gzip, deflate, br',
  //'accept-language': 'ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7'
})
```



#### Body

POST나 PUT 요청을 받을 때, body로의 접근은 'data'와 'end' 이벤트의 이벤트 리스너에 등록해서 데이터 접근이 가능하다.

```js
const http = require('http')
let body = [];

const server = http.createServer((request,response) => {
	request.on('data', (chunk) => { //Buffer
    body.push(chunk);
  }).on('end' () => {
    body = Buffer.concat(body).toString() //String
  })
})
```



#### Error + Status Code

Error 역시, 이벤트 및 이벤트 리스너를 제공한다.

```js
const http = require('http')

const server = http.createServer((request,response) => {
	request.on('error'(err) => { // ERROR
    if(err) { // Ex, 에러 발생시 에러에 따라 상태 코드 적용 가능
     		response.statuscode = 404;
    		response.end() 
    }
  }).on('data', (chunk) => { //Buffer
    body.push(chunk);
  }).on('end' () => {
    body = Buffer.concat(body).toString() //String
  })
})
```



### Response

-------------------------



#### Header

header에서는 대소문자는 중요하지 않으며, 헤더를 여러 번 설정한다면 마지막에 설정한 값을 보낸다. 헤더 설정은 setHeader나 명시적인 header를 작성하고 싶다면 writeHead를 통해 설정한다.

```js
const http = require('http')

const setHeader = {
	'content-type', 'application/json',
  'x-Powered-By', 'bacon'
}

const server = http.createServer((request,response) => {
	response.setHeader('content-type', 'application/json')
  response.setHeader('x-Powered-By', 'bacon')
  // ===
  response.writeHead(200, setHeader)
  response.end();
})
```



#### Body

일반적인 스트림 메서드를 이용해서 작성한다.

```js
const http = require('http')

const setBody = {
	'<html><body><h1>Hello, World!</h1></body></html>'
}

const server = http.createServer((request,response) => {
	response.write(setBody)
  response.end();
  // === 
  response.end('<html><body><h1>Hello, World!</h1></body></html>');
})
```



### Routing

라우팅은 위의 과정을 활용하여 if문으로 라우팅 가능하다.

```js
const http = require('http');

const server = http.createServer((request, response) => {
  if (request.method === 'POST' && request.url === '/echo') { // Routing
    request.pipe(response); //request는 Readable, response는 Writable로 스트림끼리 직접 연결하는 파이프 또한 사용가능하다.
  } else {
    response.statusCode = 404;
    response.end();
  }
}).listen(8080);

```



[http 모듈을 활용한 웹 서버 예제 + Cors(preflight)](https://github.com/apfl99/im-sprint-mini-node-server)

<br>

# Express을 활용한 웹 서버 구축

---------------------------

Express는 HTTP 모듈과 다르게 미들웨어 추가가 편리하고, 자체 라우터를 제공하기 때문에 구현하기 쉬워 많이 사용된다.




## 미들웨어

미들웨어는 말 그대로 프로세스 중간에 관여하여 특정 역할을 수행하는 역할을 한다.

![스크린샷 2022-08-04 오후 4.57.56](../../images/2022-08-04-webserverbasic/스크린샷 2022-08-04 오후 4.57.56.png)

{: .align-center}



미들 웨어는 위 그림과 같이 구성되어 있으며, 미들웨어 함수를 사용할 때에는 요청-응답 주기를 종료하지 않으면 next() 다음 미들웨어 함수에 제어를 전달하기 위해 호출해야 한다.



 또한, 미들 웨어는 주로 

1. 모든 요청에 대해 url이나 메소드를 확인할때

2. POST 요청 등에 포함된 body(payload)를 구조화할 때

3. 모든 요청/응답에 CORS 헤더를 붙여야 할 때

4. 요청 헤더에 사용자 인증 정보가 담겨있는지 확인 할 때

사용한다.

이를 활용하면 NodeJs만으로 구현한 서버보다 쉽게 적용가능하다.



### 1. 모든 요청에 대해 url이나 메소드를 확인할 때

---------------------------

```js
const express = require('express')
const app = express()

const myLogger = function (req,res,next) {
console.log(
      `http request method is ${req.method}, url is ${req.url}`
    );
  next(); //next로 넘겨주지 않으면 요청이 올 때 다른 함수로 넘어가지 못하고 이 함수에 머무르게 된다.
}

app.use(myLogger) // 모든 요청에 대해 적용

app.listen(3000);
```



### 2. POST 요청 등에 포함된 body(payload)를 구조화할 때

---------------------------

```js
const express = require('express')
const app = express()

app.use(express.json([options]))
//content-type이 json인 것만 구문 분석하고 헤더가 일치하는 요청만 보게 한다.
//[option]을 통해 요청을 설정할 수 있다.

app.listen(3000);
```

[express.json Options](https://expressjs.com/en/api.html#express.json)



### 3. 모든 요청/응답에 CORS 헤더를 붙일 때

---------------------------

```js
const express = require('express')
const app = express()
const cors = require('cors')

//모든 요청에 대한 CORS
app.use(cors())

//특정 요청에 대한 CORS
app.get('/',cors(),function(req,res,next) {
  res.json({msg : "CORS"})
})

app.listen(3000);
```



### 4. 요청 헤더에 사용자 인증 정보가 담겨있는지 확인할 때

---------------------------

```js
const express = require('express')
const cookieParser = require('cookie-parser')
const cookieValidator = require('./cookieValidator')

const app = express()

async function validateCookies (req, res, next) {
  await cookieValidator(req.cookies) //쿠키 정보 확인
  next()
}

// 요청에 대한 쿠키 파싱 및 검증
app.use(cookieParser())
app.use(validateCookies)

// error handler
app.use((err, req, res, next) => {
  res.status(400).send(err.message)
})

app.listen(3000)
```

[Express을 활용한 웹 서버 예제 + Cors(preflight)](https://github.com/apfl99/im-sprint-mini-express-server)

[Express를 활용한 비행기예약/조회 시스템 예제](https://github.com/apfl99/im-sprint-statesairline-server)





<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
  <a>https://nodejs.org/ko/docs/guides/anatomy-of-an-http-transaction/</a>
  <br>
  <a>https://expressjs.com/en/guide/routing.html</a>
  <br>
  <a>https://expressjs.com/en/guide/writing-middleware.html</a>
  <br>
  <a>https://expressjs.com/en/api.html#express.json</a>
  <br>
</div>

