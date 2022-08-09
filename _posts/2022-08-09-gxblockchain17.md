---
title: "[Solidity & Klaytn SDK] Klaytn SDK caver.js"
categories:
 - GroundX
tags: [] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **Using Klaytn SDK**
---------------



## **Klaytn SDK(Software Development Kit)**

- klaytn은 BApp 개발을 위해 필요한 SDK를 제공
- caver-js는 Node.js로 Klaytn BApp을 만들때 필요한 라이브러리를 제공
- https://docs.klaytn.com



## **개발 환경 셋팅**



### **Node.js 설치**

- https://nodejs.org에서 LTS 설치



### **개발 디렉토리 생성 및 Caver-js 설치**

- 성공적으로 Node.js를 설치한 뒤 원하는 위치에 개발 디렉토리를 생성
  - mkdir Count && cd Count
- 디렉토리 생성 후 npm으로 Node.js 프로젝트를 초기화, caver-js를 설치
  - npm init
  - npm install caver-js



## **Baobab 테스트넷에 연결**

<img src="../../images/2022-08-09-gxblockchain17/img-20220809152251439.png" alt="img" style="zoom:200%;" />



## **Wallet 생성**

<img src="../../images/2022-08-09-gxblockchain17/img-20220809152251435.png" alt="img" style="zoom:200%;" />



## **토큰 전송 TX 생성 및 서명**

<img src="../../images/2022-08-09-gxblockchain17/img-20220809152251447.png" alt="img" style="zoom:200%;" />



## **서명된 TX 전송**

<img src="../../images/2022-08-09-gxblockchain17/img-20220809152251449.png" alt="img" style="zoom:200%;" />



## **스마트 컨트랙트 배포**

<img src="../../images/2022-08-09-gxblockchain17/img-20220809152251399.png" alt="img" style="zoom:200%;" />



## **스마트 컨트랙트 함수 실행(mutation vs constant)**

<img src="../../images/2022-08-09-gxblockchain17/img-20220809152251454.png" alt="img" style="zoom:200%;" />



<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>