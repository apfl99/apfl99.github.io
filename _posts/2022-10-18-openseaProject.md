---
title: "Project 1 : Opensea Clone Coding"
categories:
 - BEB
tags: [] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기


---



# Project 1 : Opensea Clone Coding

---

Git Repo : [https://github.com/apfl99/BEB-06-FIRST-04](https://github.com/apfl99/BEB-06-FIRST-04)



## Opensea Clone Coding 프로젝트 요구사항

task 1) 헤더 생성

-  웹페이지 로고를 누르면 메인 페이지로 돌아감

-  지갑 연결 버튼
  -  지갑 연결을 했을 때 ) 지갑 연결이 되어 있는 상태를 표시할 수 있는 로고
  -  지갑 연결을 하지 않았을 때) 지갑 연결되어 있지 않는 상태를 표시할 수 있는 로고

-  마켓, NFT민팅, 마이페이지 내비게이션

task 2) 메인 페이지 (랜딩 페이지)

-  NFT 리스트
  -  10페이지 단위씩 볼 수 있도록 페이지네이션으로 개발
  -  게시글 제목에 커서를 대면 커서가 누르는 모양으로 바뀌어야 함
  -  서버에서 NFT 리스트 데이터를 받아와야 함

task 3) NFT 민팅 페이지

-  리액트에서 제공하는 기본 폼으로 NFT 민팅 페이지를 개발
-  이미지, 이름, 상세내용 칸 있어야 함
-  민팅 버튼을 누르면, 지갑 서명 통해 트랜잭션 수행해야 함

task 4) 테마별 NFT 리스트 페이지

-  탭 기능 사용하여 테마별 NFT 리스트 조회 가능해야 함
-  NFT 이미지와 작가 이름 존재해야 함
-  목록 버튼을 누르면 게시글 리스트의 클릭했던 목록으로 돌아감

task 5) NFT 상세 페이지

-  이미지를 포함한 NFT 상세 정보를 보여줄 수 있어야 함
-  거래되는 가격을 보여줄 수 있어야함
-  구입버튼을 누르면 지갑을 통해 트랜잭션을 수행할 수 있어야 함

task 6) 마이 페이지

-  프로필, 보유한 NFT, 발행한 NFT 를 볼 수 있어야 함
-  지갑 연결을 해제할 수 있어야함



### 내가 맡은 기능

---

- task 3) NFT 민팅 페이지
- task 5) NFT 상세 페이지 - NFT 거래
  - NFT 판매 등록
  - NFT 구매



## WorkFlow

![flowchart](../../images/2022-10-18-openseaProject/flowchart.png)

{: .align-center}





## NFT 민팅 페이지

Minting contract : 0x01E95863D9B2e85Ca5ad754dE1248B63eaD9Da48

![mint](../../images/2022-10-18-openseaProject/mint.gif)

{: .align-center}

![mint_result](../../images/2022-10-18-openseaProject/mint_result.gif)

{: .align-center}





## NFT 상세 페이지 - 판매등록

TransactionNFT contract : 0x95602fd56336Ca99b2a08143B47838B2dC0F27e3

![approve](../../images/2022-10-18-openseaProject/approve.gif)

{: .align-center}



![listing](../../images/2022-10-18-openseaProject/listing.gif)

{: .align-center}



![listing_result](../../images/2022-10-18-openseaProject/listing_result.gif)

{: .align-center}



## NFT 상세 페이지 - 구매

TransactionNFT contract : 0x95602fd56336Ca99b2a08143B47838B2dC0F27e3

![buy](../../images/2022-10-18-openseaProject/buy.gif)

{: .align-center}



![buy_result](../../images/2022-10-18-openseaProject/buy_result.gif)

{: .align-center}



### 어려웠던 점..

---

가장 어려웠던 문제는 컨트랙트간 함수 호출이었습니다. 판매자 대신 거래를 대행 하려면, 거래 컨트랙은 판매자가 판매한 토큰에 대한 승인을 받아야합니다. 이 과정에서 저는 거래 컨트랙 내부에서 msg.sender를 유지하는 delegate call을 통해 판매한 토큰 컨트랙의 approve 함수에 접근하여 사용하려고 했습니다. 그러나 delegate call은 call과 다르게 접근한 컨트랙의 메모리를 바꾸지 못한다는 것을 학습하였고, call로는 msg.sender가 유지되지 않아 프론트 단에서 판매자가 직접 approve를 처리하게 하였고, 이후 거래 컨트랙에서 구매로직의 토큰 전송은 이미 허가되었기 때문에 call을 사용할 수도 있었지만, 토큰의 collection 주소를 IERC721로 가져와 함수를 실행시키는 것이 가독성이 좋다고 판단하여 이를 통해 해결하였습니다.

[Delegate Call](https://solidity-by-example.org/delegatecall/)






<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
</div>