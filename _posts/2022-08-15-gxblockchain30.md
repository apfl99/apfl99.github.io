---
title: "[블록체인 사용자 경험] 블록체인 UX의 문제"
categories:
 - GroundX
tags: [Blockchain UX, Blockchain Cost, Blockchain Performance] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# 블록체인 문제

----------



## 성능

- 한 블록이 담을 수 있는 트랜잭션 개수의 제한
- 주기적인 블록 합의로 인해 발생하는 지연
  - 합의 알고리즘에 대한 개선 요구



## 비용

- 합의에 참여하기 위해 필요한 비용
- 블록을 유지하기 위해 필요한 비용
  - 노드 단의 개선 요구



## UX

- 사용자가 가스비를 내야하는 불편함
  - 사용자는 플랫폼 사용료를 지불해야하는 것을 이해 못 함
  - 사용자가 코인을 취득하기 어려움
    - Klaytn의 경우 sender와 payer를 분리하므로써 가스비 대납기능으로 해결
    - 물론 이에 따라 검증 복잡도 증가로 플랫폿 성능 저하 우려
- 사용자가 직접 키를 관리해야 하는 불편함
  - 키 관리 시스템(KMS)
    - 사용자 인증에 따라 사용자에게 KMS는 서명된 트랜잭션을 전달
    - 암호화
    - 이에 따라 중앙화에 대한 우려


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>