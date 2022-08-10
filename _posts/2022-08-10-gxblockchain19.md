---
title: "[블록체인 어플리케이션] Blockchain Application이 블록체인을 사용할 때"
categories:
 - GroundX
tags: [Blockchain Application 유형, 블록체인 활용 유형, Fully Decentralized, Semi-decentralized with centralized proxy] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# 블록체인 활용 유형

--------------------

- As a Payment Channel
  - 토큰을 사용한 결제
- As a Storage
  - 블록체인을 안전한 저장소로 인식
- As a World Computer
  - 모든 노드가 동일한 연산을 수행
  - 어느 한 노드에 의존하지 않는 탈중앙화된 실행 환경
  - Ex. Smart Contract



# Blockchain Application의 유형

----------------------

- Full decentralized
  - 사용자가 직접 블록체인과 통신
  - 신뢰에 대한 검증이 필요 -> 직접 블록체인과 통신해야 하기 때문에 제약이 발생
- Semi-decentralized with centralized proxy
  - 클라이언트가 블록체인과 통신하기 위해 중개 서버와 통신
  - 블록체인 기반으로 만들어진 서비스가 있고 그 서비스를 사용자들이 사용하는 형태
  - 클라이언트 <-> 중개서버 <-> 블록체인
  - 중개서버가 신뢰에 대한 검증을 블록체인에 대신 요구하는 형태



## Fully Decentralized

- 장점
  - 높은 투명성
  - 신뢰 형성에 필요한 비용 없음
  - 경우에 따라 사용자의 익명성 보장 가능
  - 설치형 Blockchain Application의 경우 관리 비용 낮음
- 단점
  - 사용자 책임 증가, 어려운 UX
  - 로직 변경 어려움
  - 경우에 따라 사용자가 블록체인에 상시 접속할 필요 및 블록을 복제할 필요 있음
    - 검증에 대한 cost -> 연산, 네트워크 ..



## Semi-decentralized with centralized proxy

- 장점
  - 높은 수준의 UX
  - 사용자가 블록체인과 직접 통신할 필요 없음
  - 로직 변경 비교적 쉬움
- 단점
  - 신뢰비용 발생
  - 서비스가 Single Point of Failure가 됨
    - Single Point of Failure : 시스템 장애에 대한 서비스 중단
  - 관리 비용 높음


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>
