---
title: "[블록체인 응용사례] 해외 송금/정산"
categories:
 - GroundX
tags: [BoC, MAS, Cross-border Settlement] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# 해외 송금/정산

-----------------



## 기존 해외 송금/정산

- 은행을 거친 국가간 지급 결제
  - 환거래은행을 거침
  - 복잡함 + 고비용 + 즉시성 떨어짐
- 송금 전문업체의 등장
  - 웨스턴유니언, 머니그램
  - 비교적 빠른 송금 가능
- 결제 전문 서비스들의 등장
  - 환거래은행 배제
  - 송금 불가능



## 블록체인을 활용한 해외 송금/정산



### BoC & MAS Cross-border Settlement

- 두 개의 다른 네트워크 간 통신
  - BoC는 R3 Corda, MAS는 JPM Quorum
  - Payment versus Payment (PvP) 지급건마다 atomic swap 실행
    - Hash time-locked contract
    - A가 컨트랙트에 돈을 보내고 (정해진 시간동안 홀딩) B가 인증이되면 그 돈을 받아오는 구조 
  - 즉시 송금 가능 & 수수료 최소화
  - 큰 액수의 결제에는 아직 사용되지 않음


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>