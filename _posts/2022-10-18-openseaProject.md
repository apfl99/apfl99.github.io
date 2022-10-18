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



## Opensea Clone Coding 프로젝트 요구사항 제안

- Ethereum network Goeril 한정



### Bare minimum requirements

---

- 메타마스크로 로그인, 로그아웃
  - web3에서 사용자를 식별하기 위함
  - 이에 굳이 DB에 사용자의 아이디, 비밀번호 등 개인정보는 저장하지 않아도 된다고 생각.
- NFT 프로젝트(contract) 리스팅
  - 전체적인 NFT 프로젝트를 가져와서 보여주기 위함
  - 페이지 렌더링마다 노드와 통신하면 너무 느리기 때문에 DB에 어느정도 저장해야 한다고 생각
- NFT 프로젝트가 발행한 NFT 리스팅
  - NFT 프로젝트 조회시, 하단에 해당 프로젝트는 어떤 NFT를 발행하는지 NFT 리스팅
  - 페이지 렌더링마다 노드와 통신하면 너무 느리기 때문에 DB에 어느정도 저장해야 한다고 생각
- NFT 상세 보기
  - NFT에 대한 이미지, 설명, 발행 컨트랙, 토큰 ID 등 확인하기 위함
  - 페이지 렌더링마다 노드와 통신하면 너무 느리기 때문에 DB에 어느정도 저장해야 한다고 생각
- 검색 기능 : Items, collections, account로 NFT나 프로젝트(contract) 조회
  - 사용자가 특정 NFT나 프로젝트를 조회하기 위해 사용
- NFT mint
  - NFT 발행을 위함
  - 파일, 이름, 설명, 양 필수(최소)
- 개인 페이지(계정 주소 기반)
  - 로그인한 계정과 해당 계정이 소유한 NFT 확인을 위함



### Advanced

---

- NFT 프로젝트(contract) 리스팅 시, 최근 가장 많이 발행된(Trending), 현재까지 가장 많이 발행된(Top)으로  Top 10 보여주기
- 관심있는 NFT 등록 및 개인페이지에서 보여주기





### Nightmare

---

- NFT 거래(contract 필요)
  - 판매 : 개인 페이지에서 판매 등록, 판매 등록 시, 거래를 위한 컨트랙에 해당 컨트랙이 해댱 토큰에 대한 권한을 넘겨받을 수 있도록(allowance), 판매 등록 취소
  - 구매 : 검색, 리스팅 view를 통해 본 NFT에서 판매중이라면 구매 가능
  - 장바구니
    - 구매를 고민중인 상품을 담기 위함