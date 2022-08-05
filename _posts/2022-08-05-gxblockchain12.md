---
title: "[블록체인 상태와 트랜잭션] 블록체인별 트랜잭션"
categories:
 - GroundX
tags: [Transaction Code] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **트랜잭션 예시**
-------------------

![img](../../images/2022-08-05-gxblockchain12/img-20220805132530329.png)
{: .align-center}

- nonece : 어카운트 기반 블록체인 특징, 트랜잭션의 중복 전송을 방지, 어카운트가 몇번째 트랜잭션을 보내는지에 대한 seq num, 병렬화와 관련 있다.
- value : 어카운트가 보내는 트랜잭션의 수, 블록체인마다 다름

## **Ethereum 트랜잭션 예시**

![img](https://blog.kakaocdn.net/dn/9AK6p/btrlY4HuAsV/jmNXzJQaw4qIK5jwZG16TK/img.png)
{: .align-center}

- from이 없음 : 서명으로 공개키를 도출하기 때문에
- gas : 몇 개의 가스
- gasPrice : 하나의 가스 가격
  - Gas * gasPrice -> 총 수수료

- v, r, s -> 전자 서명
  - v : 식별자, 서명은 아님

## **Klaytn 트랜잭션 예시**

![img](../../images/2022-08-05-gxblockchain12/img-20220805132530299.png)
{: .align-center}

- type : 트랜잭션의 실행 종류, 보통은 to가 생략되면 트랜잭션이 뭘 수행하고자 하는지 파악 불가, 좀 더 명시적
- gas, gasPrice가 있긴 하지만 고정값이다.(합의 알고리즘에 의해 변경도 가능)


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>