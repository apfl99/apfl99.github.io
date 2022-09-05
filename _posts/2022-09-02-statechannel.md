---
title: "블록체인 레이어 2 솔루션 : 상태 채널"
categories:
 - Blockchain
tags: [statechannel] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# 블록체인 레이어 2 솔루션 : 상태 채널

---

상태 채널이란 두 당사자가 오프체인에서 많은 트랜잭션을 수행한 다음 최종 결과만 블록체인에 게시할 수 있도록 하는 P2P 프로토콜이다. 채널은 암호화를 사용하여 생성하는 요약 데이터가 실제로 유효한 중간 트랜잭션 집합의 결과임을 증명한다. 각 채널은 당사자들의 다중 서명 스마트 컨트랙트에 의해 관리된다. 



## 동작 방식

![img1](../../images/2022-09-02-statechannel/img1.png)

{: .align-center}

1. 블록체인 상태의 일부는 다중 서명 스마트 컨트랙트를 통해 잠겨 있으므로 특정 참가자 집합이 업데이트하려면 서로 완전히 동의해야 한다.
2. 참가자들은 블록체인에 제출할 수 있는 트랜잭션을 구성하고 서명함으로써 그들 사이에서 상태를 업데이트하지만 보류된다.
3. 마지막으로 참가자는 상태 채널을 닫고 상태를 다시 잠금 해제하는 블록체인에 상태를 다시 제출한다.



## 상태 채널 장단점



### 장점

- 상태 채널에는 참가자 간의 채널 내부에서 발생하기 때문에 시작 및 종료 트랜잭션만 존재하기 때문에 강력한 개인 정보 보호 속성이 있다.
- 당사자가 상태 업데이트에 서명을 하면 즉각적인 최종 보장을 하기 때문에 매우 높은 보장을 가지고 있다.
- 많은 상태 업데이트에 유리하다.



### 단점

- 모든 참가자의 100% 가용성이 필요하다.
- 참가자 집합을 변경하려면 해당 컨트랙트를 변경해야 한다.
- 계약 배포시 초기 비용이 존재한다.



<div class="notice">
  <h5>Reference</h5>
  <a>https://ethereum.org/ko/developers/docs/scaling/state-channels/</a>
  <br>
  <a>https://docs.ethhub.io/ethereum-roadmap/layer-2-scaling/state-channels/</a>
  <br>
  <a>https://education.district0x.io/general-topics/understanding-ethereum/basics-state-channels/</a>
  <br>
</div>


