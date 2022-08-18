---
title: "[인증/보안] 기초"
categories:
 - BEB
tags: [HTTPS, Hashing, Cookie, Session, Token, OAuth] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# HTTPS

-------------

HTTPS란 Hyper Text Transfer Protocol Secure Socket Layer의 약자로, HTTP 요청을 SSL 혹은 TLS로 암호화하여 데이터를 전송하는 프로토콜이다. 이는 네트워크 통신을 조작하여 통신 내용을 도청하거나 조작하는 중간자 공격(MITM)을 방지할 수 있다.



## HTTPS 특징



### 암호화

--------

기존의 HTTP 프로토콜은 요청 및 응답 패킷을 중간에 탈취한다면 전송되는 데이터의 내용을 확인 가능하다. 그러나 HTTPS의 경우 데이터를 암호화하여 전송하기 때문에 전송되는 데이터가 노출되더라도 데이터가 유출될 가능성이 HTTP 프로토콜에 비해 현저히 적다.

- 비대칭키 암호화 사용
- 키 생성(1,2 : handshake, 3,4 : 비밀키 생성, 5,6 : 상호키 검증) - 초기 연결 단계
  1. 클라이언트 -> 서버 : 접속
  2. 서버 -> 클라이언트  : 공개키, 인증서 정보 전달
  3. 클라이언트 -> 서버 : 키 제작용 랜덤 스트링 전송
  4. 서버 -> 클라이언트 : 키 제작용 랜덤 스트링 전송
  5. 클라이언트 -> 서버 : 세션 키로 암호화 된 메시지 전달
  6. 서버 -> 클라이언트 : 세션 키로 암호화 된 메시지 전달



### 인증서

--------

HTTPS 프로토콜에서 브라우저는 응답과 함께 전달된 인증서 정보를 확인할 수 있다. 이것을 활용하여, 브라우저는 해당 인증서를 발급한 CA(Cretificate Authority, 인증기관) 정보를 확인하고, 인증서 도메인과 데이터 제공자 도메인을 검증할 수 있기 때문에 사용자가 검증된 서버를 사용할 수 있게 한다.



## 사설 인증서 발급 및 HTTPS 서버

