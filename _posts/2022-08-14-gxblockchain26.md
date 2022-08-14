---
title: "[Essence of Solidity in Depth] 컨트랙트 구조"
categories:
 - GroundX
tags: [Structure of a Contract, State Variables, Functions, Function Modifiers, Events, Struct Types, Enum Types] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# 컨트랙트 구조(Structure of a Contract)

------------------



## State Variables

- 블록체인에 영구히 저장할 값들은 상태변수(State Variable)로 선언
  - 어떤 값들은 반드시 State Variable로 선언되어야 함(e.g., mapping)
- public 키워드를 사용하여 변수를 외부에 노출가능
  - 이 경우 자동으로 해당 변수 값을 돌려주는 Getter 함수가 생성됨

```solidity
contract Count {
	uint public count = 0;
	address public lasParticipant;
}
```



## Functions

- 함수는 실행 가능한 코드를 정의한 것
  - external, public, internal, private 중 하나로 visibility를 설정 가능
  - payable, view, pure 등 함수의 유형을 정의 가능

```solidity
contract Count {
	function plus public {
		count++;
		lastParticipant = msg.sender;
	}
}
```



## Function Modifiers

- 함수의 실행 전, 후의 성격을 정의
  - 대부분의 경우 함수의 실행조건을 정의하는데 사용됨

```solidity
contract Ballot {
	constructor() public { chairperson = msg.sender; } //생성자 : 함수 처음에만 실행 -> 배포한 사람(체어맨) 저장
	address chairperson;
	modifier onlyChair { // : 컨트랙 실행자가 배포한 사람(체어맨)이 아닐 경우 종료
		require(msg.sender == chairperson, "Only the chairperson can call this function.");
		_;
  }

	//투표권 주는 함수
	function giveRightToVote(address to) public onlyChair {
		// 'onlyChair' modifier ensures that this function is called by the chairperson
	}
}
```



## Events

- 이벤트는 EVM 로깅을 활용한 시스템
  - 이벤트가 실행될 때마다 트랜잭션 로그에 저장
  - 저장된 로그는 컨트랙트 주소와 연동되어 클라이언트가 RPC로 조회 가능

```solidity
// Contract
contract Ballot {
	event Voted(address voter, uint proposal); // Voted 이벤트의 트랜잭션 로깅
	function vote(uint proposal) public {
		//..
		emit Voted(msg.sender,proposal); // Voted 이벤트 실행
	}
}
```

```solidity
//Client using caver-js
const BallotContract = new caver.klay.Contract(abi,address);
BallotContract.events.Voted( // Voted 이벤트에 대한 트랜잭션 로그 리스닝 -> 이벤트가 발생할 때만
	{fromBlock: 0},
	function(error, event) {
		console.log(event);
	}
).on('error',console.error);
```



## Struct Types

- Solidity에서 제공하지 않는 새로운 형식의 자료를 만들 때 사용
  - 여러 자료를 묶어 복잡한 자료형을 만들 때 유용

```solidity
contract Ballot {
	struct Voter {
		uint weight;
		bool voted;
		address delegate;
		uint vote;
	}
}

contract SNS {
	struct Friend {
		uint id;
		mapping(uint => address) friends;
	}
}
```


## Enum Types

- Enum은 임의의 상수를 정의하기에 적합
  - e.g., 요일
  - e.g., 상태
  - 프로그램은 숫자

```solidity
contract Ballot {
	enum Status {
		open,
		closed
	}
}
```





<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
  <a>https://docs.soliditylang.org/en/v0.4.25/structure-of-a-contract.html</a>
  <br>
</div>