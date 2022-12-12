---
title: "Project 2 : Incentive Community"
categories:
 - BEB
tags: [Token economy, ERC20, ERC721] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기



---



# Project 2 : Incentive Community

----

Git Repo : [https://github.com/apfl99/BEB-06-SECOND-7ZO/tree/main](https://github.com/apfl99/BEB-06-SECOND-7ZO/tree/main)



본 프로젝트는 글 작성시 인센티브(ERC20)을 사용자에게 주고, 이를 통해 보상으로 NFT를 발행할 수 있게 함으로써, 커뮤니티를 활성화시키는 서비스 개발이었다. 

목표는 서비스 개발의 완성도가 아닌 학습과 팀원분들과의 협업을 목적으로 진행했으며, 여러 라이브러리를 활용해보고자 노력했다.



## 기술 스택

- HTML5
- CSS3
- JavaScript
- React
  - Redux
  - Antd

- Node.js
- Express
- Solidity
- Remix
- Truffle
- Web3.js
  - Ganache
  - NFT.storage
- MySQL(AWS RDS)
  - Sequelize

- Github
- Git



## Project 2 요구 사항(Front)

- task 1) 

  헤더 생성

  -  웹페이지 로고를 누르면 메인 페이지로 돌아감
  - 회원가입 / 로그인, 로그아웃 버튼 만들기
    - 로그인을 했을 때) 닉네임과 로그아웃 버튼이 생김
    - 로그인을 하지 않았을 때) ID/PW란과 로그인 및 회원가입 버튼이 있음
  - 마켓, 마이페이지 내비게이션
  - ETH Faucet 버튼

- task 2) 

  메인 페이지 (랜딩 페이지)

  - 게시글 리스트
    - 10페이지 단위씩 볼 수 있도록 페이지네이션으로 개발
    - 번호, 제목, 작성자, 생성일, 조회수가 보여야 함
    - 게시글 제목에 커서를 대면 커서가 누르는 모양으로 바뀌어야 함
    - 서버에서 게시글 리스트 데이터를 받아와야 함
  - 게시글 작성 버튼
    - 버튼을 누르면 게시글 작성 페이지로 이동함

- task 3) 

  게시글 작성 페이지

  - 리액트에서 제공하는 기본 폼으로 게시글 작성 페이지를 개발
  - 제목, 내용 칸 있어야 함
  - 작성 버튼을 누르면, 제목과 내용이 글쓴이의 아이디와 함께 서버로 전달해야 함

- task 4) 

  특정 게시글 조회 페이지

  - 번호, 제목, 작성자, 생성일, 조회수 등 정보가 있어야 함
  - 본문 있어야 함
  - 목록 버튼을 누르면 게시글 리스트의 클릭했던 목록으로 돌아감

- task 5) 

  회원가입 페이지

  - 리액트에서 제공하는 기본 폼으로 회원가입 페이지를 개발
    - 비밀번호 유효성 검사해야 함
  -  회원*가입 버튼을 누르면 서버로 폼 데이터를* 보내야 함

- task 6) 

  마이 페이지

  - 닉네임, 지갑 주소, 토큰 개수, 나의 NFT, 나의 게시글 정보를 볼 수 있어야 함
  - 타인에게 내가 가진 토큰을 줄 수 있는 칸이 있어야 함

- task 7) 

  NFT 판매 페이지

  - 사용자가 사진 주소를 올리면 민팅이 될 수 있어야 함

- task 8) 

  유저 정보 제어

  - 훅스, 리덕스, 리코일 등 상태 관리를 통해 유저 정보를 제어할 수 있어야 함



## Project 2 요구 사항(Front)

- task 1) 

  NodeJS 서버 생성

  - Express를 사용하여 기본 서버를 생성하기
  - router 설정하기
  - dotenv를 이용하여 중요정보를 환경변수로 저장하기

- task 2) 

  DB 연결

  - 서버와 연동

