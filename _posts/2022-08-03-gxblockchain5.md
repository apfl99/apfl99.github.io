---
title: "[블록체인 기본] 블록체인 비교"
categories:
 - GroundX
tags: [Public, Private, Permissionless, Permissioned] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **Public vs. Private**

---------------------------------------

퍼블릭과 프라이빗의 구분은 블록체인에 다음을 수행할 수 있는지 확인하여 결정: 

- 누구든지 기록된 정보(블록)을 자유롭게 읽을 수 있는가?
- 명시적인 등록 또는 자격취득 없이 정보를 블록체인 네트워크에 기록할 수 있는가?

블록체인의 정보가 공개되어 있고 네트워크가 정한 기준(e.g. gas fee)에 따라 정보를 기록요청할 수 있다면 그 블록체인은 **퍼블릭/공개형**이라 한다.

이와 반대로 정보가 공개되어 있지 않고 미리 자격을 득한 사용자만이 정보를 기록할 수 있다면 그 블록체인은 **프라이빗/비공개형**이라 한다.

# **Permissionless vs. Permissioned**

---------------------------------------

일반적으로 네트워크의 참여가 제한된 경우 'permissioned', 그렇지 않은 경우 'permissionless'라 정의

네트워크의 참여의 정의

- (넓은 의미) 블록체인 P2P 네트워크에 참여
- (좁은 의미) 합의과정의 참여

Public/Private의 개념이 **정보의 접근성(Access)**와 관련이 있다면 Permissionless/Permissioned는 **정보의 제어(Control)**, 즉 무엇이 블록에 포함되는 지를 결정하는 지에 더 밀접한 개념



## 유형별 블록체인 비교분석

Ethereum : Permissionless, Public

Bitcoin : Permissionless, Public

Klaytn : Permissioned, Public

- BFT 합의로 네트워크 참여자 제한 -> 정보 제어 제한

Ripple : Permissioned, Private

HyperLedger : Permissioned, Private

<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>