[사설 인증서 발급 - mkcert](https://github.com/FiloSottile/mkcert)

위 과정을 거치면 key.pem과 cert.pem가 생성되는데, cert는 인증기관 서명, key는 발급받은 개인키로 사용된다.

이러한 사설 인증을 통해, express의 경우, createServer 두번째 파라미터의 콜백 함수를 Express 미들웨어로 사용할 수 있다.

```js
const https = require('https')
const fs = require('fs')
const express = require('express')

const app = express()

https
    .createServer(
        {
            key: fs.readFileSync(__dirname + '/key.pem','utf-8'),
            cert: fs.readFileSync(__dirname + '/cert.pem','utf-8'),    
        },
        app.use('/',(req,res) => {
            res.send('Congrats! You made https server now :')
        })
    )
    .listen(3001)
```

![img1](../../images/2022-08-17-authsecurity/img1.png)

{: .align-center}



# Hashing

--------------

Hashing이란 어떠한 문자열에 '임의의 연산'을 적용하여 다른 문자열로 변환하는 것으로, 이러한 과정은 데이터의 유출을 막는 암호화와 달리 데이터 변조, 데이터 무결성을 검증하는데 사용된다.

Rule

1. 모든 값에 대해 해시 값을 계산하는데 오래걸리지 않아야 한다.
2. 최대한 해시 값을 피해야 하며, 모든 값은 고유한 해시 값을 가진다.
3. 아주 작은 단위의 변경이라도 완전히 다른 해시 값을 가져야 한다.

Ex. SHA1, SHA256, ...



## Salt

암호화해야 하는 값에 어떤 '별도의 값'을 추가하여 결과를 변형하는 것

1. 암호화만 해놓는다면 해시된 결과가 늘 동일 - 같은 값을 해시하면 같은 해시값을 반환
   - 이 경우 해시된 값과 원래 값을 대조할 수 있는 테이블(레인보우 테이블)로 만들어서 Decoding 가능

2. 원본값에 임의로 약속된 '별도의 문자열'을 추가하여 해시를 진행한다면 기존 해시값과 전혀 다른 해시값이 반환되어 해시 알고리즘이 노출되더라도 원본값을 보호

주의할 점

1. Salt는 유저와 패스워드 별로 유일한 값을 가져야 한다.
2. Salt는 재사용하면 안된다.
3. Salt는 DB의 유저 테이블에 같이 저장되어야 한다.(복호화)



# Cookie

-----------

Cookie란 서버에서 클라이언트에 데이터를 저장하는 방법 중 하나로, 이를 통해 클라이언트의 상태를 기록하여 상태 유지가 가능하다. 하지만 기본적으로 쿠키는 오랜 시간 유지될 수 있고, JS를 통해 쿠키로 접근 가능하기 때문에, 민감한 정보를 담지는 않는다. 만약 쿠키가 탈취될 경우, 클라이언트 상태를 악의적인 사용자가 활용한다면, 클라이언트의 민감한 정보에 접근 가능하다. 따라서 서버 또한, 특정 조건을 만족하는 경우에만 쿠키를 가져올 수 있다.



## Cookie Options



### Domain

------

도메인은 www.google.com과 같은 서버 이름으로, 클라이언트에서는 쿠키의 도메인 옵션과 서버의 도메인이 일치해야만 쿠키를 전송할 수 있다.

- Ex. Set-Cookie: [cookie-name]=[cookie-value]; Domain='example.com'
  - example.com (O)
  - Exam.com(X)



### Path

--------

Path는 /users과 같은 세부 경로로, 도메인과 마찬가지로 설정된 Path일 경우에 쿠키 전송이 가능하다. Path는 이하 Path 모두 허용한다.

- Ex. Set-Cookie: [cookie-name]=[cookie-value]; Path='/users'
  - /users/login (O)
  - /user (X)



### MaxAge or Expires

--------

MaxAge나 Expires는 쿠키가 유효한 시간을 정하는 옵션으로, 해당 옵션을 지정하지 않을 경우, 브라우저 탭을 닫아야만 쿠키가 제거된다.

- Ex. Set-Cookie: [cookie-name]=[cookie-value]; Maxage=24 * 6 * 60 * 10000



### Secure

--------

Secure는 쿠키를 전송해야 할 때 사용하는 프로토콜에 따른 쿠키 전송 여부를 결정하는 옵션으로, true인 경우 https프로토콜에서만 쿠키를 전송할 수 있다.

- Ex. Set-Cookie: [cookie-name]=[cookie-value]; Secure=true



### HttpOnly

--------

HttpOnly는 JS가 브라우저 쿠키에 접근 여부를 결정하는 옵션으로, true일 경우 JS로 쿠키에 접근이 불가능하다. 기본값은 false이기 때문에, JS를 통해 접근이 가능하지만, 이경우 JS 구문을 통해 cookie 탈취를 하는 공격에 취약하다.

- Ex. Set-Cookie: [cookie-name]=[cookie-value]; HttpOnly=true



### SameSite

--------

SameSite의 경우 Cross-Origin 요청을 받은 경우 요청에서 사용한 메소드와 해당 옵션의 조합으로 서버의 쿠키 전송 여부를 결정한다.

- Ex. Set-Cookie: [cookie-name]=[cookie-value]; SameSite=Lax : cross-origin에서 GET메소드만 허용
- Ex. Set-Cookie: [cookie-name]=[cookie-value]; SameSite=Strict : same-site만 허용
- Ex. Set-Cookie: [cookie-name]=[cookie-value]; SameSite=None : 모두 허용이지만 Secure 옵션 필요



# Session

---------

Session이란 서버가 Client에 유일하고 암호화된 ID를 부여하는 것으로, 서버에 저장되고, client에는 cookie를 통해 전송하여 Client를 확인한다.(인증)



## Session vs Cookie



### Cookie

------------

- 저장 경로 : 클라이언트
- 장점 : 서버에 부담 감소
- 단점 : 쿠키 자체는 상태를 기록, 인증은 아님



### Session

------------

- 저장 경로 : 서버
- 장점 : 신뢰할 수 있는 유저인지 검증 가능
- 단점 : 하나의 서버에서만 접속 상태를 가지므로 분산에 분리
- 한계 : 쿠키를 통해 인증하기 때문에 여전히 탈취의 문제가 존재, 서버 메모리를 사용하기 때문에 성능 저하 문제



## Express Session를 활용한 세션 검증

```js
const express = require('express');
const cors = require('cors');
const session = require('express-session');
const logger = require('morgan');
const fs = require('fs');
const https = require('https');
const usersRouter = require('./routes/user');

const app = express();


const PORT = process.env.PORT || 4000;

//express-session 라이브러리를 이용해 쿠키 설정
app.use(
  session({
    resave: false,
    saveUninitialized: true,
    cookie: {
      domain: 'localhost',
      path: '/',
      maxAge: 24 * 6 * 60 * 10000,
      sameSite: 'None',
      httpOnly: true,
      secure: true,
    },
  })
);
app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

//Logging
const myLogger = function (req,res,next) {
  console.log(`http request method is ${req.method}, url is ${req.url}`);
  next(); // 안넣어주면 로그 이후로 안넘어감
}
app.use(myLogger)

//cors
app.use(cors({
  origin: '*',
  methods: ['GET','POST','OPTIONS']
}));

app.use('/users', usersRouter);

let server;

// 인증서 파일들이 존재하는 경우에만 https 프로토콜을 사용하는 서버를 실행
// 만약 인증서 파일이 존재하지 않는경우, http 프로토콜을 사용하는 서버를 실행
if (fs.existsSync("./key.pem") && fs.existsSync("./cert.pem")) {
  server = https
    .createServer(
      {
        key: fs.readFileSync(__dirname + `/` + 'key.pem', 'utf-8'),
        cert: fs.readFileSync(__dirname + `/` + 'cert.pem', 'utf-8'),
      },
      app
    )
    .listen(PORT);
} else {
  server = app.listen(PORT)
}
module.exports = server;

```

위와 같은 과정을 활용하면 서버에서는 req.session을 통해 세션 객체에 대한 정보에 대한 접근이 가능하고, 클라이언트에서는 서버로부터 부여된 sessionId를 set-cookie에 담아 서버가 어떤 클라이언트인지 검증이 가능하다.

[Session 예제](https://github.com/apfl99/im-sprint-auth-session)



# Token

-----------

Session을 사용하여 사용자 인증을 했을 때에도, 여전히 sessionId를 담고 있는 쿠키가 탈취될 경우, 악의적인 해커가 해당 사용자인 척 서버에 요청을 하면 사용자의 민감한 정보에 접근가능하고, 서버에 저장되기 때문에 부하 및 분산 문제가 있었다. 이러한 문제점을 해결하기 위해, 유저 정보를 암호화한 상태로 클라이언트에 담는 토큰을 사용한다.



## JWT

JWT란 Json Web Token의 약자로, Json 포맷으로 사용자에 대한 속성을 저장하는 가장 범용적인 웹 토큰이다.



### JWT 구조

---------



#### Header

Header에는 어떤 종류의 토큰인지, 어떤 알고리즘으로 암호화할 지 적어 base64로 인코딩하여 저장한다.

Ex.

```js
{
  "algorithm": "HS256",
  "type": "JWT"
}
```



#### Payload

Payload에는 정보가 담겨있으며, 정보에 대한 권한 또한 담아 암호화 시킬 수 있다. 암호화가 될 정보지만, 민감한 정보는 담지 않는 것이 좋다.(마찬가지로 base64로 인코딩하여 저장한다.)

Ex.

```js
{
  "sub": "someInfo",
  "name": "kim",
  "iat": 123123123
}
```



#### Signature

원하는 비밀키를 사용하여 암호화한다.

Ex.

```js
HMACSHA256(base64UrlEncode(header) + '.'+ base64UrlEncode(payload), secret);
```



### JWT 종류

--------------



#### Access Token

Access Token은 사용자 정보에 접근할 수 있는 권한 부여에 사용되며, 클라이언트가 처음 인증을 받게 될 때, Access Token과 Refresh Token 두 가지를 받지만, 실제로 권한을 얻는 데 사용하는 토큰은 Access Token이다. 



#### Refresh Token

실제 권한을 부여 받는 데에는 Access Token이 사용되지만, Access Token이 탈취당했을 경우를 대비하여 Access Token의 유효기간을 짧게 두고, Refresh Token을 활용하여 다시 Access Token을 받게 한다.



## Token 인증 절차

1. 클라이언트 -> 서버 : 로그인 요청
2. 서버 : 로그인 유효성 검사 -> 토큰 생성(Access Token, Refresh Token)
3. 서버 -> 클라이언트 : 토큰 전송
4. 클라이언트 : 토큰 저장 (local storage, cookie, state)
5. 클라이언트 -> 서버 : HTTP 헤더(authorization)에 토큰을 담아 보낸다.
6. 서버 : 토큰 검증 후 유효할 경우, 클라이언트가 요청한 응답을 보낸다.



## Token 인증 장점

1. 무상태성 & 확장성
   - 서버는 클라이언트에 대한 정보를 저장할 필요 X
   - 클라이언트가 토큰을 가지고 있기 때문에 여러 서버에서 사용 가능
2. 안전
   - 암호화한 토큰을 사용하고, 암호화 키는 서버에만 저장되기 때문에(통신되지 않기 때문에) 안전
3. 어디서나 생성 가능
   - 토큰 생성용 서버가 필요하지는 않음
4. 권한 부여 용이
   - 토큰의 payload에 정보에 대한 권한 부여를 할 수 있기 때문에 데이터 항목별 접근 권한 제어가 용이하다.



## JWT를 활용한 토큰 검증



### 토큰 생성

----------

```js
const { Users } = require("../../models");
const jwt = require('jsonwebtoken')

module.exports = async (req, res) => {

    //DB 조회
    const data = await Users.findOne({
        where: { userId: req.body.userId, password: req.body.password },
    });


    //res
    if(data === null) {
        res.json({message: "not authorized"})
    } else {
        var result = data.dataValues;
        delete result.password // 패스워드 제외 : 민감한 정보
        //jwt
        var accessToken = jwt.sign( // Generate AccessToken
            result,
            process.env.ACCESS_SECRET,{
            expiresIn: '5m' // option : 유효시간
        });
        var refreshToken = jwt.sign( // Generate RefreshToken
            result,
            process.env.REFRESH_SECRET, {
            expiresIn: '5m'
            }
        )


        res.cookie('refreshToken',refreshToken) // cookie에 refresh 토큰을 담고 

        return res.json({message: "ok", data : {accessToken : accessToken}}) //body에 accessToken을 담아 전송
    }

};
```



### 토큰 검증 및 데이터 전송

---------

```js
const { Users } = require('../../models');
const jwt = require('jsonwebtoken')

module.exports = (req, res) => {
  
  if(req.headers.authorization) { // 헤더 정보 있을 때
    const token = req.headers.authorization.replace(/^Bearer\s+/, ""); // token만 추출

    jwt.verify(token, process.env.ACCESS_SECRET, (err, decoded)=> { // Decoding AccessToken
      if(err) {
        return res.json({data: null, message: "invalid access token"})
      } else {
        delete decoded.iat
        return res.send({data: {userInfo: decoded}, message: "ok"}) // Decoding된 데이터 전송
      }
    })
  }
  else {
    return res.json({data: null, message: "invalid access token"})
  }

};
```





### 토큰 재생성 및 데이터 전송

---------

```js
const { Users } = require('../../models');
const jwt = require('jsonwebtoken')

module.exports = (req, res) => {
  
  if(req.cookies.refreshToken) { // 쿠키 정보 있을 때
    const token = req.cookies.refreshToken
    jwt.verify(token,process.env.REFRESH_SECRET, (err,decoded) => { // Decoding RefreshToken
      if(err) {
        return res.json({data: null, message: "invalid refresh token, please log in again"})
      } else {
        delete decoded.iat
        var result = decoded;

        var accessToken = jwt.sign( // Generate AccessToken
          result,
          process.env.ACCESS_SECRET,{
          expiresIn: '5m'
        });

        return res.send({data: {accessToken: accessToken,userInfo: decoded}, message: "ok"}) //body에 accessToken을 담아 전송
      }
    })
  } else {
    
    return res.json({data: null, message: "refresh token not provided"})
  }

};

```

[Token 예제](https://github.com/apfl99/im-sprint-auth-token)

<br>

# OAuth

-------

OAuth란 기존의 서버에서 인증처리를 해주는 것과 달리, 인증을 중개해주는 매커니즘이다. 즉 이미 사용자 정보를 가지고 있는 웹 서비스(google, github, facebook 등)에서 사용자 인증을 대신해주고, 접근 권한에 대한 토큰을 발급한 후, 이를 이용해 서버에서 인증을 함으로써 클라이언트에게 권한을 제공해준다. 이를 통해 사용자는 편리하게 로그인할 수 있을 뿐더러, 유저의 민감한 정보가 App에 노출될 일이 없고 인증 권한에 대한 허가를 미리 유저에게 구해야 하기 때문에 안전하다.



## OAuth 용어

- Resource Owner : 액세스 중인 리소스의 유저, 기존의 client

- Client : Resource owner 대신 보호된 리소스에 액세스하는 응용프로그램

- Resource Server : Client의 요청을 수락하고 응답할 수 있는 서버

- Authorization Server : Resource Server가 엑세스 토큰을 발급받는 서버
- Authorization Grant : 클라이언트가 엑세스 토큰을 얻릉 때 사용하는 자격 증명의 유형
- Authorization Code : Access Token을 발급받기 위해 필요한 code, client secret과 code를 통해 Access Token을 생성
- Access Token : 보호된 리소스에 엑세스하는 데 사용되는 토큰
- Scope : 토큰의 권한을 정의, 주어진 엑세스 토큰을 사용하여 엑세스할 수 있는 리소스의 범위



## OAuth 절차

1. Resource Owner -> Client : 접근
2. Client -> Resource Owner : Client가 Resource Owner를 Authorization Server로 리다이렉트
3. Resource Owner -> Authorization Server : 엑세스 권한 부여 요청
4. Authorization Server-> Client : Authorization Code 제공
5. Client -> Authorization Server : Authorization Code를 Access Token으로 교환
6. Client -> Resource Server : Access Token으로 Resource에 엑세스
7. Client -> Resource Server : 엑세스 토큰과 함께 API 요청
8. Resource Server -> Client : 리소스 전달
9. Client -> Resource Owner : 리소스 제공



## OAuth with Github

[Github OAuth App 등록](https://www.oauth.com/oauth2-servers/accessing-data/create-an-application/)



### Authorization Code 발급 요청

----------

```js
import React, { Component } from 'react';

class Login extends Component {
  constructor(props) {
    super(props)
    this.socialLoginHandler = this.socialLoginHandler.bind(this)
    // OAuth 인증이 완료되면 authorization code와 함께 callback url로 리디렉션

    //인증 요청 URL client_id
    this.GITHUB_LOGIN_URL = "https://github.com/login/oauth/authorize?client_id=a93aeebd1425b31c6a53"
  }

  socialLoginHandler() {
    window.location.assign(this.GITHUB_LOGIN_URL) // 로그인 버튼 클릭 시 인증 확인 창 -> 인증 시 URL param에 
  }

  render() {
    return (
      <div className='loginContainer'>
        OAuth 2.0으로 소셜 로그인을 구현해보세요.
        <img id="logo" alt="logo" src="https://image.flaticon.com/icons/png/512/25/25231.png" />
        <button
          onClick={this.socialLoginHandler}
          className='socialloginBtn'
        >
          Github으로 로그인
          </button>
      </div>
    );
  }
}

export default Login;

```



### Access Token 발급 요청

----------

```js
import React, { Component } from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import Login from './components/Login';
import Mypage from './components/Mypage';
import axios from 'axios';
class App extends Component {
  constructor() {
    super();
    this.state = { // 로그인 여부, accessToken 상태 저장
      isLogin: false,
      accessToken: ""
    };
    this.getAccessToken = this.getAccessToken.bind(this);
  }

  async getAccessToken(authorizationCode) {
    // 받아온 authorization code로 다시 OAuth App에 요청해서 access token을 받기 
    // 서버의 /callback 엔드포인트로 authorization code를 보내주고 access token을 요청
    await axios({
      url: "http://localhost:8080/callback",
      method: "post",
      data: {
        authorizationCode: authorizationCode
      }
    }).then((response) => {
      this.setState({ // 상태 값 업데이트 
        isLogin: true, // 로그인 여부 true
        accessToken: response.data.accessToken // accessToken
      })
    })

  }

  componentDidMount() {
    const url = new URL(window.location.href)
    const authorizationCode = url.searchParams.get('code') // url param 받아 오기
    if (authorizationCode) {
      // authorization server로부터 클라이언트로 리디렉션된 경우, authorization code가 함께 전달
      // ex) http://localhost:3000/?code=5e52fb85d6a1ed46a51f
      this.getAccessToken(authorizationCode)
    }
  }

  render() {
    const { isLogin, accessToken } = this.state;
    return (
      <Router>
        <div className='App'>
          {isLogin ? (
            <Mypage accessToken={accessToken} />
          ) : (
              <Login />
            )}
        </div>
      </Router>
    );
  }
}

export default App;

```



### Access Token 발급 응답

----------

```js
require('dotenv').config();

const clientID = process.env.GITHUB_CLIENT_ID;
const clientSecret = process.env.GITHUB_CLIENT_SECRET;
const axios = require('axios');

module.exports = (req, res) => {
  // console.log(req.body.authorizationCode);

  // authorization code를 이용해 access token을 발급받기 위한 post 요청
  // https://docs.github.com/en/free-pro-team@latest/developers/apps/identifying-and-authorizing-users-for-github-apps#2-users-are-redirected-back-to-your-site-by-github
  axios({
    method: 'post',
    url: "https://github.com/login/oauth/access_token",
    headers: { 'accept': 'application/json' }, //json형식으로 응답을 받겠슴다.
    data: {
      client_id: clientID,
      client_secret: clientSecret,
      code: req.body.authorizationCode // code를 포함한 요청 
    }
  }).then((response) => {
      return res.status(200).json({accessToken : response.data.access_token}) // accessToken 응답
  })
  


}

```



### Resource 요청

----------

```js
import React, { Component } from "react";
import axios from 'axios';
const { Octokit } = require("@octokit/core");

class Mypage extends Component {

  constructor(props) {
    super(props);
    this.state = { // github로부터 받아올 상태 값
      images: [],
      name: "",
      login: "",
      url: "",
      public_repos: 0
    }
  }

  async getGitHubUserInfo() {
    // GitHub API를 통해 사용자 정보 받기
    // https://docs.github.com/en/free-pro-team@latest/rest/reference/users#get-the-authenticated-user
    const octokit = new Octokit({
      auth: this.props.accessToken // accessToken을 req에 넣어서
    })
    
   const userInfo = await octokit.request('GET /user', {})
   this.setState({
     name: userInfo.data.name,
     login: userInfo.data.login,
     url: userInfo.data.html_url,
     public_repos: userInfo.data.public_repos
   })

  }

  componentDidMount() {
    this.getGitHubUserInfo()
    this.getImages()
  }

  render() {
    const { accessToken } = this.props

    if (!accessToken) {
      return <div>로그인이 필요합니다</div>
    }

    return (
      <div>
        <div className='mypageContainer'>
          <h3>Mypage</h3>
          <hr />

          <div>안녕하세요. <span className="name" id="name">{this.state.name}</span>님! GitHub 로그인이 완료되었습니다.</div>
          <div>
            <div className="item">
              나의 로그인 아이디:
              <span id="login">{this.state.login}</span>
            </div>
            <div className="item">
              나의 GitHub 주소:
              <span id="html_url">{this.state.url}</span>
            </div>
            <div className="item">
              나의 public 레포지토리 개수:
              <span id="public_repos">{this.state.public_repos}</span>개
            </div>
        </div>
      </div >
    );
  }

}

export default Mypage;

```



[OAuth 예제](https://github.com/apfl99/im-sprint-auth-oauth)



<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
</div>