- task 3) 

  스마트 컨트랙트 연동

  - ABI 및 컨트랙트 주소로 web3.js 연동

- task 4) 

  필요한 API 작성

  - Web3.js가 필요한 부분
    - 회원가입 API
      - 서버 계정 생성
    -  글 작성 시 보상토큰을 지급받는 API
    - 유저간 토큰을 전송하는 API
    - 특정 유저 정보를 가져오는 API
    - ETH Faucet을 지급 받는 API
    - NFT 구매를 위한 API
      - 유저 NFT 민팅
  - Web3.js가 필요하지 않은 부분
    - 로그인 API
    - 게시판 GET API
    - 게시글 조회 GET API

- task 5) 

  Daemon 사용하여 트랜잭션 트래킹

  - Daemon 설치
  - Daemon 실행 후 확인



## WorkFlow & WireFrame



### WorkFlow

---

![https://user-images.githubusercontent.com/108708252/200509544-82bcc2cb-7763-4690-bffc-4ca8b4d06d03.png](../../images/2022-11-04-incentiveCommunity/200509544-82bcc2cb-7763-4690-bffc-4ca8b4d06d03.png)

{: .align-center}

![https://user-images.githubusercontent.com/108708252/200509549-a5c1e950-d566-43f8-b597-a52b26db2df5.png](../../images/2022-11-04-incentiveCommunity/200509549-a5c1e950-d566-43f8-b597-a52b26db2df5.png)

{: .align-center}



### WireFrame

---

![https://user-images.githubusercontent.com/108708252/200531775-27c8232f-3760-422c-a0a2-b555878784d5.png](../../images/2022-11-04-incentiveCommunity/200531775-27c8232f-3760-422c-a0a2-b555878784d5.png)

{: .align-center}



## API Docs

[Notion API Docs](https://www.notion.so/BEB-Project2-ToDo-List-4a82b42a6c6a42a2b17bfdcd2f47c3ba#59b15a34f486462b94cea314043ae7ef)



## 내가 맡은 기능(Back) 

- task 2) DB 연결
  - 서버와 연동
- task 3) 스마트 컨트랙트 연동
  - ABI 및 컨트랙트 주소로 web3.js 연동

- task 4) 

  필요한 API 작성

  - Web3.js가 필요한 부분
    - 회원가입 API
      - 서버 계정 생성
    - 유저간 토큰을 전송하는 API
    - 특정 유저 정보를 가져오는 API
    - ETH Faucet을 지급 받는 API
    - NFT 구매를 위한 API
      - 유저 NFT 민팅
  - Web3.js가 필요하지 않은 부분
    - 로그인 API

- task 5) 

  Daemon 사용하여 트랜잭션 트래킹

  - Daemon 설치
  - Daemon 실행 후 확인



### Transfer20 

----

![transfer20](../../images/2022-11-04-incentiveCommunity/transfer20-7969165.gif)

 {: .align-center}



### Minting

----

![ minting](../../images/2022-11-04-incentiveCommunity/ minting.gif)

{: .align-center}



### Faucet

---

![faucet](../../images/2022-11-04-incentiveCommunity/faucet-7969202.gif)

{: .align-center}



### Daemon(Tx logging)

---

![daemon](../../images/2022-11-04-incentiveCommunity/daemon-7969211.gif)

{: .align-center}



## 어려웠던 점..

이번에는 팀원분들과 DB를 공유하기 위해 MySQL을 AWS RDS를 활용해서 올렸다. 이미 활용한 경험이 있지만, Sequelize를 사용하여 js에서 모델 핸들링을 하는 것이 처음이라 여러 레퍼런스를 참고하고 오류도 많이 발생했던 것 같다. 그 이외에는 해본 것들이라 수월하게 진행했지만, 아무래도 아쉬웠던 점은 컨트랙트가 testnet이 아닌 ganache로 배포하기로 해서, 팀원분들과 블록체인 데이터를 공유할 수 없다는 점이 아쉬웠다. 







<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
</div>






