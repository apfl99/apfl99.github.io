---
title: "[Essence of Solidity in Depth] 자료형"
categories:
 - GroundX
tags: [Data Types, Address, Reference Types, Arrays, bytesN vs bytes/string vs byte, Mapping Types ] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---

# 자료형(Data Types)

---------------

- Solidity는 다음과 같은 자료형을 지원
  - Booleans(bool)
  - Integers(int ...)
  - Address
  - Fixed-size byte arrays(bytes1, ...)
  - Reference Types
  - Arrays
  - Mapping Types
  - Contract Types



## Address

- 어카운트 주소를 표현
  - Klaytn 주소의 길이는 20바이트: address => bytes20 형변환 가능
  - address vs address payable(거래 가능한 주소)
    - Address payable => address (O)
    - address -> address payable (X) -> uint160을 거쳐서만 가능



## Reference Types

- Reference type은 struct, arrays, mapping과 같이 크기가 정해지지 않은 데이터를 위해 사용
  - Value Type들은 변수끼리 대입할 경우 값을 복사
  - Reference Type들은 (같은 영역을 사용하는) 변수끼리 대입할 경우 같은 값을 참조
    - 포인터같은 느낌
- Reference Type 데이터는 저장되는 위치를 반드시 명시
  - memory(함수 내에서 유효한 영역에 저장)
  - Storage(state variables와 같이 영속적으로 저장되는 영역에 저장, 블록체인에 저장되는 데이터)
  - Calldata(external 함수 인자에 사용되는 공간)
- 서로 다른 영역을 참조하는 변수 간 대입이 발생 시 데이터 복사
  - storage => memory/calldata
  - Anything => storage



## Arrays

- JS와 개념은 같으나 사용법이 상이
  - State Variable로 사용할 때(i.e., 저장공간 = storage)
    - T[k] x; -> k개의 T를 가진 배열 x 선언
    - e.g., uint[5] arr; -> arr은 5개의 uint를 가진 배열 arr[0] -> 첫번째 uint
    - T[] x; -> x는 T를 담을 수 있는 배열 x의 크기는 변할 수 있음 (dynamic array)
    - T[] [k] x; -> k개의 T를 담을 수 있는 배열 x를 선언 (dynamic array)
    - T[] [k] x가 주어질 때 x[i][j]는 (i-1)번째 배열의 (j-1)번째 T를 불러옴
- 모든 유형의 데이터를 배열에 담을 수 있음
  - mapping, struct 포함
- .push(T item) and .length
  - 기존 기능과 마찬가지
- 런타임에 생성되는 (i.e., 함수 내에서) memory 배열은 new 키워드를 써서 선언
  - storage 배열과는 달리 memory 배열은 dynamic array 사용이 불가



## bytesN vs bytes/string vs byte[]

- 가능하면 언제나 bytes를 사용
  - byte[]는 배열 아이템 간 31바이트 패딩이 추가됨
- 기본 룰
  - 임의의 길이의 바이트 데이터를 담을 때는 bytes
  - 임의의 길이의 데이터가 UTF-8과 같이 문자로 인코딩 될 수 있을 때는 string
  - 바이트 데이터의 길이가 정해져있을 때는 value type의 bytes1, ... , bytes32를 사용
  - byte[]는 지양



## Mapping Types

- 해시테이블과 유사, 배열처럼 사용
  - Storage영역에만 저장 가능 (i.e., state variable로만 선언 가능)
  - 함수 인자, 또는 public 리턴값으로 사용할 수 없음





<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
  <a>https://docs.soliditylang.org/en/v0.4.25/types.html</a>
  <br>
</div>