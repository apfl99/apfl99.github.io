---
title: "[Solidity & Klaytn SDK] Byte Code & ABI"
categories:
 - GroundX
tags: [Byte Code, ABI] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---



# ByteCode & ABI

------------------

Solidity 소스 코드를 컴파일하면 Bytecode(.bin파일)과 ABI(.abi 파일)이 생성



## Bytecode

- 컨트랙트를 배포할 때 블록체인이 저장하는 정보
- Bytecode는 Solidity 소스코드를 EVM이 이해할 수 있는 형태로 변환한 것
- 컨트랙트 배포시 HEX로 표현된 Bytecode를 TX에 담아 전달



## ABI

- ABI는 컨트랙트 함수를 JSON 형태로 표현한 정보로 EVM이 컨트랙트 함수를 실행할 때 필요
- 컨트랙트 함수를 실행하려는 사람은 ABI 정보를 노드에 제공
- 프로그램 헤더와 같은 역할



<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>
