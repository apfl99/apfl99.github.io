---
title: "[Essence of Solidity in Depth] 컨트랙트 빌트인 값과 함수"
categories:
 - GroundX
tags: [Special Variables and Functions, Blocks and Transaction Properties, Error Handling, Cryptographic Functions] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---



# 컨트랙트 빌트인 값과 함수(Special Variables and Functions)

-----------



## Blocks and Transaction Properties 

- blockhash(uint blocknumber) returns (bytes32) : 블록 해시(최근 256블록까지만 조회가능) -> 블록 존재 여부 확인 가능
- block.number (uint) : 현재 블록 번호 -> 조건
- block.timestamp (uint) : 현재 블록 타임스탬프
- gasleft() returns (uint256) : 남은 가스량 -> 다른 컨트랙트 호출 시 체크
- msg.data (bytes calldata) : 메시지(현재 트랙잭션)에 포함된 실행 데이터(input)
- msg.sender (address payable) :  현재 함수 실행 주체의 주소
- msg.sig (bytes4) : calldate의 첫 4 바이트 (함수 해시)
- msg.value (uint) : 메시지와 전달된 KLAY(peb단위) 양
- now (uint) : block.timestamp와 동일
- tx.gasprice (uint) : 트랜잭션 gas price
- tx.origin (address payable) : 트랜잭션 주체 (sender)



## Error Handling

- assert(bool condition)

  - condition의 false일 경우 실행 중인 함수가 변경한 내역을 모두 이전 상태로 되돌림(로직 체크에 사용)

- require(bool condition)

  - condition이 false일 경우 실행 중인 함수가 변경한 내역을 모두 이전 상태로 되돌림(외부 변수 검증에 사용)

- require(bool condition, string memory message)
  - require(bool)과 동일, 추가로 메시지 전달




## Cryptographic Functions

- 해쉬 함수 같은 경우는 연산량이 많아 많은 가스비 요구, 주의해서 사용
- keccak256(bytes memory) return (bytes32);
  - 주어진 값으로 keccak-256 해시 생성
- sha256(bytes memory) returns (bytes32);
  - 주어진 값으로 SHA-256 해시 생성
- Ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)
  - 서명(v,r,s)으로부터 어카운트 주소를 도출(서명 -> 공개키 -> 주소)



<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
  <a>https://docs.soliditylang.org/en/v0.8.16/units-and-global-variables.html</a>
  <br>
</div>

