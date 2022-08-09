---
title: "[Solidity & Klaytn SDK] 스마트 컨트랙트, 솔리디티"
categories:
 - GroundX
tags: [Smart Contract, Solidity] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **스마트 컨트랙트(Smart Contracts)**

-----------------------

- 블록체인에 저장되어 있는 프로그램(어카운트 처리)
- 특정 주소에 배포되어 있는 TX로 실행 가능한 코드
  - 스마트 컨트랙트 소스코드는 함수와 상태를 표현; 컨트랙트 소스코드는 블록체인에 저장
  - 함수는 상태를 변경하는 함수, 상태를 변경하지 않는 함수로 분류
  - 사용자(end user, EOA owner)가 스마트 컨트랙트 함수를 실행하거나 상태를 읽을 때 주소가 필요
- 스마트 컨트랙트는 사용자가 실행
  - 상태를 변경하는 함수를 실행하려면 그에 맞는 TX를 생성하여 블록에 추가(TX 체결 = 함수의 실행)
  - 상태를 변경하지 않는 함수, 상태를 읽는 행위는 TX가 필요 없음(노드에서 실행)





# **Soildity**

-----------------------

- Ethereum/Kalytn에서 지원하는 스마트 컨트랙트 언어
- Kalytn은 Solidity 버전 0.4.24, 0.5.6을 지원
- 일반적인 프로그래밍 언어와 그 문법과 사용이 유사하나 몇가지 제약이 존재
  - e.g., 포인터의 개념이 없기 때문에 recursive type의 선언이 불가능



## Contract = Code + Data

- Solidity 컨트랙트는 코드(함수)와 데이터(상태)로 구성
- Solidity 함수는 코드 안에 변수로 선언된 상태로 변경하거나 불러옴
- 아래 예시에서 set get은 함수, storedData는 상태

![img](../../images/2022-08-09-gxblockchain14/img.png)

<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>