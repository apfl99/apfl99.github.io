---
title: "[블록체인 상태와 트랜잭션] 트랜잭션"
categories:
 - GroundX
tags: [Ethereum Account, Transaction, Gas] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---



# **(Recall) Ethereum 어카운트의 종류**
------------------

1. External Account: 사용자(end user)가 사용하는 어카운트(a.k.a. EOA) -> 능동적 
2. Contract Account: 스마트 컨트랙트를 표현하는 어카운트 -> 수동적

- Ethereum은 EOA와 스마트 컨트랙트의 상태를 기록 및 유지
  - 스마트 컨트랙트는 특정 주소에 존재하는 실행 가능한 프로그램
  - 프로그램은 상태를 가지기 때문에 Ethereum/Klaytn은 스마트 컨트랙트를 어카운트로 표현
- EOA는 블록에 기록되는 TX를 생성
  - 블록에 기록되는 TX들은 명시적인 변경을 일으킴(e.g., 토큰 전송, 스마트 컨트랙트 배포/실행)



# **트랜잭션(TX)와 가스(Gas)**
------------------

- TX의 목적은 블록체인의 상태를 변경하는 것
  - TX는 보내는 사람(sender, from)과 받는 사람(recipient, to)이 지정되어 있으며
  - to가 누구냐에 따라 TX의 목적이 세분화
    - to 사용자라면 토큰 전송
    - to 컨트랙트라면 컨트랙트의 실행
    - to ???라면 새로운 컨트랙트의 배포
- Gas: TX를 처리하는데 발생하는 비용
  - TX를 처리하는데 필요한 자원(computing power, storage)을 비용으로 전환한 것이 가스(Gas)
  - Sender는 TX의 처리를 위해 필요한 가스의 총량과 같은 가치의 플랫폼 토큰을 제공해야 함
  - 이때 지출되는 플랫폼 토큰을 가스비(Gas Fee)라 정의
  - 가스비는 블록을 생성한 노드가 수집
    - Ethereum의 경우, 사용자의 가스비를 기준으로 트랜잭션을 처리해줌 -> 이더리움이 네트워크가 느리기 때문에
    - Klaytn의 경우, 가스비가 없음, 먼저 들어간게 먼저 처리

<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>