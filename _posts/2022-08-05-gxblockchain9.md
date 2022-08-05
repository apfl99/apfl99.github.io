---
title: "[블록체인 상태와 트랜잭션] 블록체인의 상태"
categories:
 - GroundX
tags: [Blockchain State Machine] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **(어카운트 기반) 블록체인의 상태**
------------------

블록체인은 트랜잭션으로 변화하는 상태 기계

![img](../../images/2022-08-05-gxblockchain9/img-20220805130625664.png) 
{: .align-center}

- 이전의 블록의 최종값은 현재 블록의 초기값
  - from Coinbase : 마이닝
- 최종값은 현재 블록의 상태, 초기값은 이전 블록의 상태
<br>
<br>

# **상태 기계**
------------------

- 블록체인은 초기 상태에서 변경 사항을 적용하여 최종 상태로 변화하는 상태 기계
  - 이전 블록의 최종 상태는 현재 블록의 초기 상태
  - Gen block의 경우 임의의 초기값들이 설정되는데 이것이 gen block의 초기상태이자 최종상태
- (어카운트 기반) 블록체인의 상태
  - TX는 어카운트를 생성하거나 변경
    - e.g., Alice가 기존에 존재하지 않던 주소 X에 1 ETH를 전송하면 새로운 EOA가 생성
    - e.g., Alice가 새로운 스마트 컨트랙트를 배포(컨트랙트도 어카운트)
    - e.g., Alice가 Bob에게 S ETH를 전송하는 TX가 체결되면 Alice의 Bob의 잔고가 변경
  - 항상 같은 결과를 보장하기 위해 하나의 TX가 반영되는 과정에서 다른 TX의 개입은 제한됨



<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>