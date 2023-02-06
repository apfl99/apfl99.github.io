---
title: "DankSharding의 등장 배경"
categories:
 - DankSharding
tags: [Ethereum 2.0, Vitalik Buterin’s Endgame, A rollup-centric ethereum roadmap] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기



---


# DankSharding의 등장 배경

----



## Ethereum 2.0 기존의 로드맵

2018년 타이페이에서 열린 연구 워크숍에서 결정한 "Ethereum 2.0" 로드맵은 다음과 같다.

![e2 roadmap](../../images/2023-02-01-ethRollup/e2 roadmap.png)

{: .align-center}



위의 다이어그램은 로드맵의 각 단계별 내용을 보여주는데, 주요 요소는

- 비콘 체인 : 샤드 체인과 메인 체인 사이의 브리지 역할을 하며, 스테이커(PoS 블록의 유효성 검사자)를 관리한다.
- 샤드 체인 : 메인 체인을 64개로 분할하여 네트워크 부하를 분산한다.
- VM : 각 샤드가 자체적으로 트랜잭션을 처리하고 스마트 계약을 실행할 수 있게 하고, 교차 샤드 트랜잭션을 처리하기 위해 서로 통신할 수 있다.



이 중 현재 진행 사항은 비콘 체인과 메인 체인이 병합된 "The Merge"로, 원래 작업 증명 메커니즘(PoW)에서 지분 증명 메커니즘(PoS)으로 업그레이드를 완료했다.



[The Merge]()





## A rollup-centric Ethereum roadmap

2020년 Vitalik Buterin은 "A rollup-centric Ethereum roadmap"을 게시하며, 롤업 중점의 이더리움 로드맵을 주장했다.

이러한 주장의 근거는 경제적 지속 가능성과 TPS이다.

경제적 지속 가능성은 Layer 2 프로토콜을 활용하는데에서 나오는 장점으로, 공공재 자금 조달과 다양한 종류의 플랫폼을 만들려는 프로젝트에서 자체적인 지역 경제의 활성화와 기술 자율성을 유지하는, 보다 다양한 사람들에게 명확한 기회를 제공한다는 점이고,

TPS는 장기적으로 미래를 구상했을때, 

- 오늘날 이더리움은 ~15TPS를 가지고 있다.
- 모든 사람이 롤업으로 이동하면 곧 ~3000TPS가 된다.
- 1단계가 진행되고 롤업이 데이터 저장을 위해 eth2 샤드 체인으로 이동하면 이론상 최대 ~100,000TPS까지 올라간다.
- 결국 2단계가 진행되어 기본 계산 기능이 있는 eth2 샤드 체인을 가져와도 ~1000 ~ 5000TPS를 제공한다.

위와 같이 기존 로드맵에 따라 샤드 체인을 실행 계층으로 완료하더라도 샤드 체인을 롤업에서의 데이터 저장 공간으로 사용했을 때가 TPS가 더 높기 때문이다. 

또한, 기존 로드맵에 따라 샤딩 EVM 계산을 적용했을 때보다 샤딩 데이터 가용성을 사용했을 때, 샤딩된 EVM 계산의 부정직 다수 증명 검증에는 엄격하고 잠재적으로 위험한 두 에포크 동기 가정이 필요한 사기 증명이 필요하지만, 데이터 가용성 샘플링(ZKP 또는 다항식 약정으로 수행된 경우)는 비동기식에서 더 안전하기 때문에 더 적합하다고도 주장한다.



### Rollup

---









## EndGame





















<div class="notice">
  <h5>Reference</h5>
  1) <a>https://tim.mirror.xyz/CHQtTJb1NDxCK41JpULL-zAJe7YOtw-m4UDw6KDju6c</a>
  <br>
  2) <a>https://www.alchemy.com/overviews/what-is-the-ethereum-beacon-chain</a>
  <br>
  3) <a>https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698</a>
</div>