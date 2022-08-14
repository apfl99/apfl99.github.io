---
title: "[Essence of Solidity in Depth] 제어구조, 컨트랙트 자료형"
categories:
 - GroundX
tags: [Expressions and Control Structures, Creating Contracts, Visibility and Getters, Function Declarations, Fallback Function] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# 제어구조, 컨트랙트 자료형(Expressions and Control Structures)

---------



## 제어구조

- Solidity 제어 구문
  - 대부분 프로그래밍 언어와 비슷
- 예외처리 기능이 없음
  - i.e., try-catch 없음



## 컨트랙트 자료형



### Creating Contracts

---------------

- 일반적인 컨트랙트 생성 -> 배포
- 컨트랙트를 클래스처럼 사용
  - 컨트랙트를 객체지향 프로그래밍에서 사용하는 클래스로 취급할 수 있음
  - new 키워드를 사용하여 컨트랙트를 생성하여 변수에 대입
  - Ex. 

```solidity
contract A {
 B b;
 constructor() public { b = new B(10); }
 function bar(uint x) public view
 returns (uint)
 {
 return b.foo(x);
 }
}

contract B {
 uint base;
 constructor(uint _base) public { base = _base; }
 function foo(uint y) public view
 returns (uint)
 {
 return y * base;
 }
}
// 이미 만들어진 컨트랙트도 불러낼 수 있음
```



### Visibility and Getters

---------------

- 함수의 visibility(공개정도)를 목적에 맞게 설정
  - external
    - 다른 컨트랙트에서, 트랜잭션을 통해 호출 가능
    - internal 호출 불가능 (i.e., f()는 안되지만 this.f()는 허용됨)
  - public
    - 트랜잭션을 통해 호출 가능, internal 호출 가능
  - internal
    - 외부에서 호출 불가능, internal 호출 가능, 상속받은 컨트랙트에서 호출 가능
  - private
    - internal 호출 가능



### Function Declarations : pure vs view vs (none)

---------------

- 함수 제약을 설정하여 정해진 scope에서 동작할 수 있도록 설정
  - pure
    - State Variable 접근 불가 i.e., READ(X), WRITE(X)
  - view
    - State Variable 변경 불가 i.e., READ(O), WRITE(X)
  - (none)
    - 제약 없음 i.e.,READ(O), WRITE(O)



### Fallback Function

---------------

- 컨트랙트에 일치하는 함수가 없을 경우 실행 (i.e., no input/calldata)
  - 단 하나만 정의 가능 & 함수명, 함수인자, 반환값 없음
  - 반드시 external로 선언
- 컨트랙트가 KLAY를 받으려면 payable fallback function이 필요
  - Payable fallback이 없는 컨트랙트가 KLAY를 전송받으면 오류 발생
- Ex.

```solidity
contract Escrow {
 event Deposited(address sender, uint amount);
 function() external payable {
 emit Deposited(msg.sender, msg.value);
 }
}
```





<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
  <a>https://docs.soliditylang.org/en/v0.8.16/control-structures.html</a>
  <br>
  <a>https://docs.soliditylang.org/en/v0.8.16/contracts.html</a>
  <br>
</div>