---
title: "[블록체인 상태와 트랜잭션] 트랜잭션과 서명"
categories:
 - GroundX
tags: [Transaction Sign] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **트랜잭션과 서명**
----------------------

- 플랫폼은 sender가 TX가 처리되는데 필요한 가스비를 가지고 있는지 확인
  - 가스비 확인은 구현에 따라 상이
  - Ethereum/Klaytn은 노드가 TX를 수신함과 동시에 가스비 이상의 balance가 있는지 확인
  - TX의 체결과 동시에 sender의 balance에서 가스비를 차감
- TX는 sender의 서명(v, r, s)이 필요
  - 어카운트의 balance를 사용하기 때문
  - 서명의 증명은 구현마다 상이
    - Ethereum : 서명 -> 공개키 도출 -> 어카운트 주소 도출 -> 어카운트 존재유무 확인
    - Klaytn : from 주소 확인 -> 저장된 공개키 불러오기 -> 서명 직접 검증

<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>