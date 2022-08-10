---
title: "[블록체인 어플리케이션] Blockchain Application 개발 방법"
categories:
 - GroundX
tags: [Blockchain Application 개발, Frontend, Backend] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# Blockchain Application 개발

----------------------

- Frontend
  - 사용자가 직접 사용하는 프로그램(e.g., moblie app, web page/app)
  - User Interface(UI), 통신, 이벤트 처리 등을 사용자 환경을 고려하여 개발
  - UX 생성, 서명, 전송 등을 프론트엔드에서 처리
  - 프론트엔드 개발에 영향을 끼치는 실행환경 중 하나가 지갑
    - 지갑의 존재유무에 따라 개발방법이 변경
    - 특정 지갑을 사용할 경우 해당 지갑이 제공하는 라이브러리를 사용
- Backend
  - 사용자 눈에 보이지 않는 서비스
  - 프론트엔드가 사용자 요청을 전달하면 백엔드가 처리하는 구조
  - 블록체인 동기화 등 컴퓨팅 리소스가 많이 필요한 일을 처리하는 데 적합
  - 블록체인 동기화, 블록 파싱, TX 전달, 가스비 대납 등을 백엔드에서 처리
  - 블록체인 프로토콜 이외의 정보를 관리할 경우 필요
    - UX 향상 및 서비스 구현을 위해 TX 외 다른 정보가 필요할 경우 백엔드를 운영
  - 서비스 제공자가 실행환경을 결정 (e.g., SDK )



- Fully decentralized = Frontend + Blockchain
- Semi-decentralized = Frontend + Server + Blockchain


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>
