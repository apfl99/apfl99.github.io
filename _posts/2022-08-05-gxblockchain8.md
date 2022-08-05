---
title: "[블록체인 상태와 트랜잭션] Klaytn 합의 알고리즘"
categories:
 - GroundX
tags: [Klaytn BFT] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---



# **Klaytn BFT**
------------------

- Klaytn은 확장가능한 BFT를 사용(기존의 BFT의 성능은 보장)
  - N개의 노드 가운데 S개의 부분노드 집합을 확률적으로 선택(where N is large, and S is sufficient small)
    - 이때, S는 BFT를 구현하기에 충분한 소규모 집합
  - 전체 집합을 거버넌스 카운실(Governance Council),
  - 부분집합을 커미티(Committee)
  - 커미티 선택은 VRF(Verifiable Random Function)로 구해진 무작위 값에 기반 -> 랜덤 검증
- 매 블록마다 새 커미티를 뽑아 BFT를 실행
- 기존의 BFT에 비해 확장성을 크게 확산



![img](../../images/2022-08-05-gxblockchain8/img-20220805123821350.png)

{: .align-center}


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